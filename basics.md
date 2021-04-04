# Basics

This file contains a collection of code snippets related to the basics of Programming for Data Science. This includes
importing data and data exploration.

## Libraries

The libraries imported are the following:

- numpy for computational tools
- pandas for data manipulation and analysis

```python
import numpy as np
import pandas as pd

# Choose one:
import sqlite3
import sqlalchemy
```

The following snippet is useful when using Jupyter Notebooks.

```jupyterpython
% config IPCompleter.greedy = True
% matplotlib inline
```

## Importing data

### From CSV

Docs: [https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html)

```python
# file_path: string containing the path to the file
# column_names: None or array containing the names of the columns
def import_csv(file_path, column_names):
    df = pd.read_csv(
        file_path,
        names=column_names,  # optional. Specifies the names of all columns (including index)
        index_col=None,  # optional. Specifies index column
        parse_dates=False,  # optional. Specifies which columns to parse as dates
        header='infer',  # optional. Specifies which row(s) to check for column names
        comment=None,  # optional. Specifies prefix for comment-lines
        sep=',',  # optional. Values are separated by a ','
        skipinitialspace=False  # optional. Specifies whether to trim a space after the separator
    )
    return df
```

### From Excel

Docs: [https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_excel.html](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_excel.html)

```python
# file_path: string containing the path to the file
def import_excel(file_path):
    df = pd.read_excel(
        file_path,
        sheet_name=0,  # optional. Name, index or list specifying which sheets to load. If None, loads all sheets.
        header=0,  # optional. Specifies which row(s) to check for column names
        names=None,  # optional. Specifies the names of all columns (including index)
        index_col=None,  # optional. Specifies index column
        usecols=None,  # optional. Specifies which columns to read.
    )
    return df
```

### From Sqlite (using sqlite3)

#### Connecting to the database

```python
con = sqlite3.connect("database.sqlite")
```

#### Executing a SQL query

```python
# query: string containing a SQL-query
def execute_query(query):
    result = con.execute(query)
    return result
```

#### Retrieving all table names

```sql
SELECT name
FROM sqlite_master
WHERE type = 'table'
```

#### Retrieving table's column names

```python
# con: connection to the database
# table_name: string with table name
def get_column_names(con, table_name)
    result = con.execute("SELECT * FROM " + table_name)
    return result
```

#### Retrieving table as pandas DataFrame

```python
# con: connection to the database
# table_name: string with table name
def get_as_dataframe(con, table):
    result = con.execute("SELECT * FROM " + table)
    return pd.DataFrame(
        result,
        columns=list(map(lambda c: c[0], result.description))
    )

```

## Data exploration

### Basic exploration

Docs:

- [DataFrame.head()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.head.html)
- [DataFrame.describe()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.describe.html)
- [DataFrame.info()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.info.html)

```python
# df: pandas.DataFrame you want the metadata for
def print_df_metadata(df):
    print("HEAD:")
    print(df.head(5))  # first five records in dataframe
    print("DESCRIBE:")
    print(df.describe())  # description of dataframe, including count and several centre-metrics
    print("INFO:")
    print(df.info())  # dataframe-metadata: index, column names & their types, null vs non-null 
```

## DataFrame access

TODO

## DataFrame manipulation

This section covers the manipulation (changing) of pandas DataFrames.

### Removing unwanted columns using [DataFrame.drop()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.drop.html)

```python
# df: pandas.DataFrame you want to remove the columns for
# column_names: list of column names to drop
def remove_columns(df, column_names):
    return df.drop(
        columns=column_names,  # Columns to remove
        inplace=False  # optional. If False, returns a copy without the removed columns, but does not change df itself. 
    )
```

### Removing missing/empty values using [DataFrame.dropna()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.dropna.html)

```python
def remove_missing(df):
    return df.dropna(
        axis='index',  # optional. 'index' to remove rows, 'columns' to remove columns
        how='any',  # optional. 'any' to remove if at least one is empty, 'all' to remove if all values are empty
        inplace=False  # optional. If False, returns a copy without the removed columns, but does not change df itself.
    )
```

### Creating a new column with a constant value

```python

# df: pandas.DataFrame you want to add a column to
# column_name: name of the column to add
# value: value to set for all records
def create_column(df, column_name, value):
    df[column_name] = value
    return df
```

### Creating a new column of increasing integers

```python
# df: pandas.DataFrame you want to add a column to
# column_name: name of the column to add
def create_int_column(df, column_name):
    df[column_name] = list(range(len(df)))  # Creates a list from a range of length len(df): 0 to length-1 
    return df
```

### Merging/concatenating DataFrames

#### Using [pandas.concat()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.concat.html)

```python
# df1: pandas.DataFrame to merge with df2
# df2: pandas.DataFrame to merge with df1
def concat_two_dfs(df1, df2):
    return pd.concat(
        [df1, df2],  # a list (or similar) of dataframes to merge
        axis='index',  # optional. 'index' for merging rows, 'columns' for merging columns.
        ignore_index=False  # optional. If True, will discard the original indices and set the index to 0 through N-1
    )
```

#### Using [DataFrame.append()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.append.html)

```python
# df1: pandas.DataFrame
# df2: pandas.DataFrame to append to df1
def concat_two_dfs(df1, df2):
    return df1.append(
        df2,  # the 'other' dataframe
        axis='index',  # optional. 'index' for merging rows, 'columns' for merging columns.
        ignore_index=False  # optional. If True, will discard the original indices and set the index to 0 through N-1
    )
```

#### Using [DataFrame.merge()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.merge.html)

```python
# df1: pandas.DataFrame to merge with df2
# df2: pandas.DataFrame to merge with df1
def concat_two_dfs(df1, df2):
    return df1.merge(
        df2,  # the 'other' dataframe
        on=None,  # optional. If set to a column name, merge will try to find this column name and use it to join on  
        how='inner',  # optional. Determines the way to merge the DataFrames
        left_index=False,  # optional. If True, will use df1's index to join rather than a column
        right_index=False  # optional. If True, will use df2's index to join rather than a column
    )
```

#### Using [DataFrame.join()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.join.html)

```python
# df1: pandas.DataFrame to join on df2
# df2: pandas.DataFrame to join on df1
def concat_two_dfs(df1, df2):
    return df1.join(
        df2,  # the 'other' dataframe
        on=None,  # optional. If set to a column name, merge will try to find this column name and use it to join on  
        how='inner',  # optional. Determines the way to merge the DataFrames
        lsuffix='',  # optional. Sets the suffix for df1's overlapping columns 
        rsuffix='',  # optional. Sets the suffix for df2's overlapping columns 
    )
```
