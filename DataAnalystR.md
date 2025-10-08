# Data Analysis in R: Notes (based on DataCamp’s Data Analyst with R)

## 1. Introduction to R

## 1.1 Intro to basics

**What this subsection gives you:** a conceptual map of how R “thinks”, plus small runnable
snippets. You’ll learn what objects are, how R stores values (types/classes/attributes), how
missing values behave, why R is called *vectorized*, and how to help yourself with the help system.

---

### 1) How R “thinks”: objects, names, and environments (concept)

* **Everything is an object.** Numbers, text, logicals, functions, models, and plots are all objects.
* **Names point to objects.** When you write `x <- 2`, you create an object `2` and bind it to the
  name `x` in the current **environment** (a lookup table for names).
* **You transform objects by calling functions.** `mean(x)` reads `x`, computes a result, and returns
  a **new** object (R has copy-on-modify semantics; small updates may share memory internally).
* **R is 1-indexed** (first position is 1, not 0).

```r
x <- 2
y <- 3
x + y
#> [1] 5
```

**Tip:** In scripts, use `print()` (or wrap in parentheses) if you want values to appear in the
console. The REPL auto-prints, but scripts don’t.

```r
(z <- x^2 + y^2)   # assign and print
#> [1] 13
```

---

### 2) Types, classes, and attributes (concept + examples)

R separates **low-level storage type** (what the object is made of) from **high-level class**
(how functions should treat it). Many objects also carry **attributes** (metadata), like names or
dimensions.

* **Type** (low-level): `logical`, `integer`, `double` (aka numeric), `character`, `complex`, `raw`.
* **Class** (high-level behavior): e.g., `"factor"`, `"Date"`, `"data.frame"`, `"lm"`, etc.
* **Attributes**: e.g., `names`, `dim`, `dimnames`, `levels`, `class`.

```r
a <- 1         # double storage
b <- 1L        # integer storage
c <- "1"       # character storage
typeof(a); typeof(b); typeof(c)
#> [1] "double"
#> [1] "integer"
#> [1] "character"

class(a); class(b); class(c)
#> [1] "numeric"
#> [1] "integer"
#> [1] "character"

v <- c(10, 20, 30)
attributes(v)          # NULL (no attributes yet)
#> NULL
names(v) <- c("a","b","c")
attributes(v)
#> $names
#> [1] "a" "b" "c"
```

**Gotcha (coercion):** Mixing types in a single atomic vector forces a common supertype.

```r
mix <- c(1, TRUE, "3")
mix
#> [1] "1"    "TRUE" "3"
typeof(mix)
#> [1] "character"
```

**Why it matters:**
Knowing **type/class/attributes** helps you predict how objects print, subset, and interact with
functions (e.g., factors vs character; dates vs numbers). It also prevents silent coercion bugs.

---

### 3) Missing values and special numbers (concept + examples)

R distinguishes **missing** from **undefined math** and **infinity**:

* `NA` = **missing/unknown** value (propagates through most computations).
* `NaN` = “Not a Number” (result of undefined math, e.g., `0/0`), also treated as missing.
* `Inf` / `-Inf` = positive/negative infinity (e.g., division by zero with nonzero numerator).

```r
c(NA, NaN, Inf, -Inf)
#> [1]   NA  NaN  Inf -Inf

is.na(NA);  is.nan(NA)
#> [1] TRUE
#> [1] FALSE
is.na(NaN); is.nan(NaN)
#> [1] TRUE
#> [1] TRUE

x <- c(1, NA, 3)
sum(x)
#> [1] NA
sum(x, na.rm = TRUE)
#> [1] 4
```

**Why it matters:**
Real datasets have gaps. If you don’t handle `NA`, summaries become `NA`. Learn `na.rm = TRUE`,
`is.na()`, and later `complete.cases()` for data frames.

**Tip:** Distinguish missingness (data issue) from impossible math (`NaN`) or overflow (`Inf`).

---

### 4) Vectorization (concept before details in §1.2)

R is **vectorized**: many functions and operators work on **whole vectors** at once (internally implemented in C), which is both concise and fast.

