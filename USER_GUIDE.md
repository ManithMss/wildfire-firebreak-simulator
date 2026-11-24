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

## Try this

1. Set firebreak width to 0 and wind direction to East, then click "Run Monte Carlo estimate". With no firebreak, the containment probability should be close to 0.
2. Change the width to 4 with the same wind and run the estimate again. The probability should jump up, usually to 1.
3. Click "Headfire" and then "Crosswind". Both use width 1 and wind strength 0.8, but East wind pushes the fire and its embers straight at the firebreak, so it breaks through far more often than North wind, which blows along it. Run the Monte Carlo estimate for each and compare them in the history table.

## Check against the notebook

With the default settings (width 2, wind strength 0.5, East, seed 1, 50 runs) the demo gives **0.360 +/- 0.133**. Because the seed is fixed, this number is the same every time. It is close to the notebook's result for width 2 in Experiment 1, but not identical, because the notebook and the browser use different random number generators and the notebook averages over 150 runs rather than 50.

## Limitations

- The demo uses the same 50x50 grid and p0 = 0.3 as the notebook's experiments, but a different random number generator, so its numbers agree within the confidence intervals rather than exactly. The notebook's resolution check shows the probability depends on grid size, so the values are specific to this grid.
- The browser estimate uses at most 100 runs, so its confidence interval is wider than the notebook's 150-run experiments.
- Patchy fuel, slope and fuel moisture are not in the demo; only firebreak width and wind can be changed.
