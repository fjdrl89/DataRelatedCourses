# Visualize Data with Python Skill Path

## 1. Matplotlib Fundamentals

### 1.1 Make a Line Chart

#### 1.1.1 Getting Started with Jupyter Notebooks

- A Jupyter Notebook is a web-based environment for writing code and displaying results.
- Each Jupyter Notebook consists of a sequence of cells.
- `matplotlib` and `seaborn` are Python libraries that are specifically designed to make data visualizations.
- To make data viz with `matplotlib`, we import just one module (called `pyplot`) from the library, and shorten it to `plt`.
- `seaborn` is conventionally imported as `sns`.

#### 1.1.2 Anatomy of matplotlib code

- Matplotlib makes it easy to turn tabular data (data in a spreadsheet, table, or .csv) into basic visualizations.
- Here are five of the most commonly used charts and the functions we call to make them:
  * **line chart**: shows continuous change, often used to measure change over time - `plt.plot()`
  * **scatterplot**: uses position to show the relationship, or correlation, between two numeric values - `plt.scatter()`
  * **bar chart**: uses bar height to compare a measure between categorical variables - `plt.bar()`
  * **pie chart**: shows us the breakdown of a whole into its parts - `plt.pie()`
  * **histogram**: shows how one kind of data is distributed - `plt.hist()`

- In addition to graph functions, matplotlib also has **general functions**.
  * These functions allow us to change general aspects of the graph, like its title, legend, or axis scaling and labels.
  * Each of them would be added to a graph after the graph is made.

- Here are some common general functions:
  * Add a title ->	`plt.title()`
  * Add a legend	->	`plt.legend()`
  * Add axis labels	->	`plt.ylabel()`, `plt.xlabel()`
  * Adjust axes	->	`plt.xscale()`, `plt.yticks()`
  * Print graph	->	`plt.show()`

#### 1.1.3 Make a Line a Chart

- The function for a line chart is `.plot()`. As always, we need to call matplotlib (as `plt`) first, so to make a line chart we’ll use `plt.plot()`. The line graph function has parameters for x and y data: `plt.plot(x, y)`.
  * `x` is the data for the x-axis – in a line chart, this is very often a measure of time
  * `y` is the data for y-axis – generally things we want to view over time, like interest rates, the price of a slice of pizza, or population size in a country
- Depending on our data setup, x and y could be written directly into the function, pulled from a named array, or a column of a dataframe. 
- To add multiple lines to a chart, we simply call `plt.plot()` again with different y data. Matplotlib will keep adding lines to the same instance of `plt` until we clear the plot using the “clear the figure” command: `plt.clf()`.

```python
# import matplotlib and pandas
from matplotlib import pyplot as plt
import pandas as pd

# load dataset
line_data = pd.read_csv('2020-monthly-avg-temps-f.csv')
line_data.head()
```

```python
# display the charts correctly
%matplotlib inline
plt.rcParams['figure.figsize'] = (5, 3)
plt.rcParams['figure.dpi'] = 75
```

```python
## make a line chart from direct data
plt.plot(['Mon', 'Tues', 'Weds', 'Thurs', 'Fri', 'Sat', 'Sun'], [540, 500, 490, 590, 680, 710, 610])
plt.show()
```

```python
# make a line chart using named arrays
day = ['Mon', 'Tues', 'Weds', 'Thurs', 'Fri', 'Sat', 'Sun']
sales_totals = [540, 500, 490, 590, 680, 710, 610]
plt.plot(day, sales_totals)
plt.show()
```

```python
# make a line chart with columns from a dataframe
df = pd.read_csv('restaurant_data.csv')
plt.plot(df.day, df.sales_totals)
plt.show()
```

#### 1.1.4 Change line color

- To change the line color, we can simply pass an argument to the color parameter in .plot(). This argument is always a string, and it can be one of three options:
  * single-letter color code (8 basic colors)
  * full string color (1000+ recognized colors)
  * hexadecimal code (any web color)

```python
# single-letter color code 
plt.plot(x, y, color='r')

# full string
plt.plot(x, y, color='red')

# hex code
plt.plot(x, y, color='#FF0000')
```

