# Data Analysis in R: Notes (based on DataCamp’s Data Analyst with R)

## 1. Introduction to R

### 1.1 Intro to basics

**What this subsection gives you:** a conceptual map of how R “thinks”, plus small runnable
snippets. You’ll learn what objects are, how R stores values (types/classes/attributes), how
missing values behave, why R is called *vectorized*, and how to help yourself with the help system.

---

#### 1) How R “thinks”: objects, names, and environments (concept)

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

#### 2) Types, classes, and attributes (concept + examples)

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

#### 3) Missing values and special numbers (concept + examples)

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

#### 4) Vectorization (concept before details in §1.2)

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

#### 5) Equality and floating point (concept + examples)

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

#### 6) Assignment, printing, and naming (practice)

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

#### 7) Getting help and self-service (concept + examples)

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

#### Templates (generic code + expected outputs)

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

#### Tips / Hints / Hacks (optional, add where useful)

* Prefer `<-` for assignments and `=` for function arguments: clearer and less error-prone.
* Use `seq_len(n)` / `seq_along(x)` (instead of `1:n`) to avoid zero-length traps later.
* `all.equal()` for floats; or `round(x, k)` then compare if appropriate.
* Keep snippets **small and runnable**; verify outputs frequently.

---

#### Micro-exercises

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


### 1.2 Vectors

**Concept:** A (base) vector in R is a **homogeneous**, one-dimensional container: every element has
the same **atomic type** (logical, integer, double, character, complex, raw). Vectors have a
**length** and can optionally have **names**. They power almost all data manipulation in R because
most operations are **vectorized**.

**Not a vector:** A **list** is heterogeneous (elements can have different types/shapes). We’ll use
lists later; here we focus on atomic vectors.

---

#### What vectors are used for (why it matters)

* **Filtering & slicing**: pick observations by conditions (`x[x > 0]`).
* **Vectorized transforms**: compute element-wise (`x*2`, `log(x)`), fast and concise.
* **Alignment & matching**: find positions with `match()`, membership with `%in%`.
* **Dedup & ordering**: `unique()`, `duplicated()`, `order()`, `sort()`.

---

#### Creating vectors

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

#### Types, coercion, and inspection

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

#### Names (optional metadata)

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

#### Vectorized arithmetic and recycling

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

#### Logical vectors and comparisons

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

#### Subsetting vectors

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

#### Membership, alignment, ordering, deduplication

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

#### Missing values (NA) in vectors

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

#### Templates (generic code + expected outputs)

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

#### Tips / Hints / Hacks (optional)

* Count matches with `sum(cond)` and compute shares with `mean(cond)`.
* Prefer `seq_len(n)` / `seq_along(x)` for robustness.
* Use `match()` for fast lookups and alignment; `%in%` for membership tests.
* Make recycling deliberate with `rep(..., length.out = ...)`.
* Pre-allocate with `numeric(n)`/`logical(n)` when growing vectors in loops.

---

#### Micro-exercises

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

### 1.3 Matrices

**Concept:** A **matrix** is a 2D, **homogeneous** data structure: all entries share the same atomic
type (logical, integer, double, character, etc.). Internally, a matrix is just a **vector** with a
`dim` attribute `(nrow, ncol)`, stored in **column-major** order (fills down columns first).

**Why it matters:** Matrices give you compact 2D math (summaries by row/column, linear algebra), and
they teach key ideas you’ll reuse with higher-level structures (e.g., `data.frame`, `array`, `tibble`).

---

#### Creating matrices

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

#### Dimensions, names, and metadata

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

#### Indexing fundamentals

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

#### Binding, reshaping, and transpose

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

#### Element-wise vs matrix operations

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

#### Row/column summaries (fast path)

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

#### Logical selection by rows/columns

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

#### Templates (generic code + expected outputs)

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

#### Tips / Hints / Hacks (optional)

* Use `drop = FALSE` whenever you select a single row/col in pipelines.
* For repeated summaries, prefer `rowSums()`/`colSums()` over `apply()` for speed.
* When building large matrices iteratively, pre-allocate the final `matrix(nrow, ncol)` and fill by
  indices to avoid copies.
* Make recycling explicit in `cbind()`/`rbind()` by using `rep(..., length.out = ...)`.
* Remember column-major order when mentally mapping linear indices (`m[k]`).

---

#### Micro-exercises

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

---

#### Worked example — Star Wars box office (concise, end-to-end)

