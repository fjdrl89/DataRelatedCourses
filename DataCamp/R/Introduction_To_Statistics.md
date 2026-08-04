# Chapter 1: Summary Statistics

**Course:** Introduction to Statistics in R (DataCamp)  
**Instructor:** Maggie Matsui

This chapter introduces the fundamentals of statistics: what statistics is, types of data, measures of center, measures of spread, quartiles, the interquartile range (IQR), and how to detect outliers. All examples use the `msleep` dataset (mammal sleep habits) from the `ggplot2` package.

---

## 1. What is Statistics?

Statistics has **two related meanings**:

1. **The field of statistics** — the practice and study of collecting and analyzing data.
2. **A summary statistic** — a single number or fact that summarizes a dataset (e.g., an average, a percentage, a count).

### What can statistics do?

Statistics helps answer practical questions such as:

- How likely is someone to purchase a product? Does a different payment system change that probability?
- How many occupants will a hotel have? How can we optimize occupancy?
- How many jean sizes should be manufactured so they fit 95% of the population?
- **A/B testing**: Which advertisement is more effective at driving purchases?

### What can’t statistics do?

Statistics cannot answer pure “why” questions with certainty.

> ❌ *Why* is *Game of Thrones* so popular?  
> We could survey people, but answers may be incomplete or inaccurate.

> ✅ We *can* ask: “Are series with more violent scenes viewed by more people?”

Even if we find a strong association, **correlation does not imply causation**. Other factors may be responsible.

---

## 2. Types of Statistics

| Branch          | Focus                                              | Example                                                                 |
|-----------------|----------------------------------------------------|-------------------------------------------------------------------------|
| **Descriptive** | Summarize and describe the data you already have   | After surveying 4 friends: 50% drive, 25% bus, 25% bike                 |
| **Inferential** | Use a *sample* to make claims about a *population* | Using the 4-friend sample to estimate the percentage of *all* people who drive |

- Descriptive statistics stay inside the observed dataset.
- Inferential statistics go beyond the sample to make statements about a larger group.

---

## 3. Types of Data

Identifying the data type is essential because it determines which summary statistics and visualizations are appropriate.

### Numeric (Quantitative)

Made of numbers that represent quantities.

| Sub-type     | Description                          | Examples                          |
|--------------|--------------------------------------|-----------------------------------|
| **Continuous** | Measured (can take any value in a range) | Speed, waiting time, height     |
| **Discrete**   | Counted (usually whole numbers)      | Number of pets, packages shipped  |

### Categorical (Qualitative)

Values that belong to distinct groups or categories.

| Sub-type   | Description                     | Examples                                      |
|------------|---------------------------------|-----------------------------------------------|
| **Nominal**  | No inherent order               | Married/unmarried, country of residence       |
| **Ordinal**  | Natural order exists            | Strongly disagree → Strongly agree (Likert)   |

#### Categorical data can be encoded as numbers

It is common to represent categories numerically:

```r
# Nominal
married   <- 1
unmarried <- 0

# Ordinal (order matters)
strongly_disagree <- 1
somewhat_disagree <- 2
neither           <- 3
somewhat_agree    <- 4
strongly_agree    <- 5
```

**Important:** Even when encoded as numbers, these remain *categorical* variables. Calculating a mean of married/unmarried is meaningless.

### Why data type matters

| Data type     | Useful tools                          | Usually *not* useful          |
|---------------|---------------------------------------|-------------------------------|
| Numeric       | Mean, median, SD, histograms, scatterplots | Counts, bar plots            |
| Categorical   | Counts, proportions, bar plots        | Mean, scatterplots            |

Using the wrong tool produces results that do not make sense.

---

## 4. Measures of Center

When we ask “How long do mammals **typically** sleep?”, we are looking for a measure of **center**.

### Quick reminder: Histograms

A histogram groups continuous data into **bins** (ranges of values). The height of each bar shows how many observations fall into that bin. Histograms give a visual sense of the distribution; numerical measures of center give a compact summary.

### The three main measures

| Measure  | Definition                                      | R function              | Sensitive to outliers? |
|----------|-------------------------------------------------|-------------------------|------------------------|
| **Mean**   | Sum of all values ÷ number of observations      | `mean(x)`               | Yes                    |
| **Median** | Middle value after sorting                      | `median(x)`             | No                     |
| **Mode**   | Most frequent value                             | `count(..., sort=TRUE)` | No                     |

#### Mean

```r
mean(msleep$sleep_total)
# 10.43373 hours
```

$$
\bar{x} = \frac{x_1 + x_2 + \dots + x_n}{n}
$$

#### Median

Sort the data and pick the middle value (for 83 observations the 42nd value):

```r
median(msleep$sleep_total)
# 10.1 hours
```

50% of the data lie below the median and 50% lie above it.

#### Mode

Especially useful for categorical variables.

```r
msleep %>% count(sleep_total, sort = TRUE)   # mode of sleep_total = 12.5 h
msleep %>% count(vore, sort = TRUE)          # mode of diet = "herbi"
```

### Which measure should you use?

#### Effect of an outlier

Original insectivores:

```r
mean_sleep   ≈ 16.5 h
median_sleep ≈ 18.9 h
```

After adding a mystery insectivore that sleeps **0 hours**:

```r
mean_sleep   ≈ 13.2 h   # dropped by > 3 hours
median_sleep ≈ 18.1 h   # changed by < 1 hour
```

