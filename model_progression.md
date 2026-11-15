# Great Salt Lake Ecosystem Model 



## Base Model

### State Variables

| Symbol | Meaning |
|---|---|
| $S(t)$ | Brine shrimp density (individuals/L) |
| $A(t)$ | Water chlorophyll-a concentration (algae proxy) |
| $B(t)$ | Bird population |

### Equations

$$
\begin{aligned}
\frac{dS}{dt} &= \gamma_S C_A S A - C_S S B \\
\frac{dA}{dt} &= \beta_A A - C_A S A \\
\frac{dB}{dt} &= -\delta_B B + \gamma_B C_S S B
\end{aligned}
$$

### Parameters

| Symbol | Meaning |
|---|---|
| $\beta_A$ | Algae growth rate |
| $C_A$ | Interaction rate between shrimp and algae (shrimp eating algae) |
| $C_S$ | Interaction rate between birds and shrimp (birds eating shrimp) |
| $\gamma_S$ | Shrimp population gain per algae consumed |
| $\gamma_B$ | Bird population gain per shrimp consumed |
| $\delta_B$ | Natural death rate of birds |

### Interpretation of Terms

- **Interaction rates ($C$).** When two populations interact, one eats the other. For example, $C_S   S B$ is the rate at which birds eat shrimp, and $C_A   S A$ is the rate at which shrimp eat algae.
- **Conversion efficiencies ($\gamma$).** $\gamma$ is the population gain from one thing consuming another. For example, $\gamma_S C_A S A$ is the shrimp population gained from all the shrimp eating algae. These conversion factors have units determined by the population measurements; their numerical values are not universally bounded by one.
- **Natural death ($\delta$).** $\delta_B B$ is the loss of birds to natural death.

#### Term-by-term

| Equation | Term | Meaning |
|---|---|---|
| $S'$ | $+\gamma_S C_A S A$ | Shrimp growth from eating algae |
| $S'$ | $-C_S S B$ | Shrimp eaten by birds |
| $A'$ | $+\beta_A A$ | Algae growth |
| $A'$ | $-C_A S A$ | Algae eaten by shrimp |
| $B'$ | $-\delta_B B$ | Bird natural death |
| $B'$ | $+\gamma_B C_S S B$ | Bird growth from eating shrimp |


### Analysis
 
![Shrimp–algae phase plane with no birds](model1/shrimp_algae_plane.png)
 
**Shrimp–algae plane (no birds).** With birds absent, shrimp have no loss term, so $S' = \gamma_S C_A S A \ge 0$ and the shrimp population can only grow. Algae rise while $S < \beta_A / C_A$ and fall once shrimp cross that nullcline. Every trajectory therefore drifts to the right and ends with algae grazed to zero and shrimp frozen at a constant level. There are no closed orbits and no equilibrium in which both survive.
 





![Time series for three bird death rates](model1/timeseries.png)
 





<details>
<summary><b>The Balance Point: Derivation</b></summary>

