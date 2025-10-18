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
- View data:
  - `df.head()` - first 5 rows  
  - `df.tail()` - last 5 rows  
  - `df.info()` - summary of columns & types  
  - `df.describe()` - quick stats
- Access data:
  - `df['colName']` / `df.colName` - single column (Series)
  - `df[['col1','col2']]` - multiple columns (DataFrame)
  - `df.loc[row_label, col_label]` - by labels  
  - `df.iloc[row_index, col_index]` - by numeric index
- Index control:
  - `df.set_index('Column', inplace=True)` - make column the index
  - `df.reset_index(inplace=True)` - restore default numeric index
  - `df.index` / `df.columns` - view or modify labels
