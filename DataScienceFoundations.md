# Data Science Foundations

After this, we will be able to:
  - articulate what can go right and what can go wrong when working with data.
  - query, manipulate, and analyze data in databases using SQL.
  - write basic programs to create custom analyses with Python.
  - clean data to prepare it for analysis.
  - ask and answer questions using appropriate statistical measures.

## Why Data Science? 

### Exploring Data with SQL

A database is a set of data stored in a computer. This data is usually structured into tables. Tables can grow large and have a multitude of columns and records.

SQL (pronounced “S-Q-L” or “sequel”) allows you to write queries which define the subset of data you are seeking. It is a great way to access data and a great entry point to programming because its syntax (the specific vocabulary that gives instructions to the computer) is very human-readable. Without knowing any SQL, you might still be able to guess what each command will do.

Imagine that we want to take a look at the churn rate. The *churn rate* is, for example, the percent of subscribers to a monthly service who have canceled. For example, in January, one service had 1,000 learners. In February, 200 learners sign up, and 250 cancel. The churn rate for February would be:

$$\frac{cancellations}{total\ subscribers} = \frac{250}{1000 + 200} = 20.8\%$$

If we want to do this using SQL we can use the following SQL query: 
```SQL
SELECT COUNT(DISTINCT user_id) AS 'enrollments',
  COUNT(CASE
       	WHEN strftime("%m", cancel_date) = '03'
        THEN user_id
  END) AS 'march_cancellations',
 	ROUND(100.0 * COUNT(CASE
       	WHEN strftime("%m", cancel_date) = '03'
        THEN user_id
  END) / COUNT(DISTINCT user_id)) AS 'churn_rate'
FROM pro_users
WHERE signup_date < '2017-04-01'
	AND (
    (cancel_date IS NULL) OR
    (cancel_date > '2017-03-01')
  );
```

### Programming and Analyzing Data with Python

After interacting with the database, it is time to analyze the data further and eventually visualize the data. And SQL cannot get us there.

Python is a general-purpose programming language. It can do almost all of what other languages can do with comparable, or faster, speed. It is often chosen by Data Analysts and Data Scientists for prototyping, visualization, and execution of data analyses on datasets.

There’s an important question here. Plenty of other programming languages, like R, can be useful in the field of data science. Why are so many people choosing Python?

One major factor is Python’s versatility. There are over 125,000 third-party Python libraries. These libraries make Python more useful for specific purposes, from the traditional (e.g. web development, text processing) to the cutting edge (e.g. AI and machine learning). 

Additionally, Python has become a go-to language for data analysis. With data-focused libraries like pandas, NumPy, and Matplotlib, anyone familiar with Python’s syntax and rules can use it as a powerful tool to process, manipulate, and visualize data.

Python is a programming language, and pandas is a special set of commands in Python that lets us analyze spreadsheet data. Pandas can do a lot of the things that SQL can do, but it’s also backed by the power of Python, so we can easily transition from analyzing our data with pandas to visualizing it using other Python tools.

Matplotlib lets us create line charts, bar charts, pie charts, and more. It gives us precise control over colors and labels so that we can create the perfect chart to communicate our findings.

For example: 
```python
import codecademylib3_seaborn
from matplotlib import pyplot as plt
import numpy as np
import pandas as pd

hour = range(24)

viewers_hour = [30, 17, 34, 29, 19, 14, 3, 2, 4, 9, 5, 48, 62, 58, 40, 51, 69, 55, 76, 81, 102, 120, 71, 63]

plt.title("Codecademy Learners Time Series")

plt.xlabel("Hour")
plt.ylabel("Viewers")

plt.plot(hour, viewers_hour)

plt.legend(['2015-01-01'])

ax = plt.subplot()

ax.set_facecolor('seashell')

ax.set_xticks(hour)
ax.set_yticks([0, 20, 40, 60, 80, 100, 120])

y_upper = [i + (i*0.15) for i in viewers_hour]
y_lower = [i - (i*0.15) for i in viewers_hour]

plt.fill_between(hour, y_lower, y_upper, alpha=0.2)

# Add the code here:
plt.show()
```

