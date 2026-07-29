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



# 

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```

```r

```
















