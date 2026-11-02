# MLB Batting Order: Traditional vs OPS-Ranked Lineups

Math 534 course project. The question is whether half-innings played under a traditional-style batting order and under an OPS-ranked batting order differ in runs scored, using five MLB teams.

## Current status

- Sample: 2025 outcomes, 2024 official player metrics, innings 1-9, complete half-innings before the first batting-roster substitution, and all nine starters with at least 100 PA in 2024 (1,573 half-innings, 211 team-games, five teams).
- Under the scope's exact rules, 0 team-games are Traditional and 1 is OPS-ranked, so the planned two-group t-test cannot be run. The team needs to choose between simulating both orders and loosening the group definitions.

## Repository layout

| Path | Contents |
|---|---|
| `MLB_Lineup_Analysis.Rmd` | Single analysis file |
| `MLB_Lineup_Analysis.html` | Knitted output of the Rmd |
| `*.xlsx` | Original source workbooks (never modified); only the play-by-play workbook is used |
| `processed/` | The half-inning sample and its lineups, written on each Knit |
| `quality/` | Sample flow and the eligibility flags for every 2025 half-inning, written on each Knit |
| `lineup_classification_plan_zh.md` | Classification and study design plan (Chinese) |
| `*.pdf` | Project scope documents |

## Reproducing

Requires R with `readxl`, `dplyr`, `tidyr`, `jsonlite`, `knitr`, and `rmarkdown`. From the repository root:

```r
rmarkdown::render("MLB_Lineup_Analysis.Rmd")
```

`source_cache/` holds cached MLB Stats API responses and is not tracked. The first Knit without it downloads about 750 game feeds and needs network access.