**Conclusion:** The mean is pulled by extreme values; the median is robust.

#### Shape of the distribution

| Shape          | Appearance                              | Mean vs Median     | Preferred measure |
|----------------|-----------------------------------------|--------------------|-------------------|
| **Symmetric**  | Mirror image left and right             | mean ≈ median      | Either (mean common) |
| **Left-skewed**  | Piled on the right, long tail on the left | mean < median    | Median            |
| **Right-skewed** | Piled on the left, long tail on the right | mean > median   | Median            |

The mean is always pulled in the direction of the long tail. Prefer the **median** when data are skewed or contain outliers.

---

## 5. Measures of Spread

**Spread** describes how close together or far apart the data points are.

### 5.1 Variance

Average of the *squared* distances from each point to the mean.

**Step-by-step calculation:**

```r
dists         <- msleep$sleep_total - mean(msleep$sleep_total)
squared_dists <- dists^2
sum_sq_dists  <- sum(squared_dists)
variance      <- sum_sq_dists / (length(msleep$sleep_total) - 1)
# 19.80568
```

Or simply:

```r
var(msleep$sleep_total)   # 19.80568
```

- Higher variance → more spread out.
- Units are **squared** (hours²), which makes interpretation less intuitive.
- We divide by \(n-1\) (not \(n\)) for an unbiased estimate of the population variance.

### 5.2 Standard Deviation (SD)

Square root of the variance → restores the original units.

```r
sd(msleep$sleep_total)          # 4.450357 hours
sqrt(var(msleep$sleep_total))   # same result
```

A typical mammal’s sleep time is about **4.5 hours** away from the mean of 10.4 hours.

### 5.3 Mean Absolute Deviation (MAD)

Average of the *absolute* distances (no squaring).

```r
mean(abs(msleep$sleep_total - mean(msleep$sleep_total)))
# 3.566701
```

**Key difference:**

- SD **squares** distances → longer distances are penalized more heavily.
- MAD treats every distance equally.
- SD is far more commonly used in practice.

---

## 6. Quartiles, Quantiles & Boxplots

### Quartiles

Values that split the data into four equal parts.

```r
quantile(msleep$sleep_total)
#   0%   25%   50%   75%  100%
# 1.90  7.85 10.10 13.75 19.90
```

- Q1 (25%) = first quartile  
- Q2 (50%) = second quartile = **median**  
- Q3 (75%) = third quartile  

### Boxplots

The box is built from the quartiles:

- Bottom of the box = Q1  
- Top of the box = Q3  
- Line inside the box = median (Q2)

```r
ggplot(msleep, aes(y = sleep_total)) +
  geom_boxplot()
```

### Quantiles (percentiles)

A generalization of quartiles — they can split the data into any number of equal pieces.

```r
# Split into five equal parts (quintiles)
quantile(msleep$sleep_total, probs = seq(0, 1, 0.2))

# Split into ten equal parts (deciles)
quantile(msleep$sleep_total, probs = seq(0, 1, 0.1))
```

---

## 7. Interquartile Range (IQR)

A robust measure of spread: the distance between the 25th and 75th percentiles (the height of the box in a boxplot).

```r
IQR(msleep$sleep_total)   # 5.9 hours
```

IQR is less influenced by outliers than the standard deviation.

---

## 8. Detecting Outliers

**Outlier** = a data point that is substantially different from the others.

**Common rule (Tukey’s method):**

A point is an outlier if:

$$
\text{value} < Q_1 - 1.5 \times \text{IQR}
\quad\text{or}\quad
\text{value} > Q_3 + 1.5 \times \text{IQR}
$$

### Practical example – body-weight outliers

```r
iqr             <- IQR(msleep$bodywt)
lower_threshold <- quantile(msleep$bodywt, 0.25) - 1.5 * iqr
upper_threshold <- quantile(msleep$bodywt, 0.75) + 1.5 * iqr

msleep %>%
  filter(bodywt < lower_threshold | bodywt > upper_threshold) %>%
  select(name, vore, sleep_total, bodywt)
```

Result: 11 outliers, including the cow (600 kg) and the Asian elephant (2547 kg).

---

## Quick Reference – Essential R Functions

| Task                        | Code                                              |
|-----------------------------|---------------------------------------------------|
| Mean                        | `mean(x)`                                         |
| Median                      | `median(x)`                                       |
| Mode (most frequent)        | `count(x, sort = TRUE)`                           |
| Variance                    | `var(x)`                                          |
| Standard deviation          | `sd(x)`                                           |
| Mean absolute deviation     | `mean(abs(x - mean(x)))`                          |
| Quartiles / any quantiles   | `quantile(x, probs = ...)`                        |
| Interquartile range         | `IQR(x)`                                          |
| Boxplot                     | `geom_boxplot()`                                  |
| Find outliers               | `filter(x < Q1 - 1.5*IQR \| x > Q3 + 1.5*IQR)`    |

---

## Chapter Summary – Key Ideas

1. Statistics is both a **field** and a collection of **summary numbers**.
2. Always identify the **data type** first — it dictates the tools you can use.
3. **Mean** is sensitive to outliers and skew; **median** is robust.
4. Use the **median** when data are skewed or contain extreme values.
5. **Standard deviation** is the most common measure of spread (units match the original data).
6. **IQR** is a robust alternative to standard deviation.
7. The **1.5 × IQR rule** is a practical, widely used way to flag potential outliers.

