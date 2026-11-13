# Leakage-free probabilistic demand forecasting with conformal calibration for inventory decisions in online retail

Reproduction materials for the paper *"Leakage-free probabilistic demand forecasting with conformal calibration for inventory decisions in online retail"* by Cihat Yıldız and Semih Özel (Marmara University), submitted to *PeerJ Computer Science*.

The study rebuilds a forecasting-to-inventory pipeline on the **UCI Online Retail II** dataset (45 product–country series, densified over a 739-day calendar) under a leak-free protocol. It covers:

- a **leakage audit**: same-day transaction, customer and price aggregates disclose the target and inflate apparent accuracy;
- **point forecasts** (LightGBM, XGBoost, random forest, LSTM, GRU, weighted ensemble) against four naive references, with MASE, Diebold–Mariano tests, 18 rolling-origin runs and panel-wide evaluation;
- **quantile forecasts** at seven levels with rearrangement and split-conformal correction (CQR), assessed by coverage, the Kupiec test and pinball skill;
- **inventory policies**: the cost-implied critical quantile used as a daily order-up-to level, compared ex ante with a static safety-stock rule over four cost ratios;
- **SHAP** explanations and a keyword-based **chat interface** with an evaluation of its intent routing.

## Repository contents

| File | Description |
|---|---|
| `Demand_Forecasting_Framework.ipynb` | Complete pipeline with executed outputs, so every number in the paper can be traced to a cell output. A revision log at the top of the notebook lists every change. |
| `online_retail_II.xlsx` | UCI Online Retail II dataset (see *Data*). |
| `requirements.txt` | Python package versions used for the reported results. |
| `make_fig_horizon_leadtime.py` | Optional: draws a figure for the additional analyses in Sections 10.7 and 10.10 (not part of the paper). |
| `LICENSE` | MIT license for the code. |

## Data

**Online Retail II** — UCI Machine Learning Repository, <https://archive.ics.uci.edu/dataset/502/online+retail+ii> (doi: [10.24432/C5CG6D](https://doi.org/10.24432/C5CG6D)). 1,067,371 invoice lines of a UK-based online retailer, December 2009 – December 2011. The copy included here is unmodified and redistributed under the license stated on the UCI page (CC BY 4.0). The notebook expects it in the working directory as `online_retail_II.xlsx` with sheets `Year 2009-2010` and `Year 2010-2011`.

## How to reproduce

**Google Colab (matches the paper's environment, CPU runtime):**
1. Open `Demand_Forecasting_Framework.ipynb` in Colab.
2. Upload `online_retail_II.xlsx` to the working directory (left panel → Files → Upload).
3. `Runtime → Run all`.

**Local:**
```bash
pip install -r requirements.txt
jupyter notebook Demand_Forecasting_Framework.ipynb
```

Section 11 starts a small Gradio interface. In Colab, Gradio may create a temporary public link; the archived notebook does not contain such a link.

## Mapping between the paper and the notebook

| Paper element | Notebook section |
|---|---|
| Data cleaning and densification (Section 3.1) | Sections 2–3 |
| Features and leakage control (Section 3.2) | Section 4 |
| Table 1 (same-day leakage ablation) | Section 10.5 |
| Table 2 (point forecasts), Diebold–Mariano tests | Sections 7 and 7.1 |
| Table 3 (rolling origin and panel) | Sections 10.2 and 10.3 |
| Table 4 (calibration at seven levels) | Section 10.4 |
| Table 5 (ex-ante inventory policies, demonstration series) | Section 10.4.1 |
| Table 6 (inventory savings over rolling origins and series) | Sections 10.2 and 10.3.1 |
| Figure 1 | Schematic diagram of the pipeline |
| Figure 2 (forecasts), Figure 4 (SHAP) | Section 10.6.1 |
| Figure 3 (calibration) | Section 10.6 |
| Chat interface evaluation (Section 4.7) | Sections 11 and 11.1 |

**Additional analyses not reported in the paper.** Sections 10.7–10.10 contain extensions carried out after the paper's analyses (multi-step horizons and lead-time base-stock policies, intermittent-demand benchmarks, a censored-demand simulation and the M5 dataset). They do not change any result of the paper: Sections 10.7 and 10.8 assert that their one-step, zero-lead-time and τ*-quantile cases reproduce the earlier results exactly. Section 10.10 needs the M5 files (`sales_train_evaluation.csv`, `calendar.csv`, `sell_prices.csv` from <https://www.kaggle.com/competitions/m5-forecasting-accuracy/data>); if they are missing, it is skipped and the rest of the notebook runs unchanged.

## Reproducibility notes

- All stochastic components are seeded (`random_state=42`; rolling-origin seeds 42, 7 and 2024; TensorFlow with `set_random_seed(42)`).
- The test period is used only for final evaluation: early stopping, ensemble weights and conformal corrections use a validation slice inside the training period, and outliers are capped with training data only.
- Results that depend only on LightGBM and XGBoost are identical across runs; random-forest, recurrent-network and ensemble values can differ in the last digit across library versions.
- Figures are written as 300-dpi PDF and PNG files to `figures_300dpi/`.

## Citation

If you use this code, please cite the paper and the archived repository: <https://doi.org/10.5281/zenodo.21433735>

## License

Code: MIT (see `LICENSE`). Online Retail II: CC BY 4.0, © the original dataset creators. M5 (Section 10.10 only): subject to the Kaggle competition terms.
