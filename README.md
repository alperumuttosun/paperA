# 
# Climate Sensitivity of Eight Crops — Analysis Code

This repository contains the R analysis pipeline behind the manuscript *"Climate Sensitivity of Eight Crops: A Global Panel Analysis."* It estimates how annual maximum temperature (TMX) is associated with crop yield for eight major crops (wheat, barley, oats, rye, rice, maize, soybean, sorghum) using a global country-level panel, 1961–2024.

All analysis is written in **R**. Each step is a standalone script that reads data, runs an analysis, and saves its results to an Excel file plus a set of figures.

## Requirements

- R (any recent version)
- R packages: `dplyr`, `readxl`, `writexl`, `fixest`, `ggplot2`, `tidyr`, `plm`, `terra`, `jsonlite`

Install the packages with:

```r
install.packages(c("dplyr", "readxl", "writexl", "fixest", "ggplot2", "tidyr", "plm", "terra", "jsonlite"))
```

## How the data is built

**Step00** turns the raw source data into the final, ready-to-analyze panel data. It runs in 7 internal stages (each stage saves its own intermediate file, `panelv1_*` through `panelv7_*`):

| Stage | What it adds | Output |
|---|---|---|
| 1 | Climate data (CRU TS4.09) and crop area (CROPGRIDS), combined into a monthly panel | `panelv1_*` |
| 2 | Aggregated to annual values, joined with FAOSTAT yield, CO2, and GDP data | `panelv2_*` |
| 3 | A few additional countries patched in (Iran, Egypt, Nigeria, Sudan) | `panelv3_*` |
| 4 | FAOSTAT irrigation data added | `panelv4_*` |
| 5 | Cropland area and irrigation ratio added | `panelv5_*` |
| 6 | Aridity Index added (both a year-by-year version and a fixed 1990–2000 baseline version) | `panelv6_*` |
| 7 | GDP per capita (World Bank WDI) and fixed-baseline production/area weights added — **this is the final version used in every later step** | `panelv7_*` |

Step00 requires several large raw input files (climate grids, FAOSTAT data, etc.) that are listed at the top of the script. These raw inputs are included in this repository so Step00 can be run from scratch if needed.

## Data files

This repository also includes the final, ready-to-use panel data for each crop — the `panelv7_*.xlsx` output of Step00:

- `panelv7_wheat.xlsx`
- `panelv7_barley.xlsx`
- `panelv7_oats.xlsx`
- `panelv7_rye.xlsx`
- `panelv7_rice.xlsx`
- `panelv7_maize.xlsx`
- `panelv7_soybean.xlsx`
- `panelv7_sorghum.xlsx`

These files are already fully processed and ready for analysis. **Step01 and Step02 only check and describe this data (missing values, correlations, VIF) — they do not change it.** If you just want to reproduce the main results, you can skip Step00, Step01, and Step02 entirely and run the analysis starting from **Step03**, using these `panelv7_*.xlsx` files directly.

## How to run

Run the scripts **in numerical order**, `step00` through `step11b`. Each step reads the Excel file(s) produced by an earlier step, so the order matters.

The output Excel file from every step is already included in this repository. This means you don't have to start from Step00 — **if you just want to reproduce a later step (e.g. the main models in Step03, or the robustness checks in Step07), you can start directly from that step**, since the Excel files it depends on are already here.

All figures are saved automatically as PDF and 1600dpi PNG, in both a "labeled" version (with titles/axis labels, for checking results) and an "unlabeled" version (for final formatting in the manuscript).

## What each step does

| Step | What it does | Main output file |
|---|---|---|
| Step00 | Builds the full panel dataset from raw climate, crop area, and economic data (see "How the data is built" above) | `panelv7_*.xlsx` (one per crop) |
| Step01 | Checks the data for missing values, coverage, and basic sanity (e.g. plausible temperature ranges) | `step01_analysis1_report.xlsx` |
| Step02 | Checks correlations between variables and tests for multicollinearity (VIF) | `step02_analysis2_correlations.xlsx` |
| Step03 | Estimates the main models — 23 model specifications (M1–M23) for each crop | `step03_analysis3_models.xlsx` |
| Step04 | Estimates temperature/precipitation response curves and tests for a nonlinear (curved) relationship | `step04_analysis4_response_curves.xlsx` |
| Step05 | Compares this study's estimates to previously published estimates in the literature (reported as a short comparison in the text; the comparison figure this step produces is not used in the manuscript) | `step05_analysis5_literature_comparison.xlsx` |
| Step06 | An early, simpler structural-break check (pre/post-1990) — **not used in the manuscript**; superseded by Step11/Step11b | `step06_analysis6_structural_break.xlsx` |
| Step07 | Robustness checks: leave-one-out (excluding each major producer country in turn), excluding the top 3/5 producers together, and winsorizing | `step07_analysis7_robustness.xlsx` |
| Step08 | Compares standard clustered standard errors to Driscoll-Kraay standard errors (robust to cross-country correlation) | `step08_analysis8_driscoll_kraay.xlsx` |
| Step09 | Projects yield changes under uniform warming scenarios (+1°C to +4°C) | `step09_analysis9_scenarios.xlsx` |
| Step09b | Projects yield changes under actual CMIP6/ISIMIP3b climate model scenarios | `step09b_cmip_sensitivity.xlsx` |
| Step11 | Searches for a structural break (a year where the temperature-yield relationship shifts) for each crop | `step11_analysis11_breakpoint_search.xlsx` |
| Step11b | Corrects Step11's results for multiple testing, using a bootstrap procedure | `step11b_bootstrap_corrected_breakpoint.xlsx` |

## A note on the structural break analysis (Step06, Step11, Step11b)

Step06, Step11, and Step11b all relate to the same question — does the temperature-yield relationship change over time? — but they are not equally reliable, and the manuscript reflects that:

- **Step06** uses a fixed 1990 cutoff for every crop and was an early, exploratory check. It is **not used** in the manuscript.
- **Step11** searches for each crop's own best-fitting break year. Because roughly 40 candidate years are tested and the single best one is kept, the raw significance level from this search **overstates how significant the result really is** — this is stated directly in Step11's own code comments.
- **Step11b** corrects for this using a bootstrap procedure. After correction, none of the eight crops show a statistically significant break. This is the result reported in the manuscript, and it is described there only briefly, as an exploratory robustness check with a null result — not as a main finding.

## Questions

For questions about the analysis or the manuscript, please contact the corresponding author.
