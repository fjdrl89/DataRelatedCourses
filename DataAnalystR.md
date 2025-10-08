# Data Analysis in R: Notes (based on DataCamp’s Data Analyst with R)

## 1. Introduction to R

### 1.1 Basics of R & the R environment

**What is R?**

* R is a programming language and environment designed for statistics, data analysis, and visualization.
* It is open-source and has a rich ecosystem (CRAN, Bioconductor, etc.).
* You can run R interactively (in console, RStudio) or via scripts (`.R` files) or literate programming notebooks (`.Rmd`, `.qmd`).

**R Interfaces / Workflow**

* **R console / REPL** — type expressions interactively.
* **Scripts** — plain text `.R` files; useful for reproducibility.
* **R Markdown / Quarto** — combine narrative text + code + output (figures, tables).
* **RStudio** — popular IDE that integrates console, script panes, visualizations, package manager.

**Comments**
Use `#` to write comments; they are ignored in execution:

```r
# This is a comment
x <- 5  # assign 5 to x
```

**Basic arithmetic & arithmetic with vectors**

R supports standard arithmetic operators: `+`, `-`, `*`, `/`, `^`, `%%`, `%/%` (mod, integer division).

```r
2 + 3
5 * 4
10 / 3
10 %% 3   # remainder = 1
10 %/% 3  # integer division = 3
2 ^ 3     # 8
```

Because R is vectorized, arithmetic is often applied elementwise:

```r
a <- c(2, 4, 6, 8)
b <- c(1, 3, 5, 7)
a + b    # c(3, 7, 11, 15)
a * 2    # c(4, 8, 12, 16)
a / b    # c(2/1, 4/3, 6/5, 8/7)
```

---

### 1.2 Vectors

Vectors are the foundational one-dimensional data structure in R.

#### Types / Modes of vectors

Each vector has a *mode* (or type). Common ones:

* Numeric (double, integer)
* Character
* Logical (TRUE / FALSE)
* Complex (less frequently used)
* Factor (categorical, but internally stored as integer + levels) — technically not *just* a vector, but worth noting now.

You can inspect a vector’s type using:

```r
x <- c(1.2, 3.4, 5.6)
class(x)    # “numeric”
typeof(x)   # “double”

y <- c("a", "b", "c")
class(y)    # “character”

z <- c(TRUE, FALSE, TRUE)
class(z)    # “logical”
```

Also, you can coerce between types (with caution), e.g. `as.numeric()`, `as.character()`, `as.logical()`.

#### Common helper functions

Useful functions that operate on vectors:

| Function            | Description                                  | Example                    |
| ------------------- | -------------------------------------------- | -------------------------- |
| `sum(x)`            | Sum of all elements                          | `sum(c(1,2,3)) == 6`       |
| `mean(x)`           | Arithmetic mean                              | `mean(c(2,4,6)) == 4`      |
| `median(x)`         | Median                                       | `median(c(1, 5, 10)) == 5` |
| `sd(x)`             | Sample standard deviation                    | —                          |
| `var(x)`            | Sample variance                              | —                          |
| `min(x)` / `max(x)` | Minimum / Maximum value                      | —                          |
| `range(x)`          | c(min, max)                                  | —                          |
| `length(x)`         | Number of elements                           | —                          |
| `sort(x)`           | Sorts the elements                           | —                          |
| `rev(x)`            | Reverses the element order                   | —                          |
| `unique(x)`         | Unique values                                | —                          |
| `which(x)`          | Indices of TRUE elements in a logical vector | —                          |

Example:

```r
x <- c(5, 3, 8, 1, 3)
sum(x)      # 20
mean(x)     # 4
sd(x)
unique(x)   # c(5, 3, 8, 1)
which(x == 3)  # indices where x equals 3 → c(2, 5)
```

#### Subsetting and indexing

One of the most powerful features of R is subsetting. You can extract parts of a vector using `[]`.

* **Numeric (positional) indexing**
  R indexes start at **1** (not 0).

  ```r
  v <- c(10, 20, 30, 40, 50)
  v[1]         # 10
  v[3]         # 30
  v[c(1, 5)]    # c(10, 50)
  v[2:4]        # c(20, 30, 40)
  ```

* **Negative indexing (exclusion)**
  You can exclude certain indices by using negative numbers:

  ```r
  v[-1]       # all except the first → c(20, 30, 40, 50)
  v[-c(2,4)]  # exclude 2nd and 4th → c(10, 30, 50)
  ```

* **Logical indexing / Boolean masking**
  Use a logical (TRUE / FALSE) vector of the same length:

  ```r
  x <- c(-1, 0, 5, 8, -3)
  sel <- x > 0            # produces c(FALSE, FALSE, TRUE, TRUE, FALSE)
  x[sel]                  # c(5, 8)
  # You can do it in one line:
  x[x > 0]                # c(5, 8)
  ```

* **Named vectors**
  You can assign names to elements, then use names to index:

  ```r
  w <- c(a = 10, b = 20, c = 30)
  w["b"]     # 20
  w[c("a", "c")]  # c(a = 10, c = 30)
  ```

* **Indexing out-of-range or invalid indices**

  * Accessing beyond the length returns `NA` (e.g. `v[10]` if `v` has length 5).
  * Using a logical vector of wrong length throws an error or recycles (but be careful with recycling rules).

