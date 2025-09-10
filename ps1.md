Problem Set 1
================
Catherine Shingler
2025-09-08

### Introduction

This problem set is designed to test your understanding of data
wrangling concepts using the **`tidyverse`**, specifically **`dplyr`**
and **`tidyr`**, as well as foundational R concepts. You’ll primarily
work with the `mtcars`, a simulated sales dataset, and new simulated
student demographic/grade datasets, applying core verbs and functions to
prepare data for analysis. This activity should take approximately 60
minutes.

### Instructions

- Read each task carefully.
- Write your R code in the provided code chunks.
- Run the code to see your output and verify your results.
- `mtcars` is a built-in R dataset (or part of the `tidyverse`
  installation).

------------------------------------------------------------------------

## Part 0: R Fundamentals (10 minutes)

This section will test your understanding of basic R syntax, data types,
and vector operations.

### Task 0.1: Data Types and Logical Operations

1.  Create a variable `course_name` and assign it the string
    `"Introduction to Data Science"`.
2.  Create a variable `num_students` and assign it the integer value
    `45`.
3.  Check the data type (`class()`) of `course_name` and `num_students`.
4.  Create a numeric variable `pi_value` with the value $3.14159$.
5.  Create a logical variable `is_active` and set it to `TRUE`.
6.  Evaluate the following logical expressions and print their results:
    - Is `num_students` greater than or equal to `50`?
    - Is `course_name` exactly equal to `"introduction to data science"`
      (case-sensitive)?

``` r
# Your code here
course_name <- "Introduction to Data Science"
num_students <- 45
class(course_name)
```

    ## [1] "character"

``` r
class(num_students)
```

    ## [1] "numeric"

``` r
pi_value <- 3.14159
is_active <- TRUE
print(num_students >= 50)
```

    ## [1] FALSE

``` r
print(course_name == "introduction to data science")
```

    ## [1] FALSE

------------------------------------------------------------------------

### Task 0.2: Working with Vectors

1.  Create a numeric vector named `temperatures` with the values
    $22, 25, 19, 28, 23$.
2.  Calculate the `sum()` and `mean()` of the `temperatures` vector.
3.  Add a new temperature, $30$, to the `temperatures` vector (re-assign
    the variable).
4.  Create a new logical vector named `is_hot` that is `TRUE` for
    temperatures greater than $25$ and `FALSE` otherwise.

``` r
# Your code here
temperatures <- c(22, 25, 19, 28, 23)
sum(temperatures)
```

    ## [1] 117

``` r
mean(temperatures)
```

    ## [1] 23.4

``` r
temperatures <- c(22, 25, 19, 28, 23, 30)
is_hot <- c(temperatures>25)
```

------------------------------------------------------------------------

## Part 1: `dplyr` Fundamentals with `mtcars` (25 minutes)

This section focuses on applying core `dplyr` verbs to the classic
`mtcars` dataset. The `mtcars` dataset contains information about
various car models from 1973-74.

### Task 1.1: Filtering and Arranging

1.  The `mtcars` dataset has been pre-processed for you to include
    `car_model` as a column.
2.  Filter the dataset to include only cars with `cyl` (number of
    cylinders) equal to `4` or `6` AND `mpg` (miles per gallon) greater
    than `20`.
3.  Arrange the results first by `mpg` in ascending order, and then by
    `cyl` in descending order.

``` r
# Pre-processing step: Convert rownames to car_model
mtcars_processed <- mtcars %>%
  rownames_to_column("car_model")

# Your code starts here: Filter and arrange mtcars_processed
mtcars_processed %>% filter(cyl == 4 | cyl ==6 & mpg>20) %>% arrange(mpg, desc(cyl))
```

    ##         car_model  mpg cyl  disp  hp drat    wt  qsec vs am gear carb
    ## 1       Mazda RX4 21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
    ## 2   Mazda RX4 Wag 21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
    ## 3  Hornet 4 Drive 21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
    ## 4      Volvo 142E 21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
    ## 5   Toyota Corona 21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
    ## 6      Datsun 710 22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
    ## 7        Merc 230 22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
    ## 8       Merc 240D 24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
    ## 9   Porsche 914-2 26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
    ## 10      Fiat X1-9 27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
    ## 11    Honda Civic 30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
    ## 12   Lotus Europa 30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
    ## 13       Fiat 128 32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
    ## 14 Toyota Corolla 33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1

