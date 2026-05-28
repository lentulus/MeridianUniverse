# Turchin 2013 — Equation Inventory and Variable Cross-Reference

**Source:** Turchin, Peter. 2013. "Modeling Social Pressures Toward Political Instability." *Cliodynamics* 4.2: 241–280.

**Numbering:** The paper numbers 14 equations; unnumbered display equations are assigned U1–U12 here in order of appearance.

---

## Equations

### Political Stress Indicator Components

**U1** &emsp; $\Psi = \text{MMP} \times \text{EMP} \times \text{SFD}$

**U2** &emsp; $\text{MMP} = w^{-1} \dfrac{N_\text{urb}}{N} A_{20\text{–}29}$

**U3** &emsp; $\text{EMP} = \varepsilon^{-1} \dfrac{E}{sN}$

**U4** &emsp; $\text{SFD} = \dfrac{Y}{G} \cdot D_\text{trust}$

> Note: D is used for two distinct variables — labor demand in the wage equations, and public distrust in U4/Eq. 14. Distinguished here as D and D_trust.

---

### Social Mobility

**U5** &emsp; $\mu = \mu_0 \!\left(\dfrac{w_0}{w} - 1\right)$

---

### Relative Elite Income — Full Definition

**U6** &emsp; $\varepsilon = \dfrac{1}{g} \cdot \dfrac{G - WL}{E}$

---

### Relative Elite Numbers — Simplified Dynamics

**U7** &emsp; $\dot{e} = \mu_0 \dfrac{w_0 - w}{w}$

*Derived from Eq. 4 by assuming elites and commoners share the same demographic growth rate; e = E/N.*

---

### Wage — Pure-Capitalist Simplification (no cultural factor)

**U8** &emsp; $W_{t+\tau} = a \!\left(\dfrac{G_t}{N_t}\right)^\alpha \!\left(\dfrac{D_t}{S_t}\right)^\beta$

*Eq. 1 with C dropped; used for Antebellum US.*

---

### Wage — Command-Economy Simplification

**U9** &emsp; $W_t = C_t \!\left(\dfrac{G_t}{N_t}\right)$

*Eq. 1 with D/S term dropped; used for USSR-type economies.*

---

### Rural Population — Simple Exponential

**U10** &emsp; $\dot{N}_\text{rur} = r_\text{rur} N_\text{rur}$

*Unconstrained growth; superseded by Eq. 6.*

---

### Migration Rate

**U11** &emsp; $M = r_\text{rur} \!\left(\dfrac{N_\text{rur}}{K}\right)^\theta$

---

### Labor Demand Proxy (Contemporary Model)

**U12** &emsp; $D_t = \dfrac{G_t}{2000\, P_t}$

---

### Numbered Equations (Paper's Own Numbering)

**(1)** &emsp; $W_{t+\tau} = a \!\left(\dfrac{G_t}{N_t}\right)^\alpha \!\left(\dfrac{D_t}{S_t}\right)^\beta C_t^\gamma$

*General real-wage model. Three drivers — GDP per capita, labor demand/supply ratio, non-market cultural forces — combine multiplicatively. τ captures wage stickiness.*

---

**(2)** &emsp; $\log W_{t+\tau} = A + \alpha \log\!\left(\dfrac{G_t}{N_t}\right) + \beta \log\!\left(\dfrac{D_t}{S_t}\right) + \gamma \log C_t + \varepsilon_t$

*Log-linear (regression) form of Eq. 1; suitable for OLS/ARIMA estimation.*

---

**(3)** &emsp; $\dot{E} = rE + \mu N$

*Elite population ODE: natural demographic growth plus net social-mobility inflow.*

---

**(4)** &emsp; $\dot{E} = rE + \mu_0 \!\left(\dfrac{w_0 - w}{w}\right) N$

*Eq. 3 with U5 substituted; mobility is now explicitly a function of relative wage.*

---

**(5)** &emsp; $\varepsilon = \dfrac{1 - w\lambda}{e}$

*Relative elite income, simplified form. Numerator = fraction of GDP per capita not consumed by the wage bill; denominator = relative elite numbers. Simplification of U6.*

---

**(6)** &emsp; $\dot{N}_\text{rur} = r_\text{rur} N_\text{rur} - r_\text{rur} N_\text{rur}\!\left(\dfrac{N_\text{rur}}{K}\right)^\theta$

*Rural population: exponential growth minus emigration; generalised logistic with nonlinearity exponent θ.*

---

**(7)** &emsp; $\dot{N}_\text{urb} = r_\text{urb} N_\text{urb} + r_\text{rur} N_\text{rur}\!\left(\dfrac{N_\text{rur}}{K}\right)^\theta$

*Urban population: endogenous growth plus rural-to-urban immigration flow (the emigration term from Eq. 6).*

---

**(8)** &emsp; $\dot{D} = \rho D$

*Labor demand (Antebellum): grows exponentially at rate ρ.*

---

**(9)** &emsp; $W = ag\!\left(\dfrac{D}{S}\right)^\beta$

*Urban wage (Antebellum): simplified Eq. 1 with C omitted and α = 1.*

---

**(10)** &emsp; $w = \hat{a}\!\left(\dfrac{D}{\lambda N_\text{urb}}\right)^\beta$

*Relative urban wage (Antebellum): Eq. 9 expressed as w = W/g, with S = λN_urb.*

---

**(11)** &emsp; $\dot{E} = r_e E + \mu_0\!\left(\dfrac{w_0}{w} - 1\right) N_\text{urb}$

*Elite ODE (Antebellum): same structure as Eq. 4 but mobility sourced from urban population only; r replaced by r_e.*

---

**(12)** &emsp; $\varepsilon = \dfrac{1 - w\lambda}{e}$

*Relative elite income (Antebellum application): identical to Eq. 5, re-stated for this model section.*

---