---

# Chapter 1 – Practice Exercises & Solutions

**Course:** Introduction to Statistics in R (DataCamp)  
**Dataset used:** `food_consumption` (2018 Food Carbon Footprint Index by nu3)

These exercises reinforce measures of center, spread, quantiles, and outlier detection.

---

## Exercise 1: Mean and Median

The `food_consumption` dataset contains kilograms of food consumed per person per year in each country, along with the carbon footprint (`co2_emission`).

**Tasks**

1. Filter for Belgium and calculate the mean and median of `consumption`.
2. Filter for USA and calculate the mean and median of `consumption`.
3. Calculate the overall median of `consumption` for all countries.
4. Find the mode of `consumption` by counting and sorting.

### Solution

```r
# Filter for Belgium
belgium_consumption <- food_consumption %>%
  filter(country == "Belgium")

# Filter for USA
usa_consumption <- food_consumption %>%
  filter(country == "USA")

# Mean and median – Belgium
mean(belgium_consumption$consumption)
median(belgium_consumption$consumption)

# Mean and median – USA
mean(usa_consumption$consumption)
median(usa_consumption$consumption)

# Overall median
median(food_consumption$consumption)

# Mode of consumption
food_consumption %>%
  count(consumption, sort = TRUE)
```

---

## Exercise 2: Mean vs. Median (Rice)

Compare mean and median for a skewed variable.

**Tasks**

1. Filter for the `"rice"` food category.
2. Create a histogram of `co2_emission`.
3. Calculate the mean and median of `co2_emission` for rice.

### Solution

```r
# Histogram of co2_emission for rice
food_consumption %>%
  filter(food_category == "rice") %>%
  ggplot(aes(co2_emission)) +
    geom_histogram()

# Mean and median of co2_emission for rice
food_consumption %>%
  filter(food_category == "rice") %>%
  summarise(
    mean_co2   = mean(co2_emission),
    median_co2 = median(co2_emission)
  )
```

---

## Exercise 3: Variance and Standard Deviation

**Tasks**

1. Calculate the variance of `co2_emission`.
2. Calculate the standard deviation of `co2_emission`.

### Solution

```r
var(food_consumption$co2_emission)
sd(food_consumption$co2_emission)
```

---

## Exercise 4: Quartiles, Quintiles and Deciles

**Tasks**

1. Calculate the quartiles of `co2_emission`.
2. Calculate the quintiles (5 equal parts).
3. Calculate the deciles (10 equal parts).

### Solution

```r
# Quartiles
quantile(food_consumption$co2_emission)

# Quintiles
quantile(food_consumption$co2_emission, probs = seq(0, 1, 0.2))

# Deciles
quantile(food_consumption$co2_emission, probs = seq(0, 1, 0.1))
```

---

## Exercise 5: Finding Outliers using IQR

**Tasks**

1. Compute the 25th and 75th percentiles of `co2_emission` and store them as `q1` and `q3`.
2. Calculate the IQR.
3. Calculate the lower and upper cutoffs for outliers.
4. Filter the rows that are outliers.

### Solution

```r
# 25th and 75th percentiles
q1 <- quantile(food_consumption$co2_emission, 0.25)
q3 <- quantile(food_consumption$co2_emission, 0.75)

# IQR
iqr <- q3 - q1

# Lower and upper cutoffs
lower <- q1 - 1.5 * iqr
upper <- q3 + 1.5 * iqr

# Filter outliers
food_consumption %>%
  filter(co2_emission < lower | co2_emission > upper)
```

---

# Chapter 2: Probability and Distributions

**Course:** Introduction to Statistics in R (DataCamp)  
**Instructor:** Maggie Matsui

This chapter covers how we measure chance, sampling with and without replacement, independent vs dependent events, discrete and continuous probability distributions, the law of large numbers, and the binomial distribution.

---

## 1. Measuring Chance

People talk about chance all the time (“What are the chances of closing the sale?”, “What’s the chance of rain tomorrow?”).

**Probability** is the formal way to measure it.

### The basic formula

$$
P(\text{event}) = \frac{\text{number of ways the event can happen}}{\text{total number of possible outcomes}}
$$

**Example – fair coin flip**

$$
P(\text{heads}) = \frac{1 \text{ way to get heads}}{2 \text{ possible outcomes (heads or tails)}} = \frac{1}{2} = 0.5 \ (50\%)
$$

Probability is always between **0** and **1** (or 0% and 100%):

- \(0\) → the event is **impossible**
- \(1\) → the event is **certain**

---

## 2. Assigning Salespeople (A Practical Example)

Imagine a sales team of four people: **Amir, Brian, Claire, Damian**.

We put their names in a box and draw one at random to decide who goes to a client meeting.

$$
P(\text{Brian}) = \frac{1}{4} = 0.25 \ (25\%)
$$

### Doing it in R with `dplyr`

```r
sales_counts
#   name   n_sales
# 1 Amir      178
# 2 Brian     126
# 3 Claire     75
# 4 Damian     69

sales_counts %>% sample_n(1)   # randomly picks one row
```

Every time you run `sample_n(1)` you may get a different person.

To make the result **reproducible**, set a random seed:

