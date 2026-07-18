# Explainable Multi-Horizon Demand Forecasting with Probabilistic Inventory Optimization

Reproduction materials for the paper *"Explainable Multi-Horizon Demand Forecasting with Probabilistic Inventory Optimization: A Unified Framework for Online Retail"* (submitted to the Turkish Journal of Electrical Engineering & Computer Sciences).

The framework integrates six components on the UCI Online Retail II dataset: (1) a densified daily data pipeline, (2) multi-model forecasting (LightGBM, XGBoost, Random Forest, LSTM, GRU, weighted Ensemble) with 26 engineered features, (3) LightGBM quantile regression with a seven-level calibration assessment (empirical coverage, Kupiec POF test, pinball loss, skill scores, reliability diagram), (4) SHAP TreeExplainer explainability, (5) Newsvendor inventory optimization with cost-ratio sensitivity analysis, and (6) a Gradio chatbot interface supporting nine query categories.

## Repository contents

| File | Description |
|---|---|
| `Demand_Forecasting_Framework.ipynb` | Complete pipeline: data cleaning, densification, feature engineering, model training, quantile calibration, Diebold–Mariano tests, SHAP, Newsvendor optimization, sensitivity analysis, chatbot. Executed outputs are preserved so every number in the paper can be traced to a cell output. |
| `online_retail_II.xlsx` | UCI Online Retail II dataset (see *Dataset* below for source and license). |
| `requirements.txt` | Python dependencies. |
| `LICENSE` | MIT license for the code in this repository. |

## Dataset

The dataset is **Online Retail II** from the UCI Machine Learning Repository:
<https://archive.ics.uci.edu/dataset/502/online+retail+ii>

It contains 1,067,371 transactions of a UK-based online retailer between December 2009 and December 2011. The copy included here is unmodified and is redistributed under the license stated on the UCI repository page (CC BY 4.0); please credit the UCI Machine Learning Repository and the original dataset donor when reusing it. If you prefer, delete the included file and download it directly from UCI — the notebook expects it in the working directory as `online_retail_II.xlsx` with sheets `Year 2009-2010` and `Year 2010-2011`.

## How to reproduce

**Google Colab (recommended — matches the paper's environment):**
1. Open `Demand_Forecasting_Framework.ipynb` in Colab.
2. Upload `online_retail_II.xlsx` to the Colab working directory (left panel → Files → Upload).
3. `Runtime → Run all`.

**Local:**
```bash
pip install -r requirements.txt
jupyter notebook Demand_Forecasting_Framework.ipynb
```

The full run takes roughly 10–15 minutes on a GPU runtime (deep learning models train fastest on GPU; all tree-based models are CPU-bound). The final cell launches the Gradio chatbot locally; it does not open a public link.

## Reproducibility notes

- All stochastic components are seeded (`random_state=42` for scikit-learn/LightGBM/XGBoost; `tf.keras.utils.set_random_seed(42)` and `tf.config.experimental.enable_op_determinism()` for TensorFlow), so repeated runs produce identical results on the same runtime type.
- The notebook writes all paper figures as 300-dpi PNGs to `figures_300dpi/` and prints the LaTeX source of the calibration and sensitivity tables, so every table and figure in the paper regenerates from a single run.
- Consistency guards are built in: the seven-level calibration cell verifies that the P50/P75/P90 rows reproduce the paper's Table 8 values exactly before proceeding.

## Mapping between the paper and the notebook

| Paper element | Notebook section |
|---|---|
| Table 3 (dataset parameters), §3.2–3.3 | Sections 2–3 (cleaning, densification) |
| Table 4 (26 features), Figure 3 | Section 4 (feature engineering) |
| Figure 2 (EDA) | Section 5 |
| Table 7, Figures 4–5, Diebold–Mariano results | Sections 7–7.1 |
| Table 8, Figure 6, Figure 7 (reliability diagram) | Sections 8–8.1 |
| Figure 8 (SHAP) | Section 9 |
| Table 9, Figure 9, sensitivity table | Sections 10–10.1 |
| Figures 10–14 (chatbot) | Section 11 |

## Citation

If you use this code or build on the framework, please cite the paper. Archived version of this repository: <hhttps://doi.org/10.5281/zenodo.21433735> *(DOI to be minted upon Zenodo publication; see release notes).*

## License

Code: MIT (see `LICENSE`). Dataset: per the UCI repository page (CC BY 4.0), © original dataset creators.
