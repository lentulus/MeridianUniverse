# Turchin Equation Inventory

This is an AI produced summary and part of the reference material for ColonyModel

Sources:
- "Modeling Social Pressures Toward Political Instability", Turchin 2013, *Cliodynamics* 4.2: 241–280
 - "Toward Cliodynamics – an Analytical, Predictive Science of History", Turchin 2011, *Cliodynamics* 2: 167–186

---

## Section I — Political Stress Indicator (PSI) Framework
*(Turchin 2013 pp. 246–247; all components dimensionless; Ψ is a relative index)*

### Top-level composite

> **Ψ = MMP × EMP × SFD**

| Symbol | Meaning | Units |
|--------|---------|-------|
| Ψ | Political Stress Indicator | dimensionless |
| MMP | Mass Mobilization Potential | dimensionless |
| EMP | Elite Mobilization Potential | dimensionless |
| SFD | State Fiscal Distress | dimensionless |

---

### Mass Mobilization Potential

> **MMP = w⁻¹ · (N_urb / N) · A₂₀₋₂₉**

| Symbol | Meaning | Units |
|--------|---------|-------|
| w = W/g | relative wage (real wage / GDP per capita) | dimensionless |
| w⁻¹ | "misery index" — inverse relative wage | dimensionless |
| N_urb | urban population | persons |
| N | total population | persons |
| N_urb/N | urbanization rate | dimensionless (fraction) |
| A₂₀₋₂₉ | proportion of population aged 20–29 | dimensionless (fraction) |

---

### Elite Mobilization Potential

> **EMP = ε⁻¹ · (E / sN)**

| Symbol | Meaning | Units |
|--------|---------|-------|
| ε | relative elite income (avg elite income / GDP per capita) | dimensionless |
| ε⁻¹ | inverse relative elite income | dimensionless |
| E | total number of elites | persons |
| s | government employees per total population | dimensionless |
| sN | number of government positions | persons |
| E/sN | elite overproduction ratio (elites per available position) | dimensionless |

When s is constant, EMP simplifies to ε⁻¹e where e = E/N (relative elite numbers).

---

### State Fiscal Distress

> **SFD = (Y / G) · D**

| Symbol | Meaning | Units |
|--------|---------|-------|
| Y | total national debt | currency (dollars) |
| G | GDP | currency/year |
| Y/G | debt-to-GDP ratio | dimensionless |
| D | public distrust in state institutions | dimensionless (fraction 0–1) |

---

## Section II — General Wage Model
*(Turchin 2013 pp. 248–249)*

### Eq. 1 — Real wage (multiplicative form)

> **W_{t+τ} = a · (G_t/N_t)^α · (D_t/S_t)^β · C_t^γ**

| Symbol | Meaning | Units |
|--------|---------|-------|
| W | real wage (inflation-adjusted) | currency/time |
| t | time | years |
| τ | wage stickiness lag | years (≥3, empirically ~5) |
| a | scale parameter | absorbs unit conversion |
| G | GDP | currency/year |
| N | total population | persons |
| G/N | GDP per capita | currency per person per year |
| D | demand for labor | persons |
| S | supply of labor | persons |
| D/S | demand/supply ratio | dimensionless |
| C | cultural/coercive non-market forces | dimensionless |
| α, β, γ | elasticity exponents | dimensionless |

---

### Eq. 2 — Log-linearized wage (regression form)

> **log W_{t+τ} = A + α log(G_t/N_t) + β log(D_t/S_t) + γ log C_t + ε_t**

| Symbol | Meaning | Units |
|--------|---------|-------|
| A | log a (intercept) | dimensionless |
| ε_t | residual error (may be autocorrelated) | log-currency |

Eq. 2 is Eq. 1 log-transformed; the three drivers combine linearly and additively in log-space, enabling OLS regression.

---

## Section III — Elite Dynamics (General)
*(Turchin 2013 pp. 250–251)*

### Eq. 3 — Elite population ODE

