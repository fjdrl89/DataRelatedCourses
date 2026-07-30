# Introduction to Statistics in R  

**Chapter 1 – Complete Notes**  

These notes cover the entire first chapter: what statistics is, types of data, measures of center, measures of spread, and outlier detection. All examples use the `msleep` dataset (mammal sleep habits) from the `ggplot2` package.

---

## 1. What is Statistics?

Statistics has **two related meanings**:

1. **The field of statistics** – the practice and study of collecting and analyzing data.
2. **A summary statistic** – a single fact or number that summarizes some data (e.g., an average, a count, a percentage).

### What can statistics do?
With statistics we can answer many practical questions, for example:

- How likely is someone to purchase a product? Are people more likely to buy it if they can use a different payment system?
- How many occupants will a hotel have? How can we optimize occupancy?
- How many sizes of jeans need to be manufactured so they fit 95 % of the population? Should we produce the same number of each size?
- **A/B testing**: Which advertisement is more effective at getting people to purchase a product?

### What can't statistics do?
Statistics cannot answer every question, especially pure “why” questions.

> ❌ *Why* is *Game of Thrones* so popular?  
> We could survey people, but they may lie or leave out reasons.  
> ✅ We *can* ask: “Are series with more violent scenes viewed by more people?”

Even if we find a correlation, **correlation does not imply causation**. More violent scenes might be associated with higher viewership, but we cannot conclude that the violence *caused* the popularity — other factors could be responsible.

---

## 2. Types of Statistics

There are two main branches:

| Branch              | Focus                                          | Example                                              |
|---------------------|------------------------------------------------|------------------------------------------------------|
| **Descriptive**     | Describe and summarize the data you already have | After asking 4 friends: 50 % drive, 25 % take the bus, 25 % bike |
| **Inferential**     | Use a *sample* to make inferences about a larger *population* | Using the sample above to estimate what % of *all* people drive to work |

- Descriptive statistics stay inside the dataset.
- Inferential statistics go beyond the sample to make claims about a broader group.

---

## 3. Types of Data

Identifying the data type is crucial because it determines which summary statistics and visualizations make sense.

### Numeric (Quantitative)
Made up of numbers.

| Sub-type       | Description                          | Examples                          |
|----------------|--------------------------------------|-----------------------------------|
| **Continuous** | Measured (can take any value in a range) | Airplane speed, time spent waiting in line |
| **Discrete**   | Counted (usually whole numbers)      | Number of pets, number of packages shipped |

### Categorical (Qualitative)
Values that belong to distinct groups.

| Sub-type    | Description                       | Examples                                      |
|-------------|-----------------------------------|-----------------------------------------------|
| **Nominal** | No inherent order                 | Married / unmarried, country of residence     |
| **Ordinal** | Has a natural order               | Strongly disagree → Strongly agree (Likert scale) |

#### Categorical data can be represented as numbers
It is common to encode categories numerically:

```r
# Nominal
married   ← 1
unmarried ← 0

# Ordinal (order matters)
strongly_disagree ← 1
somewhat_disagree ← 2
neither           ← 3
somewhat_agree    ← 4
strongly_agree    ← 5
```

**Important**: Even when represented as numbers, these are still *categorical* variables. Treating them as numeric can lead to meaningless calculations (e.g., the “average” of married/unmarried).

### Why does data type matter?
The type of data dictates the appropriate tools:

- **Numeric** → mean, median, scatterplots, histograms, etc.
- **Categorical** → counts, proportions, bar plots.

Using the wrong tool produces results that do not make sense.

---

## 4. Measures of Center

When we ask “How long do mammals in this dataset **typically** sleep?”, we are looking for a measure of **center**.

### Quick reminder: Histograms
A histogram groups data into **bins** (ranges of values). The height of each bar shows how many data points fall into that bin.

Example bins for sleep time: 0–2 h, 2–4 h, 4–6 h, …  
Histograms give a visual summary; numerical measures of center give an even more compact summary.

### The three main measures of center

| Measure  | Definition                                      | R function          | Sensitive to outliers? |
|----------|-------------------------------------------------|---------------------|------------------------|
| **Mean**   | Sum of all values ÷ number of observations      | `mean(x)`           | Yes                    |
| **Median** | Middle value when data are sorted               | `median(x)`         | No                     |
| **Mode**   | Most frequent value                             | (use `count()`)     | No                     |

#### Mean
```r
mean(msleep$sleep_total)
# 10.43373 hours
```
\[
\bar{x} = \frac{x_1 + x_2 + \dots + x_n}{n}
\]

#### Median
Sort the data and pick the middle value (for 83 observations → index 42):

```r
sort(msleep$sleep_total)[42]   # 10.1
median(msleep$sleep_total)     # 10.1
```
50 % of the data lie below the median and 50 % lie above it.

