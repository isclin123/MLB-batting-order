# MLB Batting Order: Traditional vs OPS-Ranked Lineups

Math 534 course project. With the same nine hitters, does a Traditional batting order (A) or an OPS-ranked order (B) produce more expected runs? Five MLB teams (Dodgers, Cubs, Cardinals, Angels, Giants), 2023-2026 regular seasons.

## Current status

- **Classification (`MLB_Lineup_Analysis.Rmd`):** under the scope's exact rules, 0 of 211 eligible 2025 team-games are Traditional and 1 is OPS-ranked, so the planned two-group t-test of real lineups cannot be run. The team chose simulation instead.
- **Simulation (`MLB_Lineup_Simulation.Rmd`):** the full pipeline, from data collection to results, for all 20 team-seasons. Orders are set from each hitter's previous-season statistics. The two orders give practically the same expected runs: the rules produce identical lineups in 2 team-seasons, and in the other 18, B minus A is −0.006 to +0.020 runs per game (−1 to +3 runs over 162 games). The mean across team-seasons is 0.0015 runs per game (95% CI −0.0014 to 0.0045). Reversing a lineup costs 0.06-0.19 runs per game, so the model does respond to order. The result holds under the PM notebook's choices (same-season statistics, fixed base-running rules).
- **Data cleaning:** the supplied workbooks are replaced by two tables rebuilt from the MLB Stats API (`clean/`). In the supplied files, 11 Dodgers 2025 rows are 2024 statistics, Dodgers 2024 and Giants 2024 have fewer than nine hitters, and every 2026 probability row is all outs.

## Pipeline in `MLB_Lineup_Simulation.Rmd`

1. **Data collection** from the MLB Stats API: play-by-play of every 2023-2026 regular-season game of the five teams (2,978 games), official player and team statistics, and 2021-2022 player totals for the 2023 orders.
2. **Cleaning and checks:** every play is classified into one of eight outcomes; each team-season's play-by-play counts must equal its official totals.
3. **Two clean tables** (`clean/`) that replace the supplied workbooks, in the same column layout, plus a comparison with the supplied files.
4. **Hitters and orders:** each team-season's nine regulars (most team PA with a prior record); orders A and B by the scope's page-2 rules from each hitter's previous-season OBP/SLG/OPS.
5. **Simulation model:** each hitter's outcome rates for that team-season and base-out transitions estimated from that season's play-by-play; a Markov chain gives exact expected runs, and a Monte Carlo simulation checks it.
6. **Results:** B minus A with bootstrap intervals, a t-test across team-seasons, how much order can matter, why A and B are close, and stability checks (same-season statistics, pooled prior seasons, all-nine-by-OPS, and the PM notebook's fixed base-running rules).

## Repository layout

| Path | Contents |
|---|---|
| `MLB_Lineup_Simulation.Rmd` | Full pipeline: data, clean tables, orders, simulation, results |
| `MLB_Lineup_Analysis.Rmd` | Classification of real 2025 lineups under the scope's rules |
| `*.html` | Knitted output of each Rmd |
| `clean/player_stats_2023_2026.csv` | Table 1: official line of each hitter for each team-season (supplied stats workbook layout, plus `batter_id`, HBP, SF, IBB) |
| `clean/player_probabilities_2023_2026.csv` | Table 2: outcome probabilities of each hitter for each team-season (supplied probabilities workbook layout, plus `Strikeout`, the share of `Out` that are strikeouts) |
| `processed/simulation_*.csv` | Nine hitters with orders and rates, transition counts, results, and stability checks |
| `quality/` | Supplied-workbook check (simulation); sample flow and eligibility flags (classification) |
| `source_cache/` | Cached MLB Stats API data, so the simulation Knits offline |
| `*.xlsx` | Original supplied workbooks (never modified); the simulation reads them only to check them |
| `Monte_Carlo_Baseball.ipynb` | The PM's sample notebook (Dodgers 2025); it runs on the two clean tables with `pd.read_csv` |
| `lineup_classification_plan_zh.md` | Classification and study design plan (Chinese) |
| `*.pdf` | Project scope documents |

## Reproducing

Requires R with `readxl`, `dplyr`, `tidyr`, `jsonlite`, `stringi`, `knitr`, and `rmarkdown`. From the repository root:

```r
rmarkdown::render("MLB_Lineup_Simulation.Rmd")
rmarkdown::render("MLB_Lineup_Analysis.Rmd")
```

`source_cache/` holds the cached MLB Stats API responses: each season's parsed plays (`plays_*.rds`) and the schedule, player, and team responses. The raw game feeds behind the parsed plays (about 50 MB) are not tracked. With this cache, a Knit runs offline in about 5 minutes. Without it, the first Knit downloads about 3,000 game feeds one at a time, which takes over an hour and needs network access. Without the two supplied `.xlsx` workbooks, the Knit skips only the comparison with them. RStudio supplies pandoc; from plain R, set `RSTUDIO_PANDOC` to a pandoc folder first.