> **Ė = rE + μN**

| Symbol | Meaning | Units |
|--------|---------|-------|
| Ė = dE/dt | rate of change of elite numbers | persons/year |
| r | per capita elite demographic growth rate | year⁻¹ |
| E | number of elites | persons |
| μ | net social mobility coefficient (upward minus downward) | year⁻¹ |
| N | general population | persons |
| μN | net flow into elite class via mobility | persons/year |

---

### Social mobility coefficient (unnumbered)

> **μ = μ₀ · (w₀/w − 1)**

| Symbol | Meaning | Units |
|--------|---------|-------|
| μ₀ | mobility magnitude parameter | year⁻¹ |
| w₀ | threshold relative wage; mobility = 0 when w = w₀ | dimensionless |
| w | current relative wage | dimensionless |

When w < w₀ (wages depressed), μ > 0 → net upward mobility. When w > w₀, μ < 0 → net downward mobility.

---

### Eq. 4 — Combined elite ODE (substituting mobility)

> **Ė = rE + μ₀ · ((w₀ − w)/w) · N**

Feeds from: **w** (wage model), **N** (population model).

Simplified relative form assuming elite r = commoner r, so e = E/N:

> **ė = μ₀ · (w₀ − w) / w**

| Symbol | Meaning | Units |
|--------|---------|-------|
| ė = de/dt | rate of change of relative elite fraction | year⁻¹ |
| e = E/N | relative elite numbers | dimensionless |

---

### Eq. 5 — Relative elite income

> **ε = (1/g) · (G − WL)/E = (1 − wλ) / e**

| Symbol | Meaning | Units |
|--------|---------|-------|
| ε | relative elite income | dimensionless |
| g = G/N | GDP per capita | currency/person/year |
| G | GDP | currency/year |
| W | real wage | currency/year |
| L | labor force size | persons |
| λ = L/N | labor force participation rate | dimensionless (≈ 0.5) |
| e = E/N | relative elite numbers | dimensionless |
| wλ | wage bill as fraction of GDP per capita | dimensionless |

Feeds from: w (wages), e (elite numbers), λ (treated as ~constant ≈ 0.5). Output ε feeds back into EMP.

---

## Section IV — Antebellum Demographic Model
*(Turchin 2013 pp. 252–254)*

### Rural base growth (unnumbered)

> **Ṅ_rur = r_rur · N_rur**

### Migration rate (unnumbered)

> **M = r_rur · (N_rur/K)^θ**

| Symbol | Meaning | Units |
|--------|---------|-------|
| M | per capita migration rate from rural to urban | year⁻¹ |
| r_rur | rural per capita growth rate | year⁻¹ (set to 0.025 y⁻¹) |
| K | carrying capacity of rural land | persons (set to 3.5 million) |
| θ | nonlinearity exponent for emigration | dimensionless (set to 5) |

---

### Eq. 6 — Rural population with emigration

> **Ṅ_rur = r_rur · N_rur − r_rur · N_rur · (N_rur/K)^θ**

When N_rur ≪ K emigration ≈ 0; when N_rur → K all surplus emigrates. A generalized logistic with steeper density-dependence than the standard (θ = 1) form.

---

### Eq. 7 — Urban population with immigration inflow

> **Ṅ_urb = r_urb · N_urb + r_rur · N_rur · (N_rur/K)^θ**

| Symbol | Meaning | Units |
|--------|---------|-------|
| r_urb | per capita urban growth rate | year⁻¹ (set to 0.015 y⁻¹) |
| N_urb | urban population | persons |
| Second term | immigration from rural (= emigration from Eq. 6) | persons/year |

The second term is the emigration term from Eq. 6. N_urb then feeds into Eq. 10 (wages) and MMP.

---

### Eq. 8 — Labor demand ODE (Antebellum)

> **Ḋ = ρ · D**

