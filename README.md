# SIH
Urban agencies need reliable 24–48 hour forecasts of ground-level ozone (O3) and nitrogen dioxide (NO2) at station level to time advisories, traffic pacing, construction windows, and enforcement. The solution delivers hourly predictions using a single LightGBM model per pollutant with strict time-aware training to prevent leakage, advanced temporal features for peak accuracy, and lightweight outputs that agencies can use immediately.

What it does

Generates 24–48 h hourly O3/NO2 forecasts per site and exports a one-page PDF, machine-readable CSVs, and a lightweight web plot for quick operational use.

Provides diagnostics: RMSE/MAE/R2/RIA, overlay time-series (actual vs predicted), and predicted-vs-actual scatter visuals to ensure trust and auditability.

How it works

Chronology preserved: Inputs are sorted by datetime; splitting is strictly chronological (train first, test later), and scaling is fit only on train before transforming later slices.

Temporal engineering: Cyclic diurnal encodings (hour/day/month), multi-scale lags (1–24 h), rolling mean/std windows, synthesized wind speed from u/v, and error-aware signals via forecast_error lags where targets exist.

Modeling: LightGBM regressor with early stopping on RMSE for fast, accurate tabular forecasting; persisted scaler/model for reproducible inference; unseen inputs reindexed to training feature columns and safely filled.

Why it’s better than typical baselines

Peak timing and bias: Encoded diurnals and multi-hour memory capture photochemistry-driven morning/evening NO2 spikes and afternoon O3 surges more reliably.

Honest metrics: No shuffling; time-ordered evaluation avoids future peeking and reflects real deployment.

Robust under gaps/noise: Lags and rolling windows stabilize learning when satellite/field inputs are inconsistent, keeping the system “always-on.”

Operations and scale

Site-agnostic pipeline: Add new stations by supplying CSVs; daily runs on CPUs suffice due to LightGBM’s efficiency.

Versioned artifacts: Saved scaler/model enable traceability, quick rollbacks, and consistent citywide deployment.

Extensible: The same framework can forecast PM2.5 or AQI by swapping targets and reusing the feature builder.

Expected impact

Early warnings reduce exposure for vulnerable groups; hospitals and civic responders can plan for spikes.

Policy evaluation: Compare predicted “no-intervention” trajectories vs observed “with-intervention” to quantify benefits and optimize spend.

Immediate usability: Outputs match current agency workflows, minimizing IT friction.
