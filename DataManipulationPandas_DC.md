# Data Manipulation with pandas

This repository contains notes, examples, and exercises for the "Data Manipulation with pandas" course on DataCamp, taught by Richie Cotton. The primary goal is to learn how to explore, clean, transform, and aggregate data using the pandas library in Python.

## Why pandas?

Pandas is the most popular and powerful library for data analysis and manipulation in Python.

* **Foundation:** It is built on top of NumPy (for high-performance numerical computation) and Matplotlib (for data visualization). This NumPy foundation is what gives pandas its speed: operations are "vectorized," meaning they are applied to entire arrays at once (often in compiled C code) rather than one element at a time in a slow Python `for` loop.

* **Core Data Structure:** It introduces the `DataFrame`, a 2D labeled data structure. This is the key concept:

  * **2D:** It has rows and columns, like a spreadsheet or SQL table.

  * **Labeled:** Both the rows (the **`index`**) and the columns have labels. This makes data selection and alignment incredibly intuitive and powerful, as you can reference data by meaningful names, not just integer positions.

  * **Flexible:** Columns can be of different data types (e.g., `int`, `float`, `string`, `datetime`), making it a perfect tool for real-world, heterogeneous data.

* **Zen of Python:** The library's design attempts to follow the "Zen of Python" principle: "There should be one—and preferably only one—obvious way to do it." While pandas sometimes offers multiple ways to do one thing (e.g., subsetting), this principle guides its core design.

## Chapter 1: Transforming DataFrames

This chapter focuses on the fundamentals of the DataFrame object. We will learn how to inspect, sort, subset, and create new columns.

### 1.1. Inspecting Your Data

Before you can work with data, you must understand its structure. Always start by inspecting your DataFrame. This "data diagnostics" step is arguably the most important.

* **`import pandas as pd`**: The universal community convention for importing the library.

* **`.head()`**: Returns the first 5 rows of the DataFrame. This is the quickest way to get a "feel" for your data and see examples of values in each column.

  * **Related Method:** `.tail()` returns the *last* 5 rows, which can be useful for checking data import completeness or spotting summary rows at the end of a file.

```python
# Load your data into a DataFrame named 'dogs'
# print(dogs.head())
# print(dogs.tail())
```

* **`.info()`**: Provides a concise summary of the DataFrame. This is **critical** for:
  * **Checking `Dtype`:** Identifies the data type of each column (e.g., `int64`, `float64`, `datetime64`, `object`).
  * **Finding Missing Values:** Compare the `Non-Null Count` to the total number of entries (shown in the `RangeIndex`). If the count is lower, you have `NaN` (Not a Number) values.
  * **Spotting Red Flags:** The `object` dtype is a red flag. It usually means the column contains strings, but it can *also* mean it has "mixed types" (e.g., numbers and strings in the same column), which will cause errors during analysis.
  * **Reviewing Memory Usage:** Helps you understand how much space your DataFrame is occupying.

```python
# print(dogs.info())
# <class 'pandas.core.frame.DataFrame'>
# RangeIndex: 7 entries, 0 to 6
# Data columns (total 6 columns):
# #   Column           Non-Null Count  Dtype
# ---  ------           --------------  -----
# 0   name             7 non-null      object
# 1   breed            7 non-null      object
# 2   color            7 non-null      object
# 3   height_cm        7 non-null      int64
# 4   weight_kg        7 non-null      int64
# 5   date_of_birth    7 non-null      object  <- Pro-tip: This should be datetime!
# dtypes: int64(2), object(4)
# memory usage: 464.0+ bytes
```

* **`.shape`**: An *attribute* (not a method, so no `()`) that returns a tuple of `(rows, columns)`. It's a quick way to know the dimensions of your data.

```python
# print(dogs.shape)
# (7, 6)
```

* **`.describe()`**: Generates descriptive statistics for the **numerical columns** by default (it ignores `object` and `datetime` columns).
* **Pro-tip:** Use `df.describe(include='object')` to get summaries for categorical/string columns. This will show:
  * `count`: Number of non-null entries.
  * `unique`: Number of unique values.
  * `top`: The most frequent value (the "mode").
  * `freq`: The frequency of the `top` value.