```r
# Build a 3x2 matrix with row/col names (original trilogy)
box_office <- c(
  460.998, 314.400,   # A New Hope: US, non-US
  290.475, 247.900,   # The Empire Strikes Back
  309.306, 165.800    # Return of the Jedi
)
region <- c("US", "non-US")
titles <- c("A New Hope", "The Empire Strikes Back", "Return of the Jedi")

star_wars_matrix <- matrix(
  box_office, nrow = 3, byrow = TRUE,
  dimnames = list(titles, region)
)
star_wars_matrix
#>                          US non-US
#> A New Hope           460.998 314.400
#> The Empire Strikes Back 290.475 247.900
#> Return of the Jedi   309.306 165.800

# Worldwide totals per movie (row-wise) and add as a new column
worldwide_vector <- rowSums(star_wars_matrix)
all_wars_with_worldwide <- cbind(star_wars_matrix, Worldwide = worldwide_vector)
all_wars_with_worldwide
#>                          US non-US Worldwide
#> A New Hope           460.998 314.400   775.398
#> The Empire Strikes Back 290.475 247.900   538.375
#> Return of the Jedi   309.306 165.800   475.106
```

Add a second trilogy and **row-bind** (illustrative numbers for demo):

```r
prequel_box <- c(
  431.100, 552.500,   # The Phantom Menace: US, non-US (illustrative)
  310.700, 338.700,   # Attack of the Clones (illustrative)
  380.300, 468.500    # Revenge of the Sith (illustrative)
)
prequel_titles <- c("The Phantom Menace", "Attack of the Clones", "Revenge of the Sith")

star_wars_matrix2 <- matrix(
  prequel_box, nrow = 3, byrow = TRUE,
  dimnames = list(prequel_titles, region)   # same 2 column names: US, non-US
)
pre_worldwide <- rowSums(star_wars_matrix2)

both_trilogies <- rbind(
  all_wars_with_worldwide,
  cbind(star_wars_matrix2, Worldwide = pre_worldwide)
)
both_trilogies
#>                          US non-US Worldwide
#> A New Hope              460.998 314.4   775.398
#> The Empire Strikes Back 290.475 247.9   538.375
#> Return of the Jedi      309.306 165.8   475.106
#> The Phantom Menace      431.100 552.5   983.600
#> Attack of the Clones    310.700 338.7   649.400
#> Revenge of the Sith     380.300 468.5   848.800

# Quick integrity check: sum of row totals equals sum over columns
sum(rowSums(both_trilogies)) == sum(colSums(both_trilogies))
#> [1] TRUE
```

**Tip:** Before `rbind()`, ensure identical column names and order. If needed:

```r
star_wars_matrix2 <- star_wars_matrix2[, colnames(star_wars_matrix), drop = FALSE]
```

---

**Micro-exercises (2 quick checks)**

1. From `both_trilogies`, return the **top-2** movies by `Worldwide`.

   <details><summary>Solution</summary>

   ```r
   both_trilogies[order(both_trilogies[, "Worldwide"], decreasing = TRUE)[1:2], , drop = FALSE]
   ```

   </details>

2. Filter rows where **non-US > US**, keep the 2D shape.

   <details><summary>Solution</summary>

   ```r
   keep <- both_trilogies[, "non-US"] > both_trilogies[, "US"]
   both_trilogies[keep, , drop = FALSE]
   ```

   </details>

---

#### Row totals and saga total

```r
# Minimal setup if you don't already have all_wars_matrix (6x2: US, non-US)
if (!exists("all_wars_matrix")) {
  region <- c("US","non-US")
  titles_all <- c("A New Hope","The Empire Strikes Back","Return of the Jedi",
                  "The Phantom Menace","Attack of the Clones","Revenge of the Sith")
  all_wars_matrix <- matrix(
    c(
      460.998, 314.400,   # A New Hope
      290.475, 247.900,   # Empire
      309.306, 165.800,   # Jedi
      431.100, 552.500,   # Phantom  (illustrative)
      310.700, 338.700,   # Attack   (illustrative)
      380.300, 468.500    # Revenge  (illustrative)
    ),
    nrow = 6, byrow = TRUE,
    dimnames = list(titles_all, region)
  )
}

# Per-movie worldwide totals
worldwide <- rowSums(all_wars_matrix)
worldwide
#>           A New Hope The Empire Strikes Back    Return of the Jedi 
#>               775.398                  538.375                  475.106 
#>    The Phantom Menace    Attack of the Clones       Revenge of the Sith 
#>               983.600                  649.400                  848.800

# Total box office for the whole saga (all entries)
saga_total <- sum(all_wars_matrix)
saga_total
#> [1] 4270.679

# Integrity check: rowSums vs colSums totals
sum(worldwide) == sum(colSums(all_wars_matrix))
#> [1] TRUE
```