> **Step 1: What we're looking for.** An *equilibrium* is a state where nothing changes, so all three derivatives are zero:
>
> $$
> S' = 0, \qquad A' = 0, \qquad B' = 0.
> $$
>
> We want a *coexistence* equilibrium, where shrimp, algae and birds are all present ($S, A, B > 0$).
>
> **Step 2: Factor each equation.** Each equation has its own population as a common factor, which makes it easy to read off when it equals zero.
>
> $$
> \begin{aligned}
> A' &= \beta_A A - C_A S A = A (\beta_A - C_A S)\\
> B' &= -\delta_B B + \gamma_B C_S S B = B (-\delta_B + \gamma_B C_S S)\\
> S' &= \gamma_S C_A S A - C_S S B = S (\gamma_S C_A A - C_S B)
> \end{aligned}
> $$
>
> A product is zero only if one of its factors is zero. Since we require $A, B, S > 0$, the expression in parentheses must be zero in each case.
>
> **Step 3: What algae need.** Setting algae's bracket to zero:
>
> $$
> \beta_A - C_A S = 0 \quad\Longrightarrow\quad S = \frac{\beta_A}{C_A}.
> $$
>
> Algae stay constant only when there are exactly enough shrimp for grazing to cancel algae growth. Fewer shrimp and algae grow; more shrimp and algae shrink.
>
> **Step 4: What birds need.** Setting birds' bracket to zero:
>
> $$
> -\delta_B + \gamma_B C_S S = 0 \quad\Longrightarrow\quad S = \frac{\delta_B}{\gamma_B C_S}.
> $$
>
> Birds stay constant only when there are exactly enough shrimp for bird births to cancel bird deaths.
>
> **Step 5: The conflict.** Algae and birds each demand a specific shrimp population, and they don't consult each other. Both conditions hold at once only if the two targets are equal:
>
> $$
> \frac{\beta_A}{C_A} = \frac{\delta_B}{\gamma_B C_S} \quad\Longrightarrow\quad \boxed{ \delta_B = \frac{\gamma_B C_S \beta_A}{C_A} }
> $$
>
> With our parameters ($\gamma_B = 0.3$, $C_S = 0.4$, $\beta_A = 1$, $C_A = 0.5$):
>
> $$
> \delta_B = \frac{0.3 \times 0.4 \times 1}{0.5} = 0.24.
> $$
>
> This is the **balance point**. For any other bird death rate, no coexistence equilibrium exists.

</details>

#### Behavior Away From the Balance Point
 
**Full model (time series).** Combining the algae and bird equations shows that
 
$$
\frac{d}{dt}\left[\gamma_B C_S \ln A + C_A \ln B\right] = \gamma_B C_S \beta_A - C_A \delta_B,
$$
 
a constant, so the outcome is fixed by its sign:
 
- **$\delta_B > 0.24$:** birds and algae decay exponentially toward extinction while shrimp level off.
- **$\delta_B = 0.24$:** neutral cycles; each initial condition traces its own closed orbit.
- **$\delta_B < 0.24$:** algae and birds grow without bound while shrimp oscillations widen.



**Conclusion.** The base model is structurally unstable: realistic, sustained coexistence occurs only on a single knife-edge parameter value, and any perturbation, such as a seasonal change or measurement error in $\delta_B$, sends the system to collapse or blow-up. The next version should add a carrying capacity for algae, $\beta_A A (1 - A/K)$, and a natural death term for shrimp, $-\delta_S S$, which together produce a stable coexistence equilibrium.


## Phase 2: Logistic Algae Growth and Natural Shrimp Mortality

The base model allows algae to grow without limit and shrimp to survive indefinitely when birds are absent. We now add a carrying capacity for algae and a natural death rate for shrimp.   

### Equations

$$
\frac{dS}{dt} = \gamma_S C_A S A - C_S S B - \delta_S S
$$

$$
\frac{dA}{dt} = \beta_A A\left(1 - \frac{A}{K}\right) - C_A S A
$$

$$
\frac{dB}{dt} = -\delta_B B + \gamma_B C_S S B
$$

### New Parameters

| Symbol | Meaning |
|---|---|
| $K$ | Algae carrying capacity in the absence of shrimp grazing |
| $\delta_S$ | Natural death rate of shrimp |

All other parameters keep their meanings from the base model.

### Interpretation of New Terms

- **Logistic algae growth.** $\beta_A A(1 - A/K)$ replaces unlimited exponential growth. When algae are scarce, they grow approximately as before. As algae approach $K$, their growth slows; above $K$, the growth term becomes negative.
- **Natural shrimp death.** $-\delta_S S$ accounts for shrimp dying even when birds are absent. Shrimp must now consume enough algae to offset these losses before their population can grow.

### Direction of Analysis

We will examine whether these changes allow stable coexistence across a range of parameter values. As in the base model, we will start with the shrimp/algae phase plane without birds, then examine time series for all three populations and vary the bird death rate.

Later, we may experiment with Holling type II feeding responses to represent consumption rates that saturate as food becomes abundant. For now, feeding interactions retain the base model's form.