```r
set.seed(5)
sales_counts %>% sample_n(1)   # always returns the same person when seed = 5
```

The seed is just a starting point for R’s random-number generator. Any number works; what matters is that you use the **same** seed to get the same result.

---

## 3. Sampling With vs Without Replacement

### Sampling **without** replacement

Brian has already been chosen and cannot attend two meetings at the same time. We remove his name from the box. Now only three names remain.

$$
P(\text{Claire is chosen second}) = \frac{1}{3} \approx 0.333 \ (33\%)
$$

```r
sales_counts %>% sample_n(2)   # two different people
```

### Sampling **with** replacement

The two meetings are on different days, so the same person *could* attend both. We put the name **back** into the box.

$$
P(\text{Claire is chosen second}) = \frac{1}{4} = 0.25 \ (25\%)
$$

```r
sales_counts %>% sample_n(2, replace = TRUE)

# or for 5 meetings on different days:
sample(sales_team, 5, replace = TRUE)
```

---

## 4. Independent vs Dependent Events

| Concept               | Definition                                                                 | Typical sampling              |
|-----------------------|----------------------------------------------------------------------------|-------------------------------|
| **Independent events** | The probability of the second event is **not** affected by the first     | Sampling **with** replacement |
| **Dependent events**   | The probability of the second event **is** affected by the first         | Sampling **without** replacement |

**With replacement (independent)**  
No matter who is picked first, \(P(\text{Claire second}) = 25\%\).

**Without replacement (dependent)**  
- If Claire is picked first → \(P(\text{Claire second}) = 0\%\)  
- If someone else is picked first → \(P(\text{Claire second}) = 33\%\)

> **Rule of thumb**  
> Sampling with replacement → independent trials  
> Sampling without replacement → dependent trials

---

## 5. Discrete Probability Distributions

A **probability distribution** describes the probability of every possible outcome in a scenario.

### Fair six-sided die

| Outcome     | 1          | 2          | 3          | 4          | 5          | 6          |
|-------------|------------|------------|------------|------------|------------|------------|
| Probability | \(1/6\)    | \(1/6\)    | \(1/6\)    | \(1/6\)    | \(1/6\)    | \(1/6\)    |

This is a **discrete uniform distribution** (all outcomes equally likely).

### Expected value (the mean of a distribution)

$$
E(X) = \sum x_i \cdot P(x_i)
$$

For a fair die:

$$
E(X) = 1\cdot\frac{1}{6} + 2\cdot\frac{1}{6} + 3\cdot\frac{1}{6} + 4\cdot\frac{1}{6} + 5\cdot\frac{1}{6} + 6\cdot\frac{1}{6} = 3.5
$$

### Probability = Area under the distribution

$$
P(\text{roll} \le 2) = P(1) + P(2) = \frac{1}{6} + \frac{1}{6} = \frac{1}{3}
$$

### Uneven die (face 2 changed to 3)

| Outcome     | 1          | 2   | 3          | 4          | 5          | 6          |
|-------------|------------|-----|------------|------------|------------|------------|
| Probability | \(1/6\)    | 0   | \(1/3\)    | \(1/6\)    | \(1/6\)    | \(1/6\)    |

New expected value ≈ 3.67.

### Sampling from a discrete distribution in R

```r
die <- data.frame(n = 1:6)

# Simulate 10 rolls (with replacement)
rolls_10 <- die %>% sample_n(10, replace = TRUE)
mean(rolls_10$n)   # sample mean (will vary around 3.5)
```

---

## 6. The Law of Large Numbers

As the size of your sample increases, the **sample mean approaches the theoretical expected value**.

| Sample size | Sample mean (example) |
|-------------|-----------------------|
| 10          | ≈ 3.00                |
| 100         | ≈ 3.36                |
| 1 000       | ≈ 3.53                |

Theoretical mean of a fair die = **3.5**.

This is why larger samples give us more reliable estimates of the true underlying probability.

---

## 7. Continuous Distributions

Discrete distributions work for countable outcomes (dice, number of customers, etc.).  
For continuous variables (time, height, weight, temperature…) we need **continuous distributions**.

### Waiting for the bus (Continuous Uniform Distribution)

A bus arrives every 12 minutes. You arrive at a random time, so your waiting time can be anywhere from 0 to 12 minutes.

Because there are infinitely many possible waiting times, we cannot draw individual bars. Instead we draw a continuous density:

- The density is a **flat line** of height \(1/12\) from 0 to 12.
- This is the **continuous uniform distribution**.

### Probability is still the area under the curve

$$
P(4 \le \text{wait} \le 7) = \text{width} \times \text{height} = 3 \times \frac{1}{12} = 0.25
$$

### Using the uniform distribution in R – `punif()`

```r
# Probability of waiting ≤ 7 minutes
punif(7, min = 0, max = 12)                    # ≈ 0.583

# Probability of waiting > 7 minutes
punif(7, min = 0, max = 12, lower.tail = FALSE) # ≈ 0.417

# Probability of waiting between 4 and 7 minutes
punif(7, min = 0, max = 12) - punif(4, min = 0, max = 12)  # 0.25
```

### Important property of **all** continuous distributions

The **total area under the curve must equal 1** (or 100%).

This will also be true for the normal distribution, the Poisson distribution, and every other continuous distribution you will meet later.

---

## 8. The Binomial Distribution

### Binary outcomes

