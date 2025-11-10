# Seaborn Notes
### Abinash Patti

## Basics
- `import seaborn as sns`
- Built on Matplotlib but easier for statistical plots  
- Works directly with pandas DataFrames (`data=` parameter)

## Common Plots
- `sns.scatterplot(x, y, data=df)` → relationships between variables  
- `sns.lineplot(x, y, data=df)` → trends over continuous data  
- `sns.barplot(x, y, data=df)` → mean (with error bars)  
- `sns.countplot(x, data=df)` → counts per category (no y needed)  
- `sns.boxplot(x, y, data=df)` → distribution + outliers  
- `sns.histplot(x, data=df, bins=20)` → histogram  
- `sns.heatmap(df.corr(), annot=True)` → correlation matrix
- `sns.relplot(x, y, data=df, kind='scatter'/'line', hue=None, col=None)` → high-level wrapper for `scatterplot` & `lineplot`, creates multiple plots
  - `col` / `row` → make multiple subplots (facets)
  - Example: `sns.relplot(x='year', y='gdp', data=df, kind='line', col='country')` will show gdp across years, one plot per country

## Common Parameters
- `data` → DataFrame to pull from  
- `x`, `y` → column names  
- `hue` → group/color by a categorical variable (acts as third variable)
- `style`, `size` → vary marker or size by category  
- `palette` → color scheme
- `title` → set using `plt.title("...")`
