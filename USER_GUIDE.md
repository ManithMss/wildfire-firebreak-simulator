# User guide: Wildfire Firebreak Containment Simulator web demo

**Open the demo:** [https://manithmss.github.io/wildfire-firebreak-simulator/](https://manithmss.github.io/wildfire-firebreak-simulator/). It runs entirely in your browser (current Chrome, Edge or Firefox). Nothing is installed or uploaded.

![Screenshot of the demo](figures/demo-screenshot.png)

The page has three panels: the scenario controls on the left, the live fire grid with counters and a chart in the middle, and the Monte Carlo gauge with the scenario history on the right.

## Controls

| Control | What it does | Range | Default |
|---|---|---|---|
| Firebreak width | Width of the cleared strip at the centre of the grid (cells) | 0-6 | 2 |
| Wind strength | How strongly wind biases spread toward the wind direction | 0.0-1.0 | 0.5 |
| Wind direction | Which way the wind blows (East blows the fire toward the firebreak) | None/North/South/East/West | East |
| Animation speed | How many simulation steps are shown per second (visual only, does not change results) | 2-30 | 12 |
| Monte Carlo runs | Number of independent simulations used for the containment-probability estimate | 10-100 | 50 |
| Random seed | Fixes the pseudo-random sequence for the animated run and the Monte Carlo estimate | any positive integer | 42 |
| Start / Pause | Runs or pauses the single animated simulation (keyboard: Space) | - | - |
| Reset | Clears the run and applies the current settings (keyboard: R) | - | - |
| Quick scenarios | Load and start a preset: Calm day, Headfire, Crosswind, Wide firebreak | - | - |

## Reading the outputs

- **Grid visualisation:** each cell is coloured by state: green (unburned), flickering orange (burning), dark ash (burned, glowing briefly as it cools), tan (firebreak). Smoke drifts with the wind, faint streaks show the wind direction, and glowing sparks show ember jumps flying over the firebreak. The dashed line marks the start of the protected side; a banner reports "Fire contained" or "Firebreak breached" when the run ends.
- **Step / Burning now / Burned cells / Status:** running counters for the current animated run. The bar under Burned cells shows the share of the forest burned so far.
- **Chart:** the orange area shows how many cells are burning at each step; the grey line shows the total share of the forest burned.
- **Wind compass:** the arrow next to the wind direction rotates to the chosen direction and grows with wind strength.
- **P(contained) with 95% CI:** the quantitative output. Pressing "Run Monte Carlo estimate" (keyboard: M) runs many quick simulations (not animated) at the current settings and shows the estimated probability of containment on a gauge (red below 0.4, amber below 0.8, green above), with a 95% confidence interval, exactly as the notebook does. Each estimate is added to the Scenario history table so different settings can be compared side by side.
- **Notebook reference check:** loads the Experiment 1 settings (width 2, wind 0.6 East, seed 1, 100 runs) and runs the estimate, so the demo can be compared with the notebook in one click (see below).

## Try this

1. Set firebreak width to 0 and wind direction to East, then click "Run Monte Carlo estimate". With no firebreak, the containment probability should be close to 0.
2. Change the width to 4 with the same wind and run the estimate again. The probability should jump up, usually to 1.
3. Click "Headfire" and then "Crosswind". Both use width 1 and wind strength 0.8, but East wind pushes the fire and its embers straight at the firebreak, so it breaks through far more often than North wind, which blows along it. Run the Monte Carlo estimate for each and compare them in the history table.

## Check against the notebook

Click **Notebook reference check**. It sets width 2, wind strength 0.6, East, seed 1 and 100 runs, which are the Experiment 1 settings, and runs the estimate. Because the seed is fixed, the result is the same every time.

| Source | Settings | Result |
|---|---|---|
| Web demo | width 2, wind 0.6 East, 100 runs | **0.480 +/- 0.098** |
| Notebook, Experiment 1 | width 2, wind 0.6 East, 150 runs | **0.440 +/- 0.079** |

**Tolerance:** the two results agree if their difference is smaller than the combined 95% interval, 1.96 x sqrt(SE_demo^2 + SE_notebook^2) = 0.126. The difference is 0.040, so the check passes. They are not identical because the browser uses a different random number generator (mulberry32) and fewer runs.

## Limitations

- The demo uses the same 50x50 grid and p0 = 0.3 as the notebook's experiments, but a different random number generator, so its numbers agree within the confidence intervals rather than exactly. The notebook's resolution check shows the probability depends on grid size, so the values are specific to this grid.
- The browser estimate uses at most 100 runs, so its confidence interval is wider than the notebook's 150-run experiments.
- Patchy fuel, slope and fuel moisture are not in the demo; only firebreak width and wind can be changed.