```r
nums <- c(1, 2, 3)
nums * 2
#> [1] 2 4 6
log(nums)
#> [1] 0.0000000 0.6931472 1.0986123
```

**Why it matters:**
Thinking in vectors leads to cleaner code (fewer loops), better performance, and fewer off-by-one
errors. Subsetting + vectorized transforms are the backbone of data wrangling.

**Heads-up:** Vector **recycling** and **recycling warnings** will appear in §1.2; matrix indexing and
`drop = FALSE` show up in §1.3.

---

### 5) Equality and floating point (concept + examples)

R (like most languages) uses IEEE-754 floating-point. Many decimals are **not exactly representable**,
so avoid exact `==` on numeric results; use **tolerance-based** checks.

```r
0.1 + 0.2 == 0.3
#> [1] FALSE
all.equal(0.1 + 0.2, 0.3)
#> [1] TRUE
```

**Why it matters:**
Seemingly obvious comparisons can fail and break joins/filters/tests. Use `all.equal()` or round to
a safe precision before comparing.

---

### 6) Assignment, printing, and naming (practice)

* Use `<-` for assignment in code; use `=` for **function arguments** to avoid confusion.
* Names: letters, digits, dots, underscores; do **not** start with a digit.
* Auto-print happens in the console; in scripts call `print()` or wrap in `()`.

```r
mean(x = c(1, 2, 3))   # 'x' is an argument name (OK)
x <- c(1, 2, 3)        # assignment to an object named 'x'
print(mean(x))
#> [1] 2
```

**Tip:** `make.names("2025 value")` creates a syntactic name when you must sanitize column names.

---

### 7) Getting help and self-service (concept + examples)

Use the help system **constantly**. It’s part of the workflow.

```r
?mean                # help page (Usage, Arguments, Value, Examples)
help("sum")
args(mean)           # see function signature
example(mean)        # run official examples
apropos("mean")      # find objects that contain "mean"
vignette()           # list installed vignettes
```

**Why it matters:**
You’ll move faster by reading argument docs (e.g., `na.rm`, `trim`) and scanning examples. Most “how
do I…?” answers are a `?function` away.

---

## Templates (generic code + expected outputs)

Use these as quick patterns in your own scripts.

```r
# Inspect storage, behavior, and metadata
typeof(obj); class(obj); attributes(obj)
#> <"double" | "integer" | "character" | ...>
#> <class names, e.g., "numeric" | "factor">
#> <list of attributes or NULL>

# Handle missing values in summaries
x <- <vector possibly with NAs>
sum(x, na.rm = TRUE); mean(x, na.rm = TRUE)
#> <numeric>
#> <numeric>

# Safe printing in scripts
(res <- some_function(<args>))
#> <result>
```

---

## Tips / Hints / Hacks (optional, add where useful)

* Prefer `<-` for assignments and `=` for function arguments: clearer and less error-prone.
* Use `seq_len(n)` / `seq_along(x)` (instead of `1:n`) to avoid zero-length traps later.
* `all.equal()` for floats; or `round(x, k)` then compare if appropriate.
* Keep snippets **small and runnable**; verify outputs frequently.

---

## Micro-exercises

1. Create three objects: an **integer**, a **double**, and a **character**. Show `typeof()`, `class()`,
   and any **attributes** present.
2. Build a numeric vector containing an `NA` and an `Inf`. Show how `sum()` behaves with and without
   `na.rm = TRUE`. Explain *why* the results differ.
3. Demonstrate a floating-point pitfall and fix it with `all.equal()` (or rounding).

<details>
<summary><strong>Solutions</strong></summary>

```r
# 1
i <- 5L; d <- 5.0; s <- "5"
typeof(i); class(i); attributes(i)
#> [1] "integer"
#> [1] "integer"
#> NULL
typeof(d); class(d); attributes(d)
#> [1] "double"
#> [1] "numeric"
#> NULL
typeof(s); class(s); attributes(s)
#> [1] "character"
#> [1] "character"
#> NULL

# 2
v <- c(2, NA, Inf)
sum(v)
#> [1] Inf
sum(v, na.rm = TRUE)
#> [1] Inf
# Explanation: NA makes many operations NA, but here Inf dominates and sum is Inf.
# If you want to ignore both NA and Inf:
sum(v[is.finite(v)], na.rm = TRUE)
#> [1] 2

# 3
0.1 + 0.2 == 0.3
#> [1] FALSE
all.equal(0.1 + 0.2, 0.3)
#> [1] TRUE
round(0.1 + 0.2, 10) == round(0.3, 10)
#> [1] TRUE
```