### Short Answer

Based on your filtered and arranged results, briefly describe the
characteristics of the cars that appear at the top of your output. What
does this tell you about the relationship between cylinders and miles
per gallon in this subset?

The first 3 cars that appear at the top of the output have the lowest
mpg and also all have 6 cylinders. The rest of the cars in the output
have 4 cylinders and mpg greater than or equal to 21.4 mpg, the highest
of the 6 cylinder cars that are over 20 mpg. This suggests that in this
subset, cars with fewer cylinders have higher mpg.

------------------------------------------------------------------------

### Task 1.2: Mutating and Selecting

Using the `mtcars_processed` dataset:

1.  Create a new column `hp_per_wt` by dividing `hp` (horsepower) by
    `wt` (weight in 1000 lbs).
2.  Create another new column `qsec_category` based on `qsec` (1/4 mile
    time):
    - `"Fast"` if `qsec < 17`
    - `"Medium"` if `qsec >= 17` and `qsec < 19`
    - `"Slow"` if `qsec >= 19`
3.  Select only the `car_model`, `mpg`, `hp`, `wt`, `hp_per_wt`, and
    `qsec_category` columns.

``` r
# Your code here
mtcars_processed %>% mutate(hp_per_wt = hp/wt, qsec_category = case_when(qsec < 17 ~ "Fast", qsec >= 17 & qsec < 19 ~ "Medium", qsec >= 19 ~ "Slow")) %>% select(car_model, mpg, hp, wt, hp_per_wt, qsec_category)
```

    ##              car_model  mpg  hp    wt hp_per_wt qsec_category
    ## 1            Mazda RX4 21.0 110 2.620  41.98473          Fast
    ## 2        Mazda RX4 Wag 21.0 110 2.875  38.26087        Medium
    ## 3           Datsun 710 22.8  93 2.320  40.08621        Medium
    ## 4       Hornet 4 Drive 21.4 110 3.215  34.21462          Slow
    ## 5    Hornet Sportabout 18.7 175 3.440  50.87209        Medium
    ## 6              Valiant 18.1 105 3.460  30.34682          Slow
    ## 7           Duster 360 14.3 245 3.570  68.62745          Fast
    ## 8            Merc 240D 24.4  62 3.190  19.43574          Slow
    ## 9             Merc 230 22.8  95 3.150  30.15873          Slow
    ## 10            Merc 280 19.2 123 3.440  35.75581        Medium
    ## 11           Merc 280C 17.8 123 3.440  35.75581        Medium
    ## 12          Merc 450SE 16.4 180 4.070  44.22604        Medium
    ## 13          Merc 450SL 17.3 180 3.730  48.25737        Medium
    ## 14         Merc 450SLC 15.2 180 3.780  47.61905        Medium
    ## 15  Cadillac Fleetwood 10.4 205 5.250  39.04762        Medium
    ## 16 Lincoln Continental 10.4 215 5.424  39.63864        Medium
    ## 17   Chrysler Imperial 14.7 230 5.345  43.03087        Medium
    ## 18            Fiat 128 32.4  66 2.200  30.00000          Slow
    ## 19         Honda Civic 30.4  52 1.615  32.19814        Medium
    ## 20      Toyota Corolla 33.9  65 1.835  35.42234          Slow
    ## 21       Toyota Corona 21.5  97 2.465  39.35091          Slow
    ## 22    Dodge Challenger 15.5 150 3.520  42.61364          Fast
    ## 23         AMC Javelin 15.2 150 3.435  43.66812        Medium
    ## 24          Camaro Z28 13.3 245 3.840  63.80208          Fast
    ## 25    Pontiac Firebird 19.2 175 3.845  45.51365        Medium
    ## 26           Fiat X1-9 27.3  66 1.935  34.10853        Medium
    ## 27       Porsche 914-2 26.0  91 2.140  42.52336          Fast
    ## 28        Lotus Europa 30.4 113 1.513  74.68605          Fast
    ## 29      Ford Pantera L 15.8 264 3.170  83.28076          Fast
    ## 30        Ferrari Dino 19.7 175 2.770  63.17690          Fast
    ## 31       Maserati Bora 15.0 335 3.570  93.83754          Fast
    ## 32          Volvo 142E 21.4 109 2.780  39.20863        Medium