And we will get something like
<img width="1165" alt="Screenshot 2025-06-26 at 13 27 03" src="https://github.com/user-attachments/assets/e3326f4e-6668-47e6-8028-30ca32cd7d2d" />

### Probability

In data science, probability is often used to simulate scenarios.

The following code simulates the birthday problem (“What is the probability that two people in a room have the same birthday?”). Right now the code simulates a room with only 2 people that get random birthdays, and the probability that those 2 people have the same birthday is really low.

We can note that if we make our number too big, the program will throw an error due to the way we have implemented some of the math. This is a great example of needing to be mindful of possible inputs to our programme.

---
## 1. Principles of Data Literacy

### 1.1 Introduction to Data

Garbage in, garbage out is a data-world phrase that means “our data-driven conclusions are only as strong, robust, and well-supported as the data behind them.”

### 1.2 Thinking About Data

#### 1.2.1 Statistical Thinking

**Summary statistics** are essential tools in data analysis, providing a quick and insightful overview of datasets. They help in understanding the central tendency, dispersion, and shape of data distributions. 

##### 1.2.1.1 Mean (Arithmetic Mean)
- **Definition:** The mean is the sum of all data points divided by the number of data points. It represents the average value of the dataset.

- **Formula:** $$\bar{x} = \frac{1}{n} \sum_{i=1}^{n} x_i$$

  where $n$ is the number of observations, and $x_i$ are the individual data points.

- **Interpretation:** The mean provides a measure of the central tendency of the data. It is the balancing point of the dataset.
- **Potential Issues:**
  * *Sensitivity to Outliers:* The mean can be heavily influenced by extreme values, which may not accurately represent the dataset.
  * *Not Suitable for Skewed Data:* In skewed distributions, the mean may not reflect the central location effectively.

- **Comparison:**
  * Unlike the median, the mean considers all data points, making it more sensitive to outliers.
  * It is often used when the data is symmetric and without extreme values.

- **Computation:**
  * Python: numpy.mean(data)  
  * R: mean(data)

##### 1.2.1.2 Median

Definition:The median is the middle value in a dataset when the data points are arranged in ascending order. It divides the dataset into two equal halves.
Formula:  

For an odd number of observations:[\text{Median} = x_{\frac{n+1}{2}}]
For an even number of observations:[\text{Median} = \frac{x_{\frac{n}{2}} + x_{\frac{n}{2} + 1}}{2}]

Interpretation:The median represents the central value of the dataset, making it a robust measure of central tendency.
Potential Issues:  

Less Information: It does not consider all data points, which may overlook important information.
Computationally Intensive: For large datasets, calculating the median requires sorting, which can be computationally expensive.

Comparison:  

More robust to outliers compared to the mean.
Preferred for skewed distributions or when outliers are present.

Computation:  

Python: numpy.median(data)  
R: median(data)


3. Mode
Definition:The mode is the value that appears most frequently in a dataset.
Formula:There is no standard formula; it is determined by identifying the most frequent value(s).
Interpretation:The mode indicates the most common value(s) in the dataset, useful for categorical data or to identify peaks in distributions.
Potential Issues:  

Multiple Modes: Datasets can have multiple modes (bimodal, multimodal), which may complicate interpretation.
Not Always Representative: In continuous data, the mode may not be a good measure of central tendency.

Comparison:  

Unlike the mean and median, the mode can be used for nominal data.
It is less affected by outliers but may not provide a complete picture of the data distribution.

Computation:  

Python: scipy.stats.mode(data)  
R: names(sort(table(data), decreasing = TRUE)[1])


4. Variance
Definition:Variance measures the average squared deviation of each data point from the mean, indicating the spread of the data.
Formula:[\sigma^2 = \frac{1}{n} \sum_{i=1}^{n} (x_i - \bar{x})^2]For sample variance:[s^2 = \frac{1}{n-1} \sum_{i=1}^{n} (x_i - \bar{x})^2]
Interpretation:A higher variance indicates greater dispersion in the data, while a lower variance suggests data points are closer to the mean.
Potential Issues:  

