# Chapter 1: Simple Linear Regression

**Course:** Introduction to Regression in R (DataCamp)  
**Instructor:** Richie Cotton

This chapter introduces **regression models**, focusing on **simple linear regression** (one explanatory variable). We cover the core jargon, how to visualize relationships between two variables, how to fit a linear model with `lm()`, how to interpret the intercept and slope, and what happens when the explanatory variable is categorical.

---

## 1. A Tale of Two Variables

### Swedish Motor Insurance Data

A classic simple dataset with only two numeric variables:

- Each row = one geographic region in Sweden (63 rows total).
- `n_claims` — number of insurance claims in that region.
- `total_payment_sek` — total payment made by the insurer (in Swedish kronor).

```r
library(dplyr)

swedish_motor_insurance %>%
  summarize_all(mean)
#   n_claims  total_payment_sek
#       22.9               98.2

swedish_motor_insurance %>%
  summarize(correlation = cor(n_claims, total_payment_sek))
#   correlation
#         0.881
```

There is a **strong positive correlation** ($r \approx 0.88$): regions with more claims tend to have higher total payments.

### What is Regression?

**Regression models** are statistical models that explore the relationship between:

- a **response variable** (the variable you want to predict), and
- one or more **explanatory variables** (the variables that help explain / predict the response).

Given values of the explanatory variable(s), the model lets you **predict** the value of the response.

**Example thought experiment**  
If a region had 200 claims, how much would the insurance company expect to pay?

### Jargon

| Term                          | Also known as          | Meaning                                      |
|-------------------------------|------------------------|----------------------------------------------|
| Response variable             | Dependent variable     | The variable you want to predict             |
| Explanatory variable(s)       | Independent variable(s)| The variable(s) used to make the prediction  |

The two pairs of terms are completely interchangeable.

### Linear vs. Logistic Regression

| Type of regression | Response variable type | Scope in this course          |
|--------------------|------------------------|-------------------------------|
| Linear regression  | Numeric                | Yes (Chapters 1–3)            |
| Logistic regression| Logical (TRUE/FALSE)   | Yes (Chapter 4)               |

**Simple** linear / logistic regression = only **one** explanatory variable.

### Visualizing Two Numeric Variables

Always start with a scatterplot:

```r
library(ggplot2)

ggplot(swedish_motor_insurance, aes(n_claims, total_payment_sek)) +
  geom_point()
```

Add a **linear trend line** with `geom_smooth()`:

```r
ggplot(swedish_motor_insurance, aes(n_claims, total_payment_sek)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE)
```

- `method = "lm"` forces a straight line from a linear model.
- `se = FALSE` removes the standard-error ribbon (the shaded band).

The trend line follows the data reasonably well → a linear model is a sensible starting point.

---

## 2. Fitting a Linear Regression

### Anatomy of a Straight Line

A straight line is completely defined by two numbers:

- **Intercept** — the value of $y$ when $x = 0$.
- **Slope** — the amount $y$ increases when $x$ increases by 1.

The equation is:

$$
y = \text{intercept} + \text{slope} \times x
$$

### Estimating Intercept and Slope by Eye

Looking at the trend line for the insurance data:

1. The line crosses the vertical axis slightly below 20 → intercept $\approx 20$.
2. Choose two convenient points on the line:
   - From $x = 40$ to $x = 110$ (difference of $70$).
   - Corresponding $y$ values $\approx 150$ and $\approx 400$ (difference of $250$).
3. Slope $\approx 250 / 70 \approx 3.5$.

### Running the Model with `lm()`

```r
lm(total_payment_sek ~ n_claims, data = swedish_motor_insurance)
```

**Output (coefficients only):**

```
Coefficients:
(Intercept)    n_claims
     19.994       3.414
```

The fitted equation is:

$$
\hat{y} = 19.994 + 3.414\, x
$$

where $y$ = `total_payment_sek` and $x$ = `n_claims`.

**Interpretation**

- Intercept $\approx 20$: when there are zero claims, the model predicts a payment of about $20$ SEK (a baseline amount).
- Slope $\approx 3.414$: **for every additional claim**, the total payment is expected to increase by approximately $3.414$ SEK.

Our visual estimates (20 and 3.5) were very close to the exact least-squares solution.

---

## 3. Categorical Explanatory Variables

### Fish Dataset

- 128 fish sold at a market.
- Variables of interest: `species` (categorical, 4 levels) and `mass_g` (numeric response).

