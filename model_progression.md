# Great Salt Lake Ecosystem Model 



## Base Model

### State Variables

| Symbol | Meaning |
|---|---|
| $S(t)$ | Brine shrimp population |
| $A(t)$ | Algae population |
| $B(t)$ | Bird population |

### Equations

$$
\begin{aligned}
\frac{dS}{dt} &= \gamma_S \, C_A \, S A \;-\; C_S \, S B \\[4pt]
\frac{dA}{dt} &= \beta_A \, A \;-\; C_A \, S A \\[4pt]
\frac{dB}{dt} &= -\delta_B \, B \;+\; \gamma_B \, C_S \, S B
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

- **Interaction rates ($C$).** When two populations interact, one eats the other. For example, $C_S \, S B$ is the rate at which birds eat shrimp, and $C_A \, S A$ is the rate at which shrimp eat algae.
- **Conversion efficiencies ($\gamma$).** $\gamma$ is the population gain from one thing consuming another. For example, $\gamma_S \, C_A \, S A$ is the shrimp population gained from all the shrimp eating algae. We always have $\gamma < 1$, because a shrimp must eat many algae before it can support producing an additional offspring (and likewise for birds eating shrimp).
- **Natural death ($\delta$).** $\delta_B \, B$ is the loss of birds to natural death.

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
> $$S' = 0, \qquad A' = 0, \qquad B' = 0.$$
>
> We want a *coexistence* equilibrium, where shrimp, algae and birds are all present ($S, A, B > 0$).
>
> **Step 2: Factor each equation.** Each equation has its own population as a common factor, which makes it easy to read off when it equals zero.
>
> $$
> \begin{aligned}
> A' &= \beta_A A - C_A S A = A\,(\beta_A - C_A S)\\
> B' &= -\delta_B B + \gamma_B C_S S B = B\,(-\delta_B + \gamma_B C_S S)\\
> S' &= \gamma_S C_A S A - C_S S B = S\,(\gamma_S C_A A - C_S B)
> \end{aligned}
> $$
>
> A product is zero only if one of its factors is zero. Since we require $A, B, S > 0$, the expression in parentheses must be zero in each case.
>
> **Step 3: What algae need.** Setting algae's bracket to zero:
>
> $$\beta_A - C_A S = 0 \quad\Longrightarrow\quad S = \frac{\beta_A}{C_A}.$$
>
> Algae stay constant only when there are exactly enough shrimp for grazing to cancel algae growth. Fewer shrimp and algae grow; more shrimp and algae shrink.
>
> **Step 4: What birds need.** Setting birds' bracket to zero:
>
> $$-\delta_B + \gamma_B C_S S = 0 \quad\Longrightarrow\quad S = \frac{\delta_B}{\gamma_B C_S}.$$
>
> Birds stay constant only when there are exactly enough shrimp for bird births to cancel bird deaths.
>
> **Step 5: The conflict.** Algae and birds each demand a specific shrimp population, and they don't consult each other. Both conditions hold at once only if the two targets are equal:
>
> $$\frac{\beta_A}{C_A} = \frac{\delta_B}{\gamma_B C_S} \quad\Longrightarrow\quad \boxed{\;\delta_B = \frac{\gamma_B\, C_S\, \beta_A}{C_A}\;}$$
>
> With our parameters ($\gamma_B = 0.3$, $C_S = 0.4$, $\beta_A = 1$, $C_A = 0.5$):
>
> $$\delta_B = \frac{0.3 \times 0.4 \times 1}{0.5} = 0.24.$$
>
> This is the **balance point**. For any other bird death rate, no coexistence equilibrium exists.

</details>

#### Behavior Away From the Balance Point
 
**Full model (time series).** Combining the algae and bird equations shows that
 
$$\frac{d}{dt}\left[\gamma_B C_S \ln A + C_A \ln B\right] = \gamma_B C_S \beta_A - C_A \delta_B,$$
 
a constant, so the outcome is fixed by its sign:
 
- **$\delta_B > 0.24$:** birds and algae decay exponentially toward extinction while shrimp level off.
- **$\delta_B = 0.24$:** neutral cycles; each initial condition traces its own closed orbit.
- **$\delta_B < 0.24$:** algae and birds grow without bound while shrimp oscillations widen.



**Conclusion.** The base model is structurally unstable: realistic, sustained coexistence occurs only on a single knife-edge parameter value, and any perturbation, such as a seasonal change or measurement error in $\delta_B$, sends the system to collapse or blow-up. The next version should add a carrying capacity for algae, $\beta_A A (1 - A/K)$, and a natural death term for shrimp, $-\delta_S S$, which together produce a stable coexistence equilibrium.