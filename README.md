# NFL 4th-Down Expected Value & Decision Engine

A sports analytics model that computes optimal 4th-down decisions (Go For It, Field Goal, Punt) using Expected Points (EP) modeling, field goal regressions, and drive-continuation bonuses.

## Mathematical Formulation

The engine treats 4th-down decision-making as a discrete Expected Value ($EV$) optimization problem across transition states:

$$EV(\text{Decision}) = \sum_{s' \in S} P(s' \mid s, \text{Decision}) \times EP(s')$$

### 1. Expected Points Curve
The value of a possession is parameterized as a non-linear convex polynomial of line of scrimmage $y \in [1, 99]$ (distance from own goal line):

$$EP(y) = -1.35 + 0.048y + 0.00025y^2$$

### 2. Field Goal Probability
Field goal success probability is modeled via a logistic regression based on line of scrimmage plus 17 yards ($d = 100 - y + 17$):

$$P(\text{FG} \mid d) = \frac{1}{1 + e^{0.14(d - 59.5)}}$$

### 3. Conversion Likelihood & Drive Continuation
- **4th-Down Conversion Probability:** Exponential decay over yards-to-gain ($k$): $P(\text{Conversion} \mid k) = 0.72 \times e^{-0.115k}$, bounded by $[0.08, 0.75]$.
- **First-Down Reset Bonus:** Incorporates drive continuation bonuses (+0.65 EP inside opponent territory, +0.25 EP elsewhere) to capture the increased probability of an eventual touchdown over an immediate field goal.

## Features
- Calculates net Expected Points Added (EPA) across all 99 field coordinates.
- Generates an optimal decision map (Go For It vs. Field Goal vs. Punt) across yardlines and distance-to-gain.
- Plots the analytical decision frontier into `decision_frontier.png`.

## Quickstart

```bash
git clone [https://github.com/kalpost/nfl-fourth-down-engine.git](https://github.com/kalpost/nfl-fourth-down-engine.git)
cd nfl-fourth-down-engine
pip install -r requirements.txt
python fourth_down_engine.py