#### Logical / comparison operators

| Operator | Description                       |
| -------- | --------------------------------- |
| `<`      | Less than                         |
| `>`      | Greater than                      |
| `<=`     | Less than or equal to             |
| `>=`     | Greater than or equal to          |
| `==`     | Equal to (note: use `==` not `=`) |
| `!=`     | Not equal to                      |

These produce a logical vector when comparing elementwise:

```r
a <- c(1, 5, 10)
a > 5       # c(FALSE, FALSE, TRUE)
a == 5      # c(FALSE, TRUE, FALSE)
a != 1      # c(FALSE, TRUE, TRUE)
```

Also, **logical operators** combining conditions:

* `&` : elementwise “AND”
* `|` : elementwise “OR”
* `!` : logical NOT

```r
x <- c(2, 4, 6, 8, 10)
# pick elements that are > 3 AND <= 8
x[ x > 3 & x <= 8 ]  # c(4, 6, 8)
# elements that are < 3 OR > 8
x[ x < 3 | x > 8 ]   # c(2, 10)
# invert a logical vector
!(x > 5)  # c(TRUE, TRUE, FALSE, FALSE, FALSE)
```

#### Exercise suggestions

1. Let `y <- -5:5`. Extract only the **even** values (hint: use `%%` and logical indexing).
2. Given `z <- c(3, 7, 3, 9, 7, 10)`, find which positions correspond to the **maximum** value.
3. Create a named vector `scores <- c(Alice = 85, Bob = 92, Carol = 78)` and then extract Bob’s score.

---

### 1.3 Matrices

Matrices are the next step up: two-dimensional arrays with **rows** and **columns**, storing elements all of the **same type**.

#### Creating matrices

Use `matrix()` or convert from vectors:

```r
# Combine data into a vector
new_hope      <- c(460.9, 314.4, 775.3)
empire_strike <- c(290.4, 247.9, 538.3)
return_jedi   <- c(309.3, 165.8, 475.1)

box_office <- c(new_hope, empire_strike, return_jedi)

# Create a 3 × 3 matrix, filled by rows
sw_mat <- matrix(
  box_office,
  nrow = 3,
  byrow = TRUE
)

# Assign row and column names
rownames(sw_mat) <- c("New Hope", "Empire", "Jedi")
colnames(sw_mat) <- c("US", "Non-US", "Worldwide")

sw_mat
```

That gives:

```
           US Non-US Worldwide
New Hope   460.9   314.4     775.3
Empire     290.4   247.9     538.3
Jedi       309.3   165.8     475.1
```

Alternatively, filling by **columns** (default `byrow = FALSE`):

```r
m2 <- matrix(box_office, nrow = 3, byrow = FALSE)
```

#### Accessing / indexing matrices

Much like vectors, but with two dimensions using `[row, column]`.

* **Single element**:

  ```r
  sw_mat[2, 3]   # 2nd row, 3rd column → 538.3
  ```

* **Entire row or column**:

  ```r
  sw_mat[1, ]    # first row, all columns → c(460.9, 314.4, 775.3)
  sw_mat[, 2]    # second column, all rows → c(314.4, 247.9, 165.8)
  ```

* **Submatrices / slices**:

  ```r
  sw_mat[1:2, 2:3] # rows 1–2, columns 2–3
  ```

* **Named indexing**:

  ```r
  sw_mat["Jedi", "Non-US"]  # 165.8
  ```

* **Logical indexing** (vectors or matrices of TRUE/FALSE):

  ```r
  sw_mat > 300        # gives a matrix of TRUE / FALSE
  sw_mat[sw_mat > 300]  # returns a vector of elements > 300
  ```

* **Negative indexing** (exclude rows/columns):

  ```r
  sw_mat[-1, ]   # drop first row
  sw_mat[, -3]   # drop third column
  ```

#### Matrix operations

Because a matrix is a special case of an array, many vector operations extend, but also there are **matrix-specific** operations.

* **Elementwise arithmetic**:

  ```r
  2 * sw_mat        # each element multiplied by 2
  sw_mat + sw_mat   # elementwise sum
  ```

* **Matrix multiplication**:

  Use `%*%` for matrix multiplication (inner dimension must match):

  ```r
  A <- matrix(1:6, nrow = 2)
  B <- matrix(6:1, nrow = 3)
  # A is 2×3, B is 3×2 → result is 2×2
  A %*% B
  ```

* **Transpose**:

  ```r
  t(sw_mat)
  ```

* **Matrix inverse / solving linear systems** (for square, nonsingular matrices):

  ```r
  M <- matrix(c(2, 3, 1, 4), nrow = 2)
  solve(M)        # inverse
  det(M)          # determinant
  ```

* **Row/column sums and means**:

  ```r
  rowSums(sw_mat)
  colSums(sw_mat)
  rowMeans(sw_mat)
  colMeans(sw_mat)
  ```

#### Exercises to deepen understanding

1. Using `sw_mat` above, what fraction of **Worldwide** box office comes from **US** (i.e. compute `US / Worldwide` for each movie)?
2. Return the names of the movies for which **Non-US** box office > 200.
3. Create a 4 × 5 matrix filled with random integers (1–100). Compute the sum of each row and column.

---



























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