Units: Variance is in squared units, which can be difficult to interpret.
Sensitivity to Outliers: Like the mean, variance is sensitive to extreme values.

Comparison:  

Provides a measure of dispersion, unlike the mean, median, or mode.
Often used in conjunction with the standard deviation for better interpretability.

Computation:  

Python: numpy.var(data, ddof=0) for population, numpy.var(data, ddof=1) for sample  
R: var(data) (sample variance by default)


5. Standard Deviation
Definition:The standard deviation is the square root of the variance, providing a measure of dispersion in the same units as the data.
Formula:[\sigma = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (x_i - \bar{x})^2}]For sample standard deviation:[s = \sqrt{\frac{1}{n-1} \sum_{i=1}^{n} (x_i - \bar{x})^2}]
Interpretation:It indicates how much the data points deviate from the mean on average.
Potential Issues:  

Sensitivity to Outliers: Like variance, it is affected by extreme values.
Assumes Normality: Often used under the assumption of a normal distribution, which may not always hold.

Comparison:  

More interpretable than variance due to being in the same units as the data.
Commonly used in conjunction with the mean to describe data distributions.

Computation:  

Python: numpy.std(data, ddof=0) for population, numpy.std(data, ddof=1) for sample  
R: sd(data) (sample standard deviation by default)


6. Range
Definition:The range is the difference between the maximum and minimum values in a dataset.
Formula:[\text{Range} = \max(x) - \min(x)]
Interpretation:It provides a simple measure of the spread of the data.
Potential Issues:  

Sensitivity to Outliers: Extreme values can greatly affect the range.
Ignores Data Distribution: Does not provide information about the distribution between the extremes.

Comparison:  

Simplest measure of dispersion but less informative than variance or standard deviation.
Useful for a quick assessment of data spread.

Computation:  

Python: numpy.ptp(data) or max(data) - min(data)  
R: diff(range(data)) or max(data) - min(data)


7. Interquartile Range (IQR)
Definition:The IQR is the difference between the third quartile (Q3) and the first quartile (Q1), representing the middle 50% of the data.
Formula:[\text{IQR} = Q3 - Q1]
Interpretation:It measures the spread of the central portion of the data, making it robust to outliers.
Potential Issues:  

Less Sensitive: May not capture the full variability of the data.
Computationally Intensive: Requires sorting the data to find quartiles.

Comparison:  

More robust to outliers compared to the range or standard deviation.
Useful for skewed distributions or when outliers are present.

Computation:  

Python: numpy.percentile(data, 75) - numpy.percentile(data, 25)  
R: IQR(data)


8. Skewness
Definition:Skewness measures the asymmetry of the data distribution around the mean.
Formula:[\text{Skewness} = \frac{n}{(n-1)(n-2)} \sum_{i=1}^{n} \left( \frac{x_i - \bar{x}}{s} \right)^3]where ( s ) is the sample standard deviation.
Interpretation:  

Positive Skewness: Data is skewed to the right (long tail on the right).  
Negative Skewness: Data is skewed to the left (long tail on the left).  
Zero Skewness: Symmetric distribution.

Potential Issues:  

Sensitivity to Sample Size: Can be unreliable for small sample sizes.
Interpretation: Requires understanding of the data context.

Comparison:  

Unlike measures of central tendency or dispersion, skewness provides information about the shape of the distribution.
Useful for identifying deviations from normality.

Computation:  

Python: scipy.stats.skew(data)  
R: skewness(data) from the e1071 package


9. Kurtosis
Definition:Kurtosis measures the "tailedness" of the data distribution, indicating the presence of outliers.
Formula:[\text{Kurtosis} = \frac{n(n+1)}{(n-1)(n-2)(n-3)} \sum_{i=1}^{n} \left( \frac{x_i - \bar{x}}{s} \right)^4 - \frac{3(n-1)^2}{(n-2)(n-3)}]This formula is for excess kurtosis, where a normal distribution has kurtosis of 0.
Interpretation:  

Positive Kurtosis: Heavy tails, more outliers.  
Negative Kurtosis: Light tails, fewer outliers.  
Zero Kurtosis: Similar to a normal distribution.