#### Mode
The value that appears most often. Especially useful for **categorical** variables (which often have no natural numerical order).

```r
msleep %>% count(sleep_total, sort = TRUE)
# 12.5 hours appears 4 times → mode of sleep_total

msleep %>% count(vore, sort = TRUE)
# "herbi" appears 32 times → mode of diet type
```

### Which measure should you use?

#### Effect of an outlier (insectivores example)
Original insectivores:
```r
mean_sleep   ≈ 16.5 h
median_sleep ≈ 18.9 h
```

After adding a mystery insectivore that sleeps **0 hours**:
```r
mean_sleep   ≈ 13.2 h   # dropped by more than 3 hours
median_sleep ≈ 18.1 h   # changed by less than 1 hour
```

**Conclusion**: The mean is pulled strongly by extreme values; the median is robust.

#### Shape of the distribution matters

- **Symmetric data** → mean ≈ median → either is fine (mean is common).
- **Skewed data** → prefer the **median**.

**Skew definitions** (from the transcript):

| Shape          | Appearance                          | Relationship of mean & median      |
|----------------|-------------------------------------|------------------------------------|
| **Left-skewed**  | Data piled up on the **right**, long tail on the left | mean < median                     |
| **Right-skewed** | Data piled up on the **left**, long tail on the right  | mean > median                     |

The mean is always pulled in the direction of the long tail (the skew). Because of this, the median is usually the better choice when the data are skewed or contain outliers.

---

## 5. Measures of Spread

**Spread** describes how close together or far apart the data points are. Just like center, there are several ways to measure it.

### 5.1 Variance
Average of the *squared* distances from each data point to the mean.

**Step-by-step calculation**:
```r
dists         <- msleep$sleep_total - mean(msleep$sleep_total)  # distance of each point
squared_dists <- dists^2                                        # square them
sum_sq_dists  <- sum(squared_dists)                             # add them up
variance      <- sum_sq_dists / 82                              # divide by n − 1
# 19.80568
```

Or simply:
```r
var(msleep$sleep_total)   # 19.80568
```

- Higher variance → data are more spread out.
- **Units are squared** (hours² in this case), which makes interpretation less intuitive.

### 5.2 Standard Deviation (SD)
Square root of the variance → brings the units back to the original scale.

```r
sd(msleep$sleep_total)          # 4.450357 hours
sqrt(var(msleep$sleep_total))   # same result
```

Much easier to interpret: a typical mammal’s sleep time is about **4.5 hours** away from the mean of 10.4 hours.

### 5.3 Mean Absolute Deviation (MAD)
Average of the *absolute* distances (no squaring).

```r
mean(abs(msleep$sleep_total - mean(msleep$sleep_total)))
# 3.566701
```

**Key difference**:
- SD **squares** distances → longer distances are penalized more heavily.
- MAD treats every distance equally.
- Neither is “better,” but **SD is far more commonly used**.

---

## 6. Quartiles, Quantiles & Boxplots

### Quartiles
Values that split the data into **four equal parts**.

```r
quantile(msleep$sleep_total)
#   0%    25%    50%    75%   100%
# 1.90   7.85  10.10  13.75  19.90
```

- Q1 (25 %) = first quartile  
- Q2 (50 %) = second quartile = **median**  
- Q3 (75 %) = third quartile  

### Boxplots
The box in a boxplot is built from the quartiles:

- Bottom of the box = Q1  
- Top of the box = Q3  
- Middle line = median (Q2)

```r
ggplot(msleep, aes(y = sleep_total)) +
  geom_boxplot()
```

### Quantiles (percentiles)
A generalization of quartiles — they can split the data into any number of equal pieces.

```r
# Split into five equal parts
quantile(msleep$sleep_total, probs = c(0, 0.2, 0.4, 0.6, 0.8, 1))

# Same thing using seq()
quantile(msleep$sleep_total, probs = seq(0, 1, 0.2))
```

---

## 7. Interquartile Range (IQR)

A robust measure of spread: the distance between the 25th and 75th percentiles (i.e., the height of the box in a boxplot).

```r
IQR <- quantile(msleep$sleep_total, 0.75) - quantile(msleep$sleep_total, 0.25)
# 5.9 hours

# Or simply
IQR(msleep$sleep_total)
```

---

## 8. Detecting Outliers

**Outlier** = a data point that is substantially different from the others.

**Common rule (Tukey’s method)**:
A point is an outlier if:
\[
\text{data} < Q_1 - 1.5 \times \text{IQR}
\quad\text{or}\quad
\text{data} > Q_3 + 1.5 \times \text{IQR}
\]

### Practical example – body weight outliers
```r
iqr <- IQR(msleep$bodywt)

lower_threshold <- quantile(msleep$bodywt, 0.25) - 1.5 * iqr
upper_threshold <- quantile(msleep$bodywt, 0.75) + 1.5 * iqr

msleep %>%
  filter(bodywt < lower_threshold | bodywt > upper_threshold) %>%
  select(name, vore, sleep_total, bodywt)
```

Result: **11 outliers**, including the cow (600 kg) and the Asian elephant (2547 kg).

---

## Quick Reference – Essential R Functions

| Task                          | Code                                      |
|-------------------------------|-------------------------------------------|
| Mean                          | `mean(x)`                                 |
| Median                        | `median(x)`                               |
| Mode (most frequent)          | `count(x, sort = TRUE)`                   |
| Variance                      | `var(x)`                                  |
| Standard deviation            | `sd(x)`                                   |
| Mean absolute deviation       | `mean(abs(x - mean(x)))`                  |
| Quartiles / any quantiles     | `quantile(x, probs = ...)`                |
| Interquartile range           | `IQR(x)`                                  |
| Boxplot                       | `geom_boxplot()`                          |
| Find outliers                 | `filter(x < Q1-1.5*IQR \| x > Q3+1.5*IQR)` |

---

## Key Takeaways

1. Statistics is both a **field** and a collection of **summary numbers**.
2. Always identify the **data type** first — it dictates the tools you can use.
3. **Mean** is sensitive to outliers and skew; **median** is robust.
4. Use the **median** when data are skewed or contain extreme values.
5. **Standard deviation** is the most common measure of spread (units match the original data).
6. **IQR** is a robust alternative to standard deviation.
7. The **1.5 × IQR rule** is a practical, widely used way to flag potential outliers.

---

Mean and median
In this chapter, you'll be working with the food_consumption dataset from 2018 Food Carbon Footprint Index by nu3. The food_consumption dataset contains the number of kilograms of food consumed per person per year in each country, food category column food_category, the amount of consumption, and its carbon footprint (co2_emission) measured in kilograms of carbon dioxide, or CO2.

dplyr is loaded for you and food_consumption is available.

Calculate the mean of food consumption in kilograms for all countries in the food_consumption dataset.

```r
# Filter for Belgium
belgium_consumption <- food_consumption %>%
  filter(country == "Belgium")

# Filter for USA
usa_consumption <- food_consumption %>%
  filter(country == "USA")

# Calculate mean and median consumption in Belgium
mean(belgium_consumption$consumption)
median(belgium_consumption$consumption)

# Calculate mean and median consumption in USA
mean(usa_consumption$consumption)
median(usa_consumption$consumption)
```

Calculate the median of food consumption in kilograms for all countries in the food_consumption dataset. Is it the same as the mean?

```r
# Calculate median food consumption 
median(food_consumption$consumption)
```

Calculate the mode of consumption for all countries in the food_consumption dataset by counting and sorting values descending.

```r
# Calculate the mode of food consumption
food_consumption %>%
  count(consumption, sort = TRUE)
```

Mean vs. median
In the video, you learned that the mean is the sum of all the data points divided by the total number of data points, and the median is the middle value of the dataset where 50% of the data is less than the median, and 50% of the data is greater than the median. In this exercise, you'll compare these two measures of center.

The dplyr and ggplot2 libraries are loaded and food_consumption is available.

- Filter food_consumption to get the rows where food_category is "rice".
- Create a histogram of co2_emission for rice using the ggplot()function.

```r
food_consumption %>%
  # Filter for rice food category
  filter(food_category == "rice") %>%
# Create histogram of co2_emission
  ggplot(aes(co2_emission)) +
    geom_histogram()
```

- Filter food_consumption to get the rows where food_category is "rice".
- Summarize the data to get the mean and median of co2_emission, calling them mean_co2 and median_co2.

```r
food_consumption %>%
  # Filter for rice food category
  filter(food_category == "rice") %>%
  # Create histogram of co2_emission
  ggplot(aes(co2_emission)) +
    geom_histogram()

food_consumption %>%
  # Filter for rice food category
  filter(food_category == "rice") %>% 
  # Summarize the mean_co2 and median_co2
  summarise(mean_co2 = mean(co2_emission),
            median_co2 = median(co2_emission))
```

Variance and standard deviation are two of the most common ways to measure the spread of a variable, and you'll practice calculating these in this exercise. Spread is important since it can help inform expectations. For example, if a salesperson sells a mean of 20 products a day, but has a standard deviation of 10 products, there will probably be days where they sell 40 products, but also days where they only sell one or two. Information like this is important, especially when making predictions.

The dplyr and ggplot2 libraries are loaded, and food_consumption is available.

1) Calculate the variance of co2_emission in the food_consumption dataset.

```r
var(food_consumption$co2_emission)
```

2) Calculate the standard deviation of co2_emission in the food_consumption dataset.

```r
sd(food_consumption$co2_emission)
```

