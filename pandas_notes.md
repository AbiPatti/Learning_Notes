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
- `df.agg(func)` → apply one or more aggregation functions  
  - Example: `df.agg(['mean', 'sum'])`
- `df['col'].agg(func)` → apply to a single column  
  - Example: `df['Age'].agg(['min', 'max'])`
- `df.groupby(col_name, axis=0)` → group rows by column(s), axis=0 will group rows
  - Example: `df.groupby('City')['Sales'].sum()` - total sales per city

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

### Cleaning
- `df.drop_duplicates(subset="col_name", keep='first', inplace=False)` → remove duplicate rows (subset can be a column or a list of columns)
  - Example: `df.drop_duplicates(subset=['Name','Age'], keep='last')`
- `df['col'].value_counts(normalize=False, sort=True, ascending=False)` → count occurrences of unique values (normalize=True will show proportion of each value in the total)
  - Example: `df['City'].value_counts(normalize=True)`