Potential Issues:  

Sensitivity to Outliers: Can be greatly influenced by extreme values.
Interpretation: Often misunderstood; does not measure peakedness directly.

Comparison:  

Complements skewness by providing additional information about the distribution's shape.
Useful in risk management and financial analysis to assess tail risk.

Computation:  

Python: scipy.stats.kurtosis(data)  
R: kurtosis(data) from the e1071 package


Summary and Comparison

Central Tendency: Mean, Median, Mode  

Mean: Sensitive to outliers, used for symmetric data.  
Median: Robust to outliers, used for skewed data.  
Mode: Useful for categorical data or identifying peaks.


Dispersion: Variance, Standard Deviation, Range, IQR  

Variance/Standard Deviation: Measure average deviation from the mean, sensitive to outliers.  
Range: Simple but sensitive to outliers.  
IQR: Robust measure of spread, focuses on the central 50%.


Shape: Skewness, Kurtosis  

Skewness: Measures asymmetry.  
Kurtosis: Measures tailedness and outlier presence.



Each statistic provides unique insights, and the choice depends on the data characteristics and analysis goals. For a comprehensive understanding, it is often beneficial to use multiple summary statistics together.

---
the correlation coefficient. This number ranges from -1 to +1 and tells us two things about a linear relationship:

Direction: A positive coefficient means that higher values in one variable are associated with higher values in the other. A negative coefficient means higher values in one variable are associated with lower values of the other.
Strength: The farther the coefficient is from 0, the stronger the relationship and the more the points in a scatter plot look like a line.

#### 1.2.2 Movie Statistics Project

### 1.3 Visualizing Data

#### 1.3.1 Data Visualization Basics

- Data visualizations are how we communicate data.
- One big consideration when choosing a chart type is how many variables we’re comparing.
- **Univariate charts** help us visualize a change in one variable. Often that means measuring “how much,” which can either be a count or a distribution.
  * A common chart for counts is the bar graph.
  * Another common univariate chart is the histogram. **Histograms** measure the distribution or spread, of a variable. Histograms are a great way to show the concept of a normal (or skewed) distribution. 
  * A density curve also visualizes a distribution, without putting data in bins like a histogram does.
  * A more “math-forward” way to visualize distributions is a box plot or violin plot. These visualizations make percentile and quartile values obvious.

- **Bivariate** and **Multivariate Charts* show the relationships between two or more variables.
  * The classic bivariate example is the scatter plot — one variable on the x-axis, another on the y-axis, and each point helps us compare the two variables by its position on the graph. Scatterplots translate the relationship between two variables in the data into an easy-to-see spatial relationship. Because we’re relying on the idea that each variable increases as we move up the X or Y axis, the scatterplot only makes sense for numeric variables, not categorical.
  * A line chart is another common bivariate chart, often measuring a variable changing over time. A line chart with multiple lines for different variables is a multivariate chart.
  
- We can use aesthetic properties to further clarify and visualize the “details” of the data. Aesthetic properties are the attributes we use to communicate data visually:
  * Position
  * Size
  * Shape
  * Color / Pattern

- **Viewers need context to understand what a data visualization means and why it matters.**
- The big takeaway when designing for **color accessibility** is to think not only about the hue of a color (e.g. red, green, or purple), but the value as well (e.g. bright red, light green, dark purple). Good color comparisons use high contrast values, not just different hues.
- For online data visualizations, make sure to include alt text as we would for any other web image. Alt text ensures that users experiencing a visualization through a screen reader won’t miss out on whatever information it contains.
- The same accessibility goal – making our work available and easier to access for more people – is a great principle to keep in mind no matter what. (This is actually called “universal design.”). We can apply it when it comes to…
  * **Readability:** keep the reading level to a high school level whenever possible.
  * **Prior knowledge:** define unfamiliar terms and avoid unnecessary jargon.
  * **Information overload:** introduce new information with intentional pacing and organization.

---

### 1.4 Analyzing Data

#### 1.4.1 Data Analyses and Conclusions