</details>
::contentReference[oaicite:0]{index=0}


## 1.2 Vectors

**Concept:** A (base) vector in R is a **homogeneous**, one-dimensional container: every element has
the same **atomic type** (logical, integer, double, character, complex, raw). Vectors have a
**length** and can optionally have **names**. They power almost all data manipulation in R because
most operations are **vectorized**.

**Not a vector:** A **list** is heterogeneous (elements can have different types/shapes). We’ll use
lists later; here we focus on atomic vectors.

---

### What vectors are used for (why it matters)

* **Filtering & slicing**: pick observations by conditions (`x[x > 0]`).
* **Vectorized transforms**: compute element-wise (`x*2`, `log(x)`), fast and concise.
* **Alignment & matching**: find positions with `match()`, membership with `%in%`.
* **Dedup & ordering**: `unique()`, `duplicated()`, `order()`, `sort()`.

---

### Creating vectors

```r
# Combine values
x <- c(2, 4, 6)
x
#> [1] 2 4 6

# Sequences
seq(1, 5, by = 2)
#> [1] 1 3 5
seq_len(3)             # safer than 1:n
#> [1] 1 2 3
seq_along(x)
#> [1] 1 2 3

# Repetition
rep(1:2, times = 3)
#> [1] 1 2 1 2 1 2
rep(5, length.out = 4)
#> [1] 5 5 5 5

# Pre-allocate empty vectors (useful for performance in loops)
numeric(3); logical(2); character(4)
#> [1] 0 0 0
#> [1] FALSE FALSE
#> [1] "" "" "" ""
```

**Gotcha — `:` operator:** `a:b` always uses unit steps (possibly descending). Use `seq()` for
non-unit steps.

```r
1:5
#> [1] 1 2 3 4 5
5:1
#> [1] 5 4 3 2 1
```

**Tip:** Prefer `seq_len(n)` / `seq_along(x)` over `1:n` / `1:length(x)` to avoid zero-length traps.

---

### Types, coercion, and inspection

Vectors have both **storage type** (`typeof()`) and a **behavioral class** (`class()`). Atomic type
is uniform within a vector. If you mix types in `c(...)`, R **coerces** to a common supertype:

```r
mix <- c(1, TRUE, "3")   # coerces to character
mix
#> [1] "1"    "TRUE" "3"
typeof(mix)
#> [1] "character"
```

Common inspections:

```r
v <- c(10, 20, 30)
length(v)
#> [1] 3
typeof(v); class(v); str(v)
#> [1] "double"
#> [1] "numeric"
#>  num [1:3] 10 20 30
```

**Explicit coercion** (use carefully; check results):

```r
as.numeric(c("1","2","3"))
#> [1] 1 2 3
as.logical(c(1, 0, 2))
#> [1]  TRUE FALSE  TRUE

as.numeric("x")   # non-numeric text → NA + warning
#> [1] NA
```

**Why it matters:** Understanding coercion prevents silent bugs (e.g., characters sneaking into
numeric vectors and breaking calculations).

---

### Names (optional metadata)

```r
names(v) <- c("a","b","c")
v
#>  a  b  c 
#> 10 20 30

unname(v)
#> [1] 10 20 30
```

**Why it matters:** Names make slices and printouts clearer; they also help when aligning results by
label rather than position.

---

### Vectorized arithmetic and recycling

```r
x <- c(1, 2, 3)
x * 2
#> [1] 2 4 6
x + c(10, 20, 30)
#> [1] 11 22 33
```

**Recycling:** If lengths differ, R repeats the shorter vector. If the longer length is **not** a
multiple of the shorter, R issues a warning.

```r
x + c(1, 2)
#> [1] 2 4 4
# Console warning: longer object length is not a multiple of shorter object length
```

**Tip:** Make recycling **explicit** when intended:

```r
x + rep(c(1, 2), length.out = length(x))
#> [1] 2 4 4
```

Useful vector helpers:

```r
pmin(c(1,5,3), c(2,2,9))
#> [1] 1 2 3
cumsum(1:4)
#> [1] 1 3 6 10
diff(c(2, 5, 9))
#> [1] 3 4
round(c(1.234, 5.678), 2)
#> [1] 1.23 5.68
```

---

### Logical vectors and comparisons

Logical vectors (`TRUE`/`FALSE`/`NA`) are central for filtering:

```r
x <- c(-1, 0, 1, 2)
cond <- x > 0
cond
#> [1] FALSE FALSE  TRUE  TRUE

sum(cond)      # count TRUEs
#> [1] 2
mean(cond)     # share of TRUEs
#> [1] 0.5
any(x < 0); all(x >= -1)
#> [1] TRUE
#> [1] TRUE
```

**Element-wise vs first-element logic:** `&`, `|`, `!` are vectorized; `&&` and `||` look only at the
**first** element (useful in `if`).

```r
c(TRUE, FALSE) && c(FALSE, TRUE)
#> [1] FALSE
c(TRUE, FALSE) &  c(FALSE, TRUE)
#> [1] FALSE FALSE
```

**Why it matters:** Most data filtering is just “build a logical mask, then subset by it”.

---

### Subsetting vectors

```r
v <- c(10, 20, 30, 40, 50)

# Positional (1-based)
v[c(1, 3, 5)]
#> [1] 10 30 50

# Negative excludes
v[-c(2, 4)]
#> [1] 10 30 50

# Logical mask
v[v > 25]
#> [1] 30 40 50

# Names
names(v) <- c("a","b","c","d","e")
v[c("b","e")]
#>  b  e 
#> 20 50
```

Edge cases:

```r
v[0]          # zero selects nothing
#> numeric(0)
v[10]         # out-of-range → NA
#> [1] NA
```

**Positions from a condition:** `which()` turns a logical mask into indices.

```r
which(v > 25)
#> [1] 3 4 5
```

---

### Membership, alignment, ordering, deduplication

```r
letters_vec <- c("a","b","c","d","b")

# Membership
letters_vec %in% c("b","d")
#> [1] FALSE  TRUE FALSE  TRUE  TRUE

# First-match positions (alignment)
match(c("b","x"), letters_vec)
#> [1] 2 NA

# Unique and duplicates
unique(letters_vec)
#> [1] "a" "b" "c" "d"
duplicated(letters_vec)
#> [1] FALSE FALSE FALSE FALSE  TRUE

# Sorting values and getting order
x <- c(5, 2, 9, 2)
sort(x)
#> [1] 2 2 5 9
order(x)                 # permutation that sorts x
#> [1] 2 4 1 3
x[order(x, decreasing = TRUE)]
#> [1] 9 5 2 2
```

**Set operations:** work on **values** (ignore positions).

```r
u <- c("a","b","c")
w <- c("b","d")
union(u, w)
#> [1] "a" "b" "c" "d"
intersect(u, w)
#> [1] "b"
setdiff(u, w)
#> [1] "a" "c"
setequal(u, c("c","b","a"))
#> [1] TRUE
```

**Why it matters:** These tools power joins/merges, de-dup tasks, and ordering pipelines.

---

### Missing values (NA) in vectors

```r
u <- c(1, NA, 3, NA)
is.na(u)
#> [1] FALSE  TRUE FALSE  TRUE
anyNA(u)
#> [1] TRUE

sum(u)                 # NA propagates
#> [1] NA
sum(u, na.rm = TRUE)   # ignore NA in the summary
#> [1] 4

# Replace NA
replace(u, is.na(u), 0)
#> [1] 1 0 3 0
# or:
ifelse(is.na(u), 0, u)
#> [1] 1 0 3 0
```

**Tip:** For numeric data with `Inf`/`-Inf`, use `is.finite()` to keep only finite values before
summaries.

---

### Templates (generic code + expected outputs)

