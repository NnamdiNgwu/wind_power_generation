### 1. Project Goal
To forecast the power generated from wind mills in Germany.

### 2. Data Description
The Wind power dataset consists of a datapoint for every 15 minutes. So as we consider a datapoint corresponding to a timeblock, 1 hour will have 4 timeblocks and 1 day => 96 timeblocks.

In the Power generation dataset we have 2 columns

datetime
datetime column is split for every 15 minutes.
power(MW)
For every 15 minutes there is a power(MW) value recorded.

### Dataset was split in to
* Training datatime is 2011-01-01 00:00:00 to 2021-12-31 07:45:00
* datatime for validation data is 2021-01-01 to 2021-06-01
* datetime for Test data is 2021-06-01 to 2021-12-30


### 3. General Architecture
In general, Facebook prophet and ARIMA are used. In the context of this project we explore the possibility of a higher performing alternative by leveraging GridSearchCv for cross validation in combination with Lasso, and CatBoost.

### 4. Lagging Features
To investigate possible serial dependence (like cycles) in a time series, we need to create "lagged" copies of the series. Lagging a time series means to shift its values forward one or more time steps, or equivalently, to shift the times in its index backward one or more steps. In either case, the effect is that the observations in the lagged series will appear to have happened later in time.

* The Lagged features are added with a lag of 1,12,24,28,48,72 timeblocks in the power feature 
* We manually chose the time intervals - 15minutes, 3hr, 6hr, 12hr and 18hrs of lags respectively . Then to check the correlation between the lagged features using PACF and ACF.

### Choosing Lags - Patial Auto Correlation
When choosing lags to use as features, it generally won't be useful to include every lag with a large autocorrelation. for instance, the autocorrelation at lag 2 might result entirely from decayed information from lag 1. Literally, just correlation that's carried over from the previous step. If lag 2 doesn't contain anything new, there would be no reason to include it if we already have lag 1.
The partial Auto Correlation gives us the correlation of a lag compared for all the previous lags.

### 5. Forecast
Forecasting target variable 8 timeblock ahead
The target ahead timeblocks is 8. To forecast for 2 hours ahead => 2*4 = 8 timeblocks