**(13)** &emsp; $\dot{e} = \mu_0 \dfrac{w_0 - w}{w}$

*Relative elite numbers dynamics (Contemporary model): same as U7, now a formal numbered equation with recalibrated parameters.*

---

**(14)** &emsp; $\Psi = w^{-1} \dfrac{N_\text{urb}}{N} A_{20\text{–}29} \cdot \varepsilon^{-1} e \cdot \dfrac{Y}{N} D_\text{trust}$

*Full expanded PSI: all components written out explicitly for empirical estimation.*

---

## Variable Reference Table

**"Computed"** = the equation listed produces this variable as output.
**"Derived"** = simple algebraic transformation of another variable; no ODE.
**"Measured"** = empirical data used directly.

| # | Symbol | Description | How obtained | Units |
|---|--------|-------------|--------------|-------|
| 1 | Ψ | Political Stress Indicator | Computed: U1, Eq. 14 | dimensionless (relative index) |
| 2 | MMP | Mass Mobilization Potential | Computed: U2 | dimensionless |
| 3 | EMP | Elite Mobilization Potential | Computed: U3 | dimensionless |
| 4 | SFD | State Fiscal Distress | Computed: U4 | dimensionless |
| 5 | W | Real wage (inflation-adjusted) | Computed: Eqs. 1, 9 | $ yr⁻¹ (per worker) |
| 6 | w | Relative wage = W/g | Derived from W and g; output of Eq. 10 in Antebellum model | dimensionless |
| 7 | w⁻¹ | Inverse relative wage ("misery index") | Derived from w; enters U2, Eq. 14 | dimensionless |
| 8 | G | Total GDP (inflation-adjusted dollars) | Measured: MeasuringWorth (Officer & Williamson) | $ yr⁻¹ |
| 9 | N | Total population | Measured: census / HSUS / BLS | persons |
| 10 | g | GDP per capita = G/N | Derived: G ÷ N | $ person⁻¹ yr⁻¹ |
| 11 | D | Demand for labor (worker-years) | Computed: Eq. 8 (Antebellum); U12 (Contemporary) | persons (FTE workers) |
| 12 | S | Supply of labor (labor force size) | Measured: BLS / HSUS; approximated as λN or λN_urb | persons |
| 13 | C | Non-market cultural/coercive forces on wages | Measured: real minimum wage as proxy (US Dept of Labor) | $ hr⁻¹ (used as index; same scale as W) |
| 14 | α | Elasticity of wage to GDP per capita | Estimated: regression (Eq. 2); set to 1 in Antebellum | dimensionless |
| 15 | β | Elasticity of wage to D/S ratio | Estimated: regression (Eq. 2); set to 0.5 in Antebellum | dimensionless |
| 16 | γ | Elasticity of wage to cultural factor C | Estimated: regression (Eq. 2) | dimensionless |
| 17 | a, â | Scale parameter in wage equation | Estimated: regression | absorbs unit conversion |
| 18 | A | log(a); intercept in log-transformed regression | Estimated: regression (Eq. 2) | log($ yr⁻¹) |
| 19 | τ | Wage stickiness lag | Estimated empirically; ~5 years in Contemporary model | years |
| 20 | ε_t | Regression error term | Statistical residual (Eq. 2); modelled as ARIMA(1,0,1) | log($ yr⁻¹) |
| 21 | E | Absolute elite numbers | Computed: Eqs. 3, 4, 11 | persons |
| 22 | e | Relative elite numbers = E/N | Derived: E ÷ N; dynamics from U7, Eq. 13 | dimensionless (fraction) |
| 23 | ε | Relative elite income (avg elite income / GDP per capita) | Computed: U6 (full form), Eqs. 5 and 12 (simplified) | dimensionless |
| 24 | ε⁻¹ | Inverse relative elite income (intraelite competition index) | Derived from ε; enters U3, Eq. 14 | dimensionless |
| 25 | μ | Net social mobility coefficient | Computed: U5 | yr⁻¹ |
| 26 | μ₀ | Mobility magnitude parameter | Set: 0.002 y⁻¹ (Antebellum), 0.1 y⁻¹ (Contemporary) | yr⁻¹ |
| 27 | w₀ | Threshold wage at which net mobility = 0 | Set: 1.0 in both models | dimensionless |
| 28 | L | Labor force size | Measured: BLS / HSUS | persons |
| 29 | λ | Labor force participation rate ≈ L/N | Measured / set: ≈ 0.5 | dimensionless (fraction) |
| 30 | r | Per capita demographic growth rate | Measured: set from historical census data | yr⁻¹ |
| 31 | r_e | Elite natural demographic growth rate | Set: = r_rur in Antebellum model | yr⁻¹ |
| 32 | N_rur | Rural population | Computed: Eq. 6 | persons |
| 33 | N_urb | Urban population | Computed: Eq. 7 | persons |
| 34 | r_rur | Per capita growth rate of rural population | Set: 0.025 y⁻¹ (Antebellum) | yr⁻¹ |
| 35 | r_urb | Endogenous growth rate of urban population | Set: 0.015 y⁻¹ (Antebellum) | yr⁻¹ |
| 36 | K | Rural carrying capacity (agricultural land limit) | Set: 3.5 million persons for four Northeastern states | persons |
| 37 | θ | Migration nonlinearity exponent | Set: 5 | dimensionless |
| 38 | M | Per capita migration rate, rural → urban | Computed: U11; used in Eqs. 6, 7 | yr⁻¹ |
| 39 | ρ | Exponential growth rate of labor demand | Set: 0.03 y⁻¹ (Antebellum) | yr⁻¹ |
| 40 | P | Labor productivity | Measured: BLS (1947–present); Ferguson & Wascher (pre-1947) | $ hr⁻¹ |
| 41 | Y | Total national debt | Measured: US Department of the Treasury | $ |
| 42 | D_trust | Public distrust in government (proportion distrusting) | Measured: Pew Research Center surveys (from 1958) | dimensionless (fraction 0–1) |
| 43 | s | Government employees per total population | Measured: Carter et al. 2004 (Table Ea894–903) | dimensionless (fraction) |
| 44 | A₂₀₋₂₉ | Youth bulge index: proportion of population aged 20–29 | Measured: US Census Bureau | dimensionless (fraction) |
| 45 | N_urb / N | Urbanization rate | Measured: Carter et al. 2004, *Historical Statistics of the United States* | dimensionless (fraction) |

