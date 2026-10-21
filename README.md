# South Bay Indoor Air Quality Study

**Quantifying indoor air pollution from Tijuana River valley emissions**

Analysis code for the capstone paper examining how outdoor PM₁/PM₂.₅ pollution from the Tijuana River estuary infiltrates indoor environments across South Bay San Diego communities.

---

## Study Overview

- **Study period:** April 3 – May 14, 2026
- **Indoor sensors:** 10 AirGradient sensors (AG-3 through AG-19) at 7 residential and institutional locations
- **Outdoor sensors:** 7 QuantAQ MODULAIR PM monitors across South Bay
- **H₂S tracer:** Teledyne T101 analyzer at Nestor (trailer) and a coastal site
- **Event classification:** "High H₂S + PM₁" days (n = 9) vs. "Clean Reference" days (n = 7)

---

## Repository Structure

```
southbay-indoor-aq/
├── notebooks/
│   └── southbay_indoor_aq_analysis.ipynb   # Main analysis notebook
├── data/
│   ├── raw/          # Raw sensor exports (not tracked by git — see below)
│   └── processed/    # Processed/derived datasets (CSVs produced by notebook)
├── figures/          # Saved figure outputs (PNGs)
└── README.md
```

---

## Data

### Raw data

Raw sensor files are included in `data/raw/`. Update the `BASE` path in **Cell 3 (CONFIG)** of the notebook to point to this folder.

| File | Description |
|------|-------------|
| `export (21).csv` | AirGradient bulk export (all AG sensors) |
| `MOD-00364-a02bf9e1bc9041548768478c40eff5aa.csv` | QuantAQ SIO |
| `MOD-00365-097157fe8eb6491c87812cb84da2e94b.csv` | QuantAQ TR Estuary |
| `MOD-00366-5b0e9b2bc27641b78c4662cc05817f6b.csv` | QuantAQ SY |
| `MOD-00367-1c28104a02aa4539b2f96d90bc6ad6a5.csv` | QuantAQ Coast |
| `MOD-00369-b950720ea2c44a7fa7afde050f5ed26d.csv` | QuantAQ IB City Hall |
| `MOD-00370-6c7754515cd148d0a3bf3eeedba87d5c.csv` | QuantAQ ML (mobile lab co-location) |
| `MOD-00371-6da146653d0740789e0af15319122820 (1).csv` | QuantAQ CABR |
| `H2S - trailer-data-2026-06-23 10_42_56 (2).csv` | H₂S Teledyne T101, Nestor trailer |
| `H2S - coast-data-2026-06-23 10_43_35 (1).csv` | H₂S Teledyne T101, coastal site |
| `BulkExport-11013101-20260623234323 (3).csv` | USGS Tijuana River stage (NWIS) |
| `mlh_solar_extended (2).csv` | Mixing layer height + solar radiation |

### Processed outputs

The notebook writes these CSVs to the working directory (move to `data/processed/`):

| File | Description |
|------|-------------|
| `daily_summary_stats.csv` | Per-day stats by event type |
| `io_ratio_summary.csv` | I/O ratio (PM₂.₅) per sensor |
| `infiltration_factor_summary.csv` | OLS infiltration factors |
| `toxicity_potential_summary.csv` | Toxicity potential (Tₚ) |
| `cumulative_exposure_summary.csv` | Cumulative exposure estimates |
| `table1_final.csv` | Paper Table 1 data |
| `cooking_event_log_grigorev.csv` | Flagged cooking events |

---

## Running the Notebook

The notebook was developed in Google Colab. To run locally:

1. Install dependencies:
   ```bash
   pip install numpy pandas xarray matplotlib seaborn scipy statsmodels windrose
   ```

2. Place raw data files in `data/raw/` (see above).

3. Open `notebooks/southbay_indoor_aq_analysis.ipynb` and update `BASE` in **Cell 3**:
   ```python
   BASE = 'data/raw'   # or absolute path to your data folder
   ```

4. Run cells top to bottom. The notebook is organized as:
   - **Cells 1–8:** Imports, config, data loading, PM₁ calibration correction
   - **Cells 9–12:** Cooking filter (Grigorev et al. 2023 peak-shape criteria)
   - **Cell 13:** Event day classification (H₂S + PM₁ dual criterion)
   - **Cells 14–28:** Paper figures (H₂S scatterplot, time series, heatmaps, boxplots)
   - **Cells 29–37:** Statistical analysis and summary tables
   - **Cells 38–48:** Data exports (AirGradient, QuantAQ, atmospheric parameters CSVs)
   - **Cells 49–57:** Infiltration factor, I/O ratio, and stats tests

> **Note:** The Google Drive mount cell has been removed. If running in Colab, add `from google.colab import drive; drive.mount('/content/drive')` before Cell 3 and adjust `BASE` accordingly.

---

## PM₁ Calibration

AirGradient PM₁ readings are corrected using a single-linear regression (SLR) derived from overnight OPS co-location (Aug 19–20, 2026):

```
PM1_corrected = 0.1910 × PM1_raw + 1.1234
```

See Cell 8 for details.

---

## Event Day Classification

"High H₂S + PM₁" days require all three:
1. ≥ 3 hourly H₂S averages ≥ 30 ppb (CARB 1-hour standard)
2. Daily mean PM₂.₅ ≥ 9 µg/m³
3. ≥ 3 hourly outdoor PM₁ averages above the study-period top quartile

"Clean Reference" days: 7 days with no H₂S spikes and low PM.

---

## Contact

Marina Minic · mminic@ucsd.edu · UCSD Capstone 2026