Quartiles, quantiles, and quintiles
Quantiles are a great way of summarizing numerical data since they can be used to measure center and spread, as well as to get a sense of where a data point stands in relation to the rest of the dataset. For example, you might want to give a discount to the 10% most active users on a website.

In this exercise, you'll calculate quartiles, quintiles, and deciles, which split up a dataset into 4, 5, and 10 pieces, respectively.

The dplyr package is loaded and food_consumption is available.

1. Calculate the quartiles of the co2_emission column of food_consumption.

```r
# Calculate the quartiles of co2_emission
quantile(food_consumption$co2_emission)
```

2. Calculate the quintiles of the co2_emission column of food_consumption that split up the data into 5 pieces.

```r
# Calculate the quintiles of co2_emission
quantile(food_consumption$co2_emission, probs = seq(0, 1, 0.2))
```

3. Calculate the quantiles of co2_emission that split up the data into ten pieces.

```r
# Calculate the deciles of co2_emission
quantile(food_consumption$co2_emission, probs = seq(0, 1, 0.1))
```

Finding outliers using IQR
Interquartile range, or IQR, is another way of measuring spread that's less influenced by outliers. IQR is also often used to find outliers. If a value is less than $Q1 - 1.5 x IQR$ or greater than $Q3 + 1.5 x IQR$, it's considered an outlier. In fact, this is how the lengths of the whiskers in a ggplot2 box plot are calculated.

<img width="1394" height="458" alt="image" src="https://github.com/user-attachments/assets/4a6fe7ca-74ac-4514-8a44-20735ddcb252" />

In this exercise, you'll calculate IQR and use it to find some outliers. Both dplyr and ggplot2 libraries are loaded and food_consumption is available.

- Compute the first and third quartiles of co2_emission in food_consumption and store these as q1 and q3.
- Calculate the interquartile range (IQR) of co2_emission and store it as iqr.

```r
# Compute the 25th percentile and 75th percentile of co2_emission
q1 <- quantile(food_consumption$co2_emission, 0.25)
q3 <- quantile(food_consumption$co2_emission, 0.75)

# Compute the IQR of co2_emission
iqr <- q3 - q1
iqr
```

- Calculate the lower and upper cutoffs for outliers of co2_emission, and store these as lower and upper.

```r
# Compute the 25th percentile and 75th percentile of co2_emission
q1 <- quantile(food_consumption$co2_emission, 0.25)
q3 <- quantile(food_consumption$co2_emission, 0.75)

# Compute the IQR of co2_emission
iqr <- q3 - q1

# Calculate the lower and upper cutoffs for outliers
lower <- q1 - 1.5 * iqr
upper <- q3 + 1.5 * iqr

lower
upper
```

- Use filter() to get countries with a co2_emission greater than the upper cutoff or a co2_emission less than the lower cutoff.

```r
# Compute the 25th percentile and 75th percentile of co2_emission
q1 <- quantile(food_consumption$co2_emission, 0.25)
q3 <- quantile(food_consumption$co2_emission, 0.75)

# Compute the IQR of co2_emission
iqr <- q3 - q1

# Calculate the lower and upper cutoffs for outliers
lower <- q1 - 1.5 * iqr
upper <- q3 + 1.5 * iqr

# Filter food_consumption to find outliers
food_consumption %>%
  filter(co2_emission < lower | co2_emission > upper)
```

-----
-----

**Chapter 2 – Probability & Distributions**  

These notes cover the full second chapter: how we measure chance, sampling with and without replacement, discrete and continuous probability distributions, the law of large numbers, and the binomial distribution.

---

## 1. Measuring Chance

People talk about chance all the time (“What are the chances of closing the sale?”, “What’s the chance of rain?”).  
**Probability** is the formal way to measure it.

### The basic formula

$$
\[
P(\text{event}) = \frac{\text{number of ways the event can happen}}{\text{total number of possible outcomes}}
\]
$$

**Example – fair coin flip**

$$
\[
P(\text{heads}) = \frac{1 \text{ way to get heads}}{2 \text{ possible outcomes (heads or tails)}} = \frac{1}{2} = 50\%
\]
$$

Probability is always between **0 %** and **100 %**:
- \(0\%\) → the event is **impossible**
- \(100\%\) → the event is **certain**

---

## 2. Assigning Salespeople (A Practical Example)

Imagine a sales team of four people: **Amir, Brian, Claire, Damian**.  
We put their names in a box and draw one at random to decide who goes to a client meeting.

$$
\[
P(\text{Brian}) = \frac{1}{4} = 25\%
\]
$$

### Doing it in R with `dplyr`

```r
sales_counts
#   name   n_sales
# 1 Amir       178
# 2 Brian      126
# 3 Claire      75
# 4 Damian      69

sales_counts %>% sample_n(1)   # randomly picks one row
```

Every time you run `sample_n(1)` you may get a different person.  
To make the result **reproducible** (important when showing the team how the choice was made), we set a **random seed**:

```r
set.seed(5)
sales_counts %>% sample_n(1)   # always returns Brian when seed = 5
```

The seed is just a starting point for R’s random-number generator. Any number works; what matters is that you use the **same** seed to get the same result.

---

## 3. Sampling With vs Without Replacement

### Sampling **without** replacement
Brian has already been chosen and cannot attend two meetings at the same time.  
We remove his name from the box. Now only three names remain.

$$
\[
P(\text{Claire is chosen second}) = \frac{1}{3} \approx 33\%
\]
$$

In R:
```r
sales_counts %>% sample_n(2)          # two different people
```

### Sampling **with** replacement
The two meetings are on different days, so the same person *could* attend both.  
We put Brian’s name **back** into the box.

$$
\[
P(\text{Claire is chosen second}) = \frac{1}{4} = 25\%
\]
$$

In R:
```r
sales_counts %>% sample_n(2, replace = TRUE)
# or for 5 meetings on different days:
sample(sales_team, 5, replace = TRUE)
```

---

## 4. Independent vs Dependent Events

| Concept | Definition | Typical sampling |
|---------|------------|------------------|
| **Independent events** | The probability of the second event is **not** affected by the outcome of the first | Sampling **with** replacement |
| **Dependent events** | The probability of the second event **is** affected by the outcome of the first | Sampling **without** replacement |

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

| Outcome | 1 | 2 | 3 | 4 | 5 | 6 |
|---------|---|---|---|---|---|---|
| Probability | \(\frac{1}{6}\) | \(\frac{1}{6}\) | \(\frac{1}{6}\) | \(\frac{1}{6}\) | \(\frac{1}{6}\) | \(\frac{1}{6}\) |

This is a **discrete uniform distribution** (all outcomes equally likely).

### Expected value (the mean of a distribution)

$$
\[
E(X) = \sum x_i \cdot P(x_i)
\]
$$

For a fair die:

$$
\[
E(X) = 1\cdot\frac{1}{6} + 2\cdot\frac{1}{6} + 3\cdot\frac{1}{6} + 4\cdot\frac{1}{6} + 5\cdot\frac{1}{6} + 6\cdot\frac{1}{6} = 3.5
\]
$$

### Probability = Area under the distribution

$$
\[
P(\text{roll} \le 2) = P(1) + P(2) = \frac{1}{6} + \frac{1}{6} = \frac{1}{3}
\]
$$

### Uneven die (2 turned into a 3)

| Outcome | 1 | 2 | 3 | 4 | 5 | 6 |
|---------|---|---|---|---|---|---|
| Probability | \(\frac{1}{6}\) | \(0\) | \(\frac{1}{3}\) | \(\frac{1}{6}\) | \(\frac{1}{6}\) | \(\frac{1}{6}\) |

New expected value:

$$
\[
E(X) = 1\cdot\frac{1}{6} + 2\cdot 0 + 3\cdot\frac{1}{3} + 4\cdot\frac{1}{6} + 5\cdot\frac{1}{6} + 6\cdot\frac{1}{6} \approx 3.67
\]
$$

$$
\[
P(\text{roll} \le 2) = \frac{1}{6} + 0 = \frac{1}{6}
\]
$$

### Sampling from a discrete distribution in R

```r
die <- data.frame(n = 1:6)

# Simulate 10 rolls (with replacement so we always sample from the same distribution)
rolls_10 <- die %>% sample_n(10, replace = TRUE)
mean(rolls_10$n)   # sample mean (will vary)
```

Visualize with a histogram:
```r
ggplot(rolls_10, aes(n)) + geom_histogram(bins = 6)
```

---

## 6. The Law of Large Numbers

As the size of your sample increases, the **sample mean approaches the theoretical expected value**.

| Sample size | Sample mean (example) |
|-------------|-----------------------|
| 10          | 3.00                  |
| 100         | 3.36                  |
| 1 000       | 3.53                  |

Theoretical mean of a fair die = **3.5**

This is why larger samples give us more reliable estimates of the true underlying probability.

---

## 7. Continuous Distributions

Discrete distributions work for countable outcomes (dice, number of customers, etc.).  
For continuous variables (time, height, weight, temperature…) we need **continuous distributions**.

### Waiting for the bus (Continuous Uniform Distribution)

A bus arrives every 12 minutes. You arrive at a random time, so your waiting time can be anywhere from 0 to 12 minutes.

Because there are infinitely many possible waiting times, we cannot draw individual bars. Instead we draw a continuous density:

- The density is a **flat line** of height \(\frac{1}{12}\) from 0 to 12.  
- This is the **continuous uniform distribution**.

