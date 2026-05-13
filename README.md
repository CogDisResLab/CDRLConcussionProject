# TriNetX Neuropsychiatric Outcomes Analysis Pipeline

R pipeline for parsing TriNetX cohort exports, generating publication-ready figures, computing prospective statistical power, and exporting consolidated results tables for manuscript preparation.

---

# Overview

This repository contains:

* Raw TriNetX export files
* An R analysis pipeline
* Publication-ready figures
* Consolidated results tables
* Statistical power analyses

The workflow parses TriNetX MOA and Kaplan–Meier exports across three propensity-score-matched concussion cohorts and generates manuscript figures and supplementary analytic outputs automatically.

---

# Repository Structure

```text
.
├── data/
│   ├── Concuss_1x_5_year_V12_424528/
│   ├── Concuss_2x_5_year_V12_(1)_(1)_677193/
│   └── Concuss_3x_5_year_V12_327389/
│
├── R/
│   └── make_figures.R
│
├── results/
│   ├── figure2_forest.*
│   ├── figure3_power.*
│   ├── power_table.csv
│   ├── results.csv
│   ├── figure_captions.txt
│   └── session_info.txt
│
└── README.md
```

---

# Requirements

## R Version

* R 4.3 or newer recommended

## Required Packages

The script automatically loads:

```r
ggplot2
dplyr
readr
stringr
purrr
scales
grid
gtable
patchwork
```

Install missing packages with:

```r
install.packages(c(
  "ggplot2",
  "dplyr",
  "readr",
  "stringr",
  "purrr",
  "scales",
  "gtable",
  "patchwork"
))
```

---

# Running the Pipeline

From the repository root directory, run:

```bash
Rscript R/make_figures.R
```

The script will automatically:

* Parse all TriNetX outcome tables
* Build consolidated results datasets
* Apply BH-FDR correction
* Compute prospective statistical power
* Generate publication-ready figures
* Export all outputs to `./results`

No additional configuration is required if the repository structure is preserved.

---

# Input Data

The pipeline expects three comparison directories inside `./data`:

| Comparison                             | Description                                       |
| -------------------------------------- | ------------------------------------------------- |
| `Concuss_1x_5_year_V12_424528`         | Single concussion vs orthopedic fracture controls |
| `Concuss_2x_5_year_V12_(1)_(1)_677193` | Two concussions vs one concussion                 |
| `Concuss_3x_5_year_V12_327389`         | Three concussions vs two concussions              |

Each directory must contain TriNetX exports including:

* MOA tables
* KM tables
* associated graph CSVs
* baseline characteristic files

The analysis script specifically parses:

```text
Outcome_*_Result_a_MOA_table.csv
Outcome_*_Result_b_KM_table.csv
```

for outcomes 1–8.

---

# Outcomes Included

## Internalizing / Neurodevelopmental

* Mood disorders
* Anxiety disorders
* ADHD
* Unspecified psychosis

## Substance Use

* Generalized substance use disorder
* Alcohol-related disorders
* Cannabis-related disorders
* Opioid-related disorders

---

# Statistical Methods

## Relative Risk Analysis

The script extracts:

* Relative risks (RR)
* Hazard ratios (HR)
* 95% confidence intervals
* Log-rank p-values

from TriNetX exports.

## Multiple Testing Correction

Benjamini–Hochberg false discovery rate correction is applied across all 24 hypothesis tests.

## Prospective Power Analysis

Prospective statistical power is computed using:

* two-sided α = 0.05
* minimum clinically important RR = 1.20
* normal approximation to the two-proportion z-test

using matched cohort sample sizes and observed reference-group event rates.

---

# Output Files

All outputs are written to:

```text
./results/
```

## Main Tables

| File              | Description                    |
| ----------------- | ------------------------------ |
| `results.csv`     | Consolidated analytic dataset  |
| `power_table.csv` | Prospective power calculations |

## Figures

### Figure 2 — Forest Plot

Exports:

```text
figure2_forest.svg
figure2_forest.pdf
figure2_forest.png
figure2_forest.tiff
```

Displays:

* Relative risks
* 95% confidence intervals
* BH-FDR significance indicators

### Figure 3 — Prospective Power Analysis

Exports:

```text
figure3_power.svg
figure3_power.pdf
figure3_power.png
figure3_power.tiff
```

Displays:

* Statistical power across outcomes and comparisons
* Conventional 80% adequacy threshold

## Additional Outputs

| File                  | Description                      |
| --------------------- | -------------------------------- |
| `figure_captions.txt` | Manuscript-ready figure captions |
| `session_info.txt`    | Full R session metadata          |

---

# Console Output

The script prints:

* Figure captions
* Comparisons with power < 0.80
* Comparisons with power < 0.50

Example:

```text
Cells with power < 0.80:
- CX3_vs_CX2 | Opioid-related | power=0.412
```

---

# Figure Specifications

Figures are exported at:

* Width: 260 mm
* Height: 120 mm
* Resolution: 300 DPI

Formats include:

* SVG
* PDF
* PNG
* TIFF (LZW compression)

---

# Reproducibility

The analysis includes:

* deterministic random seed (`set.seed(20260421)`)
* session metadata export
* standardized plotting themes
* fully scripted data processing

---

# Notes

* The script assumes consistent TriNetX export formatting.
* Missing or malformed files will stop execution with informative errors.
* Helvetica is used as the default figure font family.

---

# License

Add your preferred license here.

Example:

```text
MIT License
```

---

# Citation

If used in academic work, please cite the associated manuscript or repository.