Many real-world situations have only two possible results:

- Heads / Tails  
- Success / Failure  
- Win / Loss  
- 1 / 0  

These are called **binary outcomes**.

### Simulating coin flips with `rbinom()`

```r
rbinom(n = number of experiments,
       size = number of trials per experiment,
       prob = probability of success)
```

| Code                    | Meaning                          | Result type              |
|-------------------------|----------------------------------|--------------------------|
| `rbinom(1, 1, 0.5)`     | 1 flip of 1 coin                 | single 0 or 1            |
| `rbinom(8, 1, 0.5)`     | 8 flips of 1 coin                | vector of eight 0/1      |
| `rbinom(1, 8, 0.5)`     | 1 experiment of 8 coins          | total number of heads    |
| `rbinom(10, 3, 0.5)`    | 10 experiments of 3 coins each   | vector of 10 totals      |

You can change the probability:

```r
rbinom(10, 3, 0.25)   # biased coin – only 25% chance of heads
```

### Definition of the binomial distribution

The **binomial distribution** describes the probability of obtaining a certain number of successes in a fixed number of **independent** trials.

It is fully characterized by two parameters:

- \(n\) = number of trials  
- \(p\) = probability of success on each trial

### Calculating probabilities

```r
# Probability of exactly 7 heads in 10 flips
dbinom(7, size = 10, prob = 0.5)   # ≈ 0.117

# Probability of 7 or fewer heads
pbinom(7, size = 10, prob = 0.5)   # ≈ 0.945

# Probability of more than 7 heads
pbinom(7, size = 10, prob = 0.5, lower.tail = FALSE)  # ≈ 0.055
# equivalent to:
1 - pbinom(7, size = 10, prob = 0.5)
```

### Expected value of a binomial random variable

$$
E(X) = n \times p
$$

Example: expected number of heads when flipping 10 fair coins

$$
E(X) = 10 \times 0.5 = 5
$$

### Critical assumption: Independence

The binomial distribution **only applies when the trials are independent**.

- Sampling **with replacement** (or independent coin flips) → OK  
- Sampling **without replacement** → probabilities change after each draw → binomial formulas are **invalid**

---

## Quick Reference – Key R Functions

| Task                              | Function     | Example                                      |
|-----------------------------------|--------------|----------------------------------------------|
| Random sample of rows             | `sample_n()` | `df %>% sample_n(5, replace = TRUE)`         |
| Set random seed                   | `set.seed()` | `set.seed(42)`                               |
| Uniform cumulative probability    | `punif()`    | `punif(7, min=0, max=12)`                    |
| Binomial random numbers           | `rbinom()`   | `rbinom(10, size=5, prob=0.3)`               |
| Binomial probability (exact)      | `dbinom()`   | `dbinom(3, size=10, prob=0.5)`               |
| Binomial cumulative probability   | `pbinom()`   | `pbinom(3, size=10, prob=0.5)`               |

---

## Chapter Summary – Key Ideas

1. **Probability** = ways an event can happen ÷ total possible outcomes.
2. **With replacement** → independent events; **without replacement** → dependent events.
3. A **probability distribution** lists the probability of every possible outcome.
4. **Expected value** is the long-run average of a distribution.
5. **Law of large numbers**: larger samples → sample mean gets closer to the true expected value.
6. Continuous distributions use **area under the curve** instead of summing discrete probabilities.
7. The **binomial distribution** models the number of successes in \(n\) independent trials with success probability \(p\).
8. Always check the **independence** assumption before applying the binomial distribution.

---

# Chapter 2 – Practice Exercises & Solutions

**Course:** Introduction to Statistics in R (DataCamp)  

These exercises cover probability calculations, sampling, discrete distributions, continuous uniform, and the binomial distribution.

---

## Exercise 1: Calculating Probabilities (Amir’s Deals)

You want to randomly select deals that Amir worked on.

**Tasks**

1. Count the number of deals Amir worked on for each product type.
2. Create a column `prob` by dividing `n` by the total number of deals.

### Solution

```r
# Count deals by product
amir_deals %>%
  count(product)

# Probability of each product
amir_deals %>%
  count(product) %>%
  mutate(prob = n / sum(n))
```

---

## Exercise 2: Sampling Deals

**Tasks**

1. Set the random seed to 31.
2. Take a sample of 5 deals **without** replacement.
3. Take a sample of 5 deals **with** replacement.

### Solution

```r
# Without replacement
set.seed(31)
amir_deals %>%
  sample_n(5)

# With replacement
set.seed(31)
amir_deals %>%
  sample_n(5, replace = TRUE)
```

---

## Exercise 3: Creating a Probability Distribution

A restaurant has 10 groups waiting. Data is in `restaurant_groups`.

**Tasks**

1. Create a histogram of `group_size` (bins = 5).
2. Create a probability distribution of group sizes and store it as `size_distribution`.
3. Calculate the expected group size.
4. Calculate the probability of selecting a group of 4 or more people.

### Solution

```r
# Histogram
ggplot(restaurant_groups, aes(group_size)) +
  geom_histogram(bins = 5)

# Probability distribution
size_distribution <- restaurant_groups %>%
  count(group_size) %>%
  mutate(probability = n / sum(n))

size_distribution

# Expected group size
expected_val <- sum(size_distribution$group_size * size_distribution$probability)
expected_val

# Probability of group of 4 or more
size_distribution %>%
  filter(group_size >= 4) %>%
  summarise(prob_4_or_more = sum(probability))
```