## Phase 3: Holling Type II Feeding

We keep logistic algae growth and natural shrimp mortality from phase 2, and now allow feeding to saturate as food becomes abundant. Both shrimp eating algae and birds eating shrimp use a Holling type II response.

### Equations

$$
\frac{dS}{dt} = \gamma_S\frac{C_A S A}{1+h_A C_A A} - \frac{C_S S B}{1+h_S C_S S} - \delta_S S
$$

$$
\frac{dA}{dt} = \beta_A A\left(1-\frac{A}{K}\right) - \frac{C_A S A}{1+h_A C_A A}
$$

$$
\frac{dB}{dt} = -\delta_B B + \gamma_B\frac{C_S S B}{1+h_S C_S S}
$$

### New Parameters

| Symbol | Meaning |
|---|---|
| $h_A$ | Handling time per algae consumed by shrimp |
| $h_S$ | Handling time per shrimp consumed by birds |

All other parameters keep their meanings from phase 2. Setting $h_A=h_S=0$ recovers that model.

### Interpretation of New Terms

- **Shrimp feeding.** $C_A A/(1+h_A C_A A)$ is consumption per shrimp. At low algae abundance it is approximately $C_A A$; for positive $h_A$, it approaches a maximum of $1/h_A$ as algae become abundant.
- **Bird feeding.** $C_S S/(1+h_S C_S S)$ is consumption per bird. For positive $h_S$, it approaches a maximum of $1/h_S$ as shrimp become abundant.
- **Population gains and losses.** Each feeding term appears as a loss for the prey and a gain, multiplied by the conversion efficiency, for the consumer.

### Direction of Analysis

The new [Phase 3 notebook](model3/phase3_model.ipynb) compares phase 2 with feeding saturation on one or both links. We will examine feeding curves, time series, coexistence candidates and their local stability, and the shrimp-algae phase plane without birds. Saturation may change stability, so we will check it rather than assume phase 2's behavior carries over.

The starting handling times are $h_A=h_S=0.5$ in model units. These are exploratory values, not estimates from observations.

## Species, units, and parameter evidence

For this school exploration, the algae state $A$ is **water-column chlorophyll-a concentration**, used directly as a proxy for algae abundance. Its units are **micrograms of chlorophyll-a per liter** ($\mu$g Chl-a/L). It is not total algal dry mass or carbon. Water chlorophyll includes multiple phytoplankton species; treating it as the food available to shrimp is a simplifying assumption.

