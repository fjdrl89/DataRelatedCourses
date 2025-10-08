
# Data Analysis in R

## 1. Introduction to R

### 1.1 Intro to basics

### 1.2 Vectors

#### Functions and Operations

Core functions are available for common data manipulations:

* The function **`sum()`** calculates the total of all elements within a **vector** (or other data structure).
* The function **`mean()`** is used to calculate the **average** of the elements.

---

#### Subsetting and Indexing Data

To **select elements** from a data structure (such as vectors, matrices, or data frames), **square brackets** (`[]`) are used. The desired elements are indicated between these brackets.

##### Numerical Indexing

* To select a single element, its **numeric position** (or **index**) is placed inside the brackets. For example, to access the first element of a vector named `data_vector`, the syntax is `data_vector[1]`.
* In some programming environments (like R), the **first element has an index of 1**, unlike other languages where the index may start at 0.
* To select **multiple, non-consecutive elements**, a vector of indices is used. For instance, to select the elements at positions 1 and 5, the syntax is `data_vector[c(1, 5)]`.
* For selecting a **range of consecutive elements**, a shorthand notation using the colon operator (`:`) is often available. For example, the vector `c(2, 3, 4)` can be simplified to **`2:4`**, which generates a sequence of natural numbers from 2 up to 4.

##### Named Indexing

* Elements can also be selected by using their **names** instead of their numeric positions, provided the elements have been named.

---

#### Logical Comparison Operators

**Logical comparison operators** are used to create conditional expressions, yielding a **`TRUE`** or **`FALSE`** result. Common operators include:

| Operator | Meaning |
| :---: | :--- |
| **`<`** | Less than |
| **`>`** | Greater than |
| **`<=`** | Less than or equal to |
| **`>=`** | Greater than or equal to |
| **`==`** | Equal to |
| **`!=`** | Not equal to |

---

#### Working with Comparisons

Working with **comparisons** can simplify **data analysis**. Instead of manually selecting a subset of data points for investigation, a user can simply instruct the system (e.g., a programming language like R) to return only those points that meet a specific condition.

For example, a **logical vector** can be created to identify elements that satisfy a condition, such as having a value greater than zero: `selection_vector <- data_vector > 0`. This vector indicates *which* elements meet the criteria.

To retrieve *the values* themselves (not just the indicator of whether they met the criteria), the logical vector is placed within the **square brackets** following the original data vector: `data_vector[selection_vector]`.

When a logical vector is used for **subsetting** in this manner, the system will select and return only the elements from the `data_vector` that correspond to **TRUE** values in the `selection_vector`.

### 1.3 Matrices

#### The Concept of a Matrix

A **matrix** is a fundamental data structure used to organize a collection of elements. It is characterized by its **two-dimensional** arrangement, meaning the elements are structured into a fixed number of **rows** and **columns**. A key characteristic is that all elements within a single matrix must be of the **same data type** (e.g., all numeric, all character, or all logical).

---

#### Matrix Construction

Matrices are typically created using a dedicated function (e.g., the `matrix()` function in R). The construction process involves several key inputs:

1.  **Data Elements:** The input must include the **collection of elements** that will populate the matrix. This is often a single vector containing all values. For convenience, a sequence shortcut (e.g., `1:9` for 1, 2, ..., 9) may be used.
2.  **Dimensions:** The desired structure is defined by specifying the number of **rows** (e.g., using an `nrow` argument) or columns.
3.  **Filling Order:** An argument (e.g., `byrow`) specifies the order in which the data elements should be placed into the matrix:
    * If **TRUE**, the matrix is filled **by row** (moving across the first row, then the second, and so on).
    * If **FALSE**, the matrix is filled **by column** (moving down the first column, then the second, and so on).

##### Example of Construction

A general function call might look like this:

`matrix(data elements, byrow = TRUE/FALSE, nrow = number)`

---

#### Workflow: From Vectors to a Matrix

The practical application of matrix construction often requires two sequential steps: **data preparation** (concatenation) and **structural definition** (using the constructor function).

##### Data Preparation: Concatenation using **`c()`**

Before a matrix can be constructed, its source data must be collected into a single, one-dimensional sequence. This process uses the **concatenation function** **`c()`**.

* **Function:** **`c()`** combines multiple atomic vectors into one longer vector, ensuring the entire dataset is treated as a unified **`data`** stream for the matrix function.
* **Contextual Example:** To unify box office figures from three source vectors (representing different films), the vectors are passed as arguments to **`c()`**:

    `box_office <- c(new_hope, empire_strikes, return_jedi)`

---

##### Structural Definition: Key Matrix Arguments

Once the unified **`data`** vector is prepared, the **`matrix()`** function is used to impose the **two-dimensional structure**. The arguments define how the data is shaped:

1.  **Data Source (First Argument):** The concatenated vector (e.g., `box_office`) serves as the data to be arranged.
2.  **Row Dimension (`nrow`):** This argument explicitly sets the number of **observations** or **entities** the matrix should hold.
    * **Study Point:** Setting `nrow = 3` signifies that each of the three films will occupy exactly one row in the final matrix.
3.  **Filling Order (`byrow`):** This boolean argument is critical for maintaining the **logical grouping** of data elements.
    * **Study Point:** Setting `byrow = TRUE` ensures that all related figures (e.g., all revenue types for the first movie) are placed sequentially along the first row *before* moving to the next row. This is essential when each **row represents a single entity** (like a movie or an experimental subject).

###### Applied Matrix Construction

The entire operation is executed and assigned to a final object:

`star_wars_matrix <- matrix(box_office, nrow = 3, byrow = TRUE)`

**Resulting Structure:** The final object, `star_wars_matrix`, is a **2D structure** where the **rows** categorize the movies and the **columns** categorize the revenue metrics (assuming the original vectors were structured consistently).


























### 1.4 Factors

### 1.5 Data Frames

### 1.6 Lists


## 2. Introduction to the Tidyverse

## 3. Data Manipulation with dplyr

## 4. Project 1 - Analyze the Popularity of Programming Languages

## 5. Joining Data with dplyr

## 6. Introduction to Statistics in R

## 7. Introduction to Data Visualization with ggplot2

## 8. Project 2 - Data Manipulation in R

## 9. Exploratory Data Analysis in R

## 10. Sampling in R

## 11. Hypothesis Testing in R

## 12. Project 3 - Hypothesis Testing with Men's and Women's Soccer Matches


