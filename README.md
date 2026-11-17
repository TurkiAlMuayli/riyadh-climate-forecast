# Riyadh Climate Trends and Temperature Forecast

Is Riyadh getting hotter? This project analyzes daily weather data from 2010 to 2025 to test the temperature trend, then builds and compares models that forecast monthly temperature.

## Questions

1. Did Riyadh's temperature rise between 2010 and 2025?
2. How accurately can we forecast monthly temperature, and which model does it best?

## Data

Daily weather for Riyadh (latitude 24.71, longitude 46.67), 2010-01-01 to 2025-12-31 (5,844 days), from the [Open-Meteo Historical Weather API](https://open-meteo.com/). The notebook downloads it directly, so no data file is needed.

Variables used: mean, max and min temperature, precipitation, wind speed and solar radiation (the last three for exploration only).

## What I did

1. **Cleaning:** checked data types, missing values and duplicates.
2. **EDA:** distributions, seasonality, precipitation, wind and radiation patterns. I found that 2025 wind speed drops in every month, which looks like a data issue, so I did not use wind for any conclusions.
3. **Trend:** linear regression and the Mann-Kendall test on yearly mean temperature.
4. **Forecasting:** monthly mean temperature with three models, using a time-based split (train 2010-2022, validation 2023-2024, test 2025):
   - Baseline (each month's historical average)
   - Prophet
   - SARIMA
5. **Final test:** the best model on validation (SARIMA) was refit on 2010-2024 and tested once on 2025, then used to forecast 2026.

## Results

**Trend:** yearly mean temperature rose by about 0.12-0.13°C per year, roughly 1.7-2°C between 2010 and 2025.

| Test | Slope (°C/year) | p-value |
|---|---|---|
| Linear regression | 0.115 | 0.0003 |
| Mann-Kendall (Sen's slope) | 0.133 | 0.001 |

**Forecasting (MAE in °C):**

| Model | Validation (2023-2024) | Test (2025) |
|---|---|---|
| Baseline | 1.33 | 1.60 |
| Prophet | 1.20 | - |
| SARIMA | 1.11 | 1.04 |

SARIMA was the best model on validation and beat the baseline on the 2025 test. The 2026 forecast peaks at about 38°C in July.

## Limitations

- The data comes from one source for one location, so it shows warming in this dataset but cannot explain the cause.
- The model comparison on validation covers only 24 months, so the gaps between models are small.
- The 2026 forecast cannot be checked against real values in this project.

## Tools

Python, pandas, matplotlib, seaborn, SciPy, scikit-learn, statsmodels, Prophet, pymannkendall.

## Data credit

Weather data by [Open-Meteo.com](https://open-meteo.com/).
