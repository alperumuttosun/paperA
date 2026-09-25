# Full R Code

## Important — Step 00 takes a long time

**Do not run Step 00.** It rebuilds the full country-year panel from raw
climate and FAO files and takes a long time to complete. The processed
panel files (`panelv7_<crop>.xlsx`, one per crop) are already available —
place them in your working directory and start from Step 01.

## What's in it

The script runs sequentially through these stages:

| Step | What it does | Main output |
|---|---|---|
| Step 00 | Data prep: builds the country-year panel from raw climate/FAO files (**skip — see above**) | `panelv7_<crop>.xlsx` (one per crop) |
| Step 01 | Data description: missingness, descriptives, coverage | `step01_analysis1_data_description.xlsx` |
| Step 02 | Correlation / multicollinearity (VIF) | `step02_analysis2_vif.xlsx` |
| Step 03 | Two-way fixed-effects models M1–M23, all 8 crops | `step03_analysis3_models.xlsx` |
| Step 04 | TMX/PRE response curves, optimal point | `step04_analysis4_response_curves.xlsx` |
| Step 05 | Summary tables + literature comparison | `step05_analysis5_summary_tables.xlsx` |
| Step 06 | Diagnostic tests (Hausman, Pesaran CD, etc.) | `step06_analysis6_diagnostics.xlsx` |
| Step 07 | Robustness: leave-one-out, large-producer exclusion, winsorizing | `step07_analysis7_robustness.xlsx` |
| Step 08 | Driscoll-Kraay standard errors | `step08_analysis8_driscoll_kraay.xlsx` |
| Step 09 | Uniform warming scenario sensitivity (+1 to +4°C) | `step09_analysis9_scenarios.xlsx` |
| Step 10 | Final assembly: consolidates Steps 01–09 into publication tables/figure inventory | assembled Excel package |
| Step 11 / 11b | Structural break test (unknown breakpoint) + bootstrap-corrected significance | included in Step 11 output |

## Before running

1. Place the `panelv7_<crop>.xlsx` files (already shared) in your working
   directory.
2. Open the script and set `base_dir` (appears at the top of Step 00, and
   again at the start of most later steps) to that directory.
3. Install the required packages:
   ```r
   install.packages(c("terra","geodata","dplyr","readxl","writexl",
                      "data.table","tidyr","zoo","rnaturalearth",
                      "rnaturalearthdata","sf","WDI","fixest","ggplot2","plm","jsonlite"))
   ```

## Running it

Skip Step 00 and start at Step 01. The remaining steps run in order, top to
bottom, in a single session — each step reads the Excel file(s) saved by the
step(s) before it.

Console output at each step reports progress, sanity checks (row counts,
missing-data warnings, N cross-checks against earlier steps) and where each
output file was saved.
