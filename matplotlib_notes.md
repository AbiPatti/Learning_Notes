# MatPlotLib Notes
### Abinash Patti

## Basics
- `import matplotlib.pyplot as plt`
- Used for plotting and visualizing data
- Most common functions: `plt.plot()`, `plt.scatter()`, `plt.bar()`, `plt.hist()`
- `plt.show()` - displays the figure
- `plt.clf()` - cleans up the plot to start fresh
- Each plot has a **Figure** (canvas) and **Axes** (the actual plot area)
- Can customize labels, titles, colors, and styles

## Plot Types & Usage
- `plt.plot(x, y)` - line plot
- `plt.scatter(x, y)` - scatter plot
- `plt.bar(x, height)` - bar chart
- `plt.hist(data, bins=10)` - histogram where bins means how many points in a bar (more bins = finer detail, less bins = overall picture)
- `plt.imshow(img)` - show image data (2D or RGB)
- `plt.show()` - display all plots

## Customization
- `plt.title("Title")`, `plt.xlabel("X")`, `plt.ylabel("Y")`
- `plt.legend()` - show legend for labeled data
- `plt.grid(True)` - show grid lines
- `plt.xlim()`, `plt.ylim()` - set axis limits
- `plt.xticks(ticks, labels)` / `plt.yticks(ticks, labels)` - set positions and labels for axis ticks, where ticks is a list of numbers (intervals) and labels is an optional list of strings to label the ticks (ex. plt.yticks([2,4,6], ['2 thousand', '4 thousand', '5 thousand']) 
- `plt.xscale("log")`, `plt.yscale("log")` - change axis scale (e.g., "linear", "log", "symlog")
- `plt.style.use("ggplot")` - change overall look
