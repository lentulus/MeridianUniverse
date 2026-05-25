# Colony Simulator — Design Document

**Files in this package**

| File | Role |
|------|------|
| `colony-sim.html` | Self-contained browser app — simulation engine + UI + chart |
| `colony_export.py` | CLI tool — queries Meridian Parquet DB, outputs world-parameter JSON |
| `colony-model.md` | Mathematical reference — full equation inventory with derivations |

---

## Purpose

This system simulates the long-term growth and political stability of a settler
colony on a habitable world many parsecs from Earth. It is designed as a
referee tool for the *Worlds* wargame campaign: the simulator runs between
sessions to advance the colony's state, and the outputs (Political Stress
Indicator, Citizen Loyalty, debt load, etc.) feed into scenario generation and
NPC disposition.

Time unit throughout is **months**, matching the *World Tamer's Handbook*
(WTH) monthly turn cycle. The model integrates three source frameworks:

- **WTH** (TNE 0311) — four-sector colonial economy, satisfaction indices,
  Political Track, monthly output roll
- **GURPS Realm Management** — Realm Value, revenue, disruption events,
  Management Skill, Habitability, Education Rating
- **Turchin 2013** ("Modeling Social Pressures Toward Political Instability",
  *Cliodynamics* 4.2) — structural-demographic Political Stress Indicator (PSI),
  elite accumulation ODEs, wage model

---

## colony_export.py

### What it does

Reads the Meridian static Parquet database (`~/projects/meridian/MeridianData`)
and converts the physical properties of a selected world into a JSON file that
can be imported directly into `colony-sim.html`. This is the bridge between
the star-system database and the simulator.

### Dependency

Requires `duckdb`. Since `duckdb` lives in the Meridian project venv, the
easiest invocation is:

```
cd ~/projects/meridian
uv run python /path/to/colony_export.py --system Sol --index 3
```

The `MERIDIAN` constant at the top of the script is the only path that needs
updating when moving the tool.

### Modes