### Short Answer

Why might creating `hp_per_wt` and `qsec_category` be useful metrics
when analyzing car performance, beyond just raw horsepower and weight?

## Creating these new variables is useful because it provides more standardized measures to judge car performance by, as cars with similar horsepower but different weights will behave differently. These variables help make it easier to see trends in car performance.

### Task 1.3: Grouped Summaries

Using the `mtcars_processed` dataset:

1.  Group the data by `cyl` (number of cylinders) and `am` (transmission
    type: 0 for automatic, 1 for manual).
2.  For each group, calculate the **average `mpg`**, the **median
    `hp`**, and the **number of cars** (`n()`).
3.  Arrange the final result first by `cyl` (ascending) and then by
    `average_mpg` (descending).

``` r
# Your code here
mtcars_processed %>% group_by(cyl, am) %>% summarize(average_mpg = mean(mpg), median_hp = median(hp), count = n()) %>% arrange(cyl, desc(average_mpg))
```

    ## # A tibble: 6 × 5
    ## # Groups:   cyl [3]
    ##     cyl    am average_mpg median_hp count
    ##   <dbl> <dbl>       <dbl>     <dbl> <int>
    ## 1     4     1        28.1      78.5     8
    ## 2     4     0        22.9      95       3
    ## 3     6     1        20.6     110       3
    ## 4     6     0        19.1     116.      4
    ## 5     8     1        15.4     300.      2
    ## 6     8     0        15.0     180      12

### Short Answer

Based on your grouped summary, what general trends do you observe
regarding average `mpg` and median `hp` across different combinations of
`cylinders` and `transmission` types? How do these summaries help
differentiate car characteristics?

Average mpg decreases as the number of cylinders increases but median
horsepower increases, across all cylinders, cars with automatic
transitions have lower average mpg, for cars with automatic
transmissions, median horsepower is higher in cars with 4 or 6
cylinders, but lower for cars with 8 cylinders than manual transmission
cars with the same number of cylinders.

------------------------------------------------------------------------

## Part 2: Reshaping and Joining Data (25 minutes)

For this part, we’ll explore reshaping and joining using a simulated
sales dataset and new simulated student enrollment data.

**Run this code chunk first to set up the data:**

``` r
# Part 2 Data Setup
product_sales_wide <- tibble(
  product = c("Laptop", "Monitor", "Keyboard"),
  `2020` = c(1000, 500, 800),
  `2021` = c(1100, 550, 850),
  `2022` = c(1250, 600, 900)
) %>%
  rename(Year_2020 = `2020`, Year_2021 = `2021`, Year_2022 = `2022`) # Rename to avoid issues with non-syntactic names

# Simulated Student and Grade Data
students_demographics <- tibble(
  student_id = c("S001", "S002", "S003", "S004", "S005", "S007"), # S007 is a new student, no grades yet
  student_name = c("Alice", "Bob", "Charlie", "David", "Eve", "Frank"),
  major = c("CS", "Math", "Physics", "CS", "Biology", "History"),
  enrollment_year = c(2020, 2021, 2020, 2022, 2021, 2023)
)

student_grades <- tibble(
  student_id = c("S001", "S002", "S003", "S001", "S006", "S004"), # S006 has grades but no demographic info
  course_id = c("CS101", "MA201", "PH301", "CS102", "BI101", "CS205"),
  semester = c("Fall 2020", "Spring 2022", "Fall 2021", "Spring 2021", "Fall 2021", "Spring 2023"),
  grade_score = c(92, 85, 88, 78, 75, 90) # Assuming numeric scores for easier calculation
)

# Display the dataframes
print(product_sales_wide)
```

    ## # A tibble: 3 × 4
    ##   product  Year_2020 Year_2021 Year_2022
    ##   <chr>        <dbl>     <dbl>     <dbl>
    ## 1 Laptop        1000      1100      1250
    ## 2 Monitor        500       550       600
    ## 3 Keyboard       800       850       900

