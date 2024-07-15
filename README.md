# Crime Data Analysis Project

## Introduction

In an ever-evolving world, analyzing crime data is crucial for understanding public safety dynamics. This project presents a comprehensive analysis of real-world crime data from 2020 to the present, uncovering patterns, trends, and factors contributing to crime rates. This analysis benefits law enforcement, policymakers, community leaders, and citizens by providing insights that inform crime prevention strategies, resource allocation, and safer community promotion. You can download the dataset used in this project from <a href="https://catalog.data.gov/dataset/crime-data-from-2020-to-present">Crime Data from 2020 to Present</a>.


## Data Cleaning

This dataset, representing crimes in Los Angeles from 2020 to October 2023, requires extensive cleaning. We address missing values, standardize time formats, consolidate crime codes, and create meaningful addresses. We also fill gaps in the victim age, premise code, and premise description columns with appropriate values. The cleaned data allows for thorough analysis and visualization of crime trends.

## Results and Findings

- **Overall Crime Trends**: Crime rates have increased by 8.3% annually.
- **Seasonal Patterns**: Crime rates drop in September, rise in October, drop again in November, and increase in January.
- **Common Crime Types**: The most common crime is stolen vehicles, followed by simple assault and identity theft.
- **Regional Differences**: The Central area has the highest crime count, while the Foothill area has the lowest.
- **Economic Factors**: Crime rates decreased during the 2022-2023 recession and rose post-recession. There is a negative correlation between unemployment rates and crime rates.
- **Day of the Week**: Most crimes occur on Fridays.
- **Major Events Impact**: Earthquakes and events like George Floyd's death and Covid-19 waves influenced crime rates.

## Predictive Analysis

Using the ARIMA model from statsmodels, we forecast future crime rates based on historical data. The model is trained on crime counts per day, and a forecast is generated for the next 90 days. This forecasting helps in understanding potential trends and aids in proactive decision-making for law enforcement and policymakers.

```python
from statsmodels.tsa.arima.model import ARIMA
import matplotlib.pyplot as plt
import pandas as pd

# Prepare time series data
df_ts = df.loc[df['DATE OCC'] < '2023-10-01']
df_ts = df_ts.groupby('DATE OCC').size().reset_index(name='COUNT')
df_ts = df_ts.set_index('DATE OCC')

# Fit ARIMA model
model = ARIMA(df_ts, order=(1, 0, 100))
model_fit = model.fit()

# Forecasting
forecast_days = 90
forecast = model_fit.forecast(steps=forecast_days)
forecast_dates = pd.date_range(start=df_ts.index[-1], periods=forecast_days)

# Plotting
plt.figure(figsize=(12, 8))
plt.plot(df_ts.index, df_ts['COUNT'], label='Observed')
plt.plot(forecast_dates, forecast, color='red', label='Forecast')
plt.xlabel('Date', fontsize=15)
plt.ylabel('Crime Count', fontsize=15)
plt.legend()
plt.title('Timeseries Forecast of Crime Data using ARIMA Model', fontsize=20)
plt.show()