- Descriptive analysis lets us describe, summarize, and visualize data so that patterns can emerge.
- Descriptive analyses, which are referred to as descriptives or summary statistics, include:
  * **measures of central tendency** (e.g., mean, median, mode) and
  * **spread** (e.g., range, quartiles, variance, standard deviation, distribution).

- **Exploratory analysis** is the next step after descriptive analysis. With exploratory analysis, we look for relationships between variables in our dataset.
- While our exploratory analyses might uncover some fascinating patterns, we should keep in mind that exploratory analyses cannot tell us why something happened: *correlation is not the same as causation*.

- **Unsupervised machine learning techniques**, such as ***clustering algorithms***, are useful tools for exploratory analysis. These techniques “learn” patterns from untagged data, or data that do not have classifications already attached to them, and they help us see relationships between many variables at once.

- A **Principal Component Analysis** or **PCA**, which compresses the variables into principal components that can be plotted against each other.
- After **PCA**, we can use **k-means clustering** to look for trends in the data.

- **A/B tests** are a popular business tool that data scientists use to optimize websites and other online platforms. A/B tests are a type of inferential analysis.
- **Inferential analysis** lets us test a hypothesis on a sample of a population and then extend our conclusions to the whole population.
- Some basic rules:
  * Sample size must be big enough compared to the total population size (10% is a good rule-of-thumb).
  * Our sample must be randomly selected and representative of the total population.
  * We can only test one hypothesis at a time.

- **Causal analysis** generally relies on carefully designed experiments, but we can sometimes also do causal analysis with observational data.
- Experiments that support causal analysis:
  * Only change one variable at a time
  * Carefully control all other variables
  * Are repeated multiple times with the same results

- **Causal inference** with observational data requires:
  * Advanced techniques to identify a causal effect
  * Meeting very strict conditions
  * Appropriate statistical tests

- **Predictive analysis** uses data and supervised machine learning techniques to identify the likelihood of future outcomes.
- Some popular supervised machine learning techniques include regression models, support vector machines, and deep learning convolutional neural networks. The actual algorithm used with each of these techniques is different, but each requires training data.

---
## 2. Learn SQL

### 2.1 Getting Started with SQL

- Databases are at the core of Data Science and are where most data is stored.
- A **database** is a set of data stored in a computer. This data is usually structured into tables. Tables can grow large and have a multitude of columns and records.
- A multi-step process where some users leave at each step is called a **funnel**.
- Example 1: You are going to combine data from three different tables:
  * `browse` - gives the timestamps of users who visited different item description pages
  * `checkout` - gives the timestamps of users who visited the checkout page
  * `purchase` - gives the timestamps of when users complete their purchase

```SQL
SELECT ROUND(
  100.0 * COUNT(DISTINCT c.user_id) /
  COUNT(DISTINCT b.user_id)
) AS browse_to_checkout_percent,
ROUND(
  100.0 * COUNT(DISTINCT p.user_id) /
   COUNT(DISTINCT c.user_id)
) AS checkout_to_purchase_percent
FROM browse b
LEFT JOIN checkout c
	ON b.user_id = c.user_id
LEFT JOIN purchase p
	ON c.user_id = p.user_id;
```

- Example 2: Analyze the churn rate.
```SQL
SELECT COUNT(DISTINCT user_id) AS enrollments,
	COUNT(CASE
       	WHEN strftime("%m", cancel_date) = '03'
        THEN user_id
  END) AS march_cancellations,
 	ROUND(100.0 * COUNT(CASE
       	WHEN strftime("%m", cancel_date) = '03'
        THEN user_id
  END) / COUNT(DISTINCT user_id)) AS churn_rate
FROM pro_users
WHERE signup_date < '2017-04-01'
	AND (
    (cancel_date IS NULL) OR
    (cancel_date > '2017-03-01')
  );
```

- A **relational database** is a type of database. It uses a structure that allows us to identify and access data in relation to another piece of data in the database. Often, data in a relational database is organized into tables.
- A **table** is a collection of data organized into rows and columns. Tables are sometimes referred to as *relations*.
- Tables can have hundreds, thousands, sometimes even millions of rows of data. These rows are often called **records**.
- Tables can also have many columns of data. Columns are labeled with a descriptive name (say, age for example) and have a specific data type.
- A **relational database management system (RDBMS)** is a program that allows you to create, update, and administer a relational database. Most relational database management systems use the SQL language to access the database.