---

## Application: Using the Wage Equation in a Colony Model

### The problem with variables 14–19

In Turchin's paper, variables 14–19 (α, β, γ, a/â, A, τ) are determined by regression against 80–200 years of US wage, GDP, and labor data. Without an existing society to fit a curve to, a colony model has no such data. These parameters therefore shift from *things to be estimated* into *design choices that encode the economic and social character of the colony*. Each one has a real-world interpretation that guides sensible values.

---

### Variable 14 — α (elasticity of wages to GDP per capita)

α answers: *when the colony's total output per person rises 1%, by how much do workers' wages rise?*

- **α = 1**: workers capture all productivity gains — the pure competitive market result. Appropriate for a high-demand frontier colony where labour is scarce and workers have strong bargaining power.
- **α = 0.5–0.8**: realistic range for most mixed economies (Turchin's fitted value is 0.60). Capital and labour share productivity gains; institutional frictions and employer power hold wages below the full gain.
- **α < 0.3**: labour captures little; most growth goes to capital or the colonial authority. Appropriate for a corporate-run extraction colony or a plantation economy.
- **α ≈ 0**: wages are set by the owner class regardless of output — the slave/indentured model, where Turchin notes the equation collapses to W = C.

**Starting point for a free-labour frontier colony:** α = 0.8–1.0.

---

### Variable 15 — β (elasticity of wages to the demand/supply ratio)

β answers: *how responsive are wages to labour market tightness?* A high β means wages rise sharply when workers are scarce relative to demand; a low β means wages barely move even when labour is very tight.

- **β = 1.65**: Turchin's fitted value for twentieth-century US. Wages are highly responsive to market conditions.
- **β = 0.5**: the Antebellum simplification; a more sluggish labour market.
- **β ≈ 0**: wages are set administratively regardless of supply and demand — corporate or state colony with fixed pay scales.
- **β > 2**: a highly volatile frontier market where a small labour shortage produces large wage spikes; possible in early-stage colonies with very thin labour markets.

**Starting point:** β = 0.5–1.0 for a managed colony; β = 1.5–2.0 for a free-market frontier.

---

### Variable 16 — γ (elasticity of wages to cultural/coercive factor C)

γ answers: *how much do non-market forces (law, custom, political power) amplify or suppress the market wage?*

In Turchin's model, C is proxied by the real minimum wage — a variable that rises when labour-friendly politics prevails and falls when it does not. For a colony:

- **γ = 0, C dropped**: pure market model (Eq. U8). Appropriate if no labour regulation exists.
- **γ > 0, C = colonial labour charter**: a legal minimum wage or colonial charter sets a floor; γ measures how binding that floor is.
- **γ < 0**: coercive suppression — the colonial authority actively holds wages below the market level; C represents the degree of suppression.

**Starting point:** set γ = 0 and use Eq. U8 for simplicity unless the colony has explicit labour law you want to model. Introduce C and γ later if the model needs political dynamics around wages.

---

### Variable 17 — a, â (scale parameter)

a has no independent economic meaning. It is a unit-conversion constant that makes the equation produce the right wage level given your chosen units for W, G, and N.

**Practical approach:** choose your initial conditions — a starting wage W₀, a starting GDP per capita g₀, and a starting D/S ratio — then solve for a:

$$a = \frac{W_0}{g_0^{\,\alpha} (D_0/S_0)^{\beta}}$$

Set a once at initialisation. It does not change over time.

---

### Variable 18 — A = log(a)

A is simply log(a), the intercept term that appears when the equation is log-transformed for regression (Eq. 2). In a simulation you work with Eq. 1 directly and set a as above; A is not needed separately.

---

### Variable 19 — τ (wage stickiness lag, years)

τ captures the delay between economic conditions changing and wages catching up. In Turchin's model it reflects multi-year labour contracts and institutional inertia (~5 years in twentieth-century US).

For a colony:

| Colony type | Suggested τ |
|-------------|-------------|
| Early-stage, no contracts, fluid labour pool | 1–2 years |
| Established colony with annual wage-setting | 1–3 years |
| Colony with multi-year indenture or union contracts | 3–7 years |
| Highly bureaucratic or state-run colony | 5–10 years |

τ primarily affects the timing of instability waves, not their eventual amplitude. A short τ means the colony responds quickly to labour market shifts; a long τ means structural problems build up before wages adjust.

---

### Practical starting configuration for a colony wage model

Use the pure-capitalist form (Eq. U8, dropping C):

$$W_{t+\tau} = a \left(\frac{G_t}{N_t}\right)^{\alpha} \left(\frac{D_t}{S_t}\right)^{\beta}$$

1. **Choose colony type** and set α and β from the ranges above.
2. **Set initial conditions** W₀, g₀ = G₀/N₀, (D/S)₀ and solve for a.
3. **Set τ** from the table above.
4. **Run sensitivity sweeps** over α and β to understand which matters most in your parameter range — this replaces the role that regression plays in fitting historical data.

If the colony has politically driven wage dynamics (labour law, company policy, indenture), introduce C and γ at that point. Turchin's fitted values (α = 0.60, β = 1.65, γ = 0.45, τ = 5) provide a useful sanity-check benchmark: if your colony produces wildly different wage trajectories from a comparable initial state, the parameter choices deserve scrutiny.

---

## Initial Values: Non-Regression Parameters

These are variables that must be set by the modeller before the simulation runs. They are not computed by the equations, and they are not regression coefficients. They are either initial conditions (the state of the colony at t = 0) or structural parameters (fixed properties of the colony that do not change unless deliberately varied). Their values determine the shape, pace, and character of the colony's instability trajectory.

They are grouped below by subsystem.

---

### Population and Demography

#### N_rur(0) and N_urb(0) — Initial rural and urban populations

These set the starting demographic balance. Turchin uses N_rur(0) = 0.3K and N_urb(0) = 0.1 × N_rur(0) for the Antebellum case — a predominantly rural colony with a small urban nucleus.

The ratio N_urb(0)/N matters because urban labour supply S = λN_urb directly sets the initial wage level through Eq. 10. A small initial urban population means labour is scarce and wages start high. As the city grows (Eq. 7), supply catches up with demand and wages fall. The timing of that fall is the first instability signal.

| Starting balance | Implication |
|-----------------|-------------|
| Very rural (N_urb/N < 0.1) | Wages start high; long initial stable period; late but steep wage collapse as cities fill |
| Mixed (N_urb/N ≈ 0.3–0.5) | Moderate initial wages; pressure begins sooner; more gradual decline |
| Already urbanised (N_urb/N > 0.6) | Wages start under pressure immediately; early instability, shorter cycle |

#### r_rur — Rural per capita growth rate

This is the intrinsic rate of natural population increase in the countryside. Turchin sets it at 0.025 yr⁻¹ (2.5% per year) for Antebellum America — a high-fertility pre-industrial demographic.

r_rur is the engine that drives emigration from the countryside to the cities (Eq. 6). A faster rate means the rural population approaches K sooner, the emigration term in Eq. 6 activates earlier, and urban labour supply grows faster, depressing wages sooner.

| r_rur value | Character |
|-------------|-----------|
| 0.01–0.015 yr⁻¹ | Low-fertility colony (screened workers, life extension, urban norms); slow demographic pressure; long quiet phase |
| 0.02–0.03 yr⁻¹ | Antebellum-equivalent; vigorous natural increase; cycle unfolds over 50–100 years |
| 0.04–0.06 yr⁻¹ | Rapid colonial expansion (encouraged natalism, young population); cycle compressed to 30–50 years; crises arrive early |

#### r_urb — Urban endogenous growth rate

Turchin sets r_urb = 0.015 yr⁻¹ — lower than r_rur, reflecting higher mortality and lower fertility in cities. The gap r_rur − r_urb determines how much of urban growth comes from natural increase versus immigration from the countryside.

For a colony with good medical infrastructure (2300AD-level), the urban/rural mortality gap may be small or absent. Setting r_urb close to r_rur means urban population grows partly on its own, not just from rural inflow. This slows the wage-depressing surge because urban supply does not spike as sharply when rural migration arrives.

#### K — Rural carrying capacity

K is the maximum rural population the available agricultural land can sustain. It sets the ceiling against which r_rur pushes. The ratio N_rur(0)/K is the most important single parameter in the Antebellum model: at 0.3 there is substantial headroom before emigration pressure builds; at 0.8 the colony is already land-stressed.

Crucially, K may not be fixed. A colony that is actively clearing land, terraforming, or expanding agricultural infrastructure has a rising K. A rising K delays the onset of emigration and extends the stable phase.

| N_rur(0)/K | Effect |
|------------|--------|
| < 0.2 | Long pre-crisis stability; rural population grows freely for generations before emigration kicks in |
| 0.3–0.4 | Turchin Antebellum baseline; 50–80 years before crisis |
| 0.6–0.8 | Colony is already near its land ceiling; emigration and wage pressure begin immediately |
| K growing at rate k | Each percentage point of k adds roughly equivalent time to the stable phase; active terraforming is a structural stabiliser |

#### θ — Emigration nonlinearity exponent

θ controls the shape of the emigration curve (U11). At θ = 1 emigration is linear — a small rural population still sends people to the cities. At θ = 5 (Turchin's value) emigration is negligible until N_rur approaches K, then rises sharply. At θ = 10 or higher the curve approaches a step function: nothing, then suddenly everyone.

θ determines whether the wage crisis arrives gradually or suddenly:

| θ value | Crisis character |
|---------|-----------------|
| 1–2 | Gentle, long-running wage decline beginning early; no sharp transition |
| 5 | Turchin baseline; moderate nonlinearity; crisis arrives as an acceleration, not a cliff |
| 10+ | Stability is maintained until near K, then an abrupt surge in urban migration depresses wages rapidly; boom-bust character |

A colony with efficient internal transport (making migration easy even at low rural density) is better modelled with low θ. A colony with difficult terrain or strong rural community ties that keep people on the land until it is truly exhausted is better modelled with high θ.

---

### Labour Market Structure

#### ρ — Growth rate of labour demand

ρ is the rate at which the economy's demand for workers expands (Eq. 8). It reflects investment, industrial expansion, and economic development — not just population. Turchin sets ρ = 0.03 yr⁻¹ for Antebellum America, just above r_urb.

The critical comparison is ρ vs. (r_urb + immigration inflow rate). If demand grows faster than supply, wages rise. If supply overtakes demand, wages fall. The transition from one regime to the other is the turning point of the wage cycle.

| ρ relative to labour supply growth | Effect |
|------------------------------------|--------|
| ρ > supply growth (early colony boom) | Wages rise during initial development phase; high MMP delayed |
| ρ ≈ supply growth | Wages flat; stable early period |
| ρ < supply growth | Wages fall immediately from founding; MMP builds from the start |
| ρ falling over time (maturing colony) | Replicates the Turchin contemporary US pattern: boom then stagnation |

#### λ — Labour force participation rate

λ ≈ L/N, the fraction of the total population that is economically active. Turchin treats it as a constant ≈ 0.5 (accounting for children, elderly, and non-workers). It scales urban labour supply: S = λN_urb.

For a colony, λ can differ substantially from 0.5:

| λ value | Colony type |
|---------|-------------|
| 0.7–0.85 | Working-age-selected settlers; few children initially; high participation rate — wages suppressed more than population alone suggests |
| 0.5 | Earth baseline; standard assumption if unknown |
| 0.3–0.4 | Colony with large dependent population (many children or elderly); effective labour supply lower than headcount; wages held up |
| Rising λ over time | Demographic echo — as founding children reach working age, λ rises and wages fall; a predictable second-wave pressure |

---

### Elite Formation

#### μ₀ — Mobility magnitude parameter

μ₀ sets how fast elites accumulate when wages are below the threshold w₀ (U5). Turchin calibrated it at 0.002 yr⁻¹ for Antebellum America and 0.1 yr⁻¹ for contemporary America — a 50-fold difference reflecting that social mobility was far more sluggish in an agrarian economy than in a modern one.

μ₀ controls the lag between wage depression and elite overproduction. A high μ₀ means elites form quickly in response to economic opportunity; EMP rises fast and the instability cycle is compressed. A low μ₀ means elite accumulation is slow even when economic conditions favour it; there is a long delay between wage fall and the elite overproduction crisis.

| μ₀ value | Elite dynamics |
|----------|----------------|
| < 0.005 yr⁻¹ | Slow, aristocratic accumulation; elite numbers barely respond to wages within a generation; instability cycle very long |
| 0.005–0.02 yr⁻¹ | Antebellum range; moderate responsiveness; elite overproduction lags wage depression by decades |
| 0.05–0.15 yr⁻¹ | Contemporary US range; fast elite formation; EMP tracks MMP closely; instability cycle compressed |
| > 0.2 yr⁻¹ | Hyperfluid social mobility (e.g. gold-rush colony); elites form and dissolve rapidly; volatile but self-correcting |

#### w₀ — Mobility threshold

w₀ is the relative wage at which net social mobility is zero (U5). Set to 1.0 in both Turchin models, meaning the crossover between upward and downward mobility happens exactly at the baseline wage level.

A higher w₀ means elites continue to accumulate even when wages are relatively good — a socially ambitious colony where prosperity breeds more aspiring elites. A lower w₀ means elite formation only accelerates during genuinely severe wage depression.

| w₀ value | Implication |
|----------|-------------|
| > 1.2 | Elite formation occurs even in prosperous conditions; EMP rises early; colony prone to elite overproduction before wages collapse |
| 1.0 | Turchin baseline |
| < 0.8 | Elite growth only when wages are badly depressed; long lag between economic stress and political elite conflict |

#### e(0) — Initial relative elite fraction

The starting proportion of elites in the population. Turchin initialises elites at 1% of the urban population. The choice affects how much margin exists before elite overproduction begins.

| e(0) | Implication |
|------|-------------|
| Very small (< 0.5%) | Long ramp-up before EMP becomes significant; colony has a pronounced stable founding phase |
| 1–2% | Turchin baseline; moderate starting competition |
| > 5% | Colony founded by or quickly dominated by an established elite class; EMP significant from the start; structural instability is baked in early |

---

### State Structure

#### s — Government positions per population

s sets the denominator in EMP: how many elite-eligible positions the colonial state provides per person (Eq. U3). A higher s means more positions relative to elites, reducing competition and lowering EMP. A lower s (or a rapidly growing elite with a fixed s) means competition for scarce offices intensifies.

In the Antebellum model s is treated as a slowly changing empirical quantity (it roughly doubled over the period). For a colony, s is a policy variable:

| s value | Implication |
|---------|-------------|
| High s (large government, many offices) | EMP structurally suppressed; colonial bureaucracy absorbs elite aspirants; stabilising |
| Low s (minimal government) | Elite competition focused on private economic positions; EMP sensitive to elite numbers; destabilising |
| s falling over time | Positions grow slower than elites; EMP rises even if elite numbers are constant — classic bureaucratic squeeze |
| s rising with economic expansion | New offices absorb new elites; EMP held down; sustained expansion is a long-run stabiliser |

#### Y(0) — Initial national debt and D_trust(0) — Initial public distrust

These set the State Fiscal Distress (SFD) component of Ψ. A colony founded debt-free and with high institutional trust starts with SFD ≈ 0, which acts as a stabiliser regardless of MMP and EMP values. A colony founded with inherited debt (e.g. corporate financing costs, terraforming loans) or with low initial trust in the colonial authority starts with SFD already elevated, amplifying whatever MMP and EMP produce.

For most colony models, SFD is best treated as a slowly evolving background variable whose initial value is a design choice about the colony's political starting conditions.

---

## Outputs

### What the model produces

Running the equations forward from initial conditions generates time series for every state variable. The most important outputs, in order of interpretive value, are:

| Output | Variable # | What it reveals |
|--------|-----------|-----------------|
| Relative wage | 6 (w) | Popular welfare; the primary driver of MMP |
| Relative elite numbers | 22 (e) | Elite overproduction; the primary driver of EMP |
| Relative elite income | 23 (ε) | Intraelite competition intensity; falling ε means the elite pie is being sliced thinner |
| MMP | 2 | Popular mobilisation pressure |
| EMP | 3 | Elite competition pressure |
| SFD | 4 | State fiscal and legitimacy stress |
| Ψ (PSI) | 1 | Composite structural pressure toward instability |

The ODEs (Eqs. 3, 4, 6, 7, 8, 13) integrate forward in time and produce smooth trajectories. Ψ is then computed algebraically from those trajectories at each time step.

---

### Interpreting Ψ (variable 1)

Ψ is a dimensionless relative index, not a probability or a count. Its absolute value at any single moment is not meaningful in isolation — what matters is its trend and its rate of change.

**Ψ as a leading indicator, not a crisis trigger.** Turchin is explicit that specific triggering events (assassinations, famines, military defeats) are outside the model. Ψ measures the structural pressure that makes a society *susceptible* to a crisis if a trigger occurs — not whether a trigger will occur. A society with low Ψ can absorb shocks; one with high Ψ tips into crisis on almost any provocation.

**The multiplicative structure amplifies alignment.** Because Ψ = MMP × EMP × SFD, the index rises steeply when all three components grow together. A society where popular distress is high but elites are unified (low EMP) and the state is solvent (low SFD) may be uncomfortable but is not on the edge of collapse. It is the *coincidence* of all three pressures that creates genuine structural crisis. Watch for the three components to begin rising in phase with each other — that alignment is more significant than the level of any single component.

**What the shape of the Ψ curve means:**

| Ψ behaviour | Interpretation |
|-------------|----------------|
| Flat or declining | Structural conditions stable or improving; no instability wave building |
| Slow monotonic rise | One or two components building; structural stress accumulating, but asymmetrically |
| Accelerating rise (curve bending upward) | All three components reinforcing each other; the colony is in the late pre-crisis phase |
| Rate of change increasing faster than Ψ itself | The system has entered a positive feedback loop; crisis is near regardless of triggers |
| Ψ plateaus then falls | A structural relief event has occurred — wages recovered, debt reduced, elite numbers thinned; crisis has been avoided or deferred |

---

### Crisis indicators

No single threshold in the equations defines a "crisis." Turchin does not set one, and the ODEs have no discontinuity. A crisis in the real world is a state transition that occurs when accumulated structural pressure meets a trigger. For modelling purposes, the following conditions, *in combination*, indicate that the system is in a pre-crisis state:

**Necessary conditions (all should be true):**

1. **w < w₀ for longer than τ years.** Wages have been structurally depressed long enough that labour contracts have fully adjusted. Workers are no longer experiencing a temporary dip — they are in a new, lower equilibrium. This is the signal that MMP has structural, not cyclical, causes.

2. **e has grown significantly from its initial value** — typically 2× or more. Elite numbers have expanded well beyond the positions the colonial economy and state can absorb. Intraelite competition for scarce offices and resources is now structurally intense.

3. **ε is falling despite a growing economy.** If the total economic surplus is growing but average elite income is falling, the elite is being diluted. This is the condition that turns passive elite competition into active factional conflict — established elites begin to feel threatened, not just crowded.

4. **Ψ has at least doubled from its historical minimum.** The index has moved well above its baseline, and the trajectory is upward.

**The strongest single crisis signal** is a simultaneous inflection in all three components of Ψ — MMP, EMP, and SFD all accelerating in the same decade. In both of Turchin's case studies (Antebellum 1840s–1850s, Contemporary US 2000s–2010s), this convergence preceded the crisis peak by 10–20 years, which is the lead time the PSI provides.

---

### Running the model piecewise

Your intuition is correct. The equations are continuous ODEs, but the parameters that govern them are not necessarily constant over the life of a colony. As the colony's structural conditions change — its economic regime, its political arrangements, its relationship to the metropole — the appropriate parameter values change. The state variables (N_rur, N_urb, E, w, ε, ...) carry over continuously; only the structural parameters are reset at each regime transition.

This is standard practice in dynamical modelling and maps naturally onto the historical phases of a colony:

**Phase 1 — Foundation.** High ρ (rapid economic expansion driven by construction and resource development), high r_rur (young immigrant population), N_rur/K low (land abundant), λ high (working-age settlers), μ₀ moderate, s high (many positions to fill). Ψ is low and may actually fall for several decades as wages rise and elites are few.

**Phase 2 — Growth and pressure.** N_rur approaches K, emigration to cities increases, urban labour supply overtakes demand, w begins to fall. ρ may moderate as the initial construction boom ends. Ψ begins to rise. At this point, a policy regime shift (increasing s, raising K through new development, changing α via labour law) can reset the trajectory.

**Phase 3 — Mature stress.** w persistently below w₀, e growing rapidly, ε being diluted. If no structural relief has been introduced, Ψ is accelerating. This is when the model's usefulness as a forecasting tool is highest — it identifies the pre-crisis window.

**Phase 4 — Resolution or collapse.** Outside the model. If a crisis occurs, the post-crisis parameters are dramatically different: E may have been drastically reduced (elite culling), Y may have spiked (war debt), D_trust may have collapsed. The post-crisis initial conditions become the starting point for the next cycle.

**What triggers a parameter reset in the model:**

| Event | Parameters affected |
|-------|-------------------|
| New industry or resource discovery | ρ rises; possibly K rises |
| Terraforming or land clearance | K rises |
| Change in immigration policy | r_rur and N_rur(0) of new settlers |
| Labour law reform | α rises; γ and C may be introduced |
| Colonial government expansion | s rises |
| Corporate takeover or regime change | α falls; β may fall; γ changes sign |
| Debt crisis or fiscal austerity | Y rises; D_trust falls; SFD jumps |
| Elite purge or emigration wave | e drops; ε recovers; EMP resets |
| Epidemic or war | N_rur, N_urb, E all drop; w spikes (labour scarcity); temporary relief |

Each of these events is a regime boundary. The model runs continuously within a phase; at a boundary, you reset the relevant parameters and continue. The state variables at the moment of transition are the initial conditions for the next phase — the history is embedded in those values.

---

### Demographic structure: what the model does and does not capture

**The short answer: A₂₀₋₂₉ is exogenous in both of Turchin's models.** It is not generated by the population ODEs — it is either held constant (Antebellum simplification) or supplied as a measured empirical time series (Contemporary model from US Census data). The ODEs for N_rur (Eq. 6) and N_urb (Eq. 7) track only *total population numbers*, with no age cohort structure at all. The youth bulge appears as a factor in MMP but the model that produces population trajectories has no mechanism that could generate it.

Turchin acknowledges this directly for the Antebellum case: because the model assumes constant per capita birth rates, "the implied age structure is constant, and there can be no youth bulges." He treats A₂₀₋₂₉ as a parameter estimated from empirical data rather than something the model itself produces.

This is a significant structural gap for a forward-looking colony model, because the youth bulge is not a random shock — it is a *predictable, lagged consequence of earlier demographic events*. A birth-rate surge today shows up in A₂₀₋₂₉ approximately 20–25 years later (the Easterlin mechanism). The gap between the demographic event and its labour-market consequence is the mechanism Turchin's verbal theory relies upon to explain why instability waves have the period they do.

**What this means for a colony model:**

The model gives you three options for handling A₂₀₋₂₉:

**Option 1 — Hold constant (Turchin Antebellum approach).** Assume age structure does not vary. Appropriate if birth rates are stable and the colony is not experiencing a demographic transition. This effectively removes the youth-bulge component from MMP and lets wages and elite dynamics do all the work. Simplest; understates MMP during any period of rapid prior population growth.

**Option 2 — Supply as a scheduled external input.** Project A₂₀₋₂₉ forward based on assumed birth rate trajectories with a fixed ~22-year lag. If you set r_rur (birth rate) as a function of time in the early colony, the resulting youth bulge can be calculated in advance and fed in as a time-indexed parameter. This is the right approach for a piecewise model: the bulge is a *known future event* that can be scheduled into the parameter table, much like a regime change. It does not require modelling cohorts — just tracking the integral of births 20–25 years prior.

Example: a colony with very high natalism in years 1–30 (r_rur = 0.04–0.06) will produce a large 20–29 cohort in years 22–52. That bulge can be entered as a step increase in A₂₀₋₂₉ at the appropriate time, simultaneous with the labour-market pressure from those workers entering the job market.

**Option 3 — Add a cohort layer.** Model age cohorts explicitly by splitting N_rur and N_urb into age bands and applying differential survival and fertility rates. This would generate A₂₀₋₂₉ endogenously and allow feedback between economic conditions and birth rates. It is a substantial extension of the model, roughly doubling its complexity, and is only warranted if the timing and shape of youth bulges are a primary question for the scenario.

**The practical consequence of ignoring the youth bulge:**

If A₂₀₋₂₉ is held constant while the colony is actually experiencing a demographic surge, MMP will be *underestimated* during the bulge years. The model will show wage pressure (from N_urb rising) but will miss the additional mobilisation potential that comes from a large cohort of young adults who are simultaneously under-employed and at peak political radicalism. In historical cases this has consistently been a multiplier on instability: falling wages alone may be insufficient to trigger a crisis, but falling wages coinciding with a youth bulge is repeatedly associated with upheaval.

For a colony that expects rapid early growth, Option 2 is the minimum adequate treatment: schedule the bulge as a known future input and observe how it interacts with the wage and elite trajectories.

---

### Has anyone integrated a cohort model? Literature note

The short answer is no. A search of the cliodynamics and demographic-structural instability literature finds no published work that integrates a full age-cohort demographic model into a Turchin-type ODE system. Turchin's own later books (*Secular Cycles* 2009, *Ages of Discord* 2016, *End Times* 2023) all treat A₂₀₋₂₉ as empirical input. Korotayev, Malkov, and Romanov use demographic growth rates in their secular-cycle models but still supply the youth bulge proportion externally. Urdal's empirical youth-bulge instability research tests the relationship statistically but is not an ODE framework. Delay differential equations as a tool for modelling the 20-year cohort lag have not been applied to this problem in the published literature.

The standard field rationale for this gap: for historical analysis, birth-rate records are already known, so there is little analytical gain from deriving the cohort structure endogenously. The calculation adds complexity without improving fit.

For a forward-looking colony model, however, the situation is reversed — birth rates are not known in advance, and the youth-bulge trajectory is precisely one of the things you want to project. A side module is warranted.

---

### A cohort pipeline as a side module

The goal of the side module is narrow: produce a time series for A₂₀₋₂₉ that feeds into MMP. Everything else in the main model remains unchanged. The module needs only one input from the main model (total population N, or its growth rate) and returns one output.

**Minimal ODE pipeline.** Divide the population up to age 29 into five compartments and advance them as a chain:

$$\frac{dC_1}{dt} = \beta N - \frac{C_1}{5} \qquad \text{(ages 0–4; births enter, cohort exits after 5 years)}$$

$$\frac{dC_i}{dt} = \frac{C_{i-1}}{5} - \frac{C_i}{5} \qquad i = 2, 3, 4 \quad \text{(ages 5–9, 10–14, 15–19)}$$

$$\frac{dC_5}{dt} = \frac{C_4}{5} - \frac{C_5}{10} \qquad \text{(ages 20–29; 10-year cohort)}$$

$$A_{20\text{–}29}(t) = \frac{C_5(t)}{N(t)}$$

where β is the per capita birth rate (initially proportional to r_rur or r_urb, adjusted as the colony matures). This adds five ODEs to the system. Their initial conditions are set by the assumed age distribution at colony founding — typically a working-age-heavy distribution (low C₁, high C₃/C₄) for a screened settler population.

**Simpler alternative: fixed lag.** If ODE complexity is undesirable, approximate A₂₀₋₂₉ directly from past birth rates:

$$A_{20\text{–}29}(t) \approx \frac{\beta(t - 22) \cdot N(t - 22)}{N(t)}$$

This is a lookup: the size of the 20–29 cohort today equals the births 22 years ago, divided by the current total population. It is easy to implement in a spreadsheet and requires only a stored history of N(t). The 22-year figure is approximate; sensitivity to the exact lag is low.

**Optional: economic feedback on birth rates.** The Easterlin hypothesis predicts that birth rates rise when the generation entering adulthood faces good economic conditions (high w) and fall when they face poor ones — because people have more children when they feel optimistic about the future. This creates a delayed feedback loop:

$$\beta(t) = \beta_0 + \delta \cdot (w(t) - w_0)$$

A higher relative wage today → higher birth rate → a larger youth cohort 22 years later → downward pressure on wages. This is the mechanism that produces approximately 50-year instability cycles — closely matching the Kondratiev wave period discussed in the future-history document. Whether to include this feedback is a design choice: it generates richer long-run dynamics but adds a parameter (δ) that is difficult to calibrate without data.

**Coupling summary.** The demographic side module touches the main model at exactly two points:

| Direction | Variable | Where |
|-----------|----------|-------|
| Main → Side | N(t) or r_rur | Birth-rate calculation in side module |
| Side → Main | A₂₀₋₂₉(t) | MMP equation (U2, Eq. 14) |

If the Easterlin feedback is included, a third coupling is added: w(t) from the main model feeds into β(t) in the side module. Otherwise the side module runs as a pure one-way satellite.

---

## Explanations

### Economic Elasticity

**Elasticity** is a measure of how sensitive one variable is to a proportional change in another. Specifically, the elasticity of Y with respect to X is: *"if X increases by 1%, by what percentage does Y change?"* An elasticity of 1 means the two move in exact proportion; an elasticity of 0.5 means Y rises only half as fast as X; an elasticity of 2 means Y rises twice as fast.

In this paper the wage equation (Eq. 1) takes the form:

$$W \propto \left(\frac{G}{N}\right)^\alpha \left(\frac{D}{S}\right)^\beta C^\gamma$$

The exponents α, β, and γ *are* the elasticities:

- **α** is the elasticity of real wages with respect to GDP per capita. If α = 1 (used in the Antebellum model), a 10% rise in GDP per capita produces a 10% rise in wages — workers capture all productivity gains. If α < 1 workers capture less than all gains.
- **β** is the elasticity of real wages with respect to the labor demand/supply ratio D/S. If β = 0.5 (Antebellum value), doubling the demand/supply ratio raises wages by only ~41% (2^0.5 − 1), not 100%. This reflects that wages respond to labor market tightness, but with diminishing returns.
- **γ** is the elasticity of real wages with respect to the cultural/coercive factor C (proxied by the real minimum wage). It captures how much non-market forces amplify or dampen the market-determined wage level.

Using a multiplicative power-law form (rather than, say, an additive form) means the elasticities are constant across the full range of the data — a standard and empirically well-supported assumption for economic relationships of this type. It also means the equation becomes linear after log-transformation (Eq. 2), which is why log-space regression can estimate α, β, and γ directly as regression coefficients.

---

### Regression

**Regression** is a statistical procedure for estimating the relationship between a dependent variable (here, real wages W) and one or more independent variables (here, GDP per capita, D/S ratio, cultural factor C). Given a dataset of observed values, regression finds the coefficients — in this paper α, β, γ, and A — that make the equation fit the data as closely as possible, typically by minimising the sum of squared errors between the predicted and observed values of the dependent variable.

**Why log-transform first (Eq. 2)?** The multiplicative form of Eq. 1 becomes additive after taking logarithms:

$$\log W = A + \alpha \log(G/N) + \beta \log(D/S) + \gamma \log C + \varepsilon_t$$

This is now a standard *linear regression* — the unknown parameters (A, α, β, γ) appear linearly, which is much easier to estimate than fitting the nonlinear Eq. 1 directly. The log transformation also stabilises variance in the data (wages vary over a wide range; their logarithms vary much less), which is a standard requirement for reliable regression estimates.

**The error term ε_t** represents everything the model does not explain — measurement error, omitted variables, random shocks. Its subscript t indicates that the errors at different time points are potentially correlated with each other (autocorrelation), which is typical of economic time series: a wage shock in one year tends to persist into the next. Turchin models this autocorrelation explicitly using an **ARIMA(1,0,1)** error structure, which accounts for both a first-order autoregressive component (AR1: this year's error depends on last year's) and a first-order moving-average component (MA1: this year's error depends on last year's shock). This prevents the regression from treating serially correlated errors as if they were independent, which would otherwise produce overconfident (too-narrow) uncertainty estimates.

**Regression in a theoretical context.** In everyday applied work, regression is often exploratory: you have data and want to discover whether and how variables relate, choosing a functional form (often linear) for convenience. Here the sequence is reversed. Structural-demographic theory specifies *which* variables belong in the wage equation and *why* — from economic reasoning about labor markets and power relations. The multiplicative power-law form of Eq. 1 is also theory-derived, not chosen for convenience. Regression then plays two roles: it **confirms** that the theoretically nominated variables account for observed wage variation, and it **calibrates** the elasticity values α, β, and γ — the theory says these factors matter but does not predict their magnitude; the data supply the magnitudes. Model selection is likewise constrained: Turchin adds variables one at a time but only considers variables the theory nominates, not a wide search over candidate predictors. The practical consequence is that a high R² here carries more evidential weight than in exploratory regression. Because the functional form and variable selection were fixed before the data were examined, high explanatory power is genuine evidence that the theory's causal story is correct — not merely that the curve fits.

**The coefficient of determination R²** (cited in the text as reaching 0.98 for the three-factor model) measures what fraction of the variance in the observed wage data is explained by the model. R² = 0 means the model explains nothing; R² = 1 means a perfect fit. An R² of 0.98 indicates that 98% of the historical variation in real wages is accounted for by the three factors — GDP per capita, labor demand/supply, and the cultural proxy — leaving only 2% unexplained.
