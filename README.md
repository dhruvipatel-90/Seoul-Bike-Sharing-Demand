# Seoul-Bike-Sharing-Demand

Predicting the hourly demand for Seoul's public bike-sharing system from weather and time information, using classical machine learning regression models.

## Table of Contents
1. [Problem Statement](#1-problem-statement)
2. [Hypothesis Generation](#2-hypothesis-generation-for-the-problem-statement)
3. [Dataset Description](#3-dataset-description)
4. [Understanding Variables](#4-understanding-variables)
5. [Project Workflow](#5-project-workflow)
6. [Models Used](#6-models-used)
7. [Results](#7-results)
8. [Tech Stack](#8-tech-stack)
9. [How to Run](#9-how-to-run)
10. [Repository Structure](#10-repository-structure)

---

## 1. Problem Statement

Many metropolitan cities now provide bike rental services to enhance urban mobility and convenience. Ensuring that rental bikes are available when needed is essential for minimizing wait times, making the steady supply of bikes a key priority. In this context, the predicted hourly demand plays a vital role.

Bike-sharing systems streamline the process of membership, rentals, and returns through a network of automated stations. Users can pick up a bike from one location and return it either to the same spot or a different one. Rentals are facilitated through membership or on-demand access, all managed by a citywide automated system.

This project forecasts the demand for Seoul's Bike Sharing Program by analyzing historical usage trends alongside factors such as temperature, time, and other variables.

---

## 2. Hypothesis Generation for the Problem Statement

### On the basis of Weather Conditions
- Does temperature have a positive impact on bike rentals (higher demand in pleasant weather)?
- Are bike rentals lower when humidity is very high?
- Are rentals less frequent during rainfall or snowfall?
- Do windy conditions discourage people from renting bikes?
- Is there a relationship between visibility and bike demand (low visibility → fewer rentals)?
- Do solar radiation levels (sunny vs cloudy days) influence rentals?

### On the basis of Time (Temporal Factors)
- Are rentals higher during morning and evening peak hours (commute times)?
- Do people rent more bikes on weekends compared to weekdays?
- Are rentals lower on holidays compared to working days?
- Do rentals vary significantly across seasons (e.g., highest in summer, lowest in winter)?
- Is there a difference in late night vs daytime rental behavior?

---

## 3. Dataset Description

The dataset records the hourly count of public bicycles rented in the Seoul Bike Sharing System, along with corresponding weather conditions and holiday information. It includes weather attributes such as temperature, humidity, wind speed, visibility, dew point, solar radiation, snowfall and rainfall, along with rental counts per hour and date-related information.

- **Rows:** 8,760 (hourly records)
- **Columns:** 14
- **Duplicate rows:** 0
- **Source file:** `SeoulBikeData.csv` (read with `encoding="latin"` because of the `°C` symbol)

---

## 4. Understanding Variables

| Column | Description |
|---|---|
| Date | day/month/year |
| Rented Bike Count | Count of bikes rented at each hour (**target**) |
| Hour | Hour of the day (0–23) |
| Temperature (°C) | Temperature in Celsius |
| Humidity (%) | Relative humidity |
| Wind speed (m/s) | Wind speed |
| Visibility (10m) | Visibility in units of 10 m |
| Dew point temperature (°C) | Dew point in Celsius |
| Solar Radiation (MJ/m2) | Solar radiation |
| Rainfall (mm) | Rainfall |
| Snowfall (cm) | Snowfall |
| Seasons | Winter, Spring, Summer, Autumn |
| Holiday | Holiday / No Holiday |
| Functioning Day | Yes (functional hours) / No (non-functional hours) |

---

## 5. Project Workflow

1. **Data loading & inspection** – shape, info, duplicates, missing values (visualised with `missingno`), unique values per column.
2. **Cleaning & pre-processing**
   - Renamed columns to snake_case.
   - Parsed `Date` (day-first format) and split it into `day`, `month`, `year` and `weekday`.
   - Created a `session` column (Early Morning, Morning, Afternoon, Evening, Night, Late Night) from `hour`, for EDA only.
3. **Exploratory Data Analysis** – univariate, bi-variate and multi-variate analysis of every column against rented bike count (distribution plots, point plots, bar plots, scatter plots, line plots).
4. **Outlier treatment** – skewed continuous features were clipped to the IQR bounds (1.5 × IQR). The target, `rainfall` and `snowfall` were intentionally left untouched.
5. **Feature engineering & selection**
   - Correlation heatmap; `dew_point_temperature` dropped (highly correlated with temperature).
   - Variance Inflation Factor (VIF) check; `year` dropped due to multicollinearity.
   - `weekday` and `session` dropped (EDA-only columns).
6. **Encoding**
   - `seasons` → one-hot encoding.
   - `holiday` and `functioning_day` → binary (0/1).
7. **Target transformation** – compared log, square-root and cube-root transforms; the **square-root** transform was applied to `rented_bike_count`.
8. **Train/test split** – 80/20 split (7,008 train rows, 1,752 test rows, 16 features), followed by `StandardScaler`.
9. **Model training & evaluation** – a reusable `predict()` function fits each model and reports MSE, RMSE, MAE, R² and Adjusted R² on train and test sets, with actual-vs-predicted plots.
10. **Hyperparameter tuning** – `GridSearchCV` (5-fold, R² scoring) on XGBoost, followed by a feature-importance analysis.

---

## 6. Models Used

- Linear Regression
- Lasso Regression
- K-Nearest Neighbors (k = 3)
- Support Vector Regressor (RBF kernel, C = 100)
- Decision Tree Regressor
- XGBoost Regressor (tuned with GridSearchCV)

---

## 7. Results

Metrics are calculated on the square-root-transformed target.

| Model | Test RMSE | Test MAE | Test R² |
|---|---|---|---|
| XGBoost (tuned) | 3.486 | 2.338 | 0.919 |
| Decision Tree | 4.560 | 3.180 | 0.862 |
| SVM (RBF) | 5.076 | 3.313 | 0.829 |
| KNN | 5.072 | 3.536 | 0.829 |
| Linear Regression | 7.279 | 5.600 | 0.649 |
| Lasso | 7.298 | 5.619 | 0.647 |

Tree-based models clearly outperform the linear models, which suggests the relationship between weather/time features and demand is strongly non-linear.

---

## 8. Tech Stack

Python · Pandas · NumPy · Matplotlib · Seaborn · missingno · Statsmodels · Scikit-learn · XGBoost · LightGBM · Jupyter Notebook

---

## 9. How to Run

```bash
# 1. Clone the repository
git clone https://github.com/dhruvipatel-90/Seoul-Bike-Sharing-Demand.git
cd Seoul-Bike-Sharing-Demand

# 2. Install dependencies
pip install pandas numpy matplotlib seaborn missingno statsmodels scikit-learn xgboost lightgbm jupyter

# 3. Launch the notebook
jupyter notebook Seoul_Bike_Sharing_Demand_Prediction.ipynb
```

Make sure `SeoulBikeData.csv` is in the same folder as the notebook.

---

## 10. Repository Structure

```
Seoul-Bike-Sharing-Demand/
├── Seoul_Bike_Sharing_Demand_Prediction.ipynb
├── SeoulBikeData.csv
└── README.md
```