### Probability is still the area under the curve

$$
\[
P(4 \le \text{wait} \le 7) = \text{width} \times \text{height} = 3 \times \frac{1}{12} = \frac{3}{12} = 0.25
\]
$$

### Using the uniform distribution in R – `punif()`

```r
# Probability of waiting ≤ 7 minutes
punif(7, min = 0, max = 12)                 # ≈ 0.583

# Probability of waiting ≥ 7 minutes
punif(7, min = 0, max = 12, lower.tail = FALSE)  # ≈ 0.417

# Probability of waiting between 4 and 7 minutes
punif(7, min = 0, max = 12) - punif(4, min = 0, max = 12)  # 0.25
```

### Important property of **all** continuous distributions

The **total area under the curve must equal 1** (or 100 %).  
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
       size = number of trials (coins) per experiment,
       prob = probability of success)
```

| Code | Meaning | Result type |
|------|---------|-------------|
| `rbinom(1, 1, 0.5)` | 1 flip of 1 coin | single 0 or 1 |
| `rbinom(8, 1, 0.5)` | 8 flips of 1 coin | vector of eight 0/1 |
| `rbinom(1, 8, 0.5)` | 1 experiment of 8 coins | total number of heads |
| `rbinom(10, 3, 0.5)` | 10 experiments of 3 coins each | vector of 10 totals |

You can change the probability:
```r
rbinom(10, 3, 0.25)   # biased coin – only 25 % chance of heads
```

### Definition of the binomial distribution

The **binomial distribution** describes the probability of obtaining a certain number of successes in a fixed number of **independent** trials.

It is fully characterized by two parameters:
- \(n\) = number of trials  
- \(p\) = probability of success on each trial

### Calculating probabilities

```r
# Probability of exactly 7 heads in 10 flips
dbinom(7, size = 10, prob = 0.5)          # ≈ 0.117

# Probability of 7 or fewer heads
pbinom(7, size = 10, prob = 0.5)          # ≈ 0.945

# Probability of more than 7 heads
pbinom(7, size = 10, prob = 0.5, lower.tail = FALSE)  # ≈ 0.055
# equivalent to:
1 - pbinom(7, size = 10, prob = 0.5)
```

### Expected value of a binomial random variable

$$
\[
E(X) = n \times p
\]
$$

Example: expected number of heads when flipping 10 fair coins  

$$
\[
E(X) = 10 \times 0.5 = 5
\]
$$

### Critical assumption: Independence

The binomial distribution **only applies when the trials are independent**.

- Sampling **with replacement** (or independent coin flips) → OK  
- Sampling **without replacement** → probabilities change after each draw → binomial formulas are **invalid**

---

## Quick Reference – Key R Functions

| Task | Function | Example |
|------|----------|---------|
| Random sample of rows | `sample_n()` | `df %>% sample_n(5, replace = TRUE)` |
| Set random seed | `set.seed()` | `set.seed(42)` |
| Uniform cumulative probability | `punif()` | `punif(7, min=0, max=12)` |
| Binomial random numbers | `rbinom()` | `rbinom(10, size=5, prob=0.3)` |
| Binomial probability (exact) | `dbinom()` | `dbinom(3, size=10, prob=0.5)` |
| Binomial cumulative probability | `pbinom()` | `pbinom(3, size=10, prob=0.5)` |

---

## Key Takeaways

1. **Probability** = ways an event can happen ÷ total possible outcomes.  
2. **With replacement** → independent events; **without replacement** → dependent events.  
3. A **probability distribution** lists the probability of every possible outcome.  
4. **Expected value** is the long-run average of a distribution.  
5. **Law of large numbers**: larger samples → sample mean gets closer to the true expected value.  
6. Continuous distributions use **area under the curve** instead of summing discrete probabilities.  
7. The **binomial distribution** models the number of successes in \(n\) independent trials with success probability \(p\).  
8. Always check the **independence** assumption before applying the binomial distribution.

---
Calculating probabilities

You're in charge of the sales team, and it's time for performance reviews, starting with Amir. As part of the review, you want to randomly select a few of the deals that he's worked on over the past year so that you can look at them more deeply. Before you start selecting deals, you'll first figure out what the chances are of selecting certain deals.

Recall that the probability of an event can be calculated by

 $$
P(event) = \frac{\text{number ways event can happen}}{\text{total number of possible outcomes}}
 $$

- Count the number of deals Amir worked on for each product type.

```r
# Count the deals for each product
amir_deals %>%
  count(product)
```

- Create a new column called `prob` by dividing `n` by the total number of deals Amir worked on; `n` is the number of deals for each product category you obtained from the previous step.

```r
# Calculate probability of picking a deal with each product
amir_deals %>%
  count(product) %>%
  mutate(prob = n / sum(n))
