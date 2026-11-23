# Wildfire Firebreak Containment Simulator

A stochastic, grid-based simulation exploring how firebreak width and wind conditions affect the probability of containing a spreading wildfire.

**[Live demo](https://ManithMss.github.io/wildfire-firebreak-simulator/)** · [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ManithMss/wildfire-firebreak-simulator/blob/main/notebook/wildfire-firebreak-simulator.ipynb)

![Screenshot of the web demo](figures/demo-screenshot.png)

## The problem

Firebreaks, cleared strips of land with no vegetation, are used to stop wildfires from spreading, but how wide a firebreak needs to be depends heavily on wind: strong wind can push flames further and carry burning embers across a strip that would otherwise stop the fire. This project simulates wildfire spread across a grid as a stochastic process, with a firebreak of adjustable width, to answer: how does firebreak width affect the probability of containing a fire under different wind strengths and directions, once the chance of embers jumping the firebreak is included?

This matters for anyone planning land management or fire-protection strips: a firebreak that is adequate in calm conditions may fail completely once wind blows directly toward it, and this project gives a quantitative way to see how much wider a firebreak needs to be as wind conditions worsen.

## Quick start

**Notebook:** click the *Open in Colab* badge above, then *Runtime -> Run all*. Everything installs and runs in about 3 minutes on a free runtime.

**Web demo:** open the [live demo](https://ManithMss.github.io/wildfire-firebreak-simulator/), or download this repository and double-click `docs/index.html`.

**Run locally (optional):**

```bash
pip install -r requirements.txt
jupyter notebook notebook/wildfire-firebreak-simulator.ipynb
```

## Using the demo

Adjust the firebreak width, wind strength and wind direction, then press Start to watch one fire spread live on the grid, colour-coded by state. Press "Run Monte Carlo estimate" to run many quick simulations in the background and get the probability that the fire is contained, with a 95% confidence interval. Full guide: [USER_GUIDE.md](USER_GUIDE.md).

## Model and mathematics

The forest is a grid of cells (unburned, burning, burned, or firebreak); the grid configuration evolves as a discrete-time Markov chain, with each unburned cell igniting with a wind-weighted probability based on its burning neighbours:

$$ p(i,j,t) = 1 - (1 - p_0)^{\,w(i,j,t)} $$

Containment probability is estimated by Monte Carlo simulation over many independent replications, with 95% confidence intervals from the law of large numbers. Full derivations, assumptions and parameters: [THEORY.md](THEORY.md).

## Verification

Six checks were run, each shown with an `assert` in the notebook: burned cells never re-ignite, total grid cells are conserved every step, zero ignition probability stops all spread beyond the initial cells, a full-width firebreak with no ember-spotting always contains the fire, identical seeds give identical results, and results are of the same order of magnitude at two different grid resolutions. All six checks pass.

![Verification plot](figures/verification.png)

## Concepts applied

This project applies discrete-time Markov chains to model the grid's stochastic state transitions, input modelling and random variate generation for ignition and ember-spotting events, and Monte Carlo simulation with confidence intervals based on the law of large numbers to estimate containment probability and quantify its uncertainty.

## Results

Containment probability rises sharply with firebreak width, from close to 0% with no firebreak to 100% at a width of 3 to 4 cells. Wind direction matters more than wind strength alone: a firebreak at width 1 contains the fire in 100% of runs when wind blows away from or parallel to it, but only around 9% of runs when wind blows directly toward it (a headfire scenario). A firebreak of at least 3 cells is needed to reach 90% or higher containment once any appreciable wind is present.

## Limitations and future work

- Vegetation density, slope and fuel moisture are not modelled; the grid is uniform except for the firebreak itself.
- Ember spotting uses a simplified random-jump rule rather than a physically derived ember-transport model; a distance-decaying, wind-calibrated spotting model would be a natural extension.
- Wind is constant within a run; a time-varying wind field would better represent a real, multi-hour wildfire.

## Key references

1. A. L. Sullivan, "Wildland surface fire spread modelling, 1990-2007. 1: Physical and quasi-physical models," *International Journal of Wildland Fire*, vol. 18, no. 4, pp. 349-368, 2009, doi: 10.1071/WF06143.
2. A. M. G. Alexandridis et al., "A cellular automata model for forest fire spread prediction," *Applied Mathematics and Computation*, vol. 204, no. 1, pp. 191-201, 2008, doi: 10.1016/j.amc.2008.06.046.

Full list: [REFERENCES.md](REFERENCES.md).

## AI assistance

Developed with the help of AI coding and writing assistants. All code was reviewed, run and verified by the author.

## Licence and credits

Code: [MIT](LICENSE). Data: [source, licence, e.g. "Weather data: Open-Meteo, CC BY 4.0"].
