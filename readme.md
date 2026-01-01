# Electricity Consumption Time-Series Forecasting (XGBoost)

## 1. Overview
This project builds a **time-series regression model** to predict electricity consumption from historical sensor data.  
The workflow includes data cleaning, time-based feature engineering, outlier removal, exploratory analysis, and model training using **XGBoost** with time-aware validation.

---

## 2. Data Loading and Merging
Three CSV files containing electricity consumption data are loaded and merged into a single dataset.

Steps:
- Load CSV files
- Standardize index
- Concatenate datasets
- Combine date and time into a single `datetime` column
- Sort data chronologically

This ensures a clean, continuous time series suitable for forecasting.

---

## 3. Data Cleaning and Validation
- Duplicate rows are removed
- Chronological order is verified
- Datetime is set as the DataFrame index

These steps prevent data leakage and ordering issues during modeling.

---

## 4. Time-Based Feature Engineering
Time-related features are extracted from the datetime column:
- Hour, day, month, and day of week
- Cyclical encodings using sine and cosine (hour and day of week)
- Time-of-day categories (Night, Morning, Afternoon, Evening)

Cyclical encoding preserves the circular nature of time (e.g., 23:00 → 00:00).

---

## 5. Outlier Removal (IQR Method)
Outliers in the target variable (`Consumption`) are removed using the Interquartile Range (IQR) method.

Steps:
- Compute Q1 and Q3
- Define lower and upper bounds
- Remove values outside the bounds

This improves model stability and reduces the influence of abnormal spikes.

---

## 6. Exploratory Data Analysis (EDA)
Several plots are used to understand consumption behavior:
- Consumption over time
- Average consumption by day of week
- Average consumption by time of day
- Weekly average consumption

These visualizations help identify seasonal and temporal patterns.

---

## 7. Time-Series Feature Creation
Lagged and rolling features are created to capture temporal dependencies:
- Lag features: 1 hour, 6 hours, 24 hours
- Rolling mean features based on past values only

Rows without sufficient history are dropped to maintain feature consistency.

---

## 8. Resampling and Train-Test Split
- Data is resampled to **hourly frequency**
- Dataset is split chronologically:
  - 80% training data
  - 20% test data

Lag and rolling features are created **after splitting** to prevent future data leakage.

---

## 9. Time-Series Cross-Validation
`TimeSeriesSplit` is used during hyperparameter tuning:
- Preserves temporal order
- Uses expanding training windows
- Validates on future data only

This provides a realistic estimate of model performance.

---

## 10. Model and Hyperparameter Tuning
An `XGBRegressor` model is trained and optimized using `GridSearchCV`.

Tuned parameters include:
- Number of trees
- Tree depth
- Learning rate
- Subsampling ratios

The optimization metric is **Root Mean Squared Error (RMSE)**.

---

## 11. Model Evaluation
Two evaluations are performed:
- **Cross-validation RMSE**: average performance across time splits
- **Test RMSE**: performance on unseen future data

These values may differ due to temporal variability and distribution shifts.

---

## 12. Final Model and Interpretation
A final XGBoost model is trained using selected hyperparameters.

Additional analysis:
- Actual vs predicted consumption plot
- Feature importance analysis using gain

This helps identify which time-based features contribute most to predictions.

---

## 13. Key Takeaways
- Time-aware validation is critical for forecasting tasks
- Feature engineering often matters more than model complexity
- Grid search improves robustness, not guaranteed test performance
- Lag and rolling features must be handled carefully to avoid leakage