```r
# Template: subset by condition + count/proportion
x <- <numeric vector>
sel <- <logical condition on x>
x[sel]
#> <elements>
sum(sel); mean(sel)
#> <count>
#> <proportion>

# Template: membership & positions
v <- <vector>
v[v %in% <set>]
which(v %in% <set>)
#> <values>
#> <indices>

# Template: replace NAs
x <- <vector with NAs>
replace(x, is.na(x), <value>)
#> <vector without NAs>
```

---

### Tips / Hints / Hacks (optional)

* Count matches with `sum(cond)` and compute shares with `mean(cond)`.
* Prefer `seq_len(n)` / `seq_along(x)` for robustness.
* Use `match()` for fast lookups and alignment; `%in%` for membership tests.
* Make recycling deliberate with `rep(..., length.out = ...)`.
* Pre-allocate with `numeric(n)`/`logical(n)` when growing vectors in loops.

---

### Micro-exercises

1. `p <- 1:10`. Return all **even** values, then compute their **count** and **share**.
2. Let `names <- c("ana","bob","cara","bob")` and `scores <- c(10, 7, 9, 8)`.
   a) Set `names(scores) <- names`.
   b) Return scores for `c("bob","cara")` in the **same order**.
   c) Return scores sorted **descending**.
3. Show a recycling **warning** with `1:5 + c(1,2)`, then fix it using `rep()` so it’s deliberate.
4. With `z <- c(1, NA, 3, NA, 5)`:
   a) Indices of missing values.
   b) Sum ignoring `NA`.
   c) Replace `NA` with `0`.
5. Use set operations: with `u <- c("a","b","c")`, `w <- c("b","d")`, compute `union`, `intersect`,
   `setdiff(u, w)`, and check `setequal(u, c("c","b","a"))`.

<details>
<summary><strong>Solutions</strong></summary>

```r
# 1
p <- 1:10
ev <- p[p %% 2 == 0]
ev
#> [1]  2  4  6  8 10
sum(p %% 2 == 0)
#> [1] 5
mean(p %% 2 == 0)
#> [1] 0.5

# 2
names <- c("ana","bob","cara","bob")
scores <- c(10, 7, 9, 8)
names(scores) <- names
scores[c("bob","cara")]
#>  bob  cara 
#>    7     9
scores[order(scores, decreasing = TRUE)]
#> ana cara  bob  bob 
#>  10    9    8    7

# 3
1:5 + c(1, 2)
#> [1] 2 4 4 6 6
# Warning in console about non-multiple length
1:5 + rep(c(1, 2), length.out = 5)
#> [1] 2 4 4 6 6

# 4
z <- c(1, NA, 3, NA, 5)
which(is.na(z))
#> [1] 2 4
sum(z, na.rm = TRUE)
#> [1] 9
replace(z, is.na(z), 0)
#> [1] 1 0 3 0 5

# 5
u <- c("a","b","c"); w <- c("b","d")
union(u, w)
#> [1] "a" "b" "c" "d"
intersect(u, w)
#> [1] "b"
setdiff(u, w)
#> [1] "a" "c"
setequal(u, c("c","b","a"))
#> [1] TRUE
```

</details>
::contentReference[oaicite:0]{index=0}

## 1.3 Matrices

**Concept:** A **matrix** is a 2D, **homogeneous** data structure: all entries share the same atomic
type (logical, integer, double, character, etc.). Internally, a matrix is just a **vector** with a
`dim` attribute `(nrow, ncol)`, stored in **column-major** order (fills down columns first).

**Why it matters:** Matrices give you compact 2D math (summaries by row/column, linear algebra), and
they teach key ideas you’ll reuse with higher-level structures (e.g., `data.frame`, `array`, `tibble`).

---

### Creating matrices

```r
# By default, matrix() fills by columns (column-major)
m <- matrix(1:6, nrow = 2, ncol = 3)
m
#>      [,1] [,2] [,3]
#> [1,]    1    3    5
#> [2,]    2    4    6

# Fill by rows
mr <- matrix(1:6, nrow = 2, ncol = 3, byrow = TRUE)
mr
#>      [,1] [,2] [,3]
#> [1,]    1    2    3
#> [2,]    4    5    6
```

**From vectors via `dim`:**

```r
v <- 1:8
dim(v) <- c(2, 4)   # now v is a 2x4 matrix (column-major fill)
v
#>      [,1] [,2] [,3] [,4]
#> [1,]    1    3    5    7
#> [2,]    2    4    6    8
```