### 2.2 Manipulation

- SQL (Structured Query Language) is a programming language used to communicate with data stored in a relational database management system.
- **File extensions** — when working with databases, take a look at the name of the file you’re writing in. If your file ends in .sqlite, you’re using a SQLite database. If your file ends in .sql, you’re working with PostgreSQL.
- **Data types** -> SQLite and PostgreSQL have slightly different data types. For example, if you want to store text in a SQLite database, you’ll use the `TEXT` data type. If you’re working with PostgreSQL, you have many more options. You could use `varchar(n)`, `char(n)`, or `text`. Each type has its own subtle differences. This is a good example of PostgreSQL being slightly more robust than SQLite, but the core concepts remaining the same.
- **Built-in tables** —> Depending on which RDBMS system you are using, the syntax for doing that will be different. Any time you’re writing SQL about the database itself, rather than the data, that syntax will likely be unique to the RDBMS you’re using.

- All data stored in a relational database is of a certain **data type**. Some of the most common data types are:
  * `INTEGER`, a positive or negative whole number;
  * `TEXT`, a text string;
  * `DATE`, the date formatted as YYYY-MM-DD;
  * `REAL`, a decimal value.

- A **statement** is text that the database recognizes as a valid command. Statements always end in a semicolon `;`. For example:
```SQL
CREATE TABLE table_name (
   column_1 data_type, 
   column_2 data_type, 
   column_3 data_type
);
```

- Let’s break down the components of a statement:
   * `CREATE TABLE` is a **clause**. Clauses perform specific tasks in SQL. By convention, clauses are written in capital letters. Clauses can also be referred to as commands.
   * `table_name` refers to the name of the table that the command is applied to.
   * `(column_1 data_type, column_2 data_type, column_3 data_type)` is a ***parameter***. A parameter is a list of columns, data types, or values that are passed to a clause as an argument. Here, the parameter is a list of column names and the associated data type.

- The structure of SQL statements vary. The number of lines used does not matter. A statement can be written all on one line, or split up across multiple lines if it makes it easier to read. 

- `CREATE` statements allow us to create a new table in the database. You can use the `CREATE` statement anytime you want to create a new table from scratch. For example, the statement below creates a new table named celebs:
```SQL
CREATE TABLE celebs (
   id INTEGER, 
   name TEXT, 
   age INTEGER
);
```

1. `CREATE TABLE` is a *clause* that tells SQL you want to create a new table.
2. `celebs` is the name of the table.
3. `(id INTEGER, name TEXT, age INTEGER)` is a list of parameters defining each column, or attribute in the table and its data type:
    * `id` is the first column in the table. It stores values of data type `INTEGER`.
    * `name` is the second column in the table. It stores values of data type `TEXT`.
    * `age` is the third column in the table. It stores values of data type `INTEGER`.

- The `INSERT` statement inserts a new row into a table. We can use the `INSERT` statement when you want to add new records. For example:
```SQL
INSERT INTO celebs (id, name, age) 
VALUES (1, 'Justin Bieber', 29);
```

- `INSERT INTO` is a *clause* that adds the specified row or rows.
- `celebs` is the table the row is added to.
- `(id, name, age)` is a parameter identifying the columns that data will be inserted into.
- `VALUES` is a *clause* that indicates the data being inserted.
- `(1, 'Justin Bieber', 29)` is a parameter identifying the values being inserted.
  * `1`: an integer that will be added to id column;
  * `'Justin Bieber'`: text that will be added to name column;
  * `29`: an integer that will be added to age column.
 
- `SELECT` statements are used to fetch data from a database. For example, in the statement below, `SELECT` returns all data in the `name` column of the `celebs` table.
```SQL 
SELECT name FROM celebs;
```

- You can also query data from all columns in a table with `SELECT`. For example:
```SQL
SELECT * FROM celebs;
```
- `*` is a special wildcard character that we have been using. It allows you to select every column in a table without having to name each one individually.

