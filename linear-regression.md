# Linear Regression

## Libraries

```python
# scikit-learn's implementation
from sklearn.linear_model import LinearRegression
```

```python
# Function to split given data into train sets and test sets
from sklearn.model_selection import train_test_split
```

## The general case using [sklearn's LinearRegression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)

[LinearRegression.fit()](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html#sklearn.linear_model.LinearRegression.fit)

```python
# x_values: pandas.DataFrame in the shape (number of samples, number of features)
# y_values: pandas.DataFrame in the shape (number of samples, number of features)
def create_lin_reg(x_values, y_values):
    return LinearRegression().fit(x_values, y_values)
```

## Single Linear Regression using [sklearn's LinearRegression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)

[LinearRegression.fit()](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html#sklearn.linear_model.LinearRegression.fit)

```python
# data: pandas.DataFrame from which to extract data
# x_column_name: string with independent variable name
# y_column_name: string with dependent variable name
def create_lin_reg(data, x_column_name, y_column_name):
    indep_vars = [x_column_name]  # singleton array
    dep_var = y_column_name

    model = LinearRegression()
    return model.fit(data[indep_vars], data[dep_var])
```

## Multiple Linear Regression

```python
# data: pandas.DataFrame from which to extract data
# indep_vars: array of strings with independent variable names
# dep_var: string with dependent variable name
def create_lin_reg(data, indep_vars, dep_var):
    model = LinearRegression()
    return model.fit(data[indep_vars], data[dep_var])
```

## Linear Regression quality (score)

### R^2 score using [LinearRegression.score()](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html#sklearn.linear_model.LinearRegression.score)

```python
# model: LinearRegression object that has been fit
# x_test: pandas.DataFrame in the shape (number of samples, number of features)
# y_test: pandas.DataFrame in the shape (number of samples, number of features)
def model_score(model, x_test, y_test):
    return model.score(x_test, y_test)
```

## Getting test data using [sklearn's train_test_split](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html)

```python
def split_data(data, indep_vars, dep_var):
    x_train, x_test, y_train, y_test = train_test_split(
        data[indep_vars],
        data[dep_var],
        test_size=0.25,  # percentage of data to be used for the test set
        random_state=None  # disable getting different values each time by setting this to anything
    )

    return x_train, x_test, y_train, y_test  # returns a 4-fold tuple, just like train_test_split does
```

## Full example

```python
def linear_regression(data, indep_vars, dep_var):
    test_size = 0.25
    random_state = 1

    # Split the data
    x_train, x_test, y_train, y_test = train_test_split(
        data[indep_vars],
        data[dep_var],
        test_size=test_size,  # percentage of data to be used for the test set
        random_state=random_state  # disable getting different values each time by setting this to anything
    )

    # Create a model 
    model = LinearRegression()
    model = model.fit(x_train, y_train)

    # Get the score
    score = model.score(x_test, y_test)

    return model, score  # tuple with the model and the score
```