---

## Exercise 4: Data Back-ups (Continuous Uniform)

Back-ups happen every 30 minutes. Amir arrives at a random time.

**Tasks**

1. Define `min` and `max` wait times.
2. Probability of waiting less than 5 minutes.
3. Probability of waiting more than 5 minutes.
4. Probability of waiting between 10 and 20 minutes.

### Solution

```r
min <- 0
max <- 30

# P(wait < 5)
prob_less_than_5 <- punif(5, min = 0, max = 30)

# P(wait > 5)
prob_greater_than_5 <- 1 - punif(5, min = 0, max = 30)
# or
punif(5, min = 0, max = 30, lower.tail = FALSE)

# P(10 ≤ wait ≤ 20)
prob_between_10_and_20 <- punif(20, min = 0, max = 30) - punif(10, min = 0, max = 30)
```

---

## Exercise 5: Simulating Wait Times

**Tasks**

1. Set the random seed to 334.
2. Generate 1000 wait times from the continuous uniform distribution (0–30 min) and store them in a column called `time`.
3. Create a histogram of the simulated wait times (30 bins).

### Solution

```r
set.seed(334)

wait_times <- wait_times %>%
  mutate(time = runif(1000, min = 0, max = 30))

ggplot(wait_times, aes(time)) +
  geom_histogram(bins = 30)
```

---

## Exercise 6: Simulating Sales Deals (Binomial)

Amir works on 3 deals per week and wins 30% of them.

**Tasks**

1. Set seed to 10 and simulate a single deal.
2. Simulate one week of 3 deals.
3. Simulate a full year (52 weeks) and calculate the mean number of deals won per week.

### Solution

```r
set.seed(10)

# Single deal
rbinom(1, size = 1, prob = 0.3)

# One week (3 deals)
rbinom(1, size = 3, prob = 0.3)

# Full year
deals <- rbinom(52, size = 3, prob = 0.3)
mean(deals)
```

---

## Exercise 7: Calculating Binomial Probabilities

Assume Amir still wins 30% of deals and works on 3 deals per week.

**Tasks**

1. Probability of closing all 3 deals.
2. Probability of closing 1 or fewer deals.
3. Probability of closing more than 1 deal.

### Solution

```r
# Exactly 3 successes
dbinom(3, size = 3, prob = 0.3)

# ≤ 1 success
pbinom(1, size = 3, prob = 0.3)

# > 1 success
pbinom(1, size = 3, prob = 0.3, lower.tail = FALSE)
```

---

## Exercise 8: Expected Number of Sales Won

Calculate the expected number of deals won per week under different win rates.

**Tasks**

- 30% win rate
- 25% win rate
- 35% win rate

### Solution

```r
# Expected value = n * p
won_30pct <- 3 * 0.3
won_25pct <- 3 * 0.25
won_35pct <- 3 * 0.35

won_30pct
won_25pct
won_35pct
```

---

# Chapter 3: More Distributions and the Central Limit Theorem

**Course:** Introduction to Statistics in R (DataCamp)  
**Instructor:** Maggie Matsui

This chapter covers the **normal distribution**, the **Central Limit Theorem (CLT)**, the **Poisson distribution**, the **exponential distribution**, and briefly introduces the **t-distribution** and **log-normal distribution**.

---

## 1. The Normal Distribution

### What is the Normal Distribution?

The normal distribution (also called the Gaussian distribution) is one of the most important probability distributions in statistics. Its shape is commonly referred to as a **bell curve**.

**Key properties:**

1. **Symmetrical** — The left side is a perfect mirror image of the right side.
2. **Area under the curve = 1** — Like any continuous probability distribution, the total probability is 1.
3. **Curve never hits zero** — The tails approach the x-axis but never actually reach it. (Only about 0.006% of the area lies beyond ±4 standard deviations on a standard normal.)

### Parameters: Mean and Standard Deviation

A normal distribution is fully described by two parameters:

- **Mean ($\mu$)** — Determines the center (location) of the curve.
- **Standard deviation ($\sigma$)** — Determines the spread (width) of the curve.

**Special case:** When $\mu = 0$ and $\sigma = 1$, we have the **standard normal distribution**.

Changing the mean shifts the curve left or right. Changing the standard deviation stretches or compresses it.

### The 68-95-99.7 Rule (Empirical Rule)

For any normal distribution:

| Distance from mean | Approximate area |
|--------------------|------------------|
| Within **1** standard deviation | **68%** |
| Within **2** standard deviations | **95%** |
| Within **3** standard deviations | **99.7%** |

This rule is extremely useful for quick mental estimates.

### Real-World Example: Women's Heights (NHANES)

Many real-world datasets are approximately normal. Women's heights from the National Health and Nutrition Examination Survey (NHANES) form a roughly normal distribution with:

- Mean $\approx 161$ cm
- Standard deviation $\approx 7$ cm

We can therefore use a normal distribution with these parameters to approximate probabilities about women's heights.

### Calculating Probabilities with `pnorm()`

`pnorm(q, mean, sd, lower.tail = TRUE)` returns the **cumulative probability** (area to the left of `q`).

**Example 1 – Percent shorter than 154 cm**

