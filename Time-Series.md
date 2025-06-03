
## 1. What Is a Time Series?

* **Definition**:
  A *time series* is a sequence of observations collected at uniform intervals over time.

  * *Examples*:

    * Daily closing prices of a stock.
    * Monthly gold prices year-over-year.
    * Quarterly GDP figures.

* **Key Characteristics**:

  1. Two-dimensional data:

     * **Time** (e.g., dates, months, years).
     * **Value** of the variable at each time point (e.g., price, count, measurement).
  2. Observations are ordered chronologically, so “past” and “future” have clear meaning.
  3. Goals include:

     * **Descriptive analysis**: Understand how the data behaves over time.
     * **Forecasting**: Use historical patterns to predict future values.

* **Why Study Time Series?**

  1. Many real-world processes evolve over time (finance, economics, weather, etc.).
  2. Recognizing patterns (e.g., seasonality around holidays, long-term growth trends) helps in planning (e.g., inventory, budgeting).
  3. Forecasting empowers decision-making:

     * Businesses forecasting sales for inventory planning.
     * Central banks forecasting inflation for interest-rate decisions.

---

## 2. Components of a Time Series

A typical time series can be thought of as the sum (or combination) of several components:

1. **Trend (T):**

   * *What it is*: A long-term increase or decrease in the data.
   * *Examples*:

     * Steadily rising gold prices over a decade.
     * Declining print newspaper subscriptions over 20 years.
   * *How to spot it*:

     * Plot the raw data: if it generally moves up (or down) over a long period, there’s a trend.

2. **Seasonality (S):**

   * *What it is*: A repeating pattern that occurs at fixed, known intervals **within one year** (i.e., within 12 months if monthly data, within 7 days if daily data).
   * *Examples*:

     * Retail sales spikes every December (holiday season).
     * Restaurant footfall peaks on weekends.
     * Electricity demand peaks in summer (air-conditioning) and winter (heating).
   * *Features*:

     * Period = known (e.g., 12 months, 7 days).
     * Amplitude (size of seasonal effect) may change over time.

3. **Cyclical Component (C):**

   * *What it is*: Up-and-down movements with durations typically > 1 year, not of fixed period.
   * *Examples*:

     * Economic expansions and recessions (boom/bust cycles).
     * Business investment cycles.
   * *Difference from seasonality*:

     * **Seasonality**: regular, known, within a year.
     * **Cycle**: irregular, longer than a year, not strictly periodic.

4. **Irregular (Random/Residual) Component (I):**

   * *What it is*: Unpredictable fluctuations not explained by T, S, or C.
   * *Examples*:

     * Natural disasters, strikes, political events that cause sudden spikes or drops.
   * *Properties*:

     * Ideally, after removing T + S + C, the residuals should look like white noise (no patterns left).

Mathematically, many methods assume an **additive decomposition**:

$$
\text{Observed (Y)} = T + S + C + I
$$

or, if the magnitude of components grows with the series (e.g., variance increases as level increases), a **multiplicative decomposition**:

$$
Y = T \times S \times C \times I.
$$

---

## 3. Visualizing a Time Series

1. **Line Plot (Time on X-axis, Value on Y-axis)**

   * Provides an immediate sense of trend, seasonality, and outliers.
   * Example (Python/pandas):

     ```python
     import pandas as pd
     import matplotlib.pyplot as plt

     df = pd.read_csv("monthly_passengers.csv", parse_dates=["Month"], index_col="Month")
     df["Passengers"].plot(figsize=(10, 4), title="Monthly Airline Passengers")
     plt.ylabel("Number of Passengers")
     plt.show()
     ```

2. **Seasonal Subseries Plot**

   * Splits the data by season (e.g., month) to see how each month behaves over years.
   * Helps understand whether January consistently has a lower/higher value than July, etc.

3. **Autocorrelation Function (ACF) & Partial Autocorrelation Function (PACF) Plots**

   * Not exactly “visualizing raw data,” but crucial for model selection:

     * **ACF plot** shows correlation between the series and its lags.
     * **PACF plot** shows correlation between the series and a given lag *after* removing the effects of intervening lags.

