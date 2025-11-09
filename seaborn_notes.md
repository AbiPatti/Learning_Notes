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

## Common Parameters
- `data` → DataFrame to pull from  
- `x`, `y` → column names  
- `hue` → group/color by a categorical variable (acts as third variable)
- `style`, `size` → vary marker or size by category  
- `palette` → color scheme  
- `title` → set using `plt.title("...")`