```

Sampling deals
In the previous exercise, you counted the deals Amir worked on. Now it's time to randomly pick five deals so that you can reach out to each customer and ask if they were satisfied with the service they received. You'll try doing this both with and without replacement.

Additionally, you want to make sure this is done randomly and that it can be reproduced in case you get asked how you chose the deals, so you'll need to set the random seed before sampling from the deals.

- Set the random seed to 31.
- Take a sample of 5 deals without replacement.

```r
# Set random seed to 31
set.seed(31)

# Sample 5 deals without replacement
amir_deals %>%
  sample_n(5)
```

- Take a sample of 5 deals with replacement.

```r
# Set random seed to 31
set.seed(31)

# Sample 5 deals with replacement
amir_deals %>%
  sample_n(5, replace=TRUE)
```

**Creating a probability distribution**

A new restaurant opened a few months ago, and the restaurant's management wants to optimize its seating space based on the size of the groups that come most often. On one night, there are 10 groups of people waiting to be seated at the restaurant, but instead of being called in the order they arrived, they will be called randomly. In this exercise, you'll investigate the probability of groups of different sizes getting picked first. Data on each of the ten groups is contained in the `restaurant_groups` data frame.

Remember that expected value can be calculated by multiplying each possible outcome with its corresponding probability and taking the sum. The `restaurant_groups` data is available and the `dplyr` and `ggplot2` libraries are loaded.

1) Create a histogram of the group_size column of restaurant_groups, setting the number of bins to 5.

```r
# Create a histogram of group_size
ggplot(restaurant_groups, aes(group_size)) +
  geom_histogram(bins = 5)
```

2) Count the number of each group_size in restaurant_groups, then add a column called probability that contains the probability of randomly selecting a group of each size. Store this in a new data frame called size_distribution.

```r
# Create probability distribution
size_distribution <- restaurant_groups %>%
  # Count number of each group size
  count(group_size) %>%
  # Calculate probability
  mutate(probability = n / sum(n))

size_distribution
```

3) Calculate the expected value of the size_distribution, which represents the expected group size.

```r
# Create probability distribution
size_distribution <- restaurant_groups %>%
  count(group_size) %>%
  mutate(probability = n / sum(n))

# Calculate expected group size
expected_val <- sum(size_distribution$group_size *
                    size_distribution$probability)
expected_val
```

<img width="565" height="429" alt="image" src="https://github.com/user-attachments/assets/1adbb153-aff9-4585-ab8e-683492ee1bb9" />

4) Calculate the probability of randomly picking a group of 4 or more people by filtering and summarizing.

```r
# Create probability distribution
size_distribution <- restaurant_groups %>%
  count(group_size) %>%
  mutate(probability = n / sum(n))

# Calculate probability of picking group of 4 or more
size_distribution %>%
  # Filter for groups of 4 or larger
  filter(group_size >= 4) %>%
  # Calculate prob_4_or_more by taking sum of probabilities
  summarise(prob_4_or_more = sum(probability))
```

**Expected value vs. sample mean**

The app to the right will take a sample from a discrete uniform distribution, which includes the numbers 1 through 9, and calculate the sample's mean. You can adjust the size of the sample using the slider. Note that the expected value of this distribution is 5.

A sample is taken, and you win twenty dollars if the sample's mean is less than 4. There's a catch: you get to pick the sample's size.

**Which distribution?**

At this point, you've learned about the two different variants of the uniform distribution: the discrete uniform distribution, and the continuous uniform distribution. In this exercise, you'll decide which situations follow which distribution.

<img width="1998" height="548" alt="image" src="https://github.com/user-attachments/assets/4b9ea0cc-712d-4aa6-a393-1b6b92851eee" />

**Data back-ups**

The sales software used at your company is set to automatically back itself up, but no one knows exactly what time the back-ups happen. It is known, however, that back-ups happen exactly every 30 minutes. Amir comes back from sales meetings at random times to update the data on the client he just met with. He wants to know how long he'll have to wait for his newly-entered data to get backed up. Use your new knowledge of continuous uniform distributions to model this situation and answer Amir's questions.

1) To model how long Amir will wait for a back-up using a continuous uniform distribution, save his lowest possible wait time as min and his longest possible wait time as max. Remember that back-ups happen every 30 minutes.

```r
# Min and max wait times for back-up that happens every 30 min
min <- 0
max <- 30
```

2) Calculate the probability that Amir has to wait less than 5 minutes, and store in a variable called prob_less_than_5.

```r
# Min and max wait times for back-up that happens every 30 min
min <- 0
max <- 30

# Calculate probability of waiting less than 5 mins
prob_less_than_5 <- punif(5, min = 0, max = 30)
prob_less_than_5
```

3) Calculate the probability that Amir has to wait more than 5 minutes, and store in a variable called prob_greater_than_5.

```r
# Min and max wait times for back-up that happens every 30 min
min <- 0
max <- 30