**Gotcha (homogeneity):** Mixing types forces coercion:

```r
matrix(c(1, "2", TRUE), nrow = 1)
#>      [,1] [,2] [,3]
#> [1,] "1"  "2"  "TRUE"
```

---

### Dimensions, names, and metadata

```r
nrow(m); ncol(m); dim(m)
#> [1] 2
#> [1] 3
#> [1] 2 3

rownames(m) <- c("r1","r2")
colnames(m) <- c("c1","c2","c3")
m
#>    c1 c2 c3
#> r1  1  3  5
#> r2  2  4  6

# Set both at once
dimnames(m) <- list(c("r1","r2"), c("c1","c2","c3"))
```

**Why it matters:** Names make indexing and printing clearer, and reduce off-by-one mistakes.

---

### Indexing fundamentals

Use `[row, col]`. Blank means “all”.

```r
m["r1", "c2"]
#> [1] 3

m[, "c3"]    # all rows, column "c3"
#> r1 r2 
#>  5  6

m["r2", ]    # row "r2", all columns
#> c1 c2 c3 
#>  2  4  6
```

**Keep the 2D shape with `drop = FALSE`:**

```r
m[1, 2, drop = FALSE]
#>     c2
#> r1   3
```

**Linear (single) index uses column-major order:**

```r
m[3]     # 3rd element, i.e., row 1 col 2 (because it fills down columns)
#> [1] 3
as.vector(m)  # column-wise flatten
#> [1] 1 2 3 4 5 6
```

**Negative, logical, and name-based indexing also work:**

```r
m[-1, ]          # drop first row
#> c1 c2 c3 
#>  2  4  6
m[m > 3]         # logical mask returns a vector (no dims)
#> [1] 4 5 6
m[, c("c1","c3")]
#>    c1 c3
#> r1  1  5
#> r2  2  6
```

**Find positions matching a condition:**

```r
which(m > 3, arr.ind = TRUE)
#>     row col
#> r2c2   2   2
#> r1c3   1   3
#> r2c3   2   3
```

**Gotcha:** The default `drop = TRUE` can reduce a 1×n or n×1 result to a vector. Use `drop = FALSE`
whenever you need to preserve matrix shape in pipelines.

---

### Binding, reshaping, and transpose

```r
a <- 1:3; b <- 4:6
cb <- cbind(a, b)   # column-bind
rb <- rbind(a, b)   # row-bind
cb
#>      a b
#> [1,] 1 4
#> [2,] 2 5
#> [3,] 3 6
rb
#>   [,1] [,2] [,3]
#> a    1    2    3
#> b    4    5    6

t(m)               # transpose
#>    r1 r2
#> c1  1  2
#> c2  3  4
#> c3  5  6
```

**Tip:** `cbind()`/`rbind()` recycle shorter vectors to match sizes; make recycling deliberate.

---

### Element-wise vs matrix operations

Element-wise arithmetic uses `+ - * /` and **recycles** as needed:

```r
m * 10
#>    c1 c2 c3
#> r1 10 30 50
#> r2 20 40 60
```

**Linear algebra** uses dedicated operators:

```r
# Matrix multiplication (conformable inner dims)
A <- matrix(1:4, nrow = 2)     # 2x2
B <- matrix(c(2,0,0,2), 2, 2)  # 2x2
A %*% B
#>      [,1] [,2]
#> [1,]    2    6
#> [2,]    4    8

# Crossproducts (often faster/more stable)
crossprod(A)     # t(A) %*% A (2x2)
#>      [,1] [,2]
#> [1,]    5   11
#> [2,]   11   25
tcrossprod(A)    # A %*% t(A) (2x2)
#>      [,1] [,2]
#> [1,]    5    8
#> [2,]    8   13
```

**Why it matters:** Distinguish element-wise (`*`) from matrix multiply (`%*%`) to avoid subtle math
bugs.

---

### Row/column summaries (fast path)

```r
rowSums(m); colSums(m)
#> r1 r2 
#>  9 12
#> c1 c2 c3 
#>  3  7 11
rowMeans(m); colMeans(m)
#> r1 r2 
#>  3  4
#>      c1      c2      c3 
#> 1.50000 3.50000 5.50000
```

