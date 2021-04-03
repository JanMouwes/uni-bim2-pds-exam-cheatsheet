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