- In addition to color, `.plot()` will take the parameters `linewidth` and `linestyle`.
  * `linewidth` takes a number that changes the thickness of the line.
  * `linestyle` takes a string: dotted, dashed, or solid.

```python
# dashed red line
plt.plot(x, y, color='r', linestyle='dashed')

# thick, solid yellow line
plt.plot(x, y, color='yellow', linewidth=5)

# thin, dotted white line
plt.plot(x, y, color='#FFFFFF', linewidth=0.5, linestyle='dotted')
```

- To put a title on any graph, we simply type the title as a string into `plt.title()`.
- To make a legend, we use the general function `plt.legend()`.

```python
plt.plot(x, y, color='red', label='Line Y')
plt.plot(x, z, color='green', label='Line Z')
plt.legend()
```

- To position the legend, we can use the `plt.legend()` parameter `bbox_to_anchor`, which takes an `(x,y)` coordinate pair as its argument.
  * For the x-coordinate, 0 is the left edge of the graph and 1 is the right edge.
  * For the y-coordinate, 0 is the bottom edge of the graph and 1 is the top.
  * For example, `plt.legend(bbox_to_anchor = (1, 0.5))` places the legend to the right of the figure (all the way at 1 on the x-coordinate), in the middle of the vertical space (0.5 on the y-coordinate).

```python
plt.plot(x, y, color='red', label='Line Y')
plt.plot(x, z, color='green', label='Line Z')
plt.legend(bbox_to_anchor = (1, 0.5))
```

- Axis labels are modified using a general function, so can be easily added onto any graph with an x or y-axis by using the `plt.xlabel()` and `plt.ylabel()` functions.
- Tick labels are also modified using a general function: in this case, we’ll use `plt.tick_params()` (short for “tick parameters”) to change the rotation of the tick labels.
- This function takes many different parameters to adjust tick marks and tick labels: position above or below the axes, length, width, and color of tick marks, as well as the font size, color, and rotation of tick labels, among other parameters.
- We can also use `.tick_params()` to create gridlines.

```python
plt.tick_params(axis='x', direction='out', color='red', labelsize='large', labelcolor='purple', labelrotation=30)
```

- To export a chart, we just call the function `plt.savefig()`, giving the filename we want as an argument.

```python
plt.savefig('my_lineplot.png')
```

- Plots will default to `.png` format, but we can also specify the format as a parameter and save a plot as a `.jpeg`, `.pdf`, or `.svg` filetype, among others.
- Two other helpful parameters are `dpi` and `bbox_inches`:
  * `dpi` allows us to set the resolution: the number of pixels in the image, or essentially, how big it is. This works the same whether the visualization is a square or a rectangle, which is convenient.
  * `bbox_inches = 'tight'` ensures that our legend, tick labels, and axes aren’t cut off. These elements exist outside the graph figure, but inside the bounding box or bbox.

```python
plt.savefig('my_lineplot.png', dpi=128, bbox_inches='tight')
```

### 1.2 PROJECT: Make a Line Chart for Research

You’re part of a meteorological research team, the Petrichor lab. Your team’s goal is to create data visualizations that help to compare different climate factors around the world.

The team has already identified sites of interest around the globe:
- Alice Springs, Australia
- Kodiak, Alaska, United States
- London, United Kindom
- Quito, Ecuador
- Samarqand, Uzbekistan
- Windhoek, Namibia

These sites vary widely in their monthly average temperatures, which the team has already explored.

The next step is to compare precipitation data in the same locations. Here’s some important background on the data, some of which will impact how the data is visualized:
- Locations will be grouped by locale (equator, northern, or southern hemisphere) using similar line colors.
- Locations will be differentiated by precipitation type (snow and rain, or rain alone) using line style.


1. Import pandas and matplotlib to get started.
```python
from matplotlib import pyplot as plt
import pandas as pd
```
```python
%matplotlib inline
```

2. Load in the dataset 2020 monthly avg precipitation around the world - imperial.csv using pandas. Check out the first few lines to make sure you know how the data is structured.¶
```python
data = pd.read_csv("2020 monthly avg precipitation around the world - imperial.csv")
# data.head()
```

3. Write the code to make a line chart with months on the x-axis and Alice Springs precipitation data on the y-axis, then print the plot.
```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```


```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```


```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```python


```

```