| Symbol | Meaning | Units |
|--------|---------|-------|
| Ḋ = dD/dt | rate of change of labor demand | persons/year |
| ρ | growth rate of labor demand | year⁻¹ (set to 0.03 y⁻¹) |
| D | demand for labor | persons |

D grows exponentially. Supply S = λN_urb grows with urban population; their ratio drives wages via Eq. 10.

---

### Eq. 9 — Wage (Antebellum, pure capitalist; simplified Eq. 1)

> **W = ag · (D/S)^β**

Drops cultural factor C (nineteenth-century US approximated as pure market); sets α = 1.

| Symbol | Meaning | Units |
|--------|---------|-------|
| S = λN_urb | urban labor supply | persons |
| β | demand/supply elasticity | dimensionless (set to 0.5) |

---

### Eq. 10 — Relative wage (Antebellum key dynamic variable)

> **w = á · (D / λN_urb)^β**

| Symbol | Meaning | Units |
|--------|---------|-------|
| w | relative wage = W/g | dimensionless |
| á | scale parameter | dimensionless |
| λ | labor force participation | dimensionless (≈ 0.5) |

Connects Eq. 7 (N_urb) and Eq. 8 (D) to the wage w, which drives Eqs. 11–12.

---

### Eq. 11 — Antebellum elite ODE (specific instance of Eqs. 3–4)

> **Ė = r_e · E + μ₀ · (w₀/w − 1) · N_urb**

| Symbol | Meaning | Units |
|--------|---------|-------|
| r_e | elite natural growth rate (set = r_rur) | year⁻¹ |
| μ₀ | mobility parameter | year⁻¹ (set to 0.002 y⁻¹) |
| w₀ | mobility threshold | dimensionless (set to 1) |
| N_urb | urban population (mobility source in 19th-c. US) | persons |

Differs from Eq. 4 in that N_urb replaces N — in Antebellum US upward mobility came primarily from urban artisans, not the full population.

---

### Eq. 12 — Antebellum relative elite income

> **ε = (1 − wλ) / e**

Same formula as Eq. 5, renumbered for the Antebellum application section.

---

## Section V — Contemporary US Model
*(Turchin 2013 pp. 260–274)*

### Labor demand proxy (unnumbered)

> **D_t = G_t / (2000 P_t)**

| Symbol | Meaning | Units |
|--------|---------|-------|
| G_t | GDP | dollars/year |
| P_t | labor productivity | dollars/hour |
| 2000 | hours per work-year (40 hr/wk × 50 wk) | hours/year per worker |
| D_t | number of workers demanded | persons |

Operationalizes the abstract D in Eq. 1 for twentieth-century regression.

---

### Eq. 13 — Contemporary elite dynamics (= simplified Eq. 4)

> **ė = μ₀ · (w₀ − w) / w**

Parameters: μ₀ = 0.1 y⁻¹, w₀ = 1. Same equation as the Eq. 4 simplified form, recalibrated for twentieth-century US.

---

### Eq. 14 — Full PSI expanded (Contemporary)

> **Ψ = w⁻¹ · (N_urb/N) · A₂₀₋₂₉ · ε⁻¹ · e · (Y/G) · D**

MMP × EMP × SFD written out in full with EMP simplified (s absorbed as constant):

| Factor | Source equation |
|--------|----------------|
| w⁻¹ | Eqs. 10, 13 — wage dynamics |
| N_urb/N | Eq. 7 — urban population |
| A₂₀₋₂₉ | empirical census data (youth bulge) |
| ε⁻¹ | Eqs. 5/12 — elite income |
| e | Eq. 13 — elite numbers |
| Y/G | US Treasury debt / GDP data |
| D | Pew distrust survey data |

---

## Equation Dependency Graph