``` r
print(students_demographics)
```

    ## # A tibble: 6 × 4
    ##   student_id student_name major   enrollment_year
    ##   <chr>      <chr>        <chr>             <dbl>
    ## 1 S001       Alice        CS                 2020
    ## 2 S002       Bob          Math               2021
    ## 3 S003       Charlie      Physics            2020
    ## 4 S004       David        CS                 2022
    ## 5 S005       Eve          Biology            2021
    ## 6 S007       Frank        History            2023

``` r
print(student_grades)
```

    ## # A tibble: 6 × 4
    ##   student_id course_id semester    grade_score
    ##   <chr>      <chr>     <chr>             <dbl>
    ## 1 S001       CS101     Fall 2020            92
    ## 2 S002       MA201     Spring 2022          85
    ## 3 S003       PH301     Fall 2021            88
    ## 4 S001       CS102     Spring 2021          78
    ## 5 S006       BI101     Fall 2021            75
    ## 6 S004       CS205     Spring 2023          90

------------------------------------------------------------------------

### Task 2.1: New Variable Creation: Wide vs. Long (`product_sales` dataset)

This task demonstrates how creating new variables that depend on
previous time periods is much easier in a long (tidy) format.

1.  **Analysis in Wide Format:** Using the `product_sales_wide` dataset,
    create new columns for the **year-over-year growth rate** for 2021
    and 2022.
    - `Growth_2021`: `(Year_2021 - Year_2020) / Year_2020 * 100`
    - `Growth_2022`: `(Year_2022 - Year_2021) / Year_2021 * 100`

``` r
# Your code here
product_sales_wide %>% mutate(Growth_2021 = (Year_2021 - Year_2020) / Year_2020 * 100, Growth_2022 = (Year_2022 - Year_2021) / Year_2021 * 100)
```

    ## # A tibble: 3 × 6
    ##   product  Year_2020 Year_2021 Year_2022 Growth_2021 Growth_2022
    ##   <chr>        <dbl>     <dbl>     <dbl>       <dbl>       <dbl>
    ## 1 Laptop        1000      1100      1250       10          13.6 
    ## 2 Monitor        500       550       600       10           9.09
    ## 3 Keyboard       800       850       900        6.25        5.88

2.  **Reshape to Long Format:** Reshape `product_sales_wide` into a long
    format called `product_sales_long`. Pivot the year columns
    (`Year_2020`, `Year_2021`, `Year_2022`) into two new columns: `Year`
    and `Sales`. Make sure `Year` is numeric.

``` r
# Your code here
product_sales_long <- product_sales_wide %>% pivot_longer(cols = "Year_2020":"Year_2022", names_to = "Year", values_to = "Sales")
product_sales_long
```

    ## # A tibble: 9 × 3
    ##   product  Year      Sales
    ##   <chr>    <chr>     <dbl>
    ## 1 Laptop   Year_2020  1000
    ## 2 Laptop   Year_2021  1100
    ## 3 Laptop   Year_2022  1250
    ## 4 Monitor  Year_2020   500
    ## 5 Monitor  Year_2021   550
    ## 6 Monitor  Year_2022   600
    ## 7 Keyboard Year_2020   800
    ## 8 Keyboard Year_2021   850
    ## 9 Keyboard Year_2022   900

3.  **Analysis in Long Format:** Using the `product_sales_long` dataset,
    calculate a single `Growth_Rate` column that represents the
    year-over-year growth for each product. You should use `group_by()`
    and `lag()`. The formula for growth rate is
    `(current_year_sales - previous_year_sales) / previous_year_sales * 100`.

