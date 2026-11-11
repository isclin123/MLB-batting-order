# MLB Batting Order: Traditional vs OPS-Ranked Lineups

Math 534 course project. The question is whether half-innings played under a traditional-style batting order and under an OPS-ranked batting order differ in runs scored, using five MLB teams.

## Current status

- **Classification (`MLB_Lineup_Analysis.Rmd`):** under the scope's exact rules, 0 of 211 eligible 2025 team-games are Traditional and 1 is OPS-ranked, so the planned two-group t-test cannot be run. The team chose simulation instead.
- **Simulation (`MLB_Lineup_Simulation.Rmd`):** each team's nine 2025 regulars, orders A and B built from 2023-2024 OBP/SLG/OPS, simulated with 2025 outcome rates and 2025 base-out transitions. The two orders give practically the same expected runs. They are identical for the Cubs and Giants, and differ by at most 0.005 runs per game for the other three teams, with every 95% interval including zero.
- **Next:** validate on 2026, which is complete (162 final games per team in the MLB Stats API and the play-by-play workbook).

## Repository layout

| Path | Contents |
|---|---|
| `MLB_Lineup_Analysis.Rmd` | Classification of real 2025 lineups under the scope's rules |
| `MLB_Lineup_Simulation.Rmd` | Simulation of orders A and B for each team's nine 2025 regulars |
| `*.html` | Knitted output of each Rmd |
| `*.xlsx` | Original source workbooks (never modified); only the play-by-play workbook is used, by the classification file |
| `processed/` | Outputs written on each Knit: the half-inning sample and its lineups (classification); the nine hitters with both orders, the transition counts, and the results (`simulation_*`) |
| `quality/` | Sample flow and the eligibility flags for every 2025 half-inning, written on each Knit |
| `lineup_classification_plan_zh.md` | Classification and study design plan (Chinese) |
| `*.pdf` | Project scope documents |

## Reproducing

Requires R with `readxl`, `dplyr`, `tidyr`, `jsonlite`, `knitr`, and `rmarkdown`. From the repository root:

```r
rmarkdown::render("MLB_Lineup_Analysis.Rmd")
rmarkdown::render("MLB_Lineup_Simulation.Rmd")
```

`source_cache/` holds cached MLB Stats API responses and is not tracked. Without it, each file's first Knit downloads about 750 game feeds and needs network access. The simulation Knit takes about a minute once the cache exists.