- The `ALTER TABLE` statement adds a new column to a table. You can use this command when you want to add columns to a table.
- For example, the statement below adds a new column `twitter_handle` to the `celebs` table:
```SQL
ALTER TABLE celebs 
ADD COLUMN twitter_handle TEXT;
```

1. `ALTER TABLE` is a *clause* that lets you make the specified changes.
2. `celebs` is the name of the table that is being changed.
3. `ADD COLUMN` is a *clause* that lets you add a new column to a table:
	* `twitter_handle` is the name of the new column being added
	* `TEXT` is the data type for the new column
4. `NULL` is a special value in SQL that represents **missing or unknown data**. Here, the rows that existed before the column was added have `NULL (∅)` values for `twitter_handle`.

- The `UPDATE` statement edits a row in a table.
- You can use the `UPDATE` statement when you want to change existing records.
- For example, the statement below updates the record with an `id` value of `4` to have the `twitter_handle` `@taylorswift13`.
```SQL
UPDATE celebs 
SET twitter_handle = '@taylorswift13' 
WHERE id = 4; 
```

1. `UPDATE` is a *clause* that edits a row in the table.
2. `celebs` is the name of the table.
3. `SET` is a *clause* that indicates the column to edit.
	* `twitter_handle` is the name of the column that is going to be updated
	* `@taylorswift13` is the new value that is going to be inserted into the `twitter_handle` column.
4. `WHERE` is a clause that indicates which row(s) to update with the new column value. Here the row with a `4` in the `id` column is the row that will have the `twitter_handle` updated to `@taylorswift13`.

- The `DELETE FROM` statement deletes one or more rows from a table.
- You can use the statement when you want to delete existing records.
- For example, the statement below deletes all records in the celebs table with no `twitter_handle`:
```SQL
DELETE FROM celebs 
WHERE twitter_handle IS NULL;
```

1. `DELETE FROM` is a *clause* that lets you delete rows from a table.
2. `celebs` is the name of the table we want to delete rows from.
3. `WHERE` is a *clause* that lets you select which rows you want to delete. Here we want to delete all of the rows where the `twitter_handle` column `IS NULL`.
4. `IS NULL` is a condition in SQL that returns **true** when the value is `NULL` and **false** otherwise.

- **Constraints** in SQL are the rules applied to the values of individual columns. They add information about how a column can be used after specifying the data type for a column. They can be used to tell the database to reject inserted data that does not adhere to a certain restriction. 

- **Constraints** that add information about how a column can be used are invoked after specifying the data type for a column.
- They can be used to tell the database to reject inserted data that does not adhere to a certain restriction.
- For example, the statement below sets constraints on the celebs table:
```SQL
CREATE TABLE celebs (
   id INTEGER PRIMARY KEY, 
   name TEXT UNIQUE,
   date_of_birth TEXT NOT NULL,
   date_of_death TEXT DEFAULT 'Not Applicable'
);
```

1. `PRIMARY KEY` columns can be used to uniquely identify the row. Attempts to insert a row with an identical value to a row already in the table will result in a constraint violation which will not allow you to insert the new row.
2. `UNIQUE` columns have a different value for every row. This is similar to `PRIMARY KEY` except a table can have many different `UNIQUE` columns.
3. `NOT NULL` columns must have a value. Attempts to insert a row without a value for a `NOT NULL` column will result in a constraint violation and the new row will not be inserted.
4. `DEFAULT` columns take an additional argument that will be the assumed value for an inserted row if the new row does not specify a value for that column.

- **SQLite** is a database engine. It is software that allows users to interact with a relational database.
- In SQLite, a database is stored in a single file — a trait that distinguishes it from other database engines. This fact allows for a great deal of accessibility: copying a database is no more complicated than copying the file that stores the data, sharing a database can mean sending an email attachment.

#### 2.2.1 SQLite

- SQLite is a database engine. It is software that allows users to interact with a relational database.
- In SQLite, a database is stored in a single file — a trait that distinguishes it from other database engines.
- This fact allows for a great deal of accessibility: copying a database is no more complicated than copying the file that stores the data, sharing a database can mean sending an email attachment.

