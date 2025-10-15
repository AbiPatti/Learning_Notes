# MatPlotLib Notes
### Abinash Patti

## Basics
- `import matplotlib.pyplot as plt`
- Used for plotting and visualizing data
- Most common functions: `plt.plot()`, `plt.scatter()`, `plt.bar()`, `plt.hist()`
- `plt.show()` displays the figure
- Each plot has a **Figure** (canvas) and **Axes** (the actual plot area)
- Can customize labels, titles, colors, and styles

## Plot Types & Usage
- `plt.plot(x, y)` - line plot
- `plt.scatter(x, y)` - scatter plot
- `plt.bar(x, height)` - bar chart
- `plt.hist(data, bins=10)` - histogram
- `plt.imshow(img)` - show image data (2D or RGB)
- `plt.show()` - display all plots

## Customization
- `plt.title("Title")`, `plt.xlabel("X")`, `plt.ylabel("Y")`
- `plt.legend()` - show legend for labeled data
- `plt.grid(True)` - show grid lines
- `plt.xlim()`, `plt.ylim()` - set axis limits
- `plt.style.use("ggplot")` - change overall look