**`--system NAME`**
Lists every body in the named system that has physical data in Meridian
(HZ-eligible bodies only, per Meridian's coverage policy). Output is a
table with all derived colony parameters visible before selecting.

```
python colony_export.py --system Sol
```

```
   #  body_id     system            world_type               atmosphere                       Hab  RVM   R_A   phi     p0  lmin×  season_mo
   1   165930  Sol               Tiny (Rock)               None                               0    0   1.00  0.25  0.020    3.5        2.9
   2   165931  Sol               Standard (Greenhouse)     Superdense/Suffocating/...        -2    1   1.00  0.78  0.030    3.0        7.4
   3   165932  Sol               Standard (Garden)         Standard/Breathable                8    0   3.10  0.62  0.035    1.0       12.0
   4   165933  Sol               Small (Rock)              None                               0    0   1.00  0.60  0.020    3.5       22.6
```

**`--nearest-garden`**
Finds the closest Garden worlds to Sol within a given radius and habitability
floor, sorted by distance.

```
python colony_export.py --nearest-garden --max-dist-pc 15 --min-habitability 5
```

**`--index N`**
Added to either mode: emits one body as JSON to stdout instead of the table.
Pipe or redirect to a `.json` file for import into the simulator.

```
python colony_export.py --system Sol --index 3 > earth.json
python colony_export.py --nearest-garden --min-habitability 5 --index 2 > target.json
```

### Meridian tables queried

```
systems/   — system_id, x_mpc/y_mpc/z_mpc, primary_name, dist_pc
bodies/    — body_id, system_id, body_type, orbit_primary_au, in_hz, mass_kg
physical/  — body_id, system_id, world_type, atmosphere_code, hydrographics,
             avg_surface_temp_k, climate_type, volcanic_activity, tectonic_activity,
             rotation_period_h, axial_tilt_deg, tidally_locked, rvm, habitability, affinity
```

The JOIN condition `p.system_id = s.system_id` is required on the physical
table in addition to `p.body_id = b.body_id` because body_ids are
sector-local sequential integers in the current Meridian build (they are not
globally unique across sectors). The system_id IS globally unique.

### Mapping formulas

| Meridian field | JSON key | Formula |
|---|---|---|
| `habitability` (0–19) | `Hab` | 1:1 |
| `rvm` (−3…+5) | `qM_mult` | `2^(rvm/3)` — RVM 0 → 1×, −3 → 0.25×, +3 → 2.83× |
| `hydrographics`, `world_type`, `avg_surface_temp_k` | `RA` | Garden: `clamp(1 + (hydro/100)×3×temp_factor, 1, 5)`; Ocean: ×1.5; other: 1.0; `temp_factor=1` if 5–35 °C, degrades outside |
| `axial_tilt_deg`, `tidally_locked` | `phi_min` | Locked: 0.25; else `clamp(0.80 − tilt/90×0.70, 0.10, 0.90)` |
| `atmosphere_code` | `lmin_mult` | Breathable: 1.0; Marginal: 1.3; Suffocating: 1.7; Highly Toxic: 2.2; Lethally Toxic/Corrosive: 3.0; None/Trace: 3.5 |
| `volcanic_activity`, `tectonic_activity` | `p0` | `0.020 + Δvol + Δtec`; delta: None=0, Light=0.005, Moderate=0.010, Heavy=0.020, Extreme=0.040 |
| `orbit_primary_au` | `season_period_months` | `orbit_au^1.5 × 12` (Kepler, assumes FGK primary) |

### JSON output format

```json
{
  "_world": {
    "system": "Sol",
    "body_id": 165932,
    "world_type": "Standard (Garden)",
    "atmosphere": "Standard/Breathable",
    "climate": "Cool",
    "hydrographics": 70,
    "avg_surface_temp_c": 14.9,
    "axial_tilt_deg": 23.44,
    "rotation_period_h": 23.9,
    "tidally_locked": false,
    "volcanic_activity": "Light",
    "tectonic_activity": "Moderate",
    "rvm": 0,
    "habitability": 8,
    "affinity": 8,
    "orbit_au": 1.0
  },
  "Hab": 8,
  "RA": 3.1,
  "phi_min": 0.62,
  "lmin_mult": 1.0,
  "p0": 0.035,
  "qM_mult": 1.0,
  "season_period_months": 12.0
}
```

`_world` is metadata for display; the remaining keys are simulator parameters.

---

## colony-sim.html

A single self-contained HTML file. No server, no build step. Open in any
modern browser. Requires internet access on first load only (fetches
Chart.js 4.4 from CDN).

### State

The simulation maintains two data structures:

- **`history`** — array of monthly snapshots, one entry per step taken.
  Each snapshot holds all derived outputs (N, G, w, Ψ, CL, etc.) and
  internal carry-forwards (`_state`, `_Psi`, `_e_new`, `_Y_new`) needed to
  advance to the next month.
- **`params`** — read fresh from the input fields on every step, so parameter
  changes mid-run take effect immediately (simulating policy interventions).

### Monthly step sequence

Each step calls two functions:

1. **`advanceState(prevSnap, params)`** — takes the previous snapshot and
   produces a new bare state `{t, N, E, AC, IC, MC, PC, AL, IL, ML, Y, CL, PT, H}`.
   This is where stochastic events fire: the d20 Political Roll, the disruption
   probability check, and population growth.

2. **`computeSnapshot(state, history, params)`** — takes the bare state and
   computes all derived quantities for charting and for feeding the next step.
   This is deterministic given the state and params (except for the output roll η
   which is drawn here).

The snapshot returned by `computeSnapshot` is appended to `history`, the chart
is redrawn, and the state row at the bottom of the screen is updated.

### Parameter groups (sidebar)

| Section | Key parameters |
|---------|---------------|
| Initial State | Starting N, E, capital units (AC/IC/MC), power (PC), labor assignments (AL/IL/ML), debt Y, Citizen Loyalty CL, Political Track PT, housing H |
| Population & Planet | Annual growth rate r₀, working fraction λ, Habitability Hab, nutrition stress exponent κN, agricultural richness RA, growing season floor phi_min, youth fraction A₂₀₋₂₉, settlement fraction f_set, season cycle length (months/orbit) |
| Economy | Sector yields q_A/q_I/q_M, minimum subsistence levels r_min/l_min, prices p_A/p_M/p_exp, materials export fraction, power demand per capital unit, Earth subsidy |
| Labor & Elite | Max elite fraction s_max, mobility coefficient μ₀, welfare threshold w₀, wage elasticity β, Education Rating ER, base labor productivity P₀ |
| Governance | Control Rating CR (2–6), Management Skill MS (−4…+4), loyalty gain per PT step α_PT, loyalty erosion per PSI unit α_Ψ, elite salary c_e, per-capita admin cost c_adm |
| Disruptions | Base disruption probability p₀, PSI sensitivity exponent κ, base response time τ₀, disruption severity |
| Capital Values | Credit value per unit of AC/IC/MC (for maintenance cost calculation) |

### Simulation equations (summary)

**Production** (per month, all sectors follow the same formula):

```
M_x = L_x                                    if L_x < C_x   (labor-limited)
M_x = min(C_x + 0.5·(L_x − C_x), 1.5·C_x)  if L_x ≥ C_x   (capital-limited)
Q_A = q_A · M_A · R_A · φ(t) · η · pf       [rations/mo]
Q_I = q_I · M_I · η · pf                     [cr/mo]
Q_M = q_M · M_M · η · pf                     [t/mo]
```

Where `φ(t) = phi_min + (1−phi_min)·0.5·(1+sin(2π(t−season_months/4)/season_months))`
is the seasonal modifier, `η ~ Uniform(0.80, 1.20)` is the WTH output roll,
and `pf = min(1, PC/P_req)` is the power availability factor.

**Satisfaction and welfare:**
```
S_N = Q_A / (N · r_min)     [nutrition: 1.0 = adequate]
S_S = H / N                  [shelter:   1.0 = everyone housed]
S_L = Q_I / (N · l_min)     [living:    1.0 = minimum standard]
w   = (S_N + S_S + S_L) / 3  [composite welfare; w₀ = 1.0 is the neutral threshold]
```

**Elite dynamics** (Turchin Eq. 4 simplified):
```
ė = μ₀ · (w₀ − w) / w
ε = (1 − w·λ) / e            [relative elite income]
```

When w < 1 (below threshold), elites accumulate. When e grows past s_max·N,
each excess elite is a frustrated aspirant contributing to EMP.

**Political Stress Indicator** (Turchin Eq. 14 adapted):
```
MMP = w⁻¹ · f_set · A_young
EMP = ε⁻¹ · (E / s_max·N)
SFD = (Y / G_monthly) · (1 − CL)
Ψ   = MMP × EMP × SFD
```

**Fiscal dynamics** (monthly):
```
R_rev  = w_col · N · RF · 0.6          [RF from CR table]
C_gov  = c_e · E + c_adm · N
C_maint = 0.00417 · Σ(C_x · v_x)      [from month 120 only]
T_rev  = p_exp · X(t−24)               [trade revenue, 24-month lag]
ΔY     = C_gov + C_maint − R_rev − T_rev − I_Earth
```

**Population growth** (monthly):
```
ΔN = (r₀/12) · h(Hab) · σ(S_N) · N
```
where `h(Hab) = Hab/10` and `σ(S_N) = min(S_N, 1)^κN`.

**Stochastic events** (per step):
- **Political Roll**: d20 + PT + MS → PT improves/degrades ±1 at extremes
- **Disruption**: probability `p_dis = p₀ · Ψ^κ`; types (Plague, Famine,
  Corruption, Political Schism, Troublesome Flora/Fauna) have different
  effects on N, CL, PT
- **Citizen Loyalty update**: `CL += α_PT·ΔPT − α_Ψ·Ψ + CL_shock`

### Outputs tracked (chart series)

| Group | Keys |
|-------|------|
| Population | N, E, e (elite fraction), A_young |
| Welfare | w, S_N, S_S, S_L |
| Economy | G (annual GDP), Q_A, Q_I, Q_M, T_export, R_rev |
| Fiscal | Y (debt), Y_G (debt/monthly-GDP ratio) |
| Politics | Ψ (PSI), MMP, EMP, SFD, CL, PT, D (distrust) |
| Elites | ε (elite income), overproduction (E/S_pos) |

Any combination can be plotted simultaneously. Two chart options:
- **Normalize to t=0** — indexes all selected series to 1.0 at month 0,
  enabling growth-rate comparison across differently-scaled outputs
- **Log Y axis** — useful when population and PSI are on the same chart

### World import (Load World button)

Accepts a `.json` file produced by `colony_export.py`. On load it sets:

| JSON key | Simulator input |
|----------|----------------|
| `Hab` | Habitability (0–19) |
| `RA` | Agricultural richness (1–5) |
| `phi_min` | Growing season floor |
| `lmin_mult` | l_min × multiplier (life-support cost) |
| `p0` | Disruption base probability |
| `qM_mult` | q_M × multiplier (RVM effect on materials yield) |
| `season_period_months` | Season cycle length (replaces hardcoded 12) |

A world info bar appears at the top of the main panel showing the system name,
world type, temperature, atmosphere, habitability, RVM, axial tilt, and orbit.

Parameters not set by the import (population, capital, labor allocation,
debt, etc.) retain their current sidebar values. The user can adjust those
before clicking Reset to start a fresh run on the new world.

---

## Key feedback loops

1. **Low-welfare spiral** — w↓ → ė↑ → e↑ → ε↓ → EMP↑ → Ψ↑ → CL↓ → D↑ → SFD↑ → Ψ↑↑
2. **Fiscal trap** — Y↑ → Y/G↑ → SFD↑ → Ψ↑ → disruptions↑ → production↓ → G↓ → Y/G↑↑
3. **Elite saturation** — w↓ → mobility↑ → E > S_pos → EMP↑ → Ψ↑
4. **Trade-lag oscillator** — surplus at t → exports → revenue arrives at t+24; conditions
   may have reversed in the interval, creating a fiscal lag cycle
5. **Stabilising loop** — good PT rolls → PT↑ → CL↑ → D↓ → SFD↓ → Ψ↓

---

## Calibrated defaults

| Parameter | Default | Source |
|-----------|---------|--------|
| r₀ | 0.02 y⁻¹ | WTH (2% annual growth) |
| λ | 0.55 | Working-age population fraction |
| s_max | 0.05 | GURPS RM (1 elite per 20 colonists) |
| μ₀ | 0.005 mo⁻¹ | Turchin, rescaled |
| w₀ | 1.0 | Neutral welfare threshold |
| β | 0.5 | Turchin Eq. 9 wage elasticity |
| Capital maintenance | 0.417%/mo from month 120 | WTH (5%/yr, starts year 10) |
| Trade lag Δ | 24 months | Setting (1-year transit each way) |
| RF (CR 4) | 0.20 | GURPS RM Revenue Factor table |
| Disruption p₀ | 0.02/mo | GURPS RM, at Ψ = 1 |

---

## Transfer checklist for Worlds project

- [ ] Copy `colony-sim.html`, `colony_export.py`, `colony-model.md` into Worlds
- [ ] Update `MERIDIAN` constant in `colony_export.py` if Meridian path changes
- [ ] `colony-sim.html` has no external dependencies except Chart.js from CDN —
      it works offline once that script is cached (or bundle it locally)
- [ ] `colony_export.py` requires `duckdb`; add to Worlds project dependencies
- [ ] `colony-model.md` is the mathematical reference; no code depends on it
- [ ] The `_world` metadata block in the exported JSON is display-only;
      the simulation uses only the six parameter keys (`Hab`, `RA`, `phi_min`,
      `lmin_mult`, `p0`, `qM_mult`, `season_period_months`)

---

## Source references

| Tag | Full citation |
|-----|--------------|
| **3turchin** | Turchin, P. (2013). "Modeling Social Pressures Toward Political Instability." *Cliodynamics* 4(2): 241–280 |
| **WTH** | *World Tamer's Handbook*, TNE 0311 (GDW). Ch. 4 (Colonial Economics), Ch. 5 (Monthly Turn) |
| **GURPS RM** | *GURPS Realm Management* (Steve Jackson Games). Ch. 1–2 (Realm Value, Revenue, Disruptions, RTM, Habitability, Education Rating) |
| **Meridian** | Static Parquet reference DB, `~/projects/meridian`. ATHYG v3.3 + synthetic bodies, GURPS Space 4e world-physical generation rules |
