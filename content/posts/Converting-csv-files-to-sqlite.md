---
title: "Converting CSV files to sqlite"
date: 2019-06-06T12:43:50+05:30
description: guide on how to convert large csv files to sqlite file
menu:
  sidebar:
    name: Converting CSV files to sqlite
    identifier: csv2sqlite
    weight: 10
---
Sometimes we only have huge number of .csv files. To load these files into memory can take some time.

Even if you decide to load it with some spreadsheet software such as Excel, there are limitations. First of all, your system will notice a surge in RAM consumption and running applications will take a hit. The maximum number of rows Excel can handle is 1,048,576. Imagine loading 10 such files at once!
[Source](https://support.office.com/en-us/article/excel-specifications-and-limits-1672b34d-7043-467e-8e27-269d656771c3)

An efficient way would be to convert these csv files to a database dump, query and select data of interest and load it in our workspace. The approach of converting csv to sqlite does have a overhead but addresses the limitations of memory concerns and gives a push to processing power. We can programmatically create a database dump instead of loading it in actual database server. 

Please note, if the volume of your data is large, I mean really large, then it is recommended to store, analyze and pre-process your data in a DBMS. 


```python
import pandas as pd
import sqlite3
from pandas.io import sql
import subprocess
```

Let us generate a sample csv file that has time series data generated at random letters and digits


```python
import numpy as np
import string
```


```python
col_a = pd.date_range('2000-01-01', periods=1048500, freq='H')
col_b = np.random.choice(list(string.ascii_lowercase),  size=len(col_a))
col_c = np.random.randint(0, 10, 1048500)
```


```python
df = pd.DataFrame({'Date': col_a, "str": col_b, "int": col_c}, index=None)
```


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>str</th>
      <th>int</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-01-01 00:00:00</td>
      <td>m</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-01 01:00:00</td>
      <td>r</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-01 02:00:00</td>
      <td>v</td>
      <td>6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000-01-01 03:00:00</td>
      <td>v</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000-01-01 04:00:00</td>
      <td>f</td>
      <td>7</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Size of the dataframe in MB
df.memory_usage().sum()/(1024)
```




    20478.59375




```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1048500 entries, 0 to 1048499
    Data columns (total 3 columns):
    Date    1048500 non-null datetime64[ns]
    str     1048500 non-null object
    int     1048500 non-null int32
    dtypes: datetime64[ns](1), int32(1), object(1)
    memory usage: 20.0+ MB
    

So we have a dataframe of size 20MB. I am creating this sample file based on just 3 columns but in reality you maybe presented with files that have 20+ columns and file size spanning hundreds of MBs or in GBs!. In such scenarios, you can then use the code given below.


```python
# Let us write this dataframe to a file
df.to_csv('sample_file.csv', encoding='utf-8', index=False)
```

When we check the size of this file, it turns out that it is ~24MB. 


```python
# In and output file paths
in_csv = r'sample_file.csv'
out_sqlite = 'my.sqlite'
```


```python
sample = pd.read_csv('sample_file.csv')
```


```python
sample.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>str</th>
      <th>int</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-01-01 00:00:00</td>
      <td>m</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-01 01:00:00</td>
      <td>r</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-01 02:00:00</td>
      <td>v</td>
      <td>6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000-01-01 03:00:00</td>
      <td>v</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000-01-01 04:00:00</td>
      <td>f</td>
      <td>7</td>
    </tr>
  </tbody>
</table>
</div>




```python
[sample.columns]
```




    [Index(['Date', 'str', 'int'], dtype='object')]




```python
table_name = 'sample_table' # name for the SQLite database table
chunksize = 100000 # number of lines to process at each iteration

# columns that should be read from the CSV file
columns = sample.columns

```


```python
sample.columns
```




    Index(['Date', 'str', 'int'], dtype='object')




```python
columns
```




    Index(['Date', 'str', 'int'], dtype='object')




```python
# columns that should be read from the CSV file
columns = ['Date', 'str', 'int']
```


```python
columns
```




    ['Date', 'str', 'int']




```python
def iterate(file_name):
    with open(file_name) as f:
        for i, line in enumerate(f):
            pass
        return i + 1
```


```python
nlines= iterate('./sample_file.csv')
```


```python
nlines
```




    1048501




```python
# connect to database
cnx = sqlite3.connect(out_sqlite)
```


```python
# Iteratively read CSV and dump lines into the SQLite table
for i in range(0, nlines, chunksize):  # change 0 -> 1 if your csv file contains a column header
    
    df = pd.read_csv(in_csv,  
            header=None,  # no header, define column header manually later
            nrows=chunksize, # number of rows to read at each iteration
            skiprows=i)   # skip rows that were already read
    
    # columns to read        
    df.columns = columns

    sql.to_sql(df, 
                name=table_name, 
                con=cnx, 
                index=False, # don't use CSV file index
                index_label='molecule_id', # use a unique column from DataFrame as index
                if_exists='append') 
cnx.close()    
```


```python
conn = sqlite3.connect('my.sqlite')
```


```python
pd.read_sql_query("""Select * from sample_table where int != 0 LIMIT 10""", conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>str</th>
      <th>int</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Date</td>
      <td>str</td>
      <td>int</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-01 00:00:00</td>
      <td>m</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-01 01:00:00</td>
      <td>r</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000-01-01 02:00:00</td>
      <td>v</td>
      <td>6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000-01-01 03:00:00</td>
      <td>v</td>
      <td>4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2000-01-01 04:00:00</td>
      <td>f</td>
      <td>7</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2000-01-01 05:00:00</td>
      <td>l</td>
      <td>3</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2000-01-01 06:00:00</td>
      <td>p</td>
      <td>8</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2000-01-01 07:00:00</td>
      <td>c</td>
      <td>9</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2000-01-01 08:00:00</td>
      <td>e</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Code Reference: http://coderscrowd.com/app/public/codes/view/316


```

