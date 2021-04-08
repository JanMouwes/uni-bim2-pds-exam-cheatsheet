# Time series analysis

## Libraries

Use [yfinance](https://pypi.org/project/yfinance/) for loading in stock-data.

```python
import yfinance as yf
```

```python
ARIMA
```

```python
plot_acf and plot_pacf
```

## Data normalisation and analysis

### Difference using [Series.diff()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.diff.html)

```python
def differences(data, column_name):
    return data[column_name].diff()
```

### Relative change (percentage change) using [Series.pct_change()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.pct_change.html)

```python
def relative_change(data, column_name):
    return data[column_name].pct_change()
```

### Analysis with the Augmented Dicky-Fuller test using [statsmodels.tsa.stattools.adfuller](https://www.statsmodels.org/stable/generated/statsmodels.tsa.stattools.adfuller.html)

```python
# ser: pandas.Series containing your data 
def show_adf_analysis(ser):
    # adfuller cannot handle missing values, so we drop them
    ser = ser.dropna()

    # Get result and split it
    result = adfuller(ser)
    adf_statistic, p_value, _, _, critical_values = result

    # The test is significant if the p-value is lower than 0.10
    is_stationary = p_value < 0.10

    # Print the values
    print('ADF Statistic: %f' % adf_statistic)
    print('p-value: %f' % p_value)
    print('Critical Values:')
    for key, value in critical_values.items():
        print('\t%s: %.3f' % (key, value))
    print("Is stationary:", str(is_stationary), "\n")
```

## Complete ARIMA example

[statsmodels.tsa.arima_model.ARIMA](https://www.statsmodels.org/stable/generated/statsmodels.tsa.arima_model.ARIMA.html)

To be able to perform ARIMA we must first determine its order. This is in the form of a 3-dimensional tuple (n, m, o),
where n, m and o are integers or lists of integers. These numbers represent the AR, I and MA components respectively.
Filling in a 0 for a parameter represents disabling this part of the model.

Given that we want to use integration, the I-component should be 1.

### Determining whether to use the Moving Average (MA) component using [plot_acf]()

```python
# ser: pandas.Series containing data you want to analyse
def prep_and_plot_acf(ser):
    arma_data = ser.dropna()

    # Interpretation: 
    # if the data shown through this plot is mostly inside the light blue uncertainty area, 
    # then it is not statistically significant. As such, the MA-component can be disabled (set to 0)
    plot_acf(arma_data, lags=5)
```

### Determining whether to use the Autoregression (AR) component using [plot_pacf]()

```python
# ser: pandas.Series containing data you want to analyse
def prep_and_plot_acf(ser):
    arma_data = ser.dropna()

    # Interpretation: 
    # if the data shown through this plot is mostly inside the light blue uncertainty area, 
    # then it is not statistically significant. As such, the MA-component can be disabled (set to 0)
    plot_pacf(arma_data, lags=5)
```

### Applying these observations to ARIMA

[ARIMA.fit()](https://www.statsmodels.org/stable/generated/statsmodels.tsa.arima_model.ARIMA.fit.html)
[ARIMAResults.plot_predict()](https://www.statsmodels.org/stable/generated/statsmodels.tsa.arima_model.ARIMAResults.plot_predict.html)

```python
# ser: pandas.Series you want to apply ARIMA on
# order: tuple (int, int, int), ARIMA-order as described above
def do_arima(ser, order):
    model = ARIMA(ser, order=order)
    model_fit = model.fit(
        disp=0  # optional. Disables printing information
    )
    summary = model_fit.summary()

    fig = model_fit.plot_predict(
        dynamic=False
    )

    print(summary)
    plt.show(fig)

    return model_fit


def fit_order_model(order):
    slotkoers_model = ARIMA(koers_aandeel['Adj Close'], order=order)
    return slotkoers_model.fit(disp=0)

# ser: pandas.Series you want to apply ARIMA on
def do_multiple_arima(ser):
    orders = [(1,1,0), (0,1,1), (1,1,2), (2,1,1), (2,1,2)]
    
    fits = {}
    for order in orders:
        slotkoers_model = ARIMA(ser, order=order)
        fit = slotkoers_model.fit(disp=0)
        aic = fit.aic
        bic = fit.bic
        
        fits[order] = (aic, bic, fit)

    print(" order\t     aic      bic")
    pp.pprint(fits)

    for _, _, fit in fits.values():
        print()
        print(fit.summary())

# Disable warnings
with warnings.catch_warnings():
    warnings.simplefilter('ignore')
    
    # 'ser' is some series that needs analysis
    do_multiple_arima(ser)
```