``` r
# Your code here
product_sales_long %>% group_by(product) %>% mutate(previous_year_sales = lag(Sales), "Growth_Rate" = (Sales - previous_year_sales) / previous_year_sales * 100)
```

    ## # A tibble: 9 × 5
    ## # Groups:   product [3]
    ##   product  Year      Sales previous_year_sales Growth_Rate
    ##   <chr>    <chr>     <dbl>               <dbl>       <dbl>
    ## 1 Laptop   Year_2020  1000                  NA       NA   
    ## 2 Laptop   Year_2021  1100                1000       10   
    ## 3 Laptop   Year_2022  1250                1100       13.6 
    ## 4 Monitor  Year_2020   500                  NA       NA   
    ## 5 Monitor  Year_2021   550                 500       10   
    ## 6 Monitor  Year_2022   600                 550        9.09
    ## 7 Keyboard Year_2020   800                  NA       NA   
    ## 8 Keyboard Year_2021   850                 800        6.25
    ## 9 Keyboard Year_2022   900                 850        5.88

### Short Answer

Compare the code required to calculate the year-over-year growth rate in
the wide format versus the long format. Which approach is more concise
and scalable if you had many more years of data? Why is the long format
generally preferred for this type of time-series calculation?

The code for the long format is a formula that can be applied if more
observations are added, whereas in the wide format, it is an individual
calculation for each observation that would have to be done if more
observations was added. The long format is generally preferred because
there is less work needed to generate the time-series calculation when
more observations are added.

------------------------------------------------------------------------

### Task 2.2: Mutating Joins: Student Demographics and Grades

This task explores combining student demographic information with their
academic performance. Pay close attention to how different join types
handle students who may or may not have corresponding grade records.

1.  **Inner Join:** Perform an `inner_join()` to combine
    `students_demographics` with `student_grades` based on `student_id`.
    This will show students who have **both** demographic information
    and at least one grade record.
2.  **Left Join:** Perform a `left_join()` to combine
    `students_demographics` (left table) with `student_grades` based on
    `student_id`. This will keep **all students** from the demographic
    data and add their grade records where available.
3.  **Advanced Mutate (after Left Join):** After performing the left
    join, calculate the **average `grade_score` for each student**. This
    will require grouping by `student_id` and `student_name`.

``` r
# Your code for inner_join and its explanation
students_demographics %>% inner_join(student_grades, by = "student_id")
```

    ## # A tibble: 5 × 7
    ##   student_id student_name major   enrollment_year course_id semester grade_score
    ##   <chr>      <chr>        <chr>             <dbl> <chr>     <chr>          <dbl>
    ## 1 S001       Alice        CS                 2020 CS101     Fall 20…          92
    ## 2 S001       Alice        CS                 2020 CS102     Spring …          78
    ## 3 S002       Bob          Math               2021 MA201     Spring …          85
    ## 4 S003       Charlie      Physics            2020 PH301     Fall 20…          88
    ## 5 S004       David        CS                 2022 CS205     Spring …          90

This includes all of the students currently enrolled in courses from the
student_demographics information.

``` r
# Your code for left_join and its explanation
left <- students_demographics %>% left_join(student_grades, by = "student_id")
left
```

    ## # A tibble: 7 × 7
    ##   student_id student_name major   enrollment_year course_id semester grade_score
    ##   <chr>      <chr>        <chr>             <dbl> <chr>     <chr>          <dbl>
    ## 1 S001       Alice        CS                 2020 CS101     Fall 20…          92
    ## 2 S001       Alice        CS                 2020 CS102     Spring …          78
    ## 3 S002       Bob          Math               2021 MA201     Spring …          85
    ## 4 S003       Charlie      Physics            2020 PH301     Fall 20…          88
    ## 5 S004       David        CS                 2022 CS205     Spring …          90
    ## 6 S005       Eve          Biology            2021 <NA>      <NA>              NA
    ## 7 S007       Frank        History            2023 <NA>      <NA>              NA

This includes all of the students, including those that aren’t enrolled
in classes currently (Eve and Frank).