Species: Bream, Perch, Pike, Roach.

### Visualizing Numeric + Categorical

Scatterplots are not ideal. Prefer **histograms faceted by the categorical variable**:

```r
ggplot(fish, aes(mass_g)) +
  geom_histogram(bins = 9) +
  facet_wrap(vars(species))
```

### Group Means

```r
fish %>%
  group_by(species) %>%
  summarize(mean_mass_g = mean(mass_g))
```

| species | mean mass (g) |
|---------|---------------|
| Bream   | 618           |
| Perch   | 382           |
| Pike    | 719           |
| Roach   | 152           |

### Fitting the Model

```r
lm(mass_g ~ species, data = fish)
```

**Coefficients:**

```
(Intercept)  speciesPerch  speciesPike  speciesRoach
      617.8        -235.6        100.9        -465.8
```

**How to read them**

- The **intercept** is the mean of the **reference level** (the first level of the factor, here Bream $\approx 617.8$ g).
- The other coefficients are **differences from the reference**:
  - Mean Perch $= 617.8 - 235.6 = 382.2$
  - Mean Pike $= 617.8 + 100.9 = 718.7$
  - Mean Roach $= 617.8 - 465.8 = 152.0$

This “difference-from-reference” coding is the default in R and becomes very useful once you have multiple explanatory variables. For a single categorical predictor it can look confusing at first.

### Removing the Intercept

Force every coefficient to be an absolute mean (relative to zero) by adding `+ 0` (or equivalently `- 1`):

```r
lm(mass_g ~ species + 0, data = fish)
```

**Coefficients now:**

```
speciesBream  speciesPerch  speciesPike  speciesRoach
       617.8         382.2        718.7         152.0
```

These are exactly the group means we calculated earlier.

**Key takeaway**  
When the only explanatory variable is a single categorical variable, a linear regression simply estimates the **mean of the response in each category**.

---

## Quick Reference

### Core Concepts

| Concept                    | Meaning / Formula                                      |
|----------------------------|--------------------------------------------------------|
| Response variable          | Variable you want to predict (y)                       |
| Explanatory variable       | Variable used for prediction (x)                       |
| Simple linear regression   | One numeric explanatory variable                       |
| Intercept                  | Predicted $y$ when $x = 0$                             |
| Slope                      | Change in predicted $y$ for a 1-unit increase in $x$   |
| Model equation             | $\hat{y} = b_0 + b_1 x$                                |

### Essential R Code

| Task                                      | Code                                                                 |
|-------------------------------------------|----------------------------------------------------------------------|
| Scatterplot                               | `ggplot(df, aes(x, y)) + geom_point()`                               |
| Add linear trend line                     | `+ geom_smooth(method = "lm", se = FALSE)`                           |
| Fit simple linear regression              | `lm(y ~ x, data = df)`                                               |
| Fit with categorical explanatory          | `lm(y ~ cat_var, data = df)`                                         |
| Force coefficients = group means          | `lm(y ~ cat_var + 0, data = df)`                                     |
| Correlation                               | `cor(x, y)`                                                          |
| Group means                               | `df %>% group_by(cat) %>% summarize(mean_y = mean(y))`               |
| Histograms by category                    | `ggplot(df, aes(y)) + geom_histogram() + facet_wrap(vars(cat))`      |

### Interpreting Coefficients (Numeric Explanatory)

- **Intercept** ($b_0$): predicted response when the explanatory variable is zero.
- **Slope** ($b_1$): expected change in the response for a one-unit increase in the explanatory variable.

### Interpreting Coefficients (Categorical Explanatory)

- Default: intercept = mean of reference level; other coefficients = differences from that mean.
- With `+ 0`: each coefficient = mean of that level.

---

## Chapter Summary

- **Regression** models the relationship between a response variable and explanatory variable(s) so we can make predictions.
- **Simple linear regression** uses one explanatory variable and assumes a straight-line relationship.
- A straight line is defined by an **intercept** and a **slope**; we obtain them with `lm(y ~ x, data = df)`.
- Always **visualize** first (scatterplot + trend line for numeric; faceted histograms for categorical).
- When the explanatory variable is **categorical**, the model estimates (or contrasts) the group means of the response.
- Adding `+ 0` to the formula removes the intercept and makes the coefficients equal the group means directly.

In the next chapter we will use fitted models to make predictions and dig deeper into the meaning of the coefficients.



```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```


