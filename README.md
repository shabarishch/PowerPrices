# PowerPrices

## Introduction

The steep drop in the price of solar panels and battery storage have resulted in significant increase in the fraction of electricity supplied by renewables. In the California grid, we frequently see solar power meeting more than 60% of the demand during the day. However, the intermittency of solar (and wind) power is a challenge to deal with. One of the ways to address this is demand response programs, where devices like smart thermostats can reduce consumption during periods of high prices brought about by a shortfall in supply.
Battery storage services also capture excess solar production and release electricity to the grid during times of high prices. For both these applications, good price forecasting is critical.

## Aim

We aim to forecast prices for a horizon of 2 hours, which corresponds to 8 time points at a frequency of 15mins. Our baseline model predicts the last known price for the next two hours. As a first step, we look into time series forecasting without using exogenous variables with SARIMA models. Subsequently, we include exogenous variables with SARIMAX models.

## Data

Our target variable is the real time price in the NP-15 zone of the California grid. The prices are set every 5 mins but we only look at a frequency of 15 mins due to computational constraints. Our exogenous variables are - 
<ol>
  <li>Hourly load and generation from solar+wind in the NP-15 region </li>
  <li>Forecasted hourly load and forecasted generation from solar+wind in the NP-15 region.</li>
  <li> Natural Gas Price for each day.</li>
</ol>
Data is already made available in csv format from https://www.eia.gov/electricity/wholesalemarkets/data.php?rto=caiso and https://www.gridstatus.io/. 

## Modeling

<ul>
  <li> We identify possible parameters for SARIMA by differencing to obtain stationarity (confirmed by ADF test), and looking at ACF and PACF plots, and select the best model by cross validation. </li>
  <li> We perform cross validation on disjoint folds of month long data in the year 2023 for March, June, September and December. In each fold, we train on days 1-27, and validate on 2 hour intervals from 6am to 6pm on the 28th.</li>
  <li> Features for SARIMAX are last known actuals and forecasting errors for load and solar+wind generation, and natural gas prices. </li>
</ul>

## Results

<ul>
  <li> SARIMA models perform significantly worse than the baseline. </li>
  <li> Adding exogenous variables improves performance, but still does not improve over baseline. </li>
  <li> For future work, we plan to identify periods of high volatility and see whether models perform better at such times and try other models. </li>
</ul>