```
Eq. 6 (rural pop) ──────────────────────────────► N_rur(t)
     │ emigration term
     ▼
Eq. 7 (urban pop) ──────────────────────────────► N_urb(t)
     │                                                │
Eq. 8 (labor demand) ──► D(t)                        │
     │                    │                           │
     └────────────────────┴───► Eq. 10 ──► w(t) ─────┤
                                (relative              │
                                 wage)                 │
                                   │                   │
                                   ▼                   ▼
                              Eq. 4/11 ──────────► E(t), e(t)
                              (elite ODE)              │
                                                       │
                              w, e, λ ──────────► Eq. 5/12 ──► ε(t)
                                                  (elite income)
                                                       │
     ┌─────────────────────────────────────────────────┘
     │
     w⁻¹, N_urb/N, A₂₀₋₂₉ ────────────────────► MMP
     ε⁻¹, e ────────────────────────────────────► EMP
     Y/G, D ────────────────────────────────────► SFD
                                                   │
     MMP × EMP × SFD ───────────────────────────► Ψ (PSI)
```

Key feedback loop: **low w → rapid elite accumulation (Eq. 4) → high e → diluted ε (Eq. 5) → high ε⁻¹ → elevated EMP → elevated Ψ**. The same low w simultaneously drives MMP up directly. Both PSI components therefore amplify together when wages fall.

---

## Calibrated Parameter Values (Antebellum model)

| Parameter | Value | Meaning |
|-----------|-------|---------|
| r_rur | 0.025 y⁻¹ | rural population growth rate |
| r_urb | 0.015 y⁻¹ | urban population growth rate |
| K | 3.5 million persons | rural carrying capacity (4 NE states) |
| θ | 5 | emigration nonlinearity exponent |
| β | 0.5 | wage elasticity to D/S ratio |
| a | 1 | scale (simplified) |
| ρ | 0.03 y⁻¹ | labor demand growth rate |
| γ | 0.01 y⁻¹ | GDP per capita growth rate |
| μ₀ | 0.002 y⁻¹ | elite mobility magnitude |
| w₀ | 1 | wage threshold for zero mobility |
| N_rur(1790) | 0.3K | initial rural population |
| N_urb(1790) | 0.1 N_rur | initial urban population |

---

## Comparison: Turchin 2013 (2013) vs. Turchin 2011 (2011)

| Feature | Turchin 2011 (2011) | Turchin 2013 (2013) |
|---------|----------------|-----------------|
| Type | Conceptual/argumentative | Formal mathematical model |
| Equations | Zero explicit equations | 14 numbered + ~10 unnumbered |
| Variable definitions | None — verbal descriptions only | Explicit, with units and calibrated parameter values |
| PSI / Ψ | Not mentioned | Central construct; fully derived |
| Population dynamics | Secular cycles described qualitatively; cites Turchin & Korotayev [35] eq. 8 (not reproduced) | Eqs. 6–7: explicit rural/urban ODEs with nonlinear emigration |
| Wages | Not modeled | Eqs. 1/2, 9/10: three-factor multiplicative model (GDP per capita, D/S, culture) |
| Elite dynamics | "Elite overproduction" named (following Goldstone) | Eqs. 3/4/11/13: explicit ODE; ε (Eq. 5) tracks income dilution |
| Empirical grounding | Tables 1–2 (instability counts across 7 cycles + China); conversion curves | Regression Table 1 (R² = 0.98); parameterized to specific US data; Figures 2–14 |
| Case studies | Statistical comparison across secular cycles | Two calibrated models: Antebellum US (1790–1880) and contemporary US (1927–2012) |
| Key contribution | Epistemological: a science of history is possible | Mechanistic: quantifies why instability builds; PSI as leading indicator |
| State variable | Mentioned (legitimacy, fiscal crisis) | Explicit SFD component in Ψ |

Turchin 2011  makes the *case* for mathematical cliodynamics by surveying empirical regularities and citing models from other papers. Turchin 2013 is the *execution*: the structural-demographic framework translated into a system of coupled differential equations, calibrated, and tested against data. The population-instability oscillation figure in Turchin 2011 (Fig. 1, from Turchin & Korotayev ) is precisely the kind of output that the Turchin 2013 equation system generates in a more empirically tractable "dynamically incomplete" form.