```r
pnorm(154, mean = 161, sd = 7)
# 0.159   → about 16% of women are shorter than 154 cm
```

**Example 2 – Percent taller than 154 cm**

```r
pnorm(154, mean = 161, sd = 7, lower.tail = FALSE)
# 0.841   → about 84% of women are taller than 154 cm
```

**Example 3 – Percent between 154 cm and 157 cm**

```r
pnorm(157, mean = 161, sd = 7) - pnorm(154, mean = 161, sd = 7)
# 0.125   → about 12.5% of women are between 154 and 157 cm
```

### Calculating Quantiles with `qnorm()`

`qnorm(p, mean, sd, lower.tail = TRUE)` is the **inverse** of `pnorm()`. It returns the value below which a given proportion of the data falls.

**Example – Height that 90% of women are shorter than**

```r
qnorm(0.9, mean = 161, sd = 7)
# ≈ 169.97 cm
```

**Example – Height that 90% of women are taller than**

```r
qnorm(0.9, mean = 161, sd = 7, lower.tail = FALSE)
# ≈ 152.03 cm
```

### Generating Random Numbers with `rnorm()`

```r
# Generate 10 random heights from N(161, 7)
rnorm(10, mean = 161, sd = 7)
```

### Key Takeaways – Normal Distribution

- Fully characterized by mean and standard deviation.
- Symmetric, continuous, area = 1, never reaches zero.
- 68-95-99.7 rule is a quick mental reference.
- `pnorm()` → probability from value  
- `qnorm()` → value from probability  
- `rnorm()` → random samples

---

## 2. The Central Limit Theorem (CLT)

### Intuition with Dice Rolls

Consider a fair six-sided die:

```r
die <- c(1, 2, 3, 4, 5, 6)
```

If we roll the die 5 times and calculate the mean, we get different values each time:

```r
sample(die, 5, replace = TRUE) %>% mean()
```

If we repeat this process many times, we obtain a **sampling distribution of the sample mean**.

### What is a Sampling Distribution?

A sampling distribution is the distribution of a summary statistic (mean, standard deviation, proportion, etc.) calculated from many random samples of the same size.

### Demonstrating the CLT

```r
# 10 sample means
sample_means <- replicate(10, sample(die, 5, replace = TRUE) %>% mean())

# 100 sample means
sample_means <- replicate(100, sample(die, 5, replace = TRUE) %>% mean())

# 1000 sample means
sample_means <- replicate(1000, sample(die, 5, replace = TRUE) %>% mean())
```

As the number of sample means increases, the histogram of those means becomes closer and closer to a **normal distribution**, even though the original die rolls follow a uniform distribution.

### Formal Statement of the Central Limit Theorem

> The sampling distribution of a statistic becomes closer to a normal distribution as the number of trials (or the sample size) increases, **provided the samples are random and independent**.

The CLT applies not only to the mean, but also to other statistics such as:

- Sample standard deviation
- Sample proportion

**Example – Sampling distribution of the proportion**

```r
sales_team <- c("Amir", "Brian", "Claire", "Damian")

# Proportion of "Claire" in many samples of size 10
sample_props <- replicate(1000, {
  mean(sample(sales_team, 10, replace = TRUE) == "Claire")
})
```

The resulting distribution is approximately normal and centered near 0.25 (Claire’s true probability).

### Estimating Population Parameters

Because sampling distributions are approximately normal, their **mean** is a good estimate of the corresponding population parameter:

```r
mean(sample_means)   # ≈ 3.5 (expected value of a fair die)
mean(sample_props)   # ≈ 0.25 (true proportion of Claire)
```

This is especially useful when:

- The underlying population distribution is unknown, or
- The population is too large to measure completely.

### Key Takeaways – Central Limit Theorem

- Sampling distributions of many statistics become normal as the number of samples grows.
- Requires random, independent samples (e.g., sampling with replacement).
- Allows us to estimate population means, standard deviations, and proportions.
- Explains why the normal distribution appears so frequently in statistics.

---

## 3. The Poisson Distribution

### Poisson Processes

A **Poisson process** is a process in which events occur at a constant average rate, but completely at random. Examples:

- Number of animals adopted from a shelter per week
- Number of people arriving at a restaurant per hour
- Number of earthquakes in California per year

### Definition

The **Poisson distribution** describes the probability of a given number of events occurring in a fixed interval of time (or space), when those events happen independently and at a constant average rate.

It is a **discrete** distribution (you count whole events).

### Parameter: Lambda ($\lambda$)

- $\lambda$ = average number of events per time interval
- $\lambda$ is also the **expected value** (mean) of the distribution
- The peak of the distribution is always at $\lambda$

Different values of $\lambda$ produce different shapes, but the peak always sits at $\lambda$.

### Probability Functions in R

| Function | Purpose |
|----------|---------|
| `dpois(x, lambda)` | Probability of **exactly** `x` events |
| `ppois(q, lambda, lower.tail = TRUE)` | Probability of **≤** `q` events |
| `rpois(n, lambda)` | Generate `n` random samples |

**Example – Animal shelter ($\lambda = 8$ adoptions per week)**

```r
# Probability of exactly 5 adoptions
dpois(5, lambda = 8)
# ≈ 0.0916 (about 9%)

# Probability of 5 or fewer adoptions
ppois(5, lambda = 8)
# ≈ 0.191 (about 19%)

# Probability of more than 5 adoptions
ppois(5, lambda = 8, lower.tail = FALSE)
# ≈ 0.809 (about 81%)

# If λ rises to 10
ppois(5, lambda = 10, lower.tail = FALSE)
# ≈ 0.933 (about 93%)
```