*Dunaliella viridis* remains the focal algae species motivating the model. [Utah DWR, Algae and algal blooms](https://wildlife.utah.gov/gslep/wildlife/algae) identifies it as a preferred food of brine shrimp. The chlorophyll proxy is not a species-specific measurement of *D. viridis*.

### Sources for chlorophyll, salinity, and shrimp observations

- [Belovsky et al. (2025), Environment and phytoplankton relative abundances in a hypersaline lake: 27 years in Great Salt Lake, USA and experiments](https://link.springer.com/article/10.1007/s10452-025-10190-1): a source for chlorophyll-a measurements, salinity information, and experiments with *D. viridis*. Laboratory chlorophyll is measured in micrograms Chl-a/L. Experimental salinities are 30, 60, 90, 120, and 150 **ppt** (parts per thousand); these are experimental treatments, not a chosen present-day lake salinity. The study also analyzes South Arm field monitoring from 1994-2020. Its experimental absolute growth measure is not directly the per-capita `beta_A` in our equations.
- [Utah DWQ Great Salt Lake Data Explorer](https://udwq.shinyapps.io/GSL_data_explorer/): a place to look for lake chlorophyll-a and salinity observations. [DWQ monitoring documentation](https://lf-public.deq.utah.gov/WebLink/ElectronicFile.aspx?dbid=0&docid=14163&eqdocs=DWQ-2021-018149&repo=Public) describes these parameters. Check the downloaded record's units, date, location, depth, and method; the dashboard was not directly inspected. Do not interchange ppt and g/L without checking the reported salinity definition and brine density.
- [Utah DWR brine shrimp sampling reports](https://wildlife.utah.gov/gslep/harvest): observations **per liter of lake water**, separated by life stage and sex. Use the appropriate live-shrimp categories for $S$ and avoid double-counting female subcategories. Cysts are separate from live shrimp.

### Chosen state units

Time is measured in **days**. Shrimp is measured as **density (individual shrimp/L)** to match the sampling reports and the concentration basis of algae. Birds remain **individual birds** in the modeled region. Population observations must describe compatible dates and habitat. The remaining bird species, shrimp life stages, season, and region are choices to make before calibration.

| Quantity | Meaning | Unit / status |
|---|---|---|
| $t$ | Elapsed time | days |
| $A$ | Water chlorophyll-a, a proxy for algae | $\mu$g Chl-a/L |
| $S$ | Live brine shrimp density | individual shrimp/L |
| $B$ | Bird abundance in the modeled region | individual birds |
| $A_0,S_0,B_0$ | Initial observations | Same units as the corresponding states |
| $T$ | Simulation duration | days |
| Salinity (not yet a model state or parameter) | Environmental context | Study experiments: ppt; retain units specified by each field dataset |

### Parameter table

The units below follow from the current equations with these state units. Default values are shared by Phases 2 and 3; handling times apply only to Phase 3. **All existing numbers remain illustrative and uncalibrated under the chosen units.** Relabeling the states does not convert or validate the old numerical values.

| Code parameter | Meaning | Required units | Illustrative default | Evidence to seek |
|---|---|---|---|---|
| `beta_A` | Maximum per-capita algae growth rate | day$^{-1}$ | 1.0 | Fit growth curves under relevant environmental conditions |
| `K` | Chlorophyll carrying capacity without grazing | $\mu$g Chl-a/L | 10.0 | Ungrazed experiments or constrained estimates; grazed maxima need not equal `K` |
| `C_A` | Shrimp-algae feeding coefficient | L/(shrimp day) | 0.5 | Per-shrimp chlorophyll removal versus food concentration |
| `C_S` | Effective bird-shrimp feeding coefficient | 1/(bird day) | 0.4 | Bird feeding and habitat volume conversions |
| `gamma_S` | Shrimp density gain per chlorophyll concentration loss | shrimp/($\mu$g Chl-a) | 0.2 | Shrimp production versus chlorophyll removal |
| `gamma_B` | Bird count gain per shrimp density loss | bird L/shrimp | 0.3 | Demographic evidence and volume conversion; review migration assumptions |
| `delta_S` | Shrimp natural per-capita mortality | day$^{-1}$ | 0.1 | Stage-specific survival, separated from predation |
| `delta_B` | Bird natural per-capita mortality | day$^{-1}$ | 0.12 | Survival, separated from migration departures |
| `h_A` | Inverse maximum per-shrimp chlorophyll consumption rate | day shrimp/($\mu$g Chl-a) | 0.5 | Saturating shrimp feeding curve |
| `h_S` | Inverse maximum per-bird shrimp density removal rate | day bird L/shrimp | 0.5 | Saturating bird feeding curve and habitat volume conversion |

`gamma_S` and `gamma_B` are unit-dependent conversion factors, not necessarily dimensionless efficiencies or bounded by one. Chlorophyll-based conversions are effective model relationships rather than direct food-mass efficiencies.

Because birds are counts and shrimp is density, the bird predation terms still require a defined habitat volume when translating physical consumption into model coefficients. For a fixed volume $V$ in liters, total bird consumption $Q$ shrimp/day removes density at $Q/V$ shrimp/L/day. Thus `C_S`, `gamma_B`, and `h_S` include spatial conversions. Retaining the current equations is acceptable for exploration with effective coefficients, but a changing-volume model would require an explicit mass balance.

Next steps: select compatible lake observations, document their region, season, depths, and units, then estimate daily parameter ranges with sources and uncertainty. Salinity is recorded as context for now; the current equations do not yet depend on it.