4. **Decomposition Plot (Additive or Multiplicative)**

   * Many libraries (e.g., statsmodels’ `seasonal_decompose`) break the series into trend, seasonal, and residual components for inspection.

---

## 4. Smoothing & Moving Averages

### 4.1. Why Smooth Data?

* **Problem**: Raw time-series data often shows large short-term fluctuations due to seasonality or random noise. Such volatility can obscure underlying trends.
* **Goal**: Extract a clearer picture of trend (and sometimes seasonality) by reducing noise.

### 4.2. Simple Moving Average (SMA)

* **Definition**: The *moving average* at time \$t\$ is the (arithmetic) average of the previous *k* observations (including \$t\$ or up to \$t – 1\$ depending on convention).

* **Formula (centered, symmetric window)** for window size \$k\$ (odd):

  
![image](https://github.com/user-attachments/assets/b699f77c-0a75-49eb-a0a8-7716af14ea35)



* **Common choice**: A 12-month moving average for monthly data (to smooth out seasonality).

* **Effect**:

  * Dampens short-term spikes/dips.
  * Lag: Because it uses past (and sometimes future) values, the MA “lags” behind actual data.

* **Example in Python**:

  ```python
  df["MA_12"] = df["Value"].rolling(window=12, center=True).mean()
  df[["Value", "MA_12"]].plot(figsize=(10,4))
  ```

### 4.3. Exponential Moving Average (EMA)

* **Variation**: Instead of a simple average, give exponentially decreasing weights to older observations.
* **Advantage**: More responsive to recent changes than a simple moving average.
* **Formula** (recursive):

  $$
  EMA_t = \alpha \, Y_t + (1 - \alpha)\, EMA_{t-1},\quad 0 < \alpha < 1.
  $$

  * \$\alpha\$ controls “memory”: smaller \$\alpha\$ → smoother, larger lag; larger \$\alpha\$ → more responsive.

---

## 5. Stationarity: Definition & Importance

### 5.1. What Is Stationarity?

A time series is said to be **strictly stationary** if the joint distribution of any set of observations is the same across all time shifts. In practice, we often use the weaker concept of **weak (or covariance) stationarity**, which requires:

1. **Constant mean** over time.
2. **Constant variance** over time (no changing volatility).
3. **Autocovariance** between \$Y\_t\$ and \$Y\_{t+k}\$ depends only on the lag \$k\$, not on time \$t\$ itself.

* **Why it matters**:

  * Many forecasting methods (e.g., ARIMA) assume stationarity.
  * If a series is non-stationary (e.g., a strong trend or changing variance), these models can give misleading results.

### 5.2. Checking Stationarity

1. **Visual Inspection**:

   * Plot the rolling mean/rolling standard deviation: If these appear roughly constant over time, the series may be (weakly) stationary.

2. **Statistical Tests**:

   * **Augmented Dickey–Fuller (ADF) Test**:

     * Tests the null hypothesis \$H\_0\$: “the series has a unit root” (i.e., is non-stationary).
     * **Interpretation**:

       * If the test statistic < critical value (or p-value < significance level, e.g. 0.05), **reject** \$H\_0\$ ⇒ evidence that the series is stationary.
       * Otherwise, fail to reject \$H\_0\$ ⇒ likely non-stationary.

   * **KPSS Test (Kwiatkowski–Phillips–Schmidt–Shin)**:

     * Tests the null hypothesis \$H\_0\$: “the series is stationary.”
     * If test statistic > critical value (or p-value < 0.05), reject \$H\_0\$ ⇒ evidence of non-stationarity.
     * Often run alongside ADF to confirm.

* **Python Example (ADF)**:

  ```python
  from statsmodels.tsa.stattools import adfuller

  result = adfuller(df["Value"])
  print("ADF Statistic:", result[0])
  print("p-value:", result[1])
  for key, crit_val in result[4].items():
      print(f"Critical Value ({key}): {crit_val}")
  ```

### 5.3. Making a Series Stationary

If non-stationary, common approaches include:

1. **Differencing**:

   * Compute the difference between consecutive observations:

     $$
     Y'_t = Y_t - Y_{t-1}.
     $$
   * If first differencing (\$d = 1\$) doesn’t fully remove trend, try second differencing:

     $$
     Y''_t = Y'_t - Y'_{t-1} = Y_t - 2Y_{t-1} + Y_{t-2}.
     $$
   * Each differencing order \$d\$ “removes” a polynomial trend of degree \$d\$.
   * **When to stop**: After differencing, re-run ADF test; if stationary, stop.

2. **Transformations**:

   * Apply log, square root, or Box–Cox transform to stabilize variance:

     * E.g., if variance increases with the level of the series (common in financial data), take \$\log(Y\_t)\$ to make fluctuations more homogeneous.

3. **Removing Trend/Seasonality Directly**:

   * Fit a deterministic trend (e.g., simple linear regression vs. time) and subtract it.
   * Seasonal differencing (e.g., for monthly data with yearly seasonality, do \$Y'*t = Y\_t - Y*{t-12}\$).
   * Combining both: Seasonal & non-seasonal differencing.

---

## 6. Autocorrelation (ACF) & Partial Autocorrelation (PACF)

Understanding ACF and PACF is crucial to specifying ARIMA-type models.

### 6.1. Autocorrelation Function (ACF)

* **Definition**: The ACF at lag \$k\$ is the correlation between the series and its lag-\$k\$ version:

  $$
  \rho(k) = \frac{\mathrm{Cov}(Y_t,\, Y_{t-k})}{\sqrt{\mathrm{Var}(Y_t)\,\mathrm{Var}(Y_{t-k})}}.
  $$
* **Interpretation**:

  * **Lag 0**: Correlation of series with itself ⇒ always 1.
  * **Lag 1**: Correlation of \$Y\_t\$ with \$Y\_{t-1}\$.
  * **Lag 2**: Correlation of \$Y\_t\$ with \$Y\_{t-2}\$, etc.
* **Plot**: Bar chart from lag 1 up to, say, lag 20.
* **Significance bounds**: Usually ±1.96/√N (where N = length of series). Bars beyond these bounds suggest significant autocorrelation at that lag.

### 6.2. Partial Autocorrelation Function (PACF)

* **Definition**: The PACF at lag \$k\$ is the correlation between \$Y\_t\$ and \$Y\_{t-k}\$ *after* removing the linear effects of all intermediate lags (1 to \$k-1\$).
* **Intuition**:

  * Example: PACF at lag 3 gives the correlation between \$Y\_t\$ and \$Y\_{t-3}\$ after accounting for the fact that \$Y\_t\$ is correlated with \$Y\_{t-1}\$ and \$Y\_{t-2}\$.
* **Usage**:

  * A quick way to identify the order of an AR (autoregressive) model:

    * If PACF cuts off (drops to zero) after lag \$p\$ (i.e., significant up to lag \$p\$, then near-zero thereafter), suggests an AR(\$p\$) process.

### 6.3. ACF/PACF & Model Identification

* **AR(\$p\$) process**:

  * Only first \$p\$ lags are significant in the PACF (cuts off after \$p\$).
  * ACF decays gradually (exponentially or in damped sine wave).

* **MA(\$q\$) process**:

  * Only first \$q\$ lags are significant in the ACF (cuts off after \$q\$).
  * PACF decays gradually.

* **ARMA(\$p,q\$) process**:

  * Both ACF and PACF tail off with a more complicated pattern.
  * Often need information‐criterion approaches (AIC/BIC) to choose \$(p,q)\$.

Plotting ACF/PACF (Python example):

```python
from statsmodels.graphics.tsaplots import plot_acf, plot_pacf

plot_acf(df["Value"], lags=30)
plot_pacf(df["Value"], lags=30)
plt.show()
```

---

## 7. Core Models: AR, MA, ARMA, ARIMA

### 7.1. Autoregressive (AR) Models

* **Formulation**: An AR(\$p\$) model predicts \$Y\_t\$ as a linear combination of its past \$p\$ values plus noise:

  $$
  Y_t = \phi_1 Y_{t-1} + \phi_2 Y_{t-2} + \dots + \phi_p Y_{t-p} + \varepsilon_t,
  $$

  where \$\varepsilon\_t\$ is white noise (zero mean, constant variance, uncorrelated over time).

* **Interpretation**: The current value depends on its own previous values.

* **Parameter Selection**:

  * Use PACF to identify \$p\$ (lag after which PACF cuts off).
  * Alternatively, fit models for different \$p\$ and compare AIC/BIC.

### 7.2. Moving Average (MA) Models

* **Formulation**: An MA(\$q\$) model predicts \$Y\_t\$ using past \$q\$ error terms:

  $$
  Y_t = \theta_0 + \varepsilon_t + \theta_1 \varepsilon_{t-1} + \dots + \theta_q \varepsilon_{t-q},
  $$

  where \$\varepsilon\_t\$ is white noise.

* **Interpretation**: The current value depends on a linear combination of current and past random shocks (errors).

* **Parameter Selection**:

  * Use ACF: identify \$q\$ as the lag after which ACF cuts off.
  * Check significance of coefficients.

### 7.3. ARMA(\$p,q\$)

* **Formulation**: Combines AR(\$p\$) and MA(\$q\$) into:

  $$
  Y_t = \phi_1 Y_{t-1} + \dots + \phi_p Y_{t-p} \;+\; \varepsilon_t + \theta_1 \varepsilon_{t-1} + \dots + \theta_q \varepsilon_{t-q}.
  $$

* **Stationarity**: Requires the series to be (weakly) stationary already.

* **Use Cases**: Stationary series without large trends/seasonality.

* **Identification**:

  * Use both ACF/PACF, but often trial and error with AIC/BIC to pick best \$(p,q)\$.

### 7.4. ARIMA(\$p,d,q\$)

* **Purpose**: Extend ARMA to handle non-stationary series via differencing.

* **Components**:

  * **\$p\$**: Order of autoregressive part.
  * **\$d\$**: Number of differences needed to make the series stationary.
  * **\$q\$**: Order of moving average part.

* **Workflow**:

  1. **Check stationarity** → If non-stationary, difference the data \$d\$ times until stationary.
  2. **Examine ACF/PACF** on the *differenced series* to suggest \$p\$ and \$q\$.
  3. **Fit ARIMA(\$p,d,q\$)** model on original data (internally applies the differencing).
  4. **Diagnostic checks on residuals**:

     * Residual ACF should show no significant autocorrelations (white noise).
     * Ljung–Box test for no autocorrelation in residuals.

* **Python Example (statsmodels)**:

  ```python
  from statsmodels.tsa.arima.model import ARIMA

  # Suppose d=1 (first differencing), p=1, q=2
  model = ARIMA(df["Value"], order=(1, 1, 2))
  result = model.fit()
  print(result.summary())
  ```

* **Interpret Forecasting**: Once fit, the model can produce forecasts (point predictions plus confidence intervals) for future time points.

---

## 8. Seasonality & SARIMA/SARIMAX

### 8.1. Why ARIMA May Fail for Seasonal Data

* **Issue**: ARIMA only handles non-seasonal differencing (\$d\$). If a series has strong seasonality (e.g., monthly data with annual seasonal cycle), an ARIMA without seasonal terms can mis‐specify the model.
* **Symptoms**:

  * Forecasts may systematically under/overestimate at certain seasons.
  * ACF of residuals shows spikes at seasonal lags (e.g., lag 12 for monthly series).

### 8.2. SARIMA (Seasonal ARIMA)

* **Notation**: SARIMA(\$p, d, q\$)\$\times\$(P, D, Q, \$s\$)

  * **\$p, d, q\$**: Non-seasonal AR, differencing, MA orders (as in ARIMA).
  * **\$P, D, Q\$**: Seasonal AR, differencing, MA orders.
  * **\$s\$**: Length of seasonality (e.g., \$s=12\$ for monthly data with yearly seasonality, \$s=7\$ for weekly seasonality in daily data).

* **Model Structure**:

  * Seasonal AR: terms like \$\Phi\_1 Y\_{t-s}\$, \$\Phi\_2 Y\_{t-2s}\$…
  * Seasonal MA: terms like \$\Theta\_1 \varepsilon\_{t-s}\$, \$\Theta\_2 \varepsilon\_{t-2s}\$…
  * Seasonal differencing: first compute \$(1 - B^s)^D Y\_t\$ where \$B\$ is the backshift operator (\$B Y\_t = Y\_{t-1}\$).

* **Example**: SARIMA(1, 1, 2)×(1, 1, 1, 12) for monthly data:

  * Non-seasonal: AR(1), differencing once, MA(2).
  * Seasonal: AR lag 12 = 1, seasonal differencing once, MA lag 12 = 1.

* **Fitting in Python**:

  ```python
  from statsmodels.tsa.statespace.sarimax import SARIMAX

  model = SARIMAX(
      df["Value"],
      order=(1, 1, 2),
      seasonal_order=(1, 1, 1, 12),
      enforce_stationarity=False,
      enforce_invertibility=False
  )
  result = model.fit(disp=False)
  print(result.summary())
  ```

### 8.3. SARIMAX (Seasonal ARIMA with Exogenous Variables)

* **Difference from SARIMA**: SARIMAX allows you to incorporate **exogenous regressors** (external variables) that may influence your target series.

  * *Example*: Gold prices (target) might be influenced by exogenous factors such as inflation rate, USD index, or interest rates.
* **Notation**: SARIMAX(\$p,d,q\$)\$\times\$(P, D, Q, \$s\$) + Exogenous
* **How to use**: Provide an additional dataframe (or array) of exogenous variables aligned with the time index.
* **In Python**:

  ```python
  exog = pd.DataFrame({
      "Inflation": inflation_series,
      "USD_Index": usd_index_series
  }, index=df.index)

  model = SARIMAX(
      df["GoldPrice"],
      order=(1, 1, 2),
      seasonal_order=(1, 1, 1, 12),
      exog=exog,
      enforce_stationarity=False,
      enforce_invertibility=False
  )
  result = model.fit(disp=False)
  print(result.summary())
  ```

---

## 9. Practical Steps for Time Series Forecasting (Python Workflow)

1. **Load & Inspect Data**

   ```python
   import pandas as pd
   df = pd.read_csv("time_series_data.csv", parse_dates=["date"], index_col="date")
   df.plot()
   ```

2. **Visualize Components**

   * Plot raw series.
   * Use `seasonal_decompose`:

     ```python
     from statsmodels.tsa.seasonal import seasonal_decompose
     decomposition = seasonal_decompose(df["Value"], model="additive", period=12)
     decomposition.plot();
     ```

3. **Check Stationarity**

   * Plot rolling mean/variance.
   * Run ADF test.
   * If non-stationary (likely), apply transformations/differencing.

4. **Make Stationary**

   * If trend present: apply first differencing: `df_diff = df["Value"].diff().dropna()`.
   * If seasonality present: apply seasonal differencing: `df_seas_diff = df["Value"].diff(12).dropna()`.
   * Possibly combine:

     ```python
     df_diff = df["Value"].diff().dropna()
     df_diff_seas = df_diff.diff(12).dropna()
     ```

5. **Plot ACF & PACF on Stationary Series**

   ```python
   from statsmodels.graphics.tsaplots import plot_acf, plot_pacf
   plot_acf(df_diff_seas, lags=30)
   plot_pacf(df_diff_seas, lags=30)
   ```

6. **Select Model Order**

   * Based on ACF (for \$q\$) and PACF (for \$p\$) patterns, pick candidate \$(p,d,q)\$ and \$(P,D,Q,s)\$ if seasonal.
   * Alternatively, use **auto\_arima** from `pmdarima` to search automatically:

     ```python
     import pmdarima as pm
     auto_model = pm.auto_arima(
         df["Value"],
         seasonal=True,
         m=12,           # seasonal period
         trace=True,
         error_action='ignore',  
         suppress_warnings=True
     )
     print(auto_model.summary())
     ```

7. **Split Train/Test**

   * Use the first 70–80% for training, last 20–30% for testing.
   * E.g.:

     ```python
     train = df.iloc[:-12]
     test = df.iloc[-12:]
     ```

8. **Fit Final Model**

   ```python
   from statsmodels.tsa.statespace.sarimax import SARIMAX

   model = SARIMAX(
       train["Value"],
       order=(p, d, q),
       seasonal_order=(P, D, Q, s),
       enforce_stationarity=False,
       enforce_invertibility=False
   )
   result = model.fit(disp=False)
   ```

9. **Diagnose Residuals**

   * Plot residuals, ACF of residuals.
   * Ljung–Box test for autocorrelation:

     ```python
     from statsmodels.stats.diagnostic import acorr_ljungbox
     lb_test = acorr_ljungbox(result.resid, lags=[10], return_df=True)
     print(lb_test)
     ```
   * If residuals still show patterns, revisit model order.

10. **Forecast & Evaluate**

    * **In-sample forecast** (for training period):

      ```python
      pred_train = result.get_prediction(start=train.index[0], end=train.index[-1], dynamic=False)
      ```
    * **Out-of-sample forecast** (for test period):

      ```python
      pred_test = result.get_forecast(steps=len(test))
      pred_mean = pred_test.predicted_mean
      conf_int = pred_test.conf_int()
      ```
    * **Evaluation Metrics**:

      * **Mean Absolute Error (MAE)**:

        $$
        \text{MAE} = \frac{1}{n} \sum_{t=1}^n |Y_t - \hat{Y}_t|.
        $$
      * **Root Mean Squared Error (RMSE)**:

        $$
        \text{RMSE} = \sqrt{\frac{1}{n} \sum_{t=1}^n (Y_t - \hat{Y}_t)^2}.
        $$
      * Or **Mean Absolute Percentage Error (MAPE)** if relative errors matter:

        $$
        \text{MAPE} = \frac{100\%}{n} \sum_{t=1}^n \left|\frac{Y_t - \hat{Y}_t}{Y_t}\right|.
        $$
    * **Plot forecasts vs. actuals**:

      ```python
      ax = df["Value"].plot(label="Actual", figsize=(10,4))
      pred_mean.plot(ax=ax, label="Forecast")
      ax.fill_between(
          test.index,
          conf_int.iloc[:, 0],
          conf_int.iloc[:, 1],
          color="pink", alpha=0.3
      )
      plt.legend()
      plt.show()
      ```

11. **Refine Model**

    * If forecasts are poor:

      * Re-examine stationarity.
      * Try different \$(p,d,q)\$ or seasonal orders.
      * Consider including exogenous variables (SARIMAX).
      * Explore alternative methods:

        * **Exponential Smoothing** (e.g., Holt–Winters).
        * **State-space models** (e.g., Prophet, TBATS).
        * **Machine learning** (e.g., random forests, gradient boosting on lagged features).

---

## 10. Common Pitfalls & Tips

1. **Over-Differencing**:

   * Differencing more times than necessary can introduce noise and cause overdamped forecasts. Always check stationarity after each differencing step.

2. **Seasonal vs Non-Seasonal Differencing**:

   * If strong seasonality, use seasonal differencing of order \$D\$ before non-seasonal differencing.
   * Order matters: often do non-seasonal differencing (\$d\$) first, then seasonal differencing (\$D\$).

3. **Large Values of \$p,q,P,Q\$ May Overfit**:

   * Use information criteria (AIC, BIC) to penalize over-parameterization.
   * Keep models as simple as possible while capturing key patterns.

4. **Check Residuals Thoroughly**:

   * Residuals should look like white noise (no autocorrelation, constant variance, zero mean).
   * If residual ACF shows spikes at seasonal lags, revisit the seasonal component.

5. **Beware of Non-Stationary Variance ("Heteroskedasticity")**:

   * If variance changes over time (e.g., financial returns), consider modeling variance separately (e.g., GARCH), or transform data (e.g., log).

6. **Use Cross-Validation for Time Series**:

   * Standard random splits break temporal order.
   * Use “rolling origin” cross-validation: train on \[\$t\_1\$ … \$t\_k\$], test on \[\$t\_{k+1}\$], then expand window.

---

## 11. Example: Forecasting Monthly Airline Passengers

Below is a simplified illustrative example to tie all concepts together. Suppose we have a dataset `AirPassengers.csv` containing monthly totals of international airline passengers from January 1949 to December 1960. We want to forecast the next 12 months.

1. **Load Data**

   ```python
   import pandas as pd
   import matplotlib.pyplot as plt

   df = pd.read_csv("AirPassengers.csv", parse_dates=["Month"], index_col="Month")
   df.rename(columns={"#Passengers": "Passengers"}, inplace=True)
   df.plot(title="Monthly Air Passengers (1949–1960)")
   plt.ylabel("Passengers (1000s)")
   plt.show()
   ```

   * Visible pattern: strong upward trend + clear seasonality.

2. **Decompose**

   ```python
   from statsmodels.tsa.seasonal import seasonal_decompose
   decomposition = seasonal_decompose(df["Passengers"], model="multiplicative", period=12)
   decomposition.plot();
   ```

   * We see:

     * *Trend* steadily rising.
     * *Seasonal* peaks in mid-year.
     * *Residual* fairly “noisy.”

3. **Stationarity Check**

   * **ADF on raw data** → very likely non-stationary (p-value > 0.05).
   * **First differencing**:

     ```python
     df_diff = df["Passengers"].diff().dropna()
     result = adfuller(df_diff)
     # Probably still non-stationary because of seasonality.
     ```
   * **Seasonal differencing (lag 12)**:

     ```python
     df_diff_seas = df["Passengers"].diff(12).dropna()
     result2 = adfuller(df_diff_seas)
     ```
   * Often need both:

     ```python
     df_diff_both = df["Passengers"].diff().diff(12).dropna()
     result3 = adfuller(df_diff_both)
     # Now p-value < 0.05 → stationary.
     ```

4. **Plot ACF & PACF on Differenced Series**

   ```python
   plot_acf(df_diff_both, lags=30)
   plot_pacf(df_diff_both, lags=30)
   ```

   * Suppose ACF shows significant spike at lag 1 and lag 12. PACF shows spikes at lag 1 and lag 12.
   * We might choose ARIMA(0, 1, 1)×(0, 1, 1, 12).

5. **Fit SARIMA**

   ```python
   from statsmodels.tsa.statespace.sarimax import SARIMAX

   model = SARIMAX(
       df["Passengers"],
       order=(0, 1, 1),
       seasonal_order=(0, 1, 1, 12),
       enforce_stationarity=False,
       enforce_invertibility=False
   )
   result = model.fit(disp=False)
   print(result.summary())
   ```

6. **Diagnose Residuals**

   ```python
   residuals = result.resid
   plot_acf(residuals, lags=30)
   # Residuals should not show significant autocorrelation.
   ```

7. **Forecast Next 12 Months**

   ```python
   pred = result.get_forecast(steps=12)
   pred_mean = pred.predicted_mean
   conf_int = pred.conf_int()

   ax = df["Passengers"].plot(label="Observed", figsize=(10,4))
   pred_mean.plot(ax=ax, label="Forecast")
   ax.fill_between(
       pred_mean.index,
       conf_int.iloc[:, 0],
       conf_int.iloc[:, 1],
       color="lightgrey", alpha=0.5
   )
   plt.legend()
   plt.title("Air Passengers Forecast")
   plt.show()
   ```

8. **Evaluate (if actuals known)**

   * If we had actual data for those next 12 months, compute MAE/RMSE.

---

## 12. Alternative Forecasting Methods (Overview)

While ARIMA/SARIMA are classical statistical methods, other approaches exist:

1. **Holt–Winters Exponential Smoothing**

   * Handles level, trend, and seasonality using exponentially weighted averages.
   * Often easier to use than ARIMA, especially for simple seasonal patterns.
   * Two variants:

     * **Additive** (trend and seasonality add to level).
     * **Multiplicative** (trend and/or seasonality multiply the level).
   * Python: `statsmodels.tsa.holtwinters.ExponentialSmoothing`.

2. **State-Space Models (ETS, Structural Time Series)**

   * More flexible: model time series as state-space system with observed and hidden (“state”) variables.
   * Can accommodate time-varying trends and seasonal patterns.
   * Python: `statsmodels`’s `UnobservedComponents`.

3. **Prophet (Facebook/Meta Prophet)**

   * Designed for business time series.
   * Decomposes series into trend, seasonality (yearly, weekly, daily), holidays.
   * Automatically finds changepoints in trend.
   * Python: install via `pip install prophet`.

4. **Machine Learning & Deep Learning**

   * Use lagged values and exogenous features as inputs to regression/ensemble models (random forests, XGBoost).
   * Recurrent Neural Networks (RNNs), Long Short-Term Memory (LSTM) networks for complex patterns.
   * Require more data and careful hyperparameter tuning.

---

## 13. Summary & Key Takeaways

1. **Understand the Data**:

   * Always begin with exploratory plots to identify trend, seasonality, and outliers.

2. **Check & Achieve Stationarity**:

   * Stationarity is fundamental for ARIMA-type models.
   * Use differencing and transformations as needed.

3. **Use ACF/PACF for Model Identification**:

   * ACF → suggests moving‐average (MA) order \$q\$.
   * PACF → suggests autoregressive (AR) order \$p\$.

4. **Include Seasonality**:

   * If seasonality is present, use SARIMA or exponential smoothing with seasonal components.
   * Always inspect seasonal lags (e.g., lag 12 for monthly data).

5. **Check Residuals**:

   * After fitting, ensure residuals behave like white noise (no autocorrelation, constant variance).

6. **Evaluate Forecasts**:

   * Use hold-out/test sets and metrics (MAE, RMSE, MAPE).
   * Plot forecasts vs. actuals with confidence intervals.

7. **Iterate & Refine**:

   * Time series modeling is often iterative—try different orders, transformations, or methods until you achieve satisfactory accuracy.

8. **Consider Exogenous Influences**:

   * If external variables (e.g., promotions, economic indicators) impact your series, use SARIMAX (or regression models on time-series features).

---

### Additional Resources for Further Learning

* **Books**:

  * *“Time Series Analysis and Its Applications”* by Shumway & Stoffer.
  * *“Forecasting: Principles and Practice”* by Hyndman & Athanasopoulos (free online at [otexts.com/fpp3/](https://otexts.com/fpp3/)).

* **Online Tutorials**:

  * Rob J. Hyndman’s blog (hyndsight).
  * Statsmodels documentation: [https://www.statsmodels.org/stable/tsa.html](https://www.statsmodels.org/stable/tsa.html).

* **Python Packages**:

  * `statsmodels` for classical time series modeling.
  * `pmdarima` for `auto_arima`.
  * `prophet` (from Meta) for automated trend/seasonality/holiday modeling.

With these notes, you should have a solid foundation to start exploring time series data, build forecasting models, and interpret results. Take time to practice on real datasets—hands-on experimentation is the best way to solidify these concepts. Good luck on your time series journey!