**Tip:** Prefer `rowSums()/rowMeans()/colSums()/colMeans()` over `apply()` for speed and clarity.

---

### Logical selection by rows/columns

```r
# Keep rows whose column 'c2' is >= 4
keep <- m[, "c2"] >= 4
m[keep, , drop = FALSE]
#>    c1 c2 c3
#> r2  2  4  6

# Keep columns whose row 'r1' is odd
keep_c <- (m["r1", ] %% 2) == 1
m[, keep_c, drop = FALSE]
#>    c1 c3
#> r1  1  5
#> r2  2  6
```

**Why it matters:** Most real tasks reduce to “build a logical mask from one axis; subset the other”.

---

### Templates (generic code + expected outputs)

```r
# Template: build and label a matrix
M <- matrix(<data>, nrow = <r>, ncol = <c>, byrow = <TRUE/FALSE>)
dimnames(M) <- list(<row_names>, <col_names>)
M
#> <expected 2D table with names>

# Template: safe subsetting (preserve 2D)
M[ <row_idx or mask>, <col_idx or mask>, drop = FALSE ]
#> <sub-matrix>

# Template: row/col summaries
rowSums(M); colSums(M); rowMeans(M); colMeans(M)
#> <numeric vectors>

# Template: positions meeting a condition
which(M < <threshold>, arr.ind = TRUE)
#> <row/col index matrix>
```

---

### Tips / Hints / Hacks (optional)

* Use `drop = FALSE` whenever you select a single row/col in pipelines.
* For repeated summaries, prefer `rowSums()`/`colSums()` over `apply()` for speed.
* When building large matrices iteratively, pre-allocate the final `matrix(nrow, ncol)` and fill by
  indices to avoid copies.
* Make recycling explicit in `cbind()`/`rbind()` by using `rep(..., length.out = ...)`.
* Remember column-major order when mentally mapping linear indices (`m[k]`).

---

### Micro-exercises

1. **By-row build + labels:** Create a `2 x 3` matrix filled **by rows** with `1:6`.
   Name rows `r1, r2` and columns `c1, c2, c3`.
   a) Select the sub-matrix first row × last two columns **without** dropping dimensions.
   b) Compute `rowSums()` and `colSums()`; verify `sum(rowSums) == sum(colSums)`.
2. **Logical selection:** Keep only rows where column `c2` is ≥ 4.
3. **Element-wise vs %*%:** Show the difference between `A * B` and `A %*% B` for two `2x2` matrices.
4. **Positions of a condition:** For your matrix in (1), get all positions where values are **odd**
   (`which(..., arr.ind = TRUE)`).

<details>
<summary><strong>Solutions</strong></summary>

```r
# 1
M <- matrix(1:6, nrow = 2, ncol = 3, byrow = TRUE)
dimnames(M) <- list(c("r1","r2"), c("c1","c2","c3"))
M
#>    c1 c2 c3
#> r1  1  2  3
#> r2  4  5  6
M_sub <- M["r1", c("c2","c3"), drop = FALSE]
M_sub
#>    c2 c3
#> r1  2  3
rs <- rowSums(M); cs <- colSums(M)
rs; cs
#> r1 r2 
#>  6 15
#> c1 c2 c3 
#>  5  7  9
sum(rs) == sum(cs)
#> [1] TRUE

# 2
keep <- M[, "c2"] >= 4
M[keep, , drop = FALSE]
#>    c1 c2 c3
#> r2  4  5  6

# 3
A <- matrix(1:4, nrow = 2)    # 2x2
B <- matrix(c(2,0,0,2), 2, 2) # 2x2
A * B
#>      [,1] [,2]
#> [1,]    2    6
#> [2,]    6    8
A %*% B
#>      [,1] [,2]
#> [1,]    2    6
#> [2,]    4    8

# 4
which(M %% 2 == 1, arr.ind = TRUE)
#>     row col
#> r1c1   1   1
#> r1c3   1   3
#> r2c2   2   2
```

</details>
::contentReference[oaicite:0]{index=0}
































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


