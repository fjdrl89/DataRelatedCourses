# Visualize Data with Python Skill Path

## 1. Matplotlib Fundamentals

- A Jupyter Notebook is a web-based environment for writing code and displaying results.
- Each Jupyter Notebook consists of a sequence of cells.
- `matplotlib` and `seaborn` are Python libraries that are specifically designed to make data visualizations.
- To make data viz with `matplotlib`, we import just one module (called `pyplot`) from the library, and shorten it to `plt`.
- `seaborn` is conventionally imported as `sns`.

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

- 