**Why:** `rowSums()`/`colSums()` give fast, intention-revealing summaries, and `sum()` over a matrix
just sums all entries (after treating it as a vector).

---

#### Selecting matrix elements (column/row slices)

```r
# Entire second column (non-US) for all movies
non_us_all <- all_wars_matrix[, "non-US"]
non_us_all
#>   A New Hope The Empire Strikes Back    Return of the Jedi 
#>        314.4                 247.9                 165.8 
#> The Phantom Menace    Attack of the Clones       Revenge of the Sith 
#>        552.5                 338.7                 468.5
mean(non_us_all)
#> [1] 347.9667

# Non-US revenue for the first two movies only
non_us_some <- all_wars_matrix[1:2, "non-US"]
non_us_some
#> A New Hope The Empire Strikes Back 
#>     314.4                 247.9
mean(non_us_some)
#> [1] 281.15
```

**Tip:** Use names (`"non-US"`) for safer slicing than numeric positions.

---

#### Element-wise arithmetic (scalar)

```r
# Estimate visitors assuming a flat USD 5 ticket
ticket_price <- 5
visitors_fixed <- all_wars_matrix / ticket_price
visitors_fixed[1:3, ]              # peek at first three movies
#>                          US  non-US
#> A New Hope           92.1996  62.880
#> The Empire Strikes Back 58.095  49.580
#> Return of the Jedi   61.8612  33.160
```

**Why:** Matrix arithmetic with scalars applies **element-wise**, just like vectors.

---

#### Element-wise arithmetic with varying prices (matrix)

```r
# Ticket prices that changed over time (illustrative numbers)
ticket_prices_matrix <- matrix(
  c(
    5.0, 5.0,   # A New Hope       (US, non-US)
    5.0, 5.5,   # Empire
    5.0, 6.0,   # Jedi
    6.0, 6.5,   # Phantom
    6.5, 7.0,   # Attack
    7.0, 7.5    # Revenge
  ),
  nrow = 6, byrow = TRUE,
  dimnames = dimnames(all_wars_matrix)
)

# Element-wise division: revenue / price = estimated visitors
visitors_varprice <- all_wars_matrix / ticket_prices_matrix
visitors_varprice[1:3, ]
#>                          US   non-US
#> A New Hope           92.1996 62.88000
#> The Empire Strikes Back 58.095 45.07273
#> Return of the Jedi   61.8612 27.63333
```

**Gotcha:** This is **not** matrix multiplication; `%*%` is for linear algebra with conformable
dimensions. Here we use plain `/` for element-wise division.

---

**Micro-exercises (quick)**

1. Using `all_wars_matrix`, compute the **mean US** revenue for the last three movies only.

   <details><summary>Solution</summary>

   ```r
   mean(all_wars_matrix[4:6, "US"])
   ```

   </details>

2. Create a logical mask for movies where **non-US > US**, then return those rows (keep 2D).

   <details><summary>Solution</summary>

   ```r
   keep <- all_wars_matrix[, "non-US"] > all_wars_matrix[, "US"]
   all_wars_matrix[keep, , drop = FALSE]
   ```

   </details>

Goal: expand subsection 1.4 “Factors” with clear, runnable R examples, pitfalls, and micro-exercises.

### 1.4 Factors

Concept: A **factor** is R’s data type for **categorical** variables (finite set of levels). Internally, a factor is an **integer vector** with a **levels** attribute. Use unordered factors for nominal categories and **ordered** factors for ranked categories.

Why it matters: Correct factor handling drives proper summaries, plotting, and modeling (contrasts, dummy variables, baseline choice).

---

#### Creating factors (nominal)

Idea: Encode a character vector as a factor; by default levels are sorted alphabetically.

Generic pattern

```r
x_chr <- c("catA","catB", ...)
x_fac <- factor(x_chr)               # unordered by default
levels(x_fac)                        # inspect level set
```

Example

```r
sex_chr <- c("Male","Female","Female","Male","Male")
sex_fac <- factor(sex_chr)
sex_fac
levels(sex_fac)
#> [1] Male   Female Female Male   Male  
#> Levels: Female Male
#> [1] "Female" "Male"
```

