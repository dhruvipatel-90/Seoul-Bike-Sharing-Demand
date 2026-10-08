# Seoul Bike Sharing Demand Prediction

Predicting the **hourly number of rented bikes** in Seoul's bike-sharing system from weather and calendar information, using regression models in Python.

---

## 1. Problem Statement

Many metropolitan cities now provide bike rental services to enhance urban mobility and convenience. Ensuring that rental bikes are available when needed is essential for minimizing wait times, making a steady supply of bikes a key priority. In this context, the predicted hourly demand plays a vital role.

Bike-sharing systems streamline membership, rentals and returns through a network of automated stations. Users can pick up a bike at one location and return it to the same or a different one.

This project forecasts demand for Seoul's Bike Sharing Program by analyzing historical usage together with weather, time and holiday information.

---

## 2. Hypothesis Generation

**Weather**
- Does pleasant (warmer) temperature increase bike rentals?
- Are rentals lower when humidity is very high?
- Are rentals lower during rainfall or snowfall?
- Do windy conditions discourage rentals?
- Does low visibility reduce rentals?
- Do sunny days (higher solar radiation) increase rentals?

**Time**
- Are rentals higher during morning and evening commute hours?
- Do weekends differ from weekdays?
- Are rentals lower on holidays?
- Do rentals vary across seasons?
- Do late-night and daytime rentals behave differently?

---

## 3. Dataset

Hourly rental counts for one year (`SeoulBikeData.csv`) with matching weather and holiday data.

- **Rows / columns:** 8,760 / 14
- **Missing values:** none
- **Duplicate rows:** none
- **Note:** the file is read with `encoding="latin"` because of the `°C` symbols in the column names.

| Column | Description |
|---|---|
| Date | day/month/year |
| Rented Bike Count | Bikes rented in that hour (**target**) |
| Hour | Hour of day (0-23) |
| Temperature(°C) | Temperature in Celsius |
| Humidity(%) | Relative humidity |
| Wind speed (m/s) | Wind speed |
| Visibility (10m) | Visibility, in units of 10 m |
| Dew point temperature(°C) | Dew point in Celsius |
| Solar Radiation (MJ/m2) | Solar radiation |
| Rainfall(mm) | Rainfall |
| Snowfall (cm) | Snowfall |
| Seasons | Winter, Spring, Summer, Autumn |
| Holiday | Holiday / No Holiday |
| Functioning Day | Yes / No (whether the system was operating) |

---

## 4. Project Workflow

### 4.1 Data cleaning and pre-processing
- Renamed columns to simple snake_case names.
- Parsed `Date` (day-first) and split it into `day`, `month`, `year` and `weekday`; the original column was dropped.
- Created an `hour` -> `session` grouping (Early Morning, Morning, Afternoon, Evening, Night, Late Night) for EDA only.

### 4.2 Exploratory data analysis
Each variable was explored with a distribution plot, a relationship plot against `rented_bike_count`, and a multi-variate view (for example demand by hour split by holiday status, or by temperature on weekends vs weekdays). Variables covered: rented bike count, hour, temperature, humidity, wind speed, visibility, dew point, solar radiation, rainfall, snowfall, seasons, holiday and functioning day.

### 4.3 Feature engineering
- **Outliers:** IQR clipping (1.5 x IQR) on the continuous weather features. The target, `rainfall` and `snowfall` were left untouched to avoid losing real information.
- **Multi-collinearity:** `dew_point_temperature` dropped (highly correlated with temperature, threshold 0.7); `year` dropped after a VIF check; `weekday` and `session` dropped as EDA-only helpers.
- **Encoding:** one-hot encoding for `seasons`; binary mapping for `holiday` and `functioning_day`.
- **Target transform:** square root of `rented_bike_count` (compared against log1p and cube root) to reduce skew. Predictions can be squared to return to the original scale.

Final feature set (16): `hour, temperature, humidity, wind_speed, visibility, solar_radiation, rainfall, snowfall, holiday, functioning_day, day, month` and four `seasons_*` dummies.

### 4.4 Modelling
- 80/20 train-test split (`random_state=33`): 7,008 training rows, 1,752 test rows.
- `StandardScaler` fitted on the training set.
- Models: Linear Regression, Lasso, K-Nearest Neighbors, Support Vector Regression, Decision Tree, XGBoost (tuned with `GridSearchCV`).
- Metrics: MSE, RMSE, MAE, R² and Adjusted R² (on the square-root scale).

---

## 5. Results

Test-set performance, sorted by R²:

| Model | Test RMSE | Test MAE | Test R² | Train R² |
|---|---|---|---|---|
| XGBoost* | 3.486 | 2.338 | 0.919 | 1.000 |
| Decision Tree | 4.560 | 3.180 | 0.862 | 0.902 |
| SVR (RBF) | 5.076 | 3.313 | 0.829 | 0.867 |
| KNN (k=3) | 5.072 | 3.536 | 0.829 | 0.918 |
| Linear Regression | 7.279 | 5.600 | 0.649 | 0.654 |
| Lasso | 7.298 | 5.619 | 0.647 | 0.653 |

\*See [Limitations](#6-limitations-and-future-work).

**Top XGBoost feature importances:** `functioning_day` (0.48), `seasons_Winter` (0.38), `rainfall` (0.07), followed by `hour` and `temperature` (about 0.015 each).

---

## 6. Limitations and Future Work

- The XGBoost hyper-parameter search should be run on the training split only, so that the test set stays unseen. Re-run the search and update the table above.
- XGBoost's train R² of 1.0 against a test R² of 0.92 points to over-fitting; stronger regularization (lower `max_depth`, `learning_rate`, `subsample`) should help.
- Try Random Forest, LightGBM and Gradient Boosting, and evaluate with time-aware cross-validation.
- `functioning_day` is a very strong signal because no bikes are rented when the system is closed; consider modelling only functioning hours.

---

## 7. How to Run

```bash
git clone https://github.com/dhruvipatel-90/Seoul-Bike-Sharing-Demand.git
cd Seoul-Bike-Sharing-Demand
pip install -r requirements.txt
jupyter notebook
```

Place `SeoulBikeData.csv` in the project root, open the notebook and run all cells.

## 8. Tech Stack

Python, pandas, NumPy, Matplotlib, Seaborn, missingno, scikit-learn, XGBoost, statsmodels.