* You can also use `df.describe(include='all')` to get all statistics (it will show `NaN` for stats that don't apply, like `mean` for an `object` column).

```python
# Describes numeric columns 'height_cm' and 'weight_kg'
# print(dogs.describe())

# Describes object columns 'name', 'breed', 'color', 'date_of_birth'
# print(dogs.describe(include='object'))
# name     breed  color date_of_birth
# count             7         7      7             7
# unique            7         6      5             7
# top     Bella  Labrador  Brown  2013-07-01
# freq              1         2      2             1
```

### 1.2. Components of a DataFrame

A DataFrame is composed of three main parts:

* **`.values`**: The data itself, returned as a NumPy array. This is where the raw data lives.
* **`.columns`**: The column labels (an `Index` object).
* **`.index`**: The row labels (also an `Index` object). By default, this is a `RangeIndex` (0, 1, 2...), but it can be set to any meaningful values (e.g., dates, customer IDs), which unlocks powerful indexing features.

```python
# print(dogs.values)
# [['Bella' 'Labrador' 'Brown' 56 24 '2013-07-01']
# ...
# ['Bernie' 'St. Bernard 'White' 77 74 '2018-02-27']]

# print(dogs.columns)
# Index(['name', 'breed', 'color', 'height_cm', 'weight_kg', 'date_of_birth'], dtype='object')

# print(dogs.index)
# RangeIndex(start=0, stop=7, step=1)
```

### 1.3. Sorting Data

* **`.sort_values()`**: The primary method for sorting a DataFrame. By default, it returns a *new* sorted DataFrame, leaving the original unchanged.
  * **Pro-tip:** While you *can* use `inplace=True` to modify the original DataFrame, this is generally discouraged. The preferred "pandas-idiomatic" way is to reassign the variable: `dogs_sorted = dogs.sort_values("weight_kg")`. This prevents unexpected side effects.

```python
# Sort by a single column in ascending order (default)
dogs.sort_values("weight_kg")

# Sort by a single column in descending order
dogs.sort_values("weight_kg", ascending=False)

# Sort by multiple columns:
# This first sorts by 'weight_kg'. For rows with the *same* weight,
# it then sorts by 'height_cm'.
dogs.sort_values(["weight_kg", "height_cm"])

# Sort by multiple columns with mixed orders
# (weight ascending, height descending)
dogs.sort_values(["weight_kg", "height_cm"], ascending=[True, False])

```

### 1.4. Subsetting Data (Selection & Filtering)

This is one of the most fundamental skills in pandas.

#### A. Subsetting Columns

* **To select a single column:** Use single brackets `[]`. This returns a 1D pandas **`Series`** object, which is like a 1D array with an index.

```python
# Returns a Series
names = dogs["name"]
```

* **To select multiple columns:** Use a *list* of column names inside the brackets `[[]]`. This returns a 2D **`DataFrame`**.

```python
# Note the double brackets: [['col1', 'col2']]
# Returns a DataFrame
subset = dogs[["breed", "height_cm"]]
```

* **Pro-tip (Dot Notation):** You *can* access a column like an attribute (e.g., `dogs.name`), but this is **discouraged**. It will fail if the column name has spaces (e.g., "dog breed"), contains special characters, or conflicts with a built-in DataFrame method (like `dogs.head`). Always use the bracket notation `dogs["name"]`.

#### B. Subsetting Rows (Filtering)

Row subsetting is done by passing a boolean `Series` (a "mask") into the `[]`. The DataFrame will return only the rows where the mask's value is `True`.

```python
# 1. Create a boolean mask (a Series of True/False)
# This operation is vectorized:
is_tall = dogs["height_cm"] > 50
# print(is_tall)
# 0     True
# 1    False
# 2    False
# 3    False
# 4     True
# 5    False
# 6     True
# Name: height_cm, dtype: bool

# 2. Pass the mask to the DataFrame
tall_dogs = dogs[is_tall]

# You can also do this in one line, which is more common:
tall_dogs = dogs[dogs["height_cm"] > 50]

# Filter based on text data (exact match)
labradors = dogs[dogs["breed"] == "Labrador"]

# Filter based on date data (pandas handles string comparison for dates)
old_dogs = dogs[dogs["date_of_birth"] < "2015-01-01"]
```

#### C. Subsetting with Multiple Conditions

This is a common point of confusion for new users.

* You **must** use the bitwise operators:
  * `&` for AND
  * `|` for OR
  * `~` for NOT

* You **cannot** use the Python keywords `and`, `or`, `not`.
* **Why?** The keywords `and`/`or` try to determine the truthiness of an *entire* `Series` (e.g., is the *whole* `Series` `True`?), which is ambiguous. The bitwise operators `&`/`|` perform element-wise comparisons (Row 1 AND Row 1, Row 2 AND Row 2, etc.), which is what you want for a filter.
* Each condition **must** be wrapped in parentheses `()` due to operator precedence.

```python
# Example: Find all Brown Labradors
# Good, clear way:

is_lab = dogs["breed"] == "Labrador"
is_brown = dogs["color"] == "Brown"
brown_labs = dogs[is_lab & is_brown]

# The common one-line version (notice the parentheses!)
brown_labs = dogs[(dogs["breed"] == "Labrador") & (dogs["color"] == "Brown")]

# Example: Find dogs that are Labradors OR Poodles
lab_or_poodle = dogs[(dogs["breed"] == "Labrador") | (dogs["breed"] == "Poodle")]
```

#### D. Subsetting with `.isin()`

When you need to filter based on a list of multiple values, `.isin()` is much cleaner and faster than chaining many `|` (OR) conditions.

```python
# The "long way" (hard to read and error-prone)
is_black_or_brown_long = (dogs["color"] == "Black") | (dogs["color"] == "Brown")

# The "pandas way" (clean, readable, and efficient)
colors_to_find = ["Black", "Brown", "Tan"]
is_in_list = dogs["color"].isin(colors_to_find)
dogs_with_colors = dogs[is_in_list]
```

### 1.5. Creating New Columns

You can create new columns by performing "vectorized" operations on existing columns (the operation is applied to every row automatically, no `for` loop needed).

```python
# Create a new column 'height_m' from 'height_cm'
# This is vectorized: it divides every value in 'height_cm' by 100

dogs["height_m"] = dogs["height_cm"] / 100

# Create a 'bmi' column using two other columns
# Vectorization handles the element-wise operations

dogs["bmi"] = dogs["weight_kg"] / (dogs["height_m"] ** 2)

# You can also use functions from other libraries, like NumPy
# import numpy as np
# dogs["is_heavy"] = np.where(dogs["weight_kg"] > 30, "Heavy", "Light")
```

### 1.6. Chaining Manipulations

You can chain these methods together to perform complex transformations in a clean, readable way. This is very common in pandas code as it avoids creating many intermediate variables (`bmi_lt_100`, `bmi_lt_100_sorted`, etc.).

* **Pro-tip:** To make long chains readable, wrap the entire chain in parentheses `()` and put each method on a new line.

```python
# Workflow:
# 1. Find dogs with a BMI under 100
# 2. Sort them by height (descending)
# 3. Select only the name, height, and BMI

# The step-by-step way (good for debugging)
bmi_lt_100 = dogs[dogs["bmi"] < 100]
bmi_lt_100_sorted = bmi_lt_100.sort_values("height_cm", ascending=False)
result = bmi_lt_100_sorted[["name", "height_cm", "bmi"]]

# The chained way (more idiomatic)
# The backslashes () are one way to break lines
result_chained = dogs[dogs["bmi"] < 100]  
.sort_values("height_cm", ascending=False)  
[["name", "height_cm", "bmi"]]

# The parentheses way (often preferred for readability)
result_chained_paren = (
dogs[dogs["bmi"] < 100]
.sort_values("height_cm", ascending=False)
[["name", "height_cm", "bmi"]]
)
```

## 2. Aggregating Data

This chapter focuses on calculating summary statistics on DataFrame columns. We will move from simple summaries of a single column to complex, grouped summaries and pivot tables.

### 2.1. Summary Statistics

Aggregating data is the process of collapsing many rows into a single summary value.

- Common Aggregations: Pandas provides many common aggregation methods as built-in Series methods:
    * `.mean()`: Average
    * `.median()`: The 50th percentile
    * `.mode()`: The most frequent value
    * `.min()`: The minimum value
    * `.max()`: The maximum value
    * `.var()`: Variance
    * `.std()`: Standard deviation
    * `.sum()`: The total
    * `.quantile()`: The nth percentile (e.g., .quantile(0.25) for the 25th)

```python
# Calculate the mean of a single column
# print(dogs["height_cm"].mean())
# 49.714285714285715
```

#### Summarizing Dates

- Aggregation methods work on dates, too. This is extremely useful for finding the start and end of a time period.
    * Pro-tip: This only works correctly if your date column is a datetime `dtype`, not `object`. (See Chapter 1 `.info() pro-tip).# Get the oldest dog's date of birth
# print(dogs["date_of_birth"].min())
# '2011-12-11'

# Get the youngest dog's date of birth
# print(dogs["date_of_birth"].max())
# '2018-02-27'
Efficient Summaries with .agg()The .agg() method (short for "aggregate") is a powerful and flexible method that allows you to:Apply your own custom aggregation functions.Apply multiple aggregations at once.# 1. Using a custom function
def pct30(column):
    return column.quantile(0.3)

# print(dogs["weight_kg"].agg(pct30))
# 22.599999999999998

# You can also apply it to multiple columns at once
# print(dogs[["weight_kg", "height_cm"]].agg(pct30))
# weight_kg    22.6
# height_cm    45.4
# dtype: float64

# 2. Applying multiple functions
def pct40(column):
    return column.quantile(0.4)

# Pass a list of functions (as strings or function names)
# print(dogs["weight_kg"].agg([pct30, pct40]))
# pct30    22.6
# pct40    24.0
# Name: weight_kg, dtype: float64

# You can even mix strings of built-in methods
# print(dogs["weight_kg"].agg(["mean", "median", pct30]))
# mean     27.428571
# median   24.000000
# pct30    22.600000
# Name: weight_kg, dtype: float64
Cumulative StatisticsSometimes you need a running summary. Cumulative statistics apply an operation cumulatively down the rows..cumsum(): Cumulative sum (a running total)..cummax(): Cumulative maximum (the running max)..cummin(): Cumulative minimum (the running min)..cumprod(): Cumulative product.# Original weight column
# 0    24
# 1    24
# 2    24
# 3    17
# 4    29
# 5     2
# 6    74
# Name: weight_kg, dtype: int64

# Cumulative sum
# print(dogs["weight_kg"].cumsum())
# 0     24  (24)
# 1     48  (24 + 24)
# 2     72  (48 + 24)
# 3     89  (72 + 17)
# 4    118  (89 + 29)
# 5    120  (118 + 2)
# 6    194  (120 + 74)
# Name: weight_kg, dtype: int64
2.2. CountingCounting is a critical part of data analysis, but it's important to avoid double counting.Dropping Duplicates (.drop_duplicates())When a dataset has one row per event (like vet_visits), but you want one row per object (like dogs), you must drop duplicates.The subset argument is key. It defines which column(s) to use to identify duplicates.# Example: vet_visits has multiple entries for 'Max' and 'Lucy'
#      date    name      breed  weight_kg
# 0  2018-09-02   Bella   Labrador      24.87
# 1  2019-06-07     Max   Chow Chow     24.01
# ...
# 72 2019-06-07     Max   Chow Chow     24.01
# 73 2018-08-20    Lucy   Chow Chow     24.40
# 74 2019-04-22     Max    Labrador     28.54

# Drop rows where the 'name' is duplicated
# This keeps the *first* occurrence of each name
unique_dogs = vet_visits.drop_duplicates(subset="name")

# Drop rows where the *combination* of 'name' AND 'breed' is duplicated
# This would keep both rows for 'Max' since ('Max', 'Chow Chow') 
# is different from ('Max', 'Labrador')
unique_dogs_breed = vet_visits.drop_duplicates(subset=["name", "breed"])
Counting Categorical Variables (.value_counts())This is the go-to method for counting the occurrences of each value in a categorical column.It returns a Series with the unique values as the index and their counts as the values.By default, it sorts the results in descending order by count.# Count the occurrences of each breed in our unique dogs list
# print(unique_dogs_breed["breed"].value_counts())
# Labrador       2
# Chow Chow      2
# Schnauzer      1
# St. Bernard    1
# Poodle         1
# Chihuahua      1
# Name: breed, dtype: int64

# Use sort=True to sort by count (default)
# print(unique_dogs_breed["breed"].value_counts(sort=True))

# Use normalize=True to get proportions (percentages)
# This is extremely useful for understanding distribution.
# print(unique_dogs_breed["breed"].value_counts(normalize=True))
# Labrador       0.250
# Chow Chow      0.250
# Schnauzer      0.125
# St. Bernard    0.125
# Poodle         0.125
# Chihuahua      0.125
# Name: breed, dtype: float64
2.3. Grouped Summary StatisticsThis is one of the most powerful features of pandas. It allows you to calculate summary statistics for sub-groups of your data.This follows the "Split-Apply-Combine" pattern:Split the data into groups based on some criteria.Apply an aggregation function to each group.Combine the results into a new data structure.The "Long Way" (Bad): You could filter for each group and then call .mean(). This is repetitive and inefficient.# dogs[dogs["color"] == "Black"]["weight_kg"].mean()
# dogs[dogs["color"] == "Brown"]["weight_kg"].mean()
# ... etc. ...
The "Pandas Way" (.groupby()):The .groupby() method is used for this. You chain it with an aggregation method.# 1. Group by "color"
# 2. Select the "weight_kg" column
# 3. Apply the .mean() function to each group
# print(dogs.groupby("color")["weight_kg"].mean())
# color
# Black    26.5
# Brown    24.0
# Gray     17.0
# Tan       2.0
# White    74.0
# Name: weight_kg, dtype: float64
Multiple Grouped SummariesJust like with .agg() before, you can use .agg() on a groupby object to get multiple summaries at once.# Get the min, max, and sum of weight for each color
# print(dogs.groupby("color")["weight_kg"].agg([min, max, sum]))
#        min  max  sum
# color
# Black   24   29   53
# Brown   24   24   48
# Gray    17   17   17
# Tan      2    2    2
# White   74   74   74
Grouping by Multiple VariablesYou can group by multiple columns by passing a list of column names to .groupby(). This creates a MultiIndex in the result.# 1. Group by both "color" AND "breed"
# 2. Get the mean of "weight_kg" for each group
# print(dogs.groupby(["color", "breed"])["weight_kg"].mean())
# color  breed
# Black  Labrador     29
#        Poodle       24
# Brown  Chow Chow    24
#        Labrador     24
# Gray   Schnauzer    17
# Tan    Chihuahua     2
# White  St. Bernard  74
# Name: weight_kg, dtype: int64

# You can also get summaries for multiple value columns
# print(dogs.groupby(["color", "breed"])[["weight_kg", "height_cm"]].mean())
#                      weight_kg  height_cm
# color breed
# Black Labrador              29         59
#       Poodle                24         43
# Brown Chow Chow             24         46
#       Labrador              24         56
# Gray  Schnauzer             17         49
# Tan   Chihuahua              2         18
# White St. Bernard           74         77
2.4. Pivot TablesPivot tables are an alternative, spreadsheet-like way to perform grouped summary statistics. They are especially good for "pivoting" or "unstacking" one of your grouping variables into new columns.values: The column to aggregate.index: The column to group by (will become the rows).aggfunc: The aggregation function to use (defaults to mean).# This .groupby() call...
# dogs.groupby("color")["weight_kg"].mean()

# ...is equivalent to this .pivot_table() call:
# print(dogs.pivot_table(values="weight_kg", index="color"))
#        weight_kg
# color
# Black       26.5
# Brown       24.0
# Gray        17.0
# Tan          2.0
# White       74.0

# Use aggfunc to change the statistic
# print(dogs.pivot_table(values="weight_kg", index="color", aggfunc="median"))

# Pass a list to aggfunc for multiple statistics
# print(dogs.pivot_table(values="weight_kg", index="color", aggfunc=["mean", "median"]))
#           mean   median
#      weight_kg weight_kg
# color
# Black       26.5      26.5
# Brown       24.0      24.0
# Gray        17.0      17.0
# Tan          2.0       2.0
# White       74.0      74.0
Pivoting on Two VariablesThe real power of pivot tables comes from using the columns argument. This "pivots" the unique values from one column into new columns in the output.# This .groupby() (which produces a MultiIndex)...
# dogs.groupby(["color", "breed"])["weight_kg"].mean()

# ...can be "pivoted" into this table:
# print(dogs.pivot_table(values="weight_kg", index="color", columns="breed"))
# breed  Chihuahua  Chow Chow  Labrador  Poodle  Schnauzer  St. Bernard
# color
# Black        NaN        NaN      29.0    24.0        NaN          NaN
# Brown        NaN       24.0      24.0     NaN        NaN          NaN
# Gray         NaN        NaN       NaN     NaN       17.0          NaN
# Tan          2.0        NaN       NaN     NaN        NaN          NaN
# White        NaN        NaN       NaN     NaN        NaN         74.0
Handling Missing Values & TotalsThe NaN (Not a Number) values appear because there are no "Black Chihuahuas" in the data. You can clean this up.fill_value: Replaces NaNs with a specific value (e.g., 0).margins: Set to True to add a "All" row and "All" column that show the totals (the mean for all breeds/colors).# Pivot with fill_value=0 and margins=True
print(
    dogs.pivot_table(
        values="weight_kg",
        index="color",
        columns="breed",
        fill_value=0,
        margins=True
    )
)
# breed  Chihuahua  Chow Chow  Labrador  Poodle  Schnauzer  St. Bernard        All
# color
# Black          0         0      29.0      24          0           0  26.500000
# Brown          0        24      24.0       0          0           0  24.000000
# Gray           0         0       0.0       0         17           0  17.000000
# Tan            2         0       0.0       0          0           0   2.000000
# White          0         0       0.0       0          0          74  74.000000
# All            2        24      26.5      24         17          74  27.714286









## 3. Slicing and Indexing Data

* Subsetting using slicing
* Indexes and subsetting using indexes

## 4. Creating and Visualizing Data

* Plotting
* Handling missing data
* Reading data into a DataFrame


