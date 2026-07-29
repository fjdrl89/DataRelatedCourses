# Introduction to Statistics in R  

**Chapter 1 Notes**  

These notes cover the fundamentals of statistics in R: what statistics is, types of data, measures of center, measures of spread, and how to detect outliers. All examples use the built-in `msleep` dataset from the `ggplot2` package (mammal sleep data).

---

## 1. What is Statistics?

**Statistics** has two related meanings:

1. **The field of statistics** – the practice and study of collecting and analyzing data.
2. **A summary statistic** – a single number that summarizes some aspect of a dataset (e.g., the average, the maximum, the proportion).

### What can statistics do?
Statistics helps answer practical questions such as:

- How likely is someone to buy a product? Does offering a different payment method increase purchases?
- How many hotel rooms will be occupied? How can we optimize occupancy?
- How many jean sizes should we manufacture so that 95 % of the population can find a fit?
- **A/B testing**: Which advertisement leads to more purchases?

### What can't statistics do?
Statistics **cannot** answer “why” questions directly.

> ❌ *Why* is *Game of Thrones* so popular?  
> ✅ *Are* series with more violent scenes watched by more people?

Even if we find a correlation, statistics alone cannot prove **causation**. More violent scenes might be associated with higher viewership, but that does not prove that violence *causes* higher viewership.

---

## 2. Types of Statistics

| Type                  | Purpose                                      | Example                                      |
|-----------------------|----------------------------------------------|----------------------------------------------|
| **Descriptive**       | Describe and summarize the data you have     | “50 % of my friends drive to work”           |
| **Inferential**       | Use a *sample* to make conclusions about a larger *population* | “What percent of *all* people drive to work?” |

**Descriptive statistics** stay within the data you collected.  
**Inferential statistics** go beyond the sample and make claims about a broader group.

---

## 3. Types of Data

Understanding the type of data is crucial because it determines which summary statistics and plots are appropriate.

### Numeric (Quantitative)
Data that can be measured or counted.

| Sub-type     | Description                  | Examples                          |
|--------------|------------------------------|-----------------------------------|
| **Continuous** | Measured (can take any value in a range) | Airplane speed, time spent waiting in line |
| **Discrete**   | Counted (whole numbers only) | Number of pets, number of packages shipped |

### Categorical (Qualitative)
Data that falls into groups or categories.

| Sub-type   | Description                  | Examples                                      |
|------------|------------------------------|-----------------------------------------------|
| **Nominal**  | Unordered categories         | Married / unmarried, country of residence     |
| **Ordinal**  | Ordered categories           | Strongly disagree → Strongly agree (Likert scale) |

#### Representing categorical data as numbers
It is common (and useful) to encode categories as numbers:

```r
# Nominal
married   ← 1
unmarried ← 0

# Ordinal (order matters!)
strongly_disagree ← 1
somewhat_disagree ← 2
neither           ← 3
somewhat_agree    ← 4
strongly_agree    ← 5
```

> **Important**: Even though ordinal data uses numbers, the *distance* between categories is not necessarily equal. You should still treat them carefully.

---

## 4. Why Data Type Matters

Different data types call for different summary statistics and visualizations.

### Numeric example – mean of car speeds
```r
car_speeds %>% 
  summarize(avg_speed = mean(speed_mph))
# → 40.09 mph
```

A scatter plot of car weight vs. speed is also natural.

### Categorical example – counts of marriage status
```r
demographics %>% 
  count(marriage_status)
```

| marriage_status | n   |
|-----------------|-----|
| single          | 188 |
| married         | 143 |
| divorced        | 124 |

A bar chart is the natural visualization.

Using the wrong tool (e.g., calculating a mean of “married/unmarried”) produces meaningless results.

---

## 5. Measures of Center

When we ask “What is a *typical* value?”, we are looking for a measure of center.

The three most common measures are:

| Measure  | Definition                              | Sensitive to outliers? |
|----------|-----------------------------------------|------------------------|
| **Mean**   | Arithmetic average                      | Yes                    |
| **Median** | Middle value when data are sorted       | No                     |
| **Mode**   | Most frequent value                     | No                     |

### Dataset used throughout: `msleep`
```r
library(ggplot2)
head(msleep)
```

Contains 83 mammals with variables such as `sleep_total` (hours of sleep per day), `vore` (diet type), `bodywt`, etc.

### 5.1 Mean
```r
mean(msleep$sleep_total)
# 10.43373
```

Mathematically:
\[
\bar{x} = \frac{x_1 + x_2 + \dots + x_n}{n}
\]

### 5.2 Median
```r
# Sort the data and pick the middle value
sort(msleep$sleep_total)[42]   # 42nd observation of 83
# 10.1

median(msleep$sleep_total)
# 10.1
```

### 5.3 Mode
R does not have a built-in `mode()` function for this purpose. The easiest way is to count frequencies:

```r
msleep %>% 
  count(sleep_total, sort = TRUE)
# Most common sleep total: 12.5 hours (appears 4 times)

msleep %>% 
  count(vore, sort = TRUE)
# Most common diet: herbi (32 mammals)
```

### 5.4 Effect of outliers – Mean vs Median

Consider only insectivores:

```r
insecti <- msleep %>% filter(vore == "insecti")

insecti %>% 
  summarize(
    mean_sleep   = mean(sleep_total),
    median_sleep = median(sleep_total)
  )
# mean ≈ 16.5 , median ≈ 18.9
```

Now add a ridiculous outlier (a mammal that sleeps 0 hours):

```r
# After adding the outlier
# mean   drops to ≈ 13.2
# median only drops to ≈ 18.1
```

**Takeaway**: The mean is pulled strongly toward extreme values; the median is robust.

### 5.5 Skew and which measure to use

- **Right-skewed** (long tail on the right) → mean > median  
- **Left-skewed** (long tail on the left)  → mean < median  
- **Symmetric** → mean ≈ median

**Rule of thumb**:
- Use the **mean** when the distribution is roughly symmetric and has no extreme outliers.
- Use the **median** when the distribution is skewed or contains outliers.

---

## 6. Measures of Spread

Center tells us *where* the data are; spread tells us *how spread out* they are.

### 6.1 Variance
Average of the *squared* distances from each point to the mean.

```r
dists        <- msleep$sleep_total - mean(msleep$sleep_total)
squared_dists <- dists^2
sum_sq_dists  <- sum(squared_dists)

# Sample variance (divide by n-1)
sum_sq_dists / 82          # 19.80568
var(msleep$sleep_total)    # same result
```

Why \(n-1\)? This is Bessel’s correction; it gives an unbiased estimate of the population variance.

### 6.2 Standard Deviation (SD)
Square root of the variance – brings the units back to the original scale.

```r
sd(msleep$sleep_total)     # 4.450357
sqrt(var(msleep$sleep_total))  # same
```

Interpretation: a typical mammal’s sleep time is about 4.45 hours away from the mean of 10.43 hours.

### 6.3 Mean Absolute Deviation (MAD)
Average of the *absolute* distances (no squaring).

```r
mean(abs(msleep$sleep_total - mean(msleep$sleep_total)))
# 3.566701
```

**Comparison**:
- SD squares distances → larger deviations are penalized more heavily.
- MAD treats every deviation equally.
- SD is far more common in practice.

---

## 7. Quartiles, Quantiles & Boxplots

### 7.1 Quartiles
Values that divide the data into four equal parts.

```r
quantile(msleep$sleep_total)
#   0%   25%   50%   75%  100%
# 1.90  7.85 10.10 13.75 19.90
```

- 25 % = first quartile (Q1)  
- 50 % = second quartile = **median**  
- 75 % = third quartile (Q3)

### 7.2 Boxplots
A visual summary based on the five-number summary (min, Q1, median, Q3, max).

```r
ggplot(msleep, aes(y = sleep_total)) +
  geom_boxplot()
```

### 7.3 Arbitrary quantiles
```r
# Every 20th percentile
quantile(msleep$sleep_total, probs = seq(0, 1, 0.2))
#  0%  20%  40%  60%  80% 100%
# 1.90 6.24 9.48 11.14 14.40 19.90
```

---

## 8. Interquartile Range (IQR)

The height of the box in a boxplot – a robust measure of spread.

```r
IQR <- quantile(msleep$sleep_total, 0.75) - 
       quantile(msleep$sleep_total, 0.25)
IQR
# 5.9
```

Or simply:
```r
IQR(msleep$sleep_total)
```

---

## 9. Detecting Outliers

A common rule (Tukey’s method):

A point is an **outlier** if it lies outside the interval

\[
[Q_1 - 1.5 \times \text{IQR},\quad Q_3 + 1.5 \times \text{IQR}]
\]

### Example with body weight

```r
iqr <- IQR(msleep$bodywt)

lower <- quantile(msleep$bodywt, 0.25) - 1.5 * iqr
upper <- quantile(msleep$bodywt, 0.75) + 1.5 * iqr

msleep %>% 
  filter(bodywt < lower | bodywt > upper) %>% 
  select(name, vore, sleep_total, bodywt)
```

This returns the heaviest mammals (cow, Asian elephant, horse, …) – clear outliers in body weight.

---

## Quick Reference – Useful R Functions

| Task                        | Function                          |
|-----------------------------|-----------------------------------|
| Mean                        | `mean(x)`                         |
| Median                      | `median(x)`                       |
| Mode (most frequent)        | `count(..., sort = TRUE)`         |
| Variance                    | `var(x)`                          |
| Standard deviation          | `sd(x)`                           |
| Mean absolute deviation     | `mean(abs(x - mean(x)))`          |
| Quartiles / quantiles       | `quantile(x, probs = ...)`        |
| Interquartile range         | `IQR(x)`                          |
| Boxplot                     | `geom_boxplot()`                  |
| Filter outliers             | `filter(x < Q1-1.5*IQR \| x > Q3+1.5*IQR)` |

---

## Key Takeaways

1. **Statistics** = collecting + analyzing data + summarizing it.
2. Choose the right summary based on **data type**.
3. **Mean** is sensitive to outliers; **median** is robust.
4. Always look at the **shape** (skew) of the distribution before deciding which measure of center to report.
5. **Standard deviation** and **IQR** are the two most common measures of spread.
6. The **1.5 × IQR rule** is a practical way to flag potential outliers.

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

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```
