Gotcha: Since R 4.0, data.frame() does NOT auto-convert strings to factors; set `stringsAsFactors = TRUE` explicitly if you need that behavior.

Micro-exercise
Make a factor from `c("yes","no","no","yes","no")`. What levels do you get?

<details><summary>Solution</summary>

```r
yn_fac <- factor(c("yes","no","no","yes","no"))
levels(yn_fac)
#> [1] "no"  "yes"
```

</details>

---

#### Setting level order and labels

Idea: Control the **order** and **names** of levels at creation or with `levels()`.

Generic pattern

```r
x_fac <- factor(x_chr, levels = c("F","M"), labels = c("Female","Male"))
# or later:
levels(x_fac) <- c("Female","Male")  # order matters!
```

Example

```r
survey_chr <- c("F","M","F","F","M")
survey_fac <- factor(survey_chr, levels = c("F","M"), labels = c("Female","Male"))
table(survey_fac)
#> survey_fac
#> Female   Male 
#>      3      2
```

Gotcha: If you rename with `levels(x) <- ...`, the new vector must match current level order exactly; otherwise you’ll mislabel categories.

Micro-exercise
Create `c("M","F","M")` and map to `c("Male","Female")` in that order.

<details><summary>Solution</summary>

```r
z <- factor(c("M","F","M"), levels = c("M","F"), labels = c("Male","Female"))
levels(z)
#> [1] "Male"   "Female"
```

</details>

---

#### Ordered factors (ordinal)

Idea: Use `ordered=TRUE` and specify `levels=` from lowest to highest to enable meaningful comparisons.

Generic pattern

```r
ord_fac <- factor(x_chr, ordered = TRUE, levels = c("Low","Medium","High"))
ord_fac[1] < ord_fac[2]  # TRUE/FALSE
```

Example

```r
speed_chr <- c("medium","slow","fast","fast","slow")
speed_fac <- factor(speed_chr, ordered = TRUE,
                    levels = c("slow","medium","fast"))
speed_fac[2] < speed_fac[3]
#> TRUE
summary(speed_fac)
#> slow  medium    fast 
#>    2       1       2
```

Gotcha: Comparisons (`<`, `>`) on **unordered** factors are invalid (warning). Order them first if ranking is intended.

Micro-exercise
Create an ordered factor for `c("S","M","L","M")` with order S < M < L and check if the first element is less than the third.

<details><summary>Solution</summary>

```r
sz <- factor(c("S","M","L","M"), ordered = TRUE, levels = c("S","M","L"))
sz[1] < sz[3]
#> TRUE
```

</details>

---

#### Summaries and counts

Idea: `summary()` and `table()` give frequency counts; `prop.table()` yields proportions.

Generic pattern

```r
summary(fac)
table(fac)
prop.table(table(fac))
```

Example

```r
table(speed_fac)
prop.table(table(speed_fac))
#> speed_fac
#>  slow medium   fast 
#>     2      1      2
#> speed_fac
#> slow  medium    fast 
#>  0.4     0.2     0.4
```

Gotcha: `table()` drops unused levels by default in display, but the factor may still carry them (see `droplevels()`).

Micro-exercise
Compute the share of “Male” in `survey_fac`.

<details><summary>Solution</summary>

```r
prop.table(table(survey_fac))["Male"]
#> 0.4
```

</details>

---

#### Dropping unused levels after subsetting

Idea: After filtering, remove levels that no longer appear.

Generic pattern

```r
x2 <- subset_df$fac
x2 <- droplevels(x2)
```

Example

```r
keep_fast <- speed_fac[speed_fac == "fast"]
levels(keep_fast)
#> [1] "slow"   "medium" "fast"
keep_fast <- droplevels(keep_fast)
levels(keep_fast)
#> [1] "fast"
```

Gotcha: Unused levels can cause confusing legends or model matrices; clean them up.

Micro-exercise
Subset `survey_fac` to “Female” only and drop unused levels.

<details><summary>Solution</summary>

```r
f_only <- droplevels(survey_fac[survey_fac == "Female"])
levels(f_only)
#> [1] "Female"
```

</details>

---

#### Changing the reference (baseline) level

Idea: The first level is the baseline in many models. Use `relevel()` to set it.

Generic pattern

```r
fac2 <- relevel(fac, ref = "desired_baseline")
```

Example