**Generating random weeks**

```r
rpois(10, lambda = 8)
# e.g. 13  6 11  7 10  8  7  3  7  6
```

### The CLT Still Applies

Even though the Poisson distribution is discrete and often skewed (especially for small $\lambda$), the sampling distribution of the sample mean from many Poisson samples becomes approximately normal for large numbers of samples.

### Key Takeaways – Poisson Distribution

- Models counts of rare or randomly occurring events over a fixed interval.
- Characterized by a single parameter $\lambda$ (rate / mean).
- Discrete.
- `dpois` / `ppois` / `rpois` follow the same naming pattern as other R distribution functions.
- CLT applies to sample means of Poisson data.

---

## 4. More Probability Distributions

### 4.1 Exponential Distribution

The **exponential distribution** models the **time between** events in a Poisson process.

- Continuous (time can take any positive real value)
- Uses the same rate parameter $\lambda$ as the corresponding Poisson process

**Relationship between $\lambda$ and expected waiting time**

$$
\text{Expected waiting time} = \frac{1}{\lambda}
$$

**Example – Customer service tickets**

On average, one ticket is created every 2 minutes  
→ $\lambda = 0.5$ tickets per minute  
→ Expected time between tickets = $1 / 0.5 = 2$ minutes

```r
# Probability of waiting less than 1 minute
pexp(1, rate = 0.5)
# ≈ 0.393 (about 39%)

# Probability of waiting more than 4 minutes
pexp(4, rate = 0.5, lower.tail = FALSE)
# ≈ 0.135 (about 13.5%)

# Probability of waiting between 1 and 4 minutes
pexp(4, rate = 0.5) - pexp(1, rate = 0.5)
# ≈ 0.471 (about 47%)
```

**Key point:** Higher $\lambda$ (higher rate) → steeper decline → shorter typical waiting times.

### 4.2 Student’s t-Distribution

- Shape is similar to the normal distribution but with **heavier tails**.
- Controlled by a parameter called **degrees of freedom (df)**.
- Lower df → thicker tails and higher standard deviation.
- As df increases, the t-distribution approaches the standard normal distribution.

Used heavily in inference when the population standard deviation is unknown and sample sizes are small.

### 4.3 Log-Normal Distribution

A random variable follows a **log-normal distribution** if its **logarithm** is normally distributed.

- Always positive and right-skewed.
- Common real-world examples:
  - Length of chess games
  - Adult blood pressure
  - Number of hospitalizations during the 2003 SARS outbreak

---

## Quick Reference – R Distribution Functions

| Distribution   | Density / PMF | CDF (`p`)          | Quantile (`q`) | Random (`r`) |
|----------------|---------------|--------------------|----------------|--------------|
| Normal         | `dnorm`       | `pnorm`            | `qnorm`        | `rnorm`      |
| Poisson        | `dpois`       | `ppois`            | `qpois`        | `rpois`      |
| Exponential    | `dexp`        | `pexp`             | `qexp`         | `rexp`       |
| t              | `dt`          | `pt`               | `qt`           | `rt`         |
| Log-normal     | `dlnorm`      | `plnorm`           | `qlnorm`       | `rlnorm`     |

**Common arguments**

- `lower.tail = TRUE` (default) → left-tail probability  
- `lower.tail = FALSE` → right-tail probability

---

## Chapter Summary – Key Ideas

1. **Normal distribution** is the workhorse of statistics; fully defined by mean and sd; follows the 68-95-99.7 rule.
2. **Central Limit Theorem** explains why many sampling distributions are normal and lets us estimate population parameters from samples.
3. **Poisson distribution** models counts of randomly occurring events; characterized by rate $\lambda$.
4. **Exponential distribution** models waiting times between Poisson events; expected waiting time = $1/\lambda$.
5. **t-distribution** has heavier tails than the normal and is useful for small-sample inference.
6. **Log-normal distribution** arises when the log of a variable is normal; produces right-skewed positive data.

---

**Distribution of Amir's sales**

Since each deal Amir worked on (both won and lost) was different, each was worth a different amount of money. These values are stored in the amount column of amir_deals As part of Amir's performance review, you want to be able to estimate the probability of him selling different amounts, but before you can do this, you'll need to determine what kind of distribution the amount variable follows.

- Create a histogram with 10 bins to visualize the distribution of the `amount`.

```r
# Histogram of amount with 10 bins
ggplot(amir_deals, aes(x = amount)) +
  geom_histogram(bins = 10)
```

**Probabilities from the normal distribution**

Since each deal Amir worked on (both won and lost) was different, each was worth a different amount of money. These values are stored in the `amount` column of `amir_deals` and follow a normal distribution with a mean of 5000 dollars and a standard deviation of 2000 dollars. As part of his performance metrics, you want to calculate the probability of Amir closing a deal worth various amounts.

1) What's the probability of Amir closing a deal worth less than $7500?

```r
# Probability of deal < 7500
pnorm(7500, mean = 5000, sd = 2000)
```

2) What's the probability of Amir closing a deal worth more than $1000?

```r
# Probability of deal > 1000
___
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








