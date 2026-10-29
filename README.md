# MLB Batting Order: Traditional vs OPS-Ranked Lineups

Math 534 course project. The question is whether half-innings played under a traditional-style batting order and under an OPS-ranked batting order differ in runs scored, using five MLB teams.

## Current status

- Data preparation for the confirmed primary sample is complete: 2025 outcomes, 2024 official player metrics, innings 1-9, complete half-innings before the first batting-roster substitution, and all nine starters with at least 100 PA in 2024 (1,573 half-innings, 211 team-games, five teams).
- Formal lineup classification has not started. A feasibility check shows that strict matching to the scope's rules leaves the Traditional group empty, so an approximate matching rule is still to be decided.

## Repository layout

| Path | Contents |
|---|---|
| `MLB_Lineup_Analysis.Rmd` | Single analysis entry point (preprocessing, checks, and all later analysis) |
| `MLB_Lineup_Analysis.html` | Knitted output of the Rmd |
| `*.xlsx` | Original source workbooks (never modified) |
| `processed/` | Derived datasets written on each Knit |
| `quality/` | Data-quality and coverage checks written on each Knit |
| `lineup_classification_plan_zh.md` | Classification and study design plan (Chinese) |
| `*.pdf` | Project scope documents |

## Reproducing

Requires R with `readxl`, `dplyr`, `tidyr`, `stringi`, `jsonlite`, `knitr`, and `rmarkdown`. From the repository root:

```r
rmarkdown::render("MLB_Lineup_Analysis.Rmd")
```

Two things are not tracked and are rebuilt on Knit:

- `source_cache/`: cached MLB Stats API responses. The first Knit without it downloads about 3,000 game feeds and needs network access.
- `processed/play_by_play_clean.csv`: larger than GitHub's 100 MB file limit.
