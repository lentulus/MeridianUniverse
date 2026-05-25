# Colony Growth and Development Model

A coupled mathematical model integrating:
- **WTH** = *World Tamer's Handbook* (TNE 0311) — colonial economic sectors and political mechanics
- **GURPS RM** = *GURPS Realm Management* — governance, revenue, and disruption framework
- **3turchin** = Turchin 2013 — structural-demographic political stress equations

**Setting:** SF colony on a habitable world ~1 year travel time from Earth (many parsecs). Colonists are functionally isolated between supply convoys. Time unit throughout: **months** (matching WTH's monthly turn cycle). Distances and masses in SI units; temperature in °C (human-scale); currency in colony credits [cr].

---

## State Variables

| Symbol | Meaning | Units |
|--------|---------|-------|
| N(t) | total colony population | persons |
| E(t) | elite population (administrators, officers, technical specialists) | persons |
| e(t) = E/N | relative elite fraction | dimensionless |
| AC(t) | agricultural capital units deployed | capital-units |
| IC(t) | industrial capital units deployed | capital-units |
| MC(t) | materials capital units deployed | capital-units |
| PC(t) | power capacity installed | MW |
| AL(t) | agricultural laborers assigned | persons |
| IL(t) | industrial laborers assigned | persons |
| ML(t) | materials laborers assigned | persons |
| D_lab(t) | labor demand (full-economy analog) | persons |
| Y(t) | cumulative colony debt to Earth | cr |
| CL(t) | Citizen Loyalty index | dimensionless ∈ [0, 1] |
| PT(t) | Political Track level | integer ∈ {−3,…,+3} |
| w(t) | relative welfare index | dimensionless |
| ε(t) | relative elite income | dimensionless |
| Ψ(t) | Political Stress Indicator | dimensionless |

---

## I — Population Dynamics

### Total population ODE

> **Ṅ = r₀ · h(Hab) · σ(S_N) · N + I(t)**

| Symbol | Meaning | Units |
|--------|---------|-------|
| r₀ | base annual growth rate = 0.02 y⁻¹ = 1/600 mo⁻¹ | mo⁻¹ |
| h(Hab) | GURPS RM habitability modifier (see §V) | dimensionless |
| σ(S_N) | nutrition stress modifier (see §III) | dimensionless |
| I(t) | immigration inflow from Earth | persons/month |

### Immigration

Colonist waves are requested at time t_req and arrive after a one-way transit of Δ = 12 months:

> **I(t) = Σ_k I_k · δ(t − t_{k,req} − 12)**

No communication faster than transit exists; a wave, once dispatched, cannot be recalled. The colony administrator's only levers before the wave lands are requests sent 12+ months earlier.

### Population compartments

> **N_w = λ · N** (working-age population, λ ≈ 0.55)
>
> **N_c = N_w − E** (common workers available for sector assignment)
>
> **AFL = AL + IL + ML** (assigned full laborers; AFL ≤ N_c)

---

## II — Economic Sector Model

### Effective production factor M (WTH formula, all sectors)

Given capital C_x and assigned labor L_x for sector x:

> **M_x(t) = L_x(t)** if L_x < C_x  (labor-limited)
>
> **M_x(t) = min(C_x + 0.5·(L_x − C_x), 1.5·C_x)** if L_x ≥ C_x  (capital-limited with surplus-labor bonus)

This encodes WTH's "output multiplier = AC + 0.5×[AL−AC]" rule. Extra labor beyond capital yields half-rate marginal output, capped at 1.5× the capital level.

### Power constraint

> **P_req(t) = p_A · AC(t) + p_I · IC(t) + p_M · MC(t)**

If P_avail(t) < P_req(t), production in all underpowered sectors is penalized by factor P_avail / P_req. (Power deficit is a hard constraint; the colony must install capacity before capital can operate at full rate.)

### Agricultural output

> **Q_A(t) = q_A · M_A(t) · R_A · φ(t) · η(t) · min(1, P_avail/P_req)**

| Symbol | Meaning | Units |
|--------|---------|-------|
| Q_A | rations produced | rations/month |
| q_A | base rations per effective capital unit per month | rations/capital-unit/month |
| R_A | planetary richness rating (from survey, 1–5) | dimensionless |
| φ(t) | growing-season modifier ∈ [0.5, 1.0] (annual cycle) | dimensionless |
| η(t) | output roll multiplier ~ Uniform(0.80, 1.20) (WTH d20 roll) | dimensionless |

### Industrial output

> **Q_I(t) = q_I · M_I(t) · η(t) · min(1, P_avail/P_req)**

| Symbol | Meaning | Units |
|--------|---------|-------|
| Q_I | credits produced | cr/month |
| q_I | base credits per effective capital unit per month | cr/capital-unit/month |

Industrial output is the colony's monetary economy. It funds capital purchases, imports, debt service, and government operations.

### Materials output

> **Q_M(t) = q_M · M_M(t) · η(t) · min(1, P_avail/P_req)**

| Symbol | Meaning | Units |
|--------|---------|-------|
| Q_M | raw materials extracted | tonnes/month |
| q_M | base tonnes per effective capital unit per month | t/capital-unit/month |

Materials feed capital construction (internal) or export (external).

### GDP equivalent

> **G(t) = 12 · [p_A · Q_A(t) + Q_I(t) + p_M · Q_M(t)]**

Units: cr/year. p_A [cr/ration] and p_M [cr/tonne] are shadow prices converting physical outputs to a common credit value. G(t) serves as the colony's GDP analog, driving all Turchin equations that require G.

---

## III — Satisfaction Indices and Relative Wage

### Standard of Nutrition

> **S_N(t) = Q_A(t) / (N(t) · r_min)**

| Symbol | Meaning | Units |
|--------|---------|-------|
| r_min | minimum adequate rations per person per month | rations/person/month |

S_N = 1 → adequate. S_N < 1 → shortfall (starvation risk). S_N > 1 → surplus available for export.

Nutrition stress modifier feeding population growth:
> **σ(S_N) = min(S_N, 1)^κ_N** with κ_N > 0

When S_N ≥ 1, σ = 1 (no penalty). When S_N < 1, growth rate suppressed and mortality rises.

### Standard of Shelter

> **S_S(t) = H(t) / N(t)**

where H(t) = housing capacity [berths], grows through construction capital deployment.

S_S = 1 → every colonist housed. S_S < 1 → overcrowding.

### Standard of Living

> **S_L(t) = Q_I(t) / (N(t) · l_min)**

| Symbol | Meaning | Units |
|--------|---------|-------|
| l_min | minimum adequate credits per person per month (goods, services) | cr/person/month |

### Composite relative welfare (Turchin w-analog)

> **w(t) = [S_N(t) + S_S(t) + S_L(t)] / 3**

This maps WTH's three satisfaction indices to the dimensionless relative wage w used throughout Turchin's structural-demographic equations. When all three standards sit at adequacy, w = 1 = w₀ (the neutral mobility threshold).

---

## IV — Labor Market and Wages

### Labor demand (monthly operational)

> **D_lab(t) = G(t) / (2000 · 12 · P_prod(t))**

Adapting Turchin's contemporary-US proxy (Eq. 14 source variable), where P_prod [cr/hour] is colony labor productivity (grows with Education Rating ER: P_prod = P_0 · (1 + ER/6)).

Labor supply: S_lab(t) = λ · N(t)

### Wage (Turchin Eq. 9 / 10 adapted)

> **W(t+τ) = á · (G(t)/N(t))^α · (D_lab(t)/S_lab(t))^β · C(t)^γ**

> **w(t+τ) = W(t+τ) / (G(t)/N(t))**

| Symbol | Meaning | Units |
|--------|---------|-------|
| τ | wage-stickiness lag (contract cycles) | months |
| á | scale parameter | dimensionless |
| α | GDP-per-capita elasticity | dimensionless (≈ 1) |
| β | labor-market elasticity | dimensionless (≈ 0.5) |
| C(t) | colonial authority coercive factor (charter rules, CR) | dimensionless |
| γ | coercion elasticity | dimensionless |

**Note:** In a colonial setting C(t) is non-trivial — the charter company can suppress wages through contract labor laws. C(t) is indexed to the colony's Control Rating (GURPS RM CR): higher CR → lower C → wage suppression.

The w computed here should converge with the satisfaction-based w(t) from §III; in practice use the satisfaction index as the observable proxy and this equation as the predictive mechanism.

---

## V — Habitability and Planetary Parameters (GURPS RM)

### Habitability modifier

GURPS RM rates habitability 0 (Disastrous) to 19 (Excellent). Define:

> **h(Hab) = Hab / 10** (normalized, 0 → 0, ≈ 10 → 1, 19 → 1.9)

Habitability affects:
- Population growth: via h(Hab) in §I
- Agricultural carrying capacity: K_A = K₀ · h(Hab) (more habitable → more arable land)
- Agricultural richness: R_A partially determined by Hab

### Education Rating modifier on labor productivity

GURPS RM Education Rating ER ∈ [0, 6]:
> **P_prod(t) = P_0 · (1 + ER(t)/6)**

ER grows slowly as the colony builds schools; it determines the productivity of each labor-unit and thus real labor demand.

### Control Rating and the coercive wage factor

| CR | C(t) | Interpretation |
|----|------|----------------|
| 2 | 1.3 | Free society; wages set by market |
| 3 | 1.1 | Mild regulation |
| 4 | 1.0 | Standard colonial charter |
| 5 | 0.85 | Heavy state control |
| 6 | 0.70 | Near-mandatory labor |

---

## VI — Elite Dynamics (Turchin Eqs. 4 / 13)

### Elite population ODE

> **Ė(t) = r_e · E(t) + μ₀ · (w₀ − w(t)) / w(t) · N(t)**

Simplified (assuming r_e ≈ r₀ so e = E/N evolves independently of N):

> **ė(t) = μ₀ · (w₀ − w(t)) / w(t)**

| Symbol | Meaning | Units |
|--------|---------|-------|
| r_e | elite natural growth rate ≈ r₀ | mo⁻¹ |
| μ₀ | social mobility rate parameter | mo⁻¹ |
| w₀ | mobility-neutral welfare threshold = 1 | dimensionless |

When w < 1 (depressed welfare), upward mobility accelerates → elite numbers swell. This is the core instability mechanism: poor colony conditions produce more people seeking elite positions than the administrative structure can absorb.

**Colony-specific modification:** elite positions are fixed by the administrative org-chart. Available slots S_pos(t) = s_max · N(t) where s_max is the colonial charter's maximum administrative ratio (e.g., 0.05 for 1 elite per 20 colonists).

When E(t) > S_pos(t): frustrated elites are politically dangerous (high EMP).

### Relative elite income (Turchin Eq. 5)

> **ε(t) = (1 − w(t)λ) / e(t)**

| Symbol | Meaning | Units |
|--------|---------|-------|
| λ | labor force participation ≈ 0.55 | dimensionless |

When e(t) is large, total elite income is diluted across more recipients → each elite gets less → ε falls → ε⁻¹ rises → EMP rises. The squeeze feeds political restlessness even when the colony is nominally prosperous.

---

## VII — Political Stress Indicator (Turchin PSI adapted)

### MMP — Mass Mobilization Potential

> **MMP(t) = w(t)⁻¹ · f_set(t) · A_young(t)**

| Symbol | Meaning | Units |
|--------|---------|-------|
| f_set(t) = N_set/N | fraction of population in permanent settlements vs. dispersed field teams | dimensionless |
| A_young(t) | fraction of colony population aged 20–29 | dimensionless |

*Rationale for f_set:* Turchin uses urbanization N_urb/N as the proxy for mobilization density. In a young colony the analog is how concentrated the population is in the central settlement; dispersed field teams are hard to organize for collective action.

### EMP — Elite Mobilization Potential

> **EMP(t) = ε(t)⁻¹ · (E(t) / S_pos(t))**

When elites are overproduced relative to available positions (E > S_pos), both terms amplify each other: income is diluted *and* there are more frustrated aspirants per slot.

### SFD — State Fiscal Distress

> **SFD(t) = (Y(t) / G(t)) · D(t)**

> **D(t) = 1 − CL(t)**

| Symbol | Meaning | Units |
|--------|---------|-------|
| Y(t) | cumulative debt owed to Earth (or charter company) | cr |
| G(t) | annual GDP equivalent | cr/year |
| Y/G | debt-to-annual-output ratio | dimensionless |
| D(t) | public distrust = inverse of Citizen Loyalty | dimensionless ∈ [0, 1] |

A colony that is both heavily indebted to Earth *and* has low loyalty to the colonial administration scores high on SFD regardless of its nominal economic size.

### Full PSI

> **Ψ(t) = w(t)⁻¹ · f_set(t) · A_young(t) · ε(t)⁻¹ · (E(t)/S_pos(t)) · (Y(t)/G(t)) · D(t)**

This is the direct colony analog of Turchin's Eq. 14, with:
- w⁻¹ from composite satisfaction §III
- f_set replacing N_urb/N
- A_young from census data
- ε⁻¹, E/S_pos from elite dynamics §VI
- Y/G from fiscal equation §VIII
- D(t) from Citizen Loyalty §IX

---

## VIII — Fiscal Dynamics

### Revenue (GURPS RM)

> **R_rev(t) = w_col(t) · N(t) · RF · 0.6**

| Symbol | Meaning | Units |
|--------|---------|-------|
| w_col | average monthly credit-income per colonist | cr/person/month |
| RF | Revenue Factor (function of CR, from GURPS RM table) | dimensionless |

RF values: CR 2 → 0.05; CR 3 → 0.10; CR 4 → 0.20; CR 5 → 0.30; CR 6 → 0.40.

w_col ≈ Q_I(t) / N(t) + shadow value of rations received (≈ p_A · r_adequate).

### Capital maintenance cost (WTH)

> **C_maint(t) = 0.00417 · [AC(t) · v_AC + IC(t) · v_IC + MC(t) · v_MC] · Θ(t − t_{deploy} − 120)**

where v_x is the credit value per capital unit, 0.00417 ≈ 5%/year ÷ 12, and Θ is the Heaviside step function (maintenance only starts in month 120 = year 10 of colony operation, per WTH rules).

### Government operating expenditure

> **C_gov(t) = c_e · E(t) + c_admin · N(t)**

Administrative costs scale with both elite headcount and total population (basic services).

### Debt dynamics

> **Ẏ(t) = [C_gov(t) + C_maint(t)] − R_rev(t) − T_rev(t) + I_Earth(t)**

| Symbol | Meaning | Units |
|--------|---------|-------|
| T_rev(t) | trade revenue received from Earth (lagged by Δ = 24 months round-trip) | cr/month |
| I_Earth(t) | Earth subsidies/investment inflow | cr/month |

I_Earth is policy-determined and subject to Earth-side political decisions that the colony cannot influence on timescales shorter than 12 months.

---

## IX — Off-world Trade and Supply-Chain Lag

### Export tonnage (WTH formula)

> **X(t) = Q_A_sur(t) / 100 + Q_M_sur(t) / 20** [displacement-tonnes/month]

| Symbol | Meaning | Units |
|--------|---------|-------|
| Q_A_sur = max(0, Q_A − N·r_min) | surplus rations above subsistence | rations/month |
| Q_M_sur | materials surplus after internal construction use | tonnes/month |

### Trade revenue with transit lag

> **T_rev(t) = p_exp · X(t − 24)**

The round-trip lag of 24 months (12 out, 12 back for payment) means the colony's current export decisions do not yield revenue until 2 years later. This lag couples the trade loop to fiscal dynamics in a way that can produce oscillations: a production surplus today builds debt for 2 years before the credits arrive.

**Import constraint:** the colony can only import what fits in the return voyage. Import capacity C_imp [displacement-tonnes/convoy] is a hard upper bound on capital imports per cycle.

---

## X — Citizen Loyalty and Governance Dynamics (GURPS RM)

### Political Track update (WTH monthly roll)

At each monthly step, draw:
> **P_roll(t) ~ DiscreteUniform(1, 20) + PT(t) + MS_adj**

| Symbol | Meaning |
|--------|---------|
| PT(t) | current Political Track level ∈ {−3,…,+3} |
| MS_adj | administrator Management Skill modifier |

Outcome:
- P_roll > 15 → PT → PT + 1 (capped at +3)
- P_roll < 5 → PT → PT − 1 (floored at −3)
- 5 ≤ P_roll ≤ 15 → no change

### Citizen Loyalty dynamics

> **CL(t+1) = clamp( CL(t) + α_PT · ΔPT(t) − α_Ψ · Ψ(t) · Δt + α_dis · ΔCL_dis(t), 0, 1 )**

| Symbol | Meaning |
|--------|---------|
| α_PT | loyalty gain per Political Track improvement step |
| α_Ψ | loyalty erosion per unit PSI per month |
| α_dis | loyalty shock from disruptions (negative) |

Distrust feeds back into PSI through D(t) = 1 − CL(t), creating a self-reinforcing loop: high Ψ erodes loyalty → D rises → SFD rises → Ψ rises further. This is the colonial analog of Turchin's instability spiral.

### GURPS RM Realm Value

> **V(t) = w_col(t) · N(t) · 0.6**

Realm Value is not a direct driver in the ODE system; it serves as a summary statistic representing the colony's strategic worth to Earth (which determines I_Earth and immigration willingness).

---

## XI — Disruptions (GURPS RM / Stochastic Shocks)

### Disruption probability

> **p_dis(t) = p₀ · Ψ(t)^κ**

High PSI periods are more disruption-prone. At each monthly step, if u ~ Uniform(0,1) < p_dis(t), draw a disruption type from the GURPS RM disruption table.

### Disruption effects (selected)

| Disruption | Effect on state variables |
|------------|--------------------------|
| Plague | N → N · (1 − δ_plag); CL ↓ by δ_plag_CL |
| Drought / Famine | Q_A → Q_A · (1 − δ_fam) for τ_fam months; R_A temporarily ↓ |
| Corruption | R_rev → R_rev · (1 − δ_cor); Y ↑ by diversion amount |
| Political Schism | Ψ spike (add δ_sch to Ψ for τ_sch months); PT − 2 |
| Trade Embargo | T_rev = 0 for τ_emb months; I_Earth = 0 if supply convoy halted |
| Troublesome Flora/Fauna | K_A ↓ (carrying capacity reduced until extirpation campaign) |
| Pollution | h(Hab) temporarily ↓; N growth rate suppressed |

### Response-time modifier (GURPS RM)

Disruption effects decay at rate governed by the administrator's Management Skill MS and Technology Level TL:

> **τ_resp = τ₀ / (1 + MS/2)**

A disruption shock X_dis decays as:
> **X_dis(t) = X_dis(0) · exp(−t / τ_resp)**

Higher MS → shorter response time → disruptions clear faster.

---

## XII — Capital Investment Dynamics

### Capital purchase constraint

Capital is purchased with industrial credits and imported from Earth:

> **ΔC_x(t) = min(credits_available, import_capacity_allocation) / v_x**

Capital units purchased at time t arrive Δ = 12 months later (one-way transit). Newly deployed capital begins operation in month t + 12 and begins incurring maintenance in month t + 12 + 120.

### Capital depreciation

> **Ċ_x(t) = −δ_cap · C_x(t)** (ongoing wear beyond maintenance replacement)

If maintenance is not funded (credits insufficient), depreciation accelerates.

---

## XIII — Complete Dependency Structure

```
Planetary survey ──► R_A, Hab, K_A
GURPS RM Hab ──────► h(Hab) ──────────────────────────────► Ṅ
                                                               │
N(t) ──────────────────────────────────────────────────────► N_w, N_c, S_lab
                                                               │
AC, AL ──► M_A ──► Q_A ──► S_N ─────────────────────────────┐│
IC, IL ──► M_I ──► Q_I ──► S_L, G, R_rev ──────────────────┐││
MC, ML ──► M_M ──► Q_M ──► S_M, X ─────────────────────────┘││
                                                              │││
                               S_N, S_S, S_L ──► w(t) ───────┘│
                                                               ││
                          D_lab, S_lab ──► w (wage model) ─────┘│
                                                                ││
                              w(t) ──► ė ──► e(t), E(t) ───────┤
                              w, e ──► ε(t) ────────────────────┤
                                                                ││
                              w⁻¹, f_set, A_young ──► MMP ──────┤
                              ε⁻¹, E/S_pos ──────► EMP ──────────┤
                              Y/G, D=1−CL ────────► SFD ──────────┤
                                                                 ││
                              MMP × EMP × SFD ──────────────────► Ψ
                                                                 │
                              Ψ ──► p_dis ──► disruptions ───────┤
                              Ψ ──► CL erosion ──► D ────────────┤
                                                                 │
                              PT roll ──► PT ──► CL ──► D ───────┘

                              Q_A_sur, Q_M_sur ──► X ──► T_rev(t+24)
                              T_rev, R_rev, C_gov, C_maint ──► Ẏ
                              Y, G ──► Y/G ──► SFD
```

**Primary feedback loops:**

1. **Low-welfare spiral:** w ↓ → ė ↑ → e ↑ → ε ↓ → ε⁻¹ ↑ → EMP ↑ → Ψ ↑ → CL ↓ → D ↑ → SFD ↑ → Ψ ↑↑
2. **Fiscal trap:** Y ↑ → Y/G ↑ → SFD ↑ → Ψ ↑ → disruptions more frequent → production ↓ → G ↓ → Y/G ↑↑
3. **Elite saturation:** w ↓ → mobility ↑ → E ↑ past S_pos → frustrated elites → EMP ↑ → Ψ ↑ → further welfare pressure
4. **Trade-lag oscillator:** surplus production t → export → T_rev arrives t+24 → credits finance capital → output rises t+36 → welfare improves; if conditions reversed in interval, lags can destabilize
5. **Stabilizing loop:** PT improvement → CL ↑ → D ↓ → SFD ↓ → Ψ ↓ (requires MS-adjusted favorable rolls)

---

## XIV — Measurable Outputs

These are the observable quantities a colonial survey or Earth oversight body could directly measure at any time step.

| Output | Formula | Units | What it signals |
|--------|---------|-------|-----------------|
| **N(t)** | total population | persons | Colony size and growth trajectory |
| **G(t)** | p_A·Q_A + Q_I + p_M·Q_M × 12 | cr/year | Economic output; fiscal baseline |
| **Ψ(t)** | MMP × EMP × SFD | dimensionless | Integrated instability risk (leading indicator) |
| **S_N(t)** | Q_A / (N·r_min) | dimensionless | Nutrition adequacy; famine risk |
| **S_S(t)** | H / N | dimensionless | Shelter adequacy; overcrowding |
| **S_L(t)** | Q_I / (N·l_min) | dimensionless | Living standards; consumer goods availability |
| **w(t)** | (S_N + S_S + S_L) / 3 | dimensionless | Composite welfare; mobility threshold proximity |
| **e(t)** | E / N | dimensionless | Elite fraction; tracks overproduction |
| **ε(t)** | (1 − wλ) / e | dimensionless | Elite income dilution |
| **CL(t)** | citizen loyalty | dimensionless | Political stability |
| **Y(t)/G(t)** | cumulative debt / annual GDP | years | Fiscal sustainability |
| **X(t)** | Q_A_sur/100 + Q_M_sur/20 | displacement-tonnes/month | Export capacity; trade viability |
| **T_rev(t)** | p_exp · X(t−24) | cr/month | Lagged trade revenue actually received |
| **p_dis(t)** | p₀ · Ψ^κ | probability/month | Disruption risk; useful for scenario planning |
| **τ_resp** | τ₀ / (1 + MS/2) | months | Governance response speed to shocks |
| **V(t)** | w_col · N · 0.6 | cr | Realm value; Earth's strategic valuation of colony |

### Threshold values of note

| Condition | Threshold | Interpretation |
|-----------|-----------|----------------|
| Adequate nutrition | S_N ≥ 1 | No starvation pressure on growth or loyalty |
| Elite saturation | e(t) > s_max | Every elite excess beyond this is "frustrated elite" |
| Welfare neutral | w(t) = w₀ = 1 | Mobility halts; system near equilibrium |
| Fiscal danger | Y/G > 3 | Debt exceeds 3 years of output; Earth likely to intervene |
| High PSI | Ψ > Ψ_crit | Colony-specific; political crisis probable within ~5 years |
| Loyalty collapse | CL < 0.25 | Risk of open revolt or secession movement |

---

## XV — Parameter Summary

| Parameter | Baseline value | Source | Notes |
|-----------|---------------|--------|-------|
| r₀ | 0.02 y⁻¹ (= 1/600 mo⁻¹) | WTH | Annual population growth |
| λ | 0.55 | assumption | Working-age fraction |
| s_max | 0.05 | GURPS RM | Max elite fraction under standard charter |
| μ₀ | 0.005 mo⁻¹ | Turchin calibrated, rescaled | Mobility parameter |
| w₀ | 1.0 | Turchin | Neutral welfare threshold |
| β | 0.5 | Turchin Eq. 9 | Wage labor-market elasticity |
| τ | 3–5 mo | Turchin | Wage-stickiness lag |
| Δ | 12 mo | setting | One-way Earth transit time |
| κ_N | 2 | assumption | Nutrition stress nonlinearity |
| p₀ | 0.02 / mo | GURPS RM | Base disruption probability at Ψ = 1 |
| κ | 1.5 | assumption | PSI-disruption sensitivity exponent |
| τ₀ | 6 mo | GURPS RM RTM | Base response time at MS 0 |
| φ_min | 0.5 | WTH | Minimum growing-season modifier |
| maintenance rate | 0.00417 / mo | WTH | 5% capital value per year, starting month 120 |

---

## Sources

- **WTH** = TNE 0311, *World Tamer's Handbook*, ch. 4 (Colonial Economics), ch. 5 (Monthly Turn)
- **GURPS RM** = *GURPS Realm Management*, ch. 1–2 (Realm Value, Revenue, Disruptions, RTM, Habitability, Education Rating)
- **3turchin** = Turchin 2013, "Modeling Social Pressures Toward Political Instability", *Cliodynamics* 4.2: 241–280; Eqs. 1–14 fully inventoried in `turchin-equations.md`
