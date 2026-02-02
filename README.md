# SIH: Urban Air Quality Forecasting (O3 & NO2)

Urban agencies need reliable 24–48 hour forecasts of ground-level ozone (O3) and nitrogen dioxide (NO2) at the station level to time advisories, traffic pacing, construction windows, and enforcement. 

This solution delivers hourly predictions using a single **LightGBM** model per pollutant with strict time-aware training to prevent leakage, advanced temporal features for peak accuracy, and lightweight outputs that agencies can use immediately.

## 🚀 Key Features

* **Hourly Forecasting:** Generates 24–48h forecasts for O3 and NO2 per site.
* **Operational Outputs:** Exports a one-page PDF, machine-readable CSVs, and lightweight web plots.
* **Diagnostics Included:** RMSE/MAE/R2/RIA metrics, overlay time-series (actual vs. predicted), and scatter visuals.
* **Data Integrity:** Strict chronological splitting (train first, test later) to prevent data leakage.
* **Robust Engineering:** Handles inconsistent satellite/field inputs using lags and rolling windows to keep the system "always-on."

## 📂 Repository Structure

```text
├── train_data/          # Historical data used for training the model
├── test_data/           # Data used for testing and validation
├── NO2_prediction.ipynb # Jupyter notebook for NO2 forecasting pipeline
├── O3_prediction.ipynb  # Jupyter notebook for O3 forecasting pipeline
└── README.md            # Project documentation
```

## 🛠️ Methodology

### How it works
1.  **Chronology Preserved:** Inputs are sorted by datetime. Scaling and transformation are fit only on training data before being applied to test slices.
2.  **Temporal Engineering:**
    * Cyclic diurnal encodings (hour/day/month).
    * Multi-scale lags (1–24 h) and rolling mean/std windows.
    * Synthesized wind speed from u/v vectors.
    * Error-aware signals via `forecast_error` lags where targets exist.
3.  **Modeling:** LightGBM regressor with early stopping on RMSE for fast, accurate tabular forecasting.
4.  **Inference:** Unseen inputs are reindexed to training feature columns and safely filled.

### Why it’s better than typical baselines
* **Peak Timing:** Encoded diurnals and multi-hour memory capture photochemistry-driven morning/evening NO2 spikes and afternoon O3 surges.
* **Honest Metrics:** No shuffling; time-ordered evaluation avoids future peeking.
* **Efficiency:** Site-agnostic pipeline that runs efficiently on standard CPUs.

## 💻 Getting Started

### Prerequisites
* Python 3.8+
* Jupyter Notebook

### Installation
Clone the repository and install the required Python packages.

```bash
git clone [https://github.com/darthvader323/SIH.git](https://github.com/darthvader323/SIH.git)
cd SIH
pip install pandas numpy lightgbm scikit-learn matplotlib seaborn
```

### Usage
1.  **Prepare Data:** Ensure your datasets are placed in the `train_data` and `test_data` directories.
2.  **Run Forecasts:**
    * Open `NO2_prediction.ipynb` to train and forecast Nitrogen Dioxide levels.
    * Open `O3_prediction.ipynb` to train and forecast Ozone levels.
3.  **View Results:** The notebooks will generate metrics (RMSE, MAE) and visualization plots directly in the output cells.

## 🌍 Impact

* **Public Health:** Early warnings reduce exposure for vulnerable groups.
* **Policy Evaluation:** Compare predicted "no-intervention" trajectories vs. observed "with-intervention" data to quantify benefits.
* **Scalability:** The framework is extensible to other pollutants like **PM2.5** or **AQI** by swapping targets.

## 📄 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