# Calculate probability of waiting more than 5 mins
prob_greater_than_5 <- 1 - punif(5, min = 0, max = 30)
prob_greater_than_5
```

4) Calculate the probability that Amir has to wait between 10 and 20 minutes, and store in a variable called prob_between_10_and_20.

```r
# Min and max wait times for back-up that happens every 30 min
min <- 0
max <- 30

# Calculate probability of waiting 10-20 mins
prob_between_10_and_20 <- punif(20, min = 0, max = 30) - punif(10, min = 0, max = 30)
prob_between_10_and_20
```

**Simulating wait times**

To give Amir a better idea of how long he'll have to wait, you'll simulate Amir waiting 1000 times and create a histogram to show him what he should expect. Recall from the last exercise that his minimum wait time is 0 minutes and his maximum wait time is 30 minutes.

A data frame called wait_times is available and the dplyr and ggplot2 libraries are loaded.

- Set the random seed to 334.

```r
# Set random seed to 334
set.seed(334)
```

- Generate 1000 wait times from the continuous uniform distribution that models Amir's wait time. Add this as a new column called time in the wait_times data frame.

```r
# Set random seed to 334
set.seed(334)

# Generate 1000 wait times between 0 and 30 mins, save in time column
wait_times %>%
  mutate(time = runif(1000, min = 0, max = 30))
```

- Create a histogram of the simulated wait times with 30 bins.

```r
# Set random seed to 334
set.seed(334)

# Generate 1000 wait times between 0 and 30 mins, save in time column
wait_times <- wait_times %>%
  mutate(time = runif(1000, min = 0, max = 30))

# Create a histogram of simulated times
ggplot(wait_times, aes(time)) +
  geom_histogram(bins = 30)
```

<img width="562" height="428" alt="image" src="https://github.com/user-attachments/assets/422e6c29-7c36-495c-97b8-7c5a7feeb59d" />

**Simulating sales deals**

Assume that Amir usually works on 3 deals per week, and overall, he wins 30% of deals he works on. Each deal has a binary outcome: it's either lost, or won, so you can model his sales deals with a binomial distribution. In this exercise, you'll help Amir simulate a year's worth of his deals so he can better understand his performance.

1) Set the random seed to 10 and simulate a single deal.

```r
# Set random seed to 10
set.seed(10)

# Simulate a single deal
rbinom(1, size = 1, prob = 0.3)
```

2) Simulate a typical week of Amir's deals, or one week of 3 deals.

```r
# Set random seed to 10
set.seed(10)

# Simulate 1 week of 3 deals
rbinom(1, size = 3, prob = 0.3)
```

3) Simulate a year's worth of Amir's deals, or 52 weeks of 3 deals each, and store in deals.
Calculate the mean number of deals he won per week.

```r
# Set random seed to 10
set.seed(10)

# Simulate 52 weeks of 3 deals
deals <- rbinom(52, size = 3, prob = 0.3)

# Calculate mean deals won per week
mean(deals)
```

**Calculating binomial probabilities**

Just as in the last exercise, assume that Amir wins 30% of deals. He wants to get an idea of how likely he is to close a certain number of deals each week. In this exercise, you'll calculate what the chances are of him closing different numbers of deals using the binomial distribution.

- What's the probability that Amir closes all 3 deals in a week?

```r
# Probability of closing 3 out of 3 deals
dbinom(3, size = 3, prob = 0.3)
```

- What's the probability that Amir closes 1 or fewer deals in a week? 

```r
# Probability of closing <= 1 deal out of 3 deals
pbinom(1, size = 3, prob = 0.3)
```

- What's the probability that Amir closes more than 1 deal?

```r
# Probability of closing > 1 deal out of 3 deals
pbinom(1, size = 3, prob = 0.3, lower.tail = FALSE)
```

**How many sales will be won?**

Now Amir wants to know how many deals he can expect to close each week if his win rate changes. Luckily, you can use your binomial distribution knowledge to help him calculate the expected value in different situations. Recall from the video that the expected value of a binomial distribution can be calculated by $n \times p$

- Calculate the expected number of sales out of the 3 he works on that Amir will win each week if he maintains his 30% win rate.
- Calculate the expected number of sales out of the 3 he works on that he'll win if his win rate drops to 25%.
- Calculate the expected number of sales out of the 3 he works on that he'll win if his win rate rises to 35%.

```r
# Expected number won with 30% win rate
won_30pct <- 3 * 0.3
won_30pct

# Expected number won with 25% win rate
won_25pct <- 3 * 0.25
won_25pct

# Expected number won with 35% win rate
won_35pct <- 3 * 0.35
won_35pct
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

```r

```

```r

```

```r

```