- SQLite creates schemas, which constrain the type of data in each column, but it does not enforce them.
- However, SQLite will not reject values of the wrong type. We could accidentally insert the wrong data types in the columns.
- SQLite is used worldwide for testing, development, and in any other scenario where it makes sense for the database to be on the same disk as the application code.

### 2.3 Queries

- One of the core purposes of the SQL language is to retrieve information stored in a database. This is commonly referred to as **querying**.
- Queries allow us to communicate with the database by asking questions and returning a result set with data relevant to the question.

- We can select individual columns by their names (separated by a comma):
```SQL
SELECT column1, column2 
FROM table_name;
```

- `AS` is a keyword in SQL that allows you to rename a column or table using an alias. The new name can be anything you want as long as you put it inside of single quotes.
- When using `AS`, the columns are not being renamed in the table. The aliases only appear in the result.

- `DISTINCT` is used to return unique values in the output. It filters out all duplicate values in the specified column(s).
```SQL
SELECT DISTINCT column_name
FROM table_name
```

- We can restrict our query results using the `WHERE` clause in order to obtain only the information we want.
- The `WHERE` clause filters the result set to only include rows where the following condition is *true*.
- The `>` is an operator. Operators create a condition that can be evaluated as either *true* or *false*.
- Comparison operators used with the WHERE clause are:
	* `=` equal to
	* `!=` not equal to
	* `>` greater than
	* `<` less than
	* `>=` greater than or equal to
	* `<=` less than or equal to

- `LIKE` can be a useful operator when you want to compare similar values.
- `LIKE` is a special operator used with the `WHERE` clause to search for a specific pattern in a column. Is not case sensitive.
- The percentage sign `%` is another wildcard character that can be used with `LIKE`. Matches zero or more missing characters in the pattern. For example:
	* `A%` matches all movies with names that begin with letter ‘A’
	* `%a` matches all movies that end with ‘a’
- We can also use `%` both before and after a pattern.

- Unknown values are indicated by `NULL`.
- It is not possible to test for `NULL` values with comparison operators, such as `=` and `!=`. Instead, we will have to use these operators -> `IS NULL` or `IS NOT NULL`.

- The `BETWEEN` operator is used in a `WHERE` clause to filter the result set within a certain range. It accepts two values that are either numbers, text or dates. For example:
```SQL
SELECT *
FROM movies
WHERE year BETWEEN 1990 AND 1999;
```
- When the values are text, `BETWEEN` filters the result set for within the alphabetical range. `BETWEEN` is case-sensitive. 
- For example, in the following statement, `BETWEEN` filters the result set to only include movies with names that begin with the letter ‘A’ up to, but not including ones that begin with ‘J’.
```SQL
SELECT *
FROM movies
WHERE name BETWEEN 'A' AND 'J';
```

- Sometimes we want to combine multiple conditions in a `WHERE` clause to make the result set more specific and useful. One way of doing this is to use the `AND` operator. For example:
```SQL
SELECT * 
FROM movies
WHERE year BETWEEN 1990 AND 1999
   AND genre = 'romance';
```
- With `AND`, both conditions must be true for the row to be included in the result.

- Similar to `AND`, the `OR` operator can also be used to combine multiple conditions in `WHERE`, but there is a fundamental difference:
	* `AND` operator displays a row if all the conditions are true.
	* `OR` operator displays a row if any condition is true.
- With `OR`, if any of the conditions are true, then the row is added to the result.

- We can sort the results using `ORDER BY`, either alphabetically or numerically. Sorting the results often makes the data more useful and easier to analyze.
	* `DESC` is a keyword used in `ORDER BY` to sort the results in descending order (high to low or Z-A).
	* `ASC` is a keyword used in `ORDER BY` to sort the results in ascending order (low to high or A-Z).
- The column that we `ORDER BY` doesn’t even have to be one of the columns that we’re displaying.
- Note: `ORDER BY` always goes after `WHERE` (if WHERE is present).



### 2.4 Aggregate Functions


### 2.5 Multiple Tables





