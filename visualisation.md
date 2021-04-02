# Visualisation

## Libraries
```python
# Matplotlib provides visualisation features
import matplotlib.pyplot as plt

# Seaborn provides extensions to matplotlib (presets, shortcuts, etc)
import seaborn as sns
```

```python
# ARIMA-specific
from statsmodels.graphics.tsaplots import plot_acf, plot_pacf
```

```python
# decision tree-specific
from sklearn.tree import plot_tree
```

## Plots

### Lineplot
Used in time series analysis, for example.

```python
# Docs: https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.plot.html

# data: pandas.Series or pandas.DataFrame containing the data you want to show
def render_line(x, y):
    return plt.plot(
        x, # x (optional). Defaults to range(len(y))
        y, # y
        alpha=.7 # optional. Sets the opacity of the line
    )
```

### Histogram
Shows frequency of elements according to some specified class. 

```python
# Docs: https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.hist.html

# ser: pandas.Series or array containing the data you want to show 
def render_hist(ser):
    return plt.hist(
        x=ser,
        bins=10 # default to 10
    )
```

### Heatmap (for correlation)
```python
# Docs: https://seaborn.pydata.org/generated/seaborn.heatmap.html

# df: pandas.DataFrame containing all data you want to show correlations of
def render_heatmap(df):
    return sns.heatmap(
        data=df.corr(), 
        annot=False, # if set to True, actual values will be shown
        center=None  # if set to 0, will centre the colourmap to a specified value 
    )
```

### Pairplot 
```python
# Docs: https://seaborn.pydata.org/generated/seaborn.pairplot.html

# df: pandas.DataFrame containing data you want to show
def render_heatmap(df):
    return sns.pairplot(
        data=df,
        hue=None,       # optional. If set to a column name, it colours the data points according to this variable's value
        vars=None,      # list of columns to pair. Columns must be numeric.
        dropna=False    # whether to drop missing values or not
    )
```

### Pairplot for Linear Regression

```python
# Docs: https://seaborn.pydata.org/generated/seaborn.pairplot.html

# data: pandas.DataFrame containing data to be shown.
# indep_vars: list of independent variables (column names)
# dep_vars: list of dependent variables (column names). Usually only one item.
def render_linreg_pairplot(data, indep_vars, dep_vars):
    return sns.pairplot(
        data=data, 
        x_vars=indep_vars, 
        y_vars=dep_vars, 
        kind="reg" # show regression line 
    )
```

### Scatterplot

#### Seaborn
The two methods below do the same thing

```python
# Docs: https://seaborn.pydata.org/generated/seaborn.scatterplot.html

# df: pandas.DataFrame containing data you want to show
def render_scatter(df, xcol, ycol):
    return sns.scatterplot(
        data=df, 
        x=xcol, 
        y=ycol, 
        hue=None # optional. If set to a column name, it colours the data points according to this variable's value
    )
```

```python
# Docs: https://seaborn.pydata.org/generated/seaborn.scatterplot.html

# x: pandas.Series containing x-axis data
# y: pandas.Series containing y-axis data
def render_scatter(x, y):
    return sns.scatterplot(
        x=x,
        y=y,
        hue=None # optional. If set to a column name, it colours the data points according to this variable's value 
    )
```

#### Pure Matplotlib
```python
# Docs: https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.scatter.html

# x: pandas.Series containing x-axis data
# y: pandas.Series containing y-axis data
def render_scatter(x, y):
    return plt.scatter(
        x=x,
        y=y
    )
```

### Boxplot

#### Seaborn
```python
# Docs: https://seaborn.pydata.org/generated/seaborn.boxplot.html
# Docs for the underlying library: 
#   https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.boxplot.html

# data: data to show boxplot for
def render_boxplot(data):
    return sns.boxplot(
        data=data,
        x=None, # optional. Can be set to a column name (must be in data) or a pandas.Series
        y=None, # optional. Can be set to a column name (must be in data) or a pandas.Series
        showfliers=True # whether or not to show the outliers
    )
```

#### Pandas
```python
# Docs: https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.boxplot.html

# data: data to show boxplot for
# columns: names of columns to show in the boxplot
def render_boxplot(data, columns):
    return data.boxplot(
        column=columns,
        by=None,
        showfliers=True, # whether or not to show the outliers
        grid=True        # whether or not to show a grid
    )
```

### Elbow plot (specific for k-means clustering)

```python
# ks: list (or iterable) of all k-values (in order)
# inertias: list of all inertias (in order)
def render_elbow(ks, inertias): 
    plt.plot(ks, inertias, 'bx-')
    plt.xlabel('k')
    plt.ylabel('Inertia')
    plt.title('The Elbow Method showing the inertias for each k') 
```

### ARIMA-plots

#### ACF vs PACF
ACF is short for 'autocorrelation function'. `plot_acf` shows self-correlation for specific lags, where this correlation is calculated using both *direct* and *indirect* correlation information.

PACF is short for 'partial autocorrelation function'. It removes the indirect correlation that is present in `plot_acf` 

Source: [https://machinelearningmastery.com/gentle-introduction-autocorrelation-partial-autocorrelation/](https://machinelearningmastery.com/gentle-introduction-autocorrelation-partial-autocorrelation/)

#### ACF
```python
# Docs: https://www.statsmodels.org/stable/generated/statsmodels.graphics.tsaplots.plot_acf.html

# arma_data: array or time-series of data you wish to show lags of.
def render_plot_acf(arma_data):
    # Imported from statsmodels.graphics.tsaplots (see Libraries)
    return plot_acf(
        arma_data,
        lags=None # optional. Amount of lags (relative correlation points) to show. Defaults to np.arange(len(corr))
    )
```

#### PACF
```python
# Docs: https://www.statsmodels.org/stable/generated/statsmodels.graphics.tsaplots.plot_pacf.html

# arma_data: array or time-series of data you wish to show lags of.
def render_plot_acf(arma_data):
    # Imported from statsmodels.graphics.tsaplots (see Libraries)
    return plot_pacf(
        arma_data,
        lags=None # optional. Amount of lags (relative correlation points) to show. Defaults to np.arange(len(corr))
    )
```

#### ARIMA

```python
# ARIMA imported from statsmodels.tsa.arima_model
# Plot docs: https://www.statsmodels.org/stable/generated/statsmodels.tsa.arima_model.ARMAResults.plot_predict.html
# ARIMA docs: https://www.statsmodels.org/stable/generated/statsmodels.tsa.arima_model.ARIMA.html

# data: pandas.Series to render
# order: (int, int, int) that contains the number of AR (autoregression), differences and MA (moving average) parameters to use
def render_plot_predict(data, order):
    model = ARIMA(data, order=order)
    model = model.fit(disp=0)
    
    model.plot_predict(dynamic=False) 
```

### Decision Trees

```python
# plot_tree imported from sklearn.tree
# Docs: https://scikit-learn.org/stable/modules/generated/sklearn.tree.plot_tree.html

# tree: decision tree to render
# var_names: names of variables the tree's nodes will split on.
def render_dec_tree(tree, var_names):
    plt.show(
        plot_tree(
            decision_tree=tree, # Tree to render 
            feature_names=var_names, # optional. If None, generic names will be used
            max_depth=3, # optional. Max depth to show
            fontsize=8, # optional. Size of font
            filled=True # optional. Whether to show colour in the nodes
        )
    )
```