```r
survey_ref <- relevel(survey_fac, ref = "Male")
levels(survey_ref)
#> [1] "Male"   "Female"
```

Gotcha: Baseline choice affects model coefficients and interpretation, not predictions.

Micro-exercise
Set “medium” as the baseline in `speed_fac`.

<details><summary>Solution</summary>

```r
speed_ref <- relevel(speed_fac, ref = "medium")
levels(speed_ref)
#> [1] "medium" "slow"   "fast"
```

</details>

---

#### Safe coercion to/from character

Idea: Convert to character for clean printing or string ops; avoid converting factors straight to numeric codes unless you truly want the underlying integer codes.

Generic pattern

```r
as.character(fac)
as.numeric(fac)          # integer codes (1..k), NOT the original numbers
as.numeric(as.character(fac_numlike))  # if labels are numeric strings
```

Example

```r
fac_num <- factor(c("10","20","10"))
as.numeric(fac_num)
as.numeric(as.character(fac_num))
#> 1 2 1
#> 10 20 10
```

Gotcha: `as.numeric(factor(...))` returns the internal codes, not the label values.

Micro-exercise
Turn factor `c("3","1","2")` into numeric vector `c(3,1,2)`.

<details><summary>Solution</summary>

```r
fx <- factor(c("3","1","2"))
as.numeric(as.character(fx))
#> 3 1 2
```

</details>

---

#### Collapsing and relabeling categories (forcats)

Idea: `forcats` functions streamline factor wrangling.

Generic pattern

```r
library(forcats)
fct_recode(f, New = "Old")
fct_relevel(f, "baseline", after = 0)
fct_collapse(f, Group = c("A","B"))
fct_lump_n(f, n = 3)   # keep top 3, lump others
```

Example

```r
# install.packages("forcats") if needed
library(forcats)
animal <- factor(c("Dog","Cat","Dog","Horse","Cat","Ferret"))
f1 <- fct_collapse(animal, Pet = c("Dog","Cat"))
f2 <- fct_lump_n(animal, n = 2)
levels(f1); table(f1)
levels(f2); table(f2)
#> [1] "Pet"    "Ferret" "Horse"
#> f1
#>   Pet Ferret  Horse 
#>     4      1      1
#> [1] "Cat"   "Dog"   "Other"
#> f2
#>   Cat   Dog Other 
#>     2     2     2
```

Gotcha: After collapsing/lumping, consider relabeling and re-ordering to keep plots and summaries interpretable.

Micro-exercise
Keep only the top 1 animal by frequency and lump the rest as “Other”.

<details><summary>Solution</summary>

```r
top1 <- forcats::fct_lump_n(animal, n = 1)
levels(top1); table(top1)
#> [1] "Dog"   "Other"
#> top1
#>   Dog Other 
#>     2     4
```

</details>

---

#### Handling unknown or missing categories

Idea: Encode unknowns explicitly; decide whether `NA` means missing or a valid “Unknown” group.

Generic pattern

```r
x_fac <- factor(x_chr, levels = c("A","B","Unknown"))
x_fac[is.na(x_fac)] <- "Unknown"
x_fac <- droplevels(x_fac)
```

Example

```r
ans <- c("A","B",NA,"B")
ans_fac <- factor(ans, levels = c("A","B","Unknown"))
ans_fac[is.na(ans_fac)] <- "Unknown"
table(ans_fac)
#> ans_fac
#>       A       B Unknown 
#>       1       2       1
```

Gotcha: Converting a factor with labels not in `levels` yields `NA`. Define all expected levels up front to avoid silent NAs.

Micro-exercise
Make `c("Low","Medium",NA,"High")` an ordered factor Low < Medium < High, replacing `NA` with “Medium”.

<details><summary>Solution</summary>

```r
x <- c("Low","Medium",NA,"High")
xf <- factor(x, ordered = TRUE, levels = c("Low","Medium","High"))
xf[is.na(xf)] <- "Medium"
xf
#> [1] Low    Medium Medium High  
#> Levels: Low < Medium < High
```

</details>

---

Tips / Hints / Hacks
• Prefer setting `levels=` and `labels=` at creation time for clarity.
• Use `droplevels()` after filtering to avoid ghost levels.
• Choose the baseline with `relevel()` before modeling; it simplifies interpretation.
• For heavy factor wrangling, `forcats` is concise and safer than manual `levels()` edits.
• Beware `as.numeric(factor)`: it returns codes, not label values.























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


