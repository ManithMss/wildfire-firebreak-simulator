# Theory and mathematics

## 1. Problem formulation

Firebreaks are cleared strips of land used to interrupt the spread of a wildfire. This project asks: how does firebreak width affect the probability that a fire is contained, under different wind strengths and directions, once the possibility of wind-carried embers jumping the firebreak is included?

The system is represented as a two-dimensional grid of cells, each either unburned, burning, burned, or part of a firebreak. A fire is seeded on one edge of the grid and spreads probabilistically to neighbouring cells; the quantity of interest is the probability that the fire burns out before reaching the far side of the firebreak.

## 2. Assumptions

1. The grid is spatially uniform except for the firebreak strip; vegetation density, slope, and fuel moisture are not modelled explicitly.
2. Fire spreads only to the eight orthogonal and diagonal neighbours of a burning cell (the Moore neighbourhood).
3. A burning cell burns for exactly one discrete time step before becoming burned fuel.
4. Wind is constant in direction and strength for the duration of a single simulation run.
5. Ember spotting is modelled as an independent random jump of limited distance in the wind direction, rather than a physically derived ember-trajectory model.

## 3. Model equations

Let the grid configuration at time step t be X(t), the collection of all cell states. Each unburned cell (i, j) transitions to burning with probability p(i, j, t):

$$
p(i,j,t) = 1 - (1 - p_0)^{\,w(i,j,t)}
$$

where p0 is a base per-neighbour ignition probability and w(i, j, t) is a wind-weighted count of burning neighbours:

$$
w(i,j,t) = \sum_{k \,\in\, \text{burning neighbours}} \Big(1 + s \cdot \max\big(0,\ \hat{d}_k \cdot \hat{u}\big)\Big)
$$

Here, d_k(hat) is the unit vector from burning neighbour k toward cell (i, j), u(hat) is the unit wind-direction vector, and s is the wind-strength parameter. With probability p_ember per burning cell per time step, a burning cell may also ignite a cell 1 to 3 cells away in the wind direction, independent of any firebreak cells in between (ember spotting).

The containment probability for a fixed configuration is estimated by Monte Carlo simulation over N independent replications:

$$
\hat{P}_{\text{contain}} = \frac{1}{N}\sum_{r=1}^{N} Y_r, \qquad \text{SE} = \sqrt{\frac{\hat{P}_{\text{contain}}(1-\hat{P}_{\text{contain}})}{N}}
$$

where Y_r is 1 if replication r was contained and 0 otherwise.

| Symbol | Meaning | Units | Value / range | Source |
|---|---|---|---|---|
| L | Grid side length | cells | 40-80 | Chosen for runtime vs detail trade-off |
| W | Firebreak width | cells | 0-6 | Swept in experiments |
| p0 | Base ignition probability | probability | 0.25 | Tuned so outcomes are neither always 0% nor always 100% |
| s | Wind strength | dimensionless, 0-1 | 0.0-1.0 | Swept in experiments |
| p_ember | Ember-spotting probability | probability per burning cell per step | 0.05 | Synthetic/assumed, tuned alongside p0 |
| N | Monte Carlo replications | count | 100-150 | Chosen for a reasonably tight 95% CI within notebook runtime budget |

## 4. Numerical method

The simulation uses next-state cellular-automaton updates: at each discrete time step, every cell's next state is computed from the current grid (not updated in place), so all transitions within a step are based on a single consistent snapshot. Random variate generation uses a seeded pseudo-random generator (NumPy's default generator in the notebook, a seeded linear-congruential-style generator in the browser demo) so that every run is reproducible given its seed. The time step and grid resolution are fixed within each experiment; a resolution-sensitivity check (Section 5) compares two grid sizes with proportionally matched firebreak width to assess how much the discretisation itself affects the result.

## 5. Verification

| Check | Expected | Obtained | Pass? |
|---|---|---|---|
| Burned cells never re-ignite | No burned-to-burning transition | None observed across 60 steps | Yes |
| Conservation of total grid cells | Cell count constant every step | Constant at 1,600 for all 80 steps tested | Yes |
| Zero ignition probability (limiting case) | Fire cannot spread past initial cells | Only the 3 initially ignited cells burned | Yes |
| Zero ember-spotting + full-width firebreak (limiting case) | Fire always contained | Contained in 20 of 20 runs | Yes |
| Reproducibility | Identical seed gives identical outcome | Identical result across repeated runs | Yes |
| Sensitivity to grid resolution | Comparable order of magnitude at different resolutions | 0.483 (40x40, width 2) vs 1.000 (80x80, width 4); same qualitative order | Yes, with a noted resolution dependence |

![Verification plot](figures/verification.png)

## 6. Extensions

Three extensions beyond the core model are implemented in the notebook (Section 6):

- **Rigorous verification (independent error-scaling check):** the Monte Carlo containment-probability estimate was repeated many times at several replication counts N (10 to 160), and the spread of the repeated estimates was fitted against N on a log-log plot. The fitted slope was -0.47, consistent with the expected -0.5 exponent for a correctly behaving Monte Carlo estimator, independently confirming the estimator in Section 3.5 behaves as intended.
- **Stretch extension (spatially varying vegetation density):** the uniform base ignition probability was replaced with a spatially varying field (a smoothly upsampled random patch grid), so some regions of the forest are more or less flammable than others, while the firebreak itself remains unaffected. At a fixed firebreak width and wind setting, patchy vegetation density gave a measurably different containment probability (0.380 +/- 0.095) compared with uniform density (0.270 +/- 0.087), showing that fuel heterogeneity is not a negligible factor.
- **Sensitivity and uncertainty analysis:** containment probability and its 95% confidence interval were computed over a full grid of firebreak width (0 to 4) and wind strength (0.0 to 0.9) combinations, visualised as two heatmaps. As expected from the statistics of a Bernoulli proportion, the confidence interval is widest near the transition region where containment probability is close to 0.5, and narrowest where the outcome is close to certain (0 or 1) in either direction.

## 7. Concepts applied

| Concept | Where it is used in this project |
|---|---|
| Discrete-time Markov chains | The grid's state-transition rule: the next configuration depends only on the current configuration and independent random draws, not on history |
| Input modelling and random variate generation | Seeded uniform random draws used for ignition decisions and ember-spotting events |
| Monte Carlo integration and the law of large numbers | Estimating containment probability and its 95% confidence interval from repeated independent stochastic replications |

## 8. References

See [REFERENCES.md](REFERENCES.md).
