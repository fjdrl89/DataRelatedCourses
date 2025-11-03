# Exploring NYC Public School Test Result Scores

## Project Description

- Every year, school test results impact the college admissions fate of millions of students.
- In this project, you will use standardized test performance data from NYC's public schools to identify the schools with top math results, look at how performance varies by borough, and find the city's top ten performing schools!

## Project Instructions

Which NYC schools have the best math results?
- The best math results are at least 80% of the *maximum possible score of 800* for math.
- Save your results in a pandas DataFrame called `best_math_schools`, including `"school_name"` and `"average_math"` columns, sorted by `"average_math"` in descending order.

What are the top 10 performing schools based on the combined SAT scores?
- Save your results as a pandas DataFrame called `top_10_schools` containing the `"school_name"` and a new column named `"total_SAT"`, with results ordered by `"total_SAT"` in descending order (`"total_SAT"` being the sum of math, reading, and writing scores).

Which single borough has the largest standard deviation in the combined SAT score?
- Save your results as a pandas DataFrame called `largest_std_dev`.
- The DataFrame should contain one row, with:
    * `"borough"` - the name of the NYC borough with the largest standard deviation of `"total_SAT"`.
    * `"num_schools"` - the number of schools in the borough.
    * `"average_SAT"` - the mean of `"total_SAT"`.
    * `"std_SAT"` - the standard deviation of `"total_SAT"`.

- Round all numeric values to two decimal places.

