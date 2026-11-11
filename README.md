# Leakage-free probabilistic demand forecasting for retail inventory decisions

Reproduction materials for the paper *"Leakage-free probabilistic demand forecasting for retail inventory decisions: evidence from two datasets"* by Cihat Yıldız and Semih Özel (Marmara University), submitted for publication.

The study evaluates a forecasting-to-inventory pipeline for daily retail demand under a leak-free protocol on two public datasets, **UCI Online Retail II** (45 product–country series) and **M5** (60 item series from Walmart store CA_1). It covers:

- a **leakage audit**: same-day transaction, customer and price aggregates disclose the target and inflate apparent accuracy;
- **point forecasts** (LightGBM, XGBoost, random forest, LSTM, GRU, ensemble) against four naive references, with MASE, RMSSE, Diebold–Mariano tests, 18 rolling-origin runs, panel-wide evaluation and direct forecasts for horizons of 1, 7 and 14 days;
- **quantile forecasts** at seven levels with rearrangement and split-conformal correction (CQR), assessed by coverage, the Kupiec test and pinball skill;
- **inventory policies**: base-stock levels set to the cost-implied critical quantile of lead-time demand (lead times 0, 3 and 7 days), compared ex ante with a static safety-stock rule over four cost ratios;
- **intermittent demand** benchmarks (Croston, SBA, TSB, hurdle model), a controlled **censored-demand** experiment with a Kaplan–Meier correction, and **SHAP** explanations.

## Repository contents

| File | Description |
|---|---|
| `Demand_Forecasting_Framework.ipynb` | Complete pipeline with executed outputs, so every number in the paper can be traced to a cell output. A revision log at the top of the notebook lists every change (R1–R14). |
| `online_retail_II.xlsx` | UCI Online Retail II dataset (see *Data*). |
| `make_fig_horizon_leadtime.py` | Draws Figure 3 from the CSV files written by the notebook. |
| `requirements.txt` | Python package versions used for the reported results. |
| `LICENSE` | MIT license for the code. |

## Data

**Online Retail II** — UCI Machine Learning Repository, <https://archive.ics.uci.edu/dataset/502/online+retail+ii> (doi: 10.24432/C5CG6D). 1,067,371 invoice lines of a UK-based online retailer, December 2009 – December 2011. The copy included here is unmodified and redistributed under the license stated on the UCI page (CC BY 4.0). The notebook expects it in the working directory as `online_retail_II.xlsx` with sheets `Year 2009-2010` and `Year 2010-2011`.

**M5** — not included because of its size and the competition's terms of use. Download `sales_train_evaluation.csv`, `calendar.csv` and `sell_prices.csv` from the Kaggle competition page <https://www.kaggle.com/competitions/m5-forecasting-accuracy/data> and place them in the working directory (or set `M5_DIR` in Section 10.10). If the files are missing, the M5 section is skipped and the rest of the notebook runs unchanged.

## How to reproduce

**Google Colab (matches the paper's environment):**
1. Open `Demand_Forecasting_Framework.ipynb` in Colab.
2. Upload `online_retail_II.xlsx` and the three M5 files to the working directory (left panel → Files → Upload), or mount Google Drive and set `M5_DIR`.
3. `Runtime → Run all`. A full run takes about 45–60 minutes; the M5 section alone takes about 15–20 minutes.
4. Optional: `python make_fig_horizon_leadtime.py` in the same directory redraws Figure 3 from the notebook's CSV outputs.

**Local:**
```bash
pip install -r requirements.txt
jupyter notebook Demand_Forecasting_Framework.ipynb
```

The last section starts a small Gradio interface that is not part of the paper. In Colab, Gradio may create a temporary public link; the archived notebook does not contain such a link.

## Reproducibility notes

- All stochastic components are seeded (`random_state=42` and the rolling-origin seeds 42, 7 and 2024; TensorFlow with `set_random_seed(42)` and deterministic ops).
- The test period is used only for final evaluation: early stopping, ensemble weights and conformal corrections use a validation slice inside the training period, and outliers are capped with training data only.
- Built-in consistency checks assert that later sections reproduce earlier results exactly (for example, the one-step and zero-lead-time results of Section 10.7 must equal those of Sections 10.2–10.4).
- Results that depend only on LightGBM and XGBoost are identical across runs; random-forest and ensemble values can differ in the last digit across library versions.
- Figures are written as 300-dpi PDF and PNG files to `figures_300dpi/`; per-series results are written as CSV files.

## Mapping between the paper and the notebook

| Paper element | Notebook section |
|---|---|
| Table 1 (series groups) | Sections 2–3 (Online Retail II), 10.8 (demand classes), 10.10 (M5) |
| Table 2 (leakage ablation) | Section 10.5 |
| Table 3 (point forecasts), Diebold–Mariano tests | Sections 7–7.1 |
| Table 4 (rolling origin and panel) | Sections 10.2–10.3 |
| Table 5 (forecast horizons) | Sections 10.7 and 10.10 |
| Table 6 (calibration), Figure 4 | Sections 10.4 and 10.6 |
| Tables 7–8 (inventory policies, L = 0) | Sections 10.3.1, 10.4.1, 10.7 and 10.10 |
| Table 9 (lead times) | Sections 10.7 and 10.10 |
| Table 10 (intermittent demand) | Sections 10.8 and 10.10 |
| Table 11 (censored demand) | Section 10.9 |
| Figure 2 (forecasts), Figure 5 (SHAP) | Section 10.6.1 |
| Figure 3 (horizon and lead time) | `make_fig_horizon_leadtime.py` (uses CSVs from Sections 10.7 and 10.10) |

Figure 1 is a schematic diagram of the pipeline.

## Citation

If you use this code, please cite the paper and the archived repository: <https://doi.org/10.5281/zenodo.21433735>

## License

Code: MIT (see `LICENSE`). Online Retail II: CC BY 4.0, © the original dataset creators. M5: subject to the Kaggle competition terms.