``` r
# Your code for advanced mutate after left join
left %>% group_by(student_id, student_name) %>% mutate(average_grade_score = mean(grade_score))
```

    ## # A tibble: 7 × 8
    ## # Groups:   student_id, student_name [6]
    ##   student_id student_name major   enrollment_year course_id semester grade_score
    ##   <chr>      <chr>        <chr>             <dbl> <chr>     <chr>          <dbl>
    ## 1 S001       Alice        CS                 2020 CS101     Fall 20…          92
    ## 2 S001       Alice        CS                 2020 CS102     Spring …          78
    ## 3 S002       Bob          Math               2021 MA201     Spring …          85
    ## 4 S003       Charlie      Physics            2020 PH301     Fall 20…          88
    ## 5 S004       David        CS                 2022 CS205     Spring …          90
    ## 6 S005       Eve          Biology            2021 <NA>      <NA>              NA
    ## 7 S007       Frank        History            2023 <NA>      <NA>              NA
    ## # ℹ 1 more variable: average_grade_score <dbl>

### Short Answer

Compare the number of rows and the content of the `inner_join()` and
`left_join()` results. Specifically, identify which student(s) are
present in one join but not the other, and explain why. What does the
average grade calculation reveal about students with multiple grades or
no grades?

Eve and Frank are present in the left join but not the inner join
because they are not currently enrolled in any courses. The average
grade calculation shows Alice’s average grade in both rows, and just
says NA for Eve and Frank, who don’t have any grades.

------------------------------------------------------------------------

### Task 2.3: Filtering Joins: Identifying Student Enrollment Status

This task focuses on using filtering joins to identify different
categories of students based on their enrollment and grade records.

1.  **Enrolled Students:** Use a `semi_join()` to identify which
    students from `students_demographics` are actually enrolled in and
    have a grade record in `student_grades`. This should return only
    columns from `students_demographics`.
2.  **Students Without Grades:** Use an `anti_join()` to identify which
    students from `students_demographics` are in the system but
    currently do *not* have any grade records in `student_grades` (e.g.,
    new students, or those who haven’t completed courses yet). This
    should return only columns from `students_demographics`.
3.  **Grades Without Students:** Use an `anti_join()` to identify any
    grade records in `student_grades` that do *not* correspond to an
    existing student in `students_demographics` (e.g., a data entry
    error or a record for a past student no longer in the demographic
    system). This should return only columns from `student_grades`.

``` r
# Your code for question 1 (semi_join)
students_demographics %>% semi_join(student_grades, by = "student_id")
```

    ## # A tibble: 4 × 4
    ##   student_id student_name major   enrollment_year
    ##   <chr>      <chr>        <chr>             <dbl>
    ## 1 S001       Alice        CS                 2020
    ## 2 S002       Bob          Math               2021
    ## 3 S003       Charlie      Physics            2020
    ## 4 S004       David        CS                 2022

``` r
# Your code for question 2 (anti_join on students_demographics)
students_demographics %>% anti_join(student_grades, by = "student_id")
```

    ## # A tibble: 2 × 4
    ##   student_id student_name major   enrollment_year
    ##   <chr>      <chr>        <chr>             <dbl>
    ## 1 S005       Eve          Biology            2021
    ## 2 S007       Frank        History            2023

``` r
# Your code for question 3 (anti_join on student_grades)
student_grades %>% anti_join(students_demographics, by = "student_id")
```

    ## # A tibble: 1 × 4
    ##   student_id course_id semester  grade_score
    ##   <chr>      <chr>     <chr>           <dbl>
    ## 1 S006       BI101     Fall 2021          75

### Short Answer

Describe the distinct insights gained from each of the three filtering
joins in this task. How do these joins help in data validation and
understanding the completeness of your student records?

From the first filtering join, I can see that there are only 4 students
in student_demographics with current grades. From the secont filtering
join, I can see the 2 students in student_demographics that do not have
current grades, and from the last filtering join, I can see that there
is also on student who has a current grade but does not appear in
student_demographics. Together, these show all of the observations in
either student_grades or student_demographics, as well as the
observations that are common between the two.
