# Pandas Notes
### Abinash Patti

## Basics
- `import pandas as pd`
- Built on NumPy — adds labeled data (rows & columns)
- Two main objects:
  - `Series` → 1D labeled array  
  - `DataFrame` → 2D table (like an Excel sheet)
- Designed for data cleaning, analysis, and exploration

## DataFrames
- 2D table (like an Excel sheet)
- Create from dict: `pd.DataFrame({'A':[1,2], 'B':[3,4]})`
- Create from list of lists: `pd.DataFrame([[1,2],[3,4]], columns=['A','B'])`
- Create from NumPy array: `pd.DataFrame(np_array, columns=[...])`
- Create from CSV/JSON/Excel: `pd.read_csv('file.csv')` (Change csv to json or excel if using that)
  - Save to CSV/JSON/Excel: `df.to_csv('file.csv', index=False)` (Change csv to json or excel if using that)

### **View data**
  - `df.head()` - first 5 rows  
  - `df.tail()` - last 5 rows  
  - `df.info()` - summary of columns & types  
  - `df.describe()` - quick stats

### **Access data**
  - `df['colName']` / `df.colName` - single column (Series)
  - `df[['col1','col2']]` - multiple columns (DataFrame)
  - `df.loc[row_label, col_label]` - by labels  
  - `df.iloc[row_index, col_index]` - by numeric index
  - **Basic slicing**:  
    - Rows by position → `df[0:5]` (first 5 rows)  
    - Rows by labels → `df.loc['a':'d']`  
    - Columns → `df[['col1', 'col2']]`
    - Accessing **Multiple Levels (Hierarchy)** → `df.loc[('USA', 'NY'):('USA', 'TX')]`
    - Slice both rows & columns with `.loc[row_start:row_end, col_start:col_end]`

### Cleaning
- `df.drop_duplicates(subset="col_name", keep='first', inplace=False)` → remove duplicate rows (subset can be a column or a list of columns)
  - Example: `df.drop_duplicates(subset=['Name','Age'], keep='last')`
- `df['col'].value_counts(normalize=False, sort=True, ascending=False)` → count occurrences of unique values (normalize=True will show proportion of each value in the total)
  - Example: `df['City'].value_counts(normalize=True)`
- `df.dropna(subset=None, how='any', inplace=False)` → remove rows with missing values (or columns with `axis=1`)
- `df.isna()` → returns DataFrame of True/False where values are missing
- `df.any(axis=0)` → True if any value along axis is True (use like `df.isna().any` to check missing data, returns True for columns that have NA) 
- `df.fillna(value, inplace=False)` → replace missing values with given value (e.g. `0`, `'unknown'`, or method='ffill')  

### **Filtering**
  - `df[condition]` → filter rows matching a condition
    - `condition`: boolean expression like `df['Age'] > 30`
  - Combine conditions:
    - `&` (and), `|` (or), `~` (not)
    - Example: `df[(df['Age'] > 30) & (df['City'] == 'NY')]`
  - `df.query("Age > 30 and City == 'NY'")` → same with query syntax
  - `df.isin([...])` → check if values are in a list  
    - Example: `df[df['City'].isin(['NY','LA'])]`
  - `df.between(a, b)` → check if values fall in range  
    - Example: `df[df['Age'].between(20,30)]`
  - `df.isnull()` / `df.notnull()` → filter missing or non-missing rows

### Aggregation/Grouping
- `df.agg(func)` → apply an aggregation function or a list of them (can use custom/user-defined functions) 
  - Example: `df.agg(['mean', 'sum'])`
- `df['col'].agg(func)` → apply to a single column  
  - Example: `df['Age'].agg(['min', 'max'])`
- `df.groupby(col_name, axis=0)` → group rows by column(s), axis=0 will group rows
  - Example: `df.groupby('City')['Sales'].sum()` - total sales per city

- `df.pivot_table(values=col_name, index=None, columns=None, aggfunc='mean', fill_value=None, margins=False)` → summarize data in a table by column or list of columns to aggregate (`values`), `index` is rows of the pivot table, `columns` are the columns of the pivot table, `aggfunc` is the function or list of functions to apply, `fill_value` is placeholder for the missing values, `margins` show row+col totals
  - Example: `df.pivot_table(values='Sales', index='City', columns='Month', aggfunc='sum', fill_value=0, margins=True)`
- `df['col'].idxmax()` / `idxmin()` → returns the index of the max/min value in a column  
  - Example: `df.loc[df['col'].idxmax()]` → row with highest value

**Joining Data**

![](https://i.sstatic.net/hMKKt.jpg)
- `pd.merge(df1, df2, on='key', how='inner', suffixes=('_left', '_right'))` → combine DataFrames on common column
  - `on`: columns to match (**only combine rows where value in this column is same on both sides**), can use `left_on` and `right_on` if column name is different in each table ('id' and 'movie_id')
  - `how`: type of join → `'inner'`, `'left'`, `'right'`, `'outer'`  
  - `suffixes`: rename overlapping column names to avoid conflicts  
  - Example: `pd.merge(df_sales, df_customers, on='CustomerID', how='inner', suffixes=('_sale', '_cust'))`

  - `pd.merge_ordered(df1, df2, on="col", fill_method=None, how="outer")` → like merge but keeps sort order; great for time-series  
    - `fill_method='ffill'` → forward fills missing values (carry last known); only in merge_ordered, not normal merge


- `pd.concat([df1, df2], axis=0, ignore_index=True, keys=None, join='outer')` → combine DataFrames along rows (`axis=0`) or columns (`axis=1`)
  - `ignore_index=True` → reset index in result  
  - `keys` → adds hierarchical index labels to show which original table those rows came from (e.g. dataset source)  
  - `join='outer/inner/left/right'` → what type of join (rows to keep)

### **Index control**
- `df.set_index('Column', inplace=True)` - make column the index
- `df.reset_index(inplace=True)` - restore default numeric index
- `df.index` / `df.columns` - view or modify labels
  
### **Sorting & Ordering**
- `df.sort_values(by='col')` → sort rows by a column (ascending by default)
- `by`: column name(s) to sort by (`'col'` or `['col1', 'col2']`)
- `ascending=False` → sort descending
- `inplace=True` → modify existing DataFrame
- `ignore_index=True` → reset index after sorting
