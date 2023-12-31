Mini Data Analysis Milestone 2
================

*To complete this milestone, you can either edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to the rest of your mini data analysis project!

In Milestone 1, you explored your data. and came up with research
questions. This time, we will finish up our mini data analysis and
obtain results for your data by:

- Making summary tables and graphs
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

We will also explore more in depth the concept of *tidy data.*

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 50 points: 45 for your analysis, and
5 for overall reproducibility, cleanliness, and coherence of the Github
submission.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

- Understand what *tidy* data is, and how to create it using `tidyr`.
- Generate a reproducible and clear report using R Markdown.
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
library(ggplot2)
library(dplyr)
library(broom)
library(here)
```

# Task 1: Process and summarize your data

From milestone 1, you should have an idea of the basic structure of your
dataset (e.g. number of rows and columns, class types, etc.). Here, we
will start investigating your data more in-depth using various data
manipulation functions.

### 1.1 (1 point)

First, write out the 4 research questions you defined in milestone 1
were. This will guide your work through milestone 2:

<!-------------------------- Start your work below ---------------------------->

1.  *Are the smoothness related to the compactness features? If so, how
    strong they are related and how to use them to predict cancer
    diagnosis?*
2.  *Can the values in each column reflect the tumor status? If in the
    real research field, how should we evaluate these values?*
3.  *In spite of smoothness, what else can we select to predict tumors?
    and how to compare the new one with smoothness?*
4.  *How are the values different in benign group? Can we find any clue
    from the values that can predict it before it turns to malignant?*
    <!----------------------------------------------------------------------------->

Here, we will investigate your data using various data manipulation and
graphing functions.

### 1.2 (8 points)

Now, for each of your four research questions, choose one task from
options 1-4 (summarizing), and one other task from 4-8 (graphing). You
should have 2 tasks done for each research question (8 total). Make sure
it makes sense to do them! (e.g. don’t use a numerical variables for a
task that needs a categorical variable.). Comment on why each task helps
(or doesn’t!) answer the corresponding research question.

Ensure that the output of each operation is printed!

Also make sure that you’re using dplyr and ggplot2 rather than base R.
Outside of this project, you may find that you prefer using base R
functions for certain tasks, and that’s just fine! But part of this
project is for you to practice the tools we learned in class, which is
dplyr and ggplot2.

**Summarizing:**

1.  Compute the *range*, *mean*, and *two other summary statistics* of
    **one numerical variable** across the groups of **one categorical
    variable** from your data.
2.  Compute the number of observations for at least one of your
    categorical variables. Do not use the function `table()`!
3.  Create a categorical variable with 3 or more groups from an existing
    numerical variable. You can use this new variable in the other
    tasks! *An example: age in years into “child, teen, adult, senior”.*
4.  Compute the proportion and counts in each category of one
    categorical variable across the groups of another categorical
    variable from your data. Do not use the function `table()`!

**Graphing:**

6.  Create a graph of your choosing, make one of the axes logarithmic,
    and format the axes labels so that they are “pretty” or easier to
    read.
7.  Make a graph where it makes sense to customize the alpha
    transparency.

Using variables and/or tables you made in one of the “Summarizing”
tasks:

8.  Create a graph that has at least two geom layers.
9.  Create 3 histograms, with each histogram having different sized
    bins. Pick the “best” one and explain why it is the best.

Make sure it’s clear what research question you are doing each operation
for!

<!------------------------- Start your work below ----------------------------->

\#Question 1: Are the smoothness related to the compactness features?
Before solving the question, let’s review the data first by use
glimpse() I have this question for exploring any potential relationship
between these two features, as both are related to the appearnce of
tumors as I assumed. I choose task1 and task7,8 in this question as
required, and later I will also test the “correlation” between
smoothness_mean and compactness_mean.

``` r
glimpse(cancer_sample) #review the data first
```

    ## Rows: 569
    ## Columns: 32
    ## $ ID                      <dbl> 842302, 842517, 84300903, 84348301, 84358402, …
    ## $ diagnosis               <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "…
    ## $ radius_mean             <dbl> 17.990, 20.570, 19.690, 11.420, 20.290, 12.450…
    ## $ texture_mean            <dbl> 10.38, 17.77, 21.25, 20.38, 14.34, 15.70, 19.9…
    ## $ perimeter_mean          <dbl> 122.80, 132.90, 130.00, 77.58, 135.10, 82.57, …
    ## $ area_mean               <dbl> 1001.0, 1326.0, 1203.0, 386.1, 1297.0, 477.1, …
    ## $ smoothness_mean         <dbl> 0.11840, 0.08474, 0.10960, 0.14250, 0.10030, 0…
    ## $ compactness_mean        <dbl> 0.27760, 0.07864, 0.15990, 0.28390, 0.13280, 0…
    ## $ concavity_mean          <dbl> 0.30010, 0.08690, 0.19740, 0.24140, 0.19800, 0…
    ## $ concave_points_mean     <dbl> 0.14710, 0.07017, 0.12790, 0.10520, 0.10430, 0…
    ## $ symmetry_mean           <dbl> 0.2419, 0.1812, 0.2069, 0.2597, 0.1809, 0.2087…
    ## $ fractal_dimension_mean  <dbl> 0.07871, 0.05667, 0.05999, 0.09744, 0.05883, 0…
    ## $ radius_se               <dbl> 1.0950, 0.5435, 0.7456, 0.4956, 0.7572, 0.3345…
    ## $ texture_se              <dbl> 0.9053, 0.7339, 0.7869, 1.1560, 0.7813, 0.8902…
    ## $ perimeter_se            <dbl> 8.589, 3.398, 4.585, 3.445, 5.438, 2.217, 3.18…
    ## $ area_se                 <dbl> 153.40, 74.08, 94.03, 27.23, 94.44, 27.19, 53.…
    ## $ smoothness_se           <dbl> 0.006399, 0.005225, 0.006150, 0.009110, 0.0114…
    ## $ compactness_se          <dbl> 0.049040, 0.013080, 0.040060, 0.074580, 0.0246…
    ## $ concavity_se            <dbl> 0.05373, 0.01860, 0.03832, 0.05661, 0.05688, 0…
    ## $ concave_points_se       <dbl> 0.015870, 0.013400, 0.020580, 0.018670, 0.0188…
    ## $ symmetry_se             <dbl> 0.03003, 0.01389, 0.02250, 0.05963, 0.01756, 0…
    ## $ fractal_dimension_se    <dbl> 0.006193, 0.003532, 0.004571, 0.009208, 0.0051…
    ## $ radius_worst            <dbl> 25.38, 24.99, 23.57, 14.91, 22.54, 15.47, 22.8…
    ## $ texture_worst           <dbl> 17.33, 23.41, 25.53, 26.50, 16.67, 23.75, 27.6…
    ## $ perimeter_worst         <dbl> 184.60, 158.80, 152.50, 98.87, 152.20, 103.40,…
    ## $ area_worst              <dbl> 2019.0, 1956.0, 1709.0, 567.7, 1575.0, 741.6, …
    ## $ smoothness_worst        <dbl> 0.1622, 0.1238, 0.1444, 0.2098, 0.1374, 0.1791…
    ## $ compactness_worst       <dbl> 0.6656, 0.1866, 0.4245, 0.8663, 0.2050, 0.5249…
    ## $ concavity_worst         <dbl> 0.71190, 0.24160, 0.45040, 0.68690, 0.40000, 0…
    ## $ concave_points_worst    <dbl> 0.26540, 0.18600, 0.24300, 0.25750, 0.16250, 0…
    ## $ symmetry_worst          <dbl> 0.4601, 0.2750, 0.3613, 0.6638, 0.2364, 0.3985…
    ## $ fractal_dimension_worst <dbl> 0.11890, 0.08902, 0.08758, 0.17300, 0.07678, 0…

``` r
#Task1: compute the summary:
summary_stats <- cancer_sample %>%
  group_by(diagnosis) %>%
  summarize(
    Mean_Smooth = mean(smoothness_mean),
    Std_Dev_SM = sd(smoothness_mean),
    Mean_Compact = mean(compactness_mean),
    Std_Dev_CP =sd(compactness_mean)
  )
print(summary_stats)
```

    ## # A tibble: 2 × 5
    ##   diagnosis Mean_Smooth Std_Dev_SM Mean_Compact Std_Dev_CP
    ##   <chr>           <dbl>      <dbl>        <dbl>      <dbl>
    ## 1 B              0.0925     0.0134       0.0801     0.0337
    ## 2 M              0.103      0.0126       0.145      0.0540

\#Test the correlation between smoothness and compactness

``` r
correlation_result <- cancer_sample %>% 
  select(smoothness_mean, compactness_mean) %>%
  summarize(correlation = cor(smoothness_mean, compactness_mean))
```

\#Visualization the correlation

``` r
ggplot(cancer_sample, aes(x = smoothness_mean, y = compactness_mean,color = diagnosis)) +
  geom_point(alpha=0.7) +
  geom_smooth(method = "loess", se = FALSE, color = "blue") +  # Add a smoothing line
  labs(x = "Smoothness Mean", y = "Compactness Mean") +
  ggtitle("Scatterplot of Smoothness vs. Compactness with Smoothing Line")
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](MDA_M2_MmZh_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->
\#Comment on the result for Question 1: From the graph, we can see the
smoothness and compactness are positive related in the trend both in M
and B groups generally. \#Question 2: Can the values in each column
reflect the tumor status? How should we evaluate these values? This is a
general question for any dataset that we may be curious about. Let’s
explore it by summerizing them. Before that, we also need to know what
the coloumns are. I choose Task1 and 7 as required.

``` r
#First let's see what columns are included in the dataset
list(colnames(cancer_sample))
```

    ## [[1]]
    ##  [1] "ID"                      "diagnosis"              
    ##  [3] "radius_mean"             "texture_mean"           
    ##  [5] "perimeter_mean"          "area_mean"              
    ##  [7] "smoothness_mean"         "compactness_mean"       
    ##  [9] "concavity_mean"          "concave_points_mean"    
    ## [11] "symmetry_mean"           "fractal_dimension_mean" 
    ## [13] "radius_se"               "texture_se"             
    ## [15] "perimeter_se"            "area_se"                
    ## [17] "smoothness_se"           "compactness_se"         
    ## [19] "concavity_se"            "concave_points_se"      
    ## [21] "symmetry_se"             "fractal_dimension_se"   
    ## [23] "radius_worst"            "texture_worst"          
    ## [25] "perimeter_worst"         "area_worst"             
    ## [27] "smoothness_worst"        "compactness_worst"      
    ## [29] "concavity_worst"         "concave_points_worst"   
    ## [31] "symmetry_worst"          "fractal_dimension_worst"

``` r
#Summerize the ones we are interested in
summary_all <- cancer_sample %>%
  group_by(diagnosis) %>%
  summarize(
    Mean_SM = mean(smoothness_mean),
    Std_Dev_SM = sd(smoothness_mean),
    Mean_RD = mean(radius_mean),
    Std_Dev_RD = sd(radius_mean),
    Mean_TT = mean(texture_mean),
    Std_Dev_TT =sd(texture_mean),
    Mean_PM = mean(perimeter_mean),
    Std_Dev_PM =sd(perimeter_mean),
    Mean_AR = mean(area_mean),
    Std_Dev_AR =sd(area_mean),
    Mean_CP = mean(compactness_mean),
    Std_Dev_CP =sd(compactness_mean),
    Mean_CV = mean(concavity_mean),
    Std_Dev_CV =sd(concavity_mean),
    Mean_CVP = mean(concave_points_mean),
    Std_Dev_CVP =sd(concave_points_mean),
    Mean_Sym = mean(symmetry_mean),
    Std_Dev_Sym =sd(symmetry_mean),
    Mean_FRD = mean(fractal_dimension_mean),
    Std_Dev_FRD =sd(fractal_dimension_mean)
      )
print(summary_all)
```

    ## # A tibble: 2 × 21
    ##   diagnosis Mean_SM Std_Dev_SM Mean_RD Std_Dev_RD Mean_TT Std_Dev_TT Mean_PM
    ##   <chr>       <dbl>      <dbl>   <dbl>      <dbl>   <dbl>      <dbl>   <dbl>
    ## 1 B          0.0925     0.0134    12.1       1.78    17.9       4.00    78.1
    ## 2 M          0.103      0.0126    17.5       3.20    21.6       3.78   115. 
    ## # ℹ 13 more variables: Std_Dev_PM <dbl>, Mean_AR <dbl>, Std_Dev_AR <dbl>,
    ## #   Mean_CP <dbl>, Std_Dev_CP <dbl>, Mean_CV <dbl>, Std_Dev_CV <dbl>,
    ## #   Mean_CVP <dbl>, Std_Dev_CVP <dbl>, Mean_Sym <dbl>, Std_Dev_Sym <dbl>,
    ## #   Mean_FRD <dbl>, Std_Dev_FRD <dbl>

\#Boxplot for the columns mentioned above

``` r
cancer_sample %>%
  pivot_longer(cols = c(smoothness_mean, radius_mean, texture_mean, perimeter_mean, area_mean, compactness_mean, concavity_mean, concave_points_mean, symmetry_mean, fractal_dimension_mean), names_to = "Variable", values_to = "Value") %>%
  ggplot(aes(x = diagnosis, y = Value, fill = diagnosis)) +
  geom_boxplot(width = 0.5,alpha = 0.7) +  
  facet_wrap(~Variable, scales = "free_y") +
  labs(title = "Boxplot of Different Features",
       x = "Diagnosis", y = "Value")
```

![](MDA_M2_MmZh_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->
\#Comment on result of Question 2: From the boxplot of all the related
feature in the dataset, we can see most of them can discriminate M and B
groups very well. \#Question 3: In spite of smoothness, what else can we
select to predict tumors? From the above boxplot, we notice the
“perimeter” probably can also predict the tumor. Let’s test it further.
Here I will take task 3:Create a categorical variable with the values
from perimeter first with 3 levels.

``` r
percentile_perimeter <- cancer_sample %>%
  summarize(
    q25_PM_mean = quantile(perimeter_mean, 0.25),
    q75_PM_mean = quantile(perimeter_mean, 0.75)
  )
print(percentile_perimeter)
```

    ## # A tibble: 1 × 2
    ##   q25_PM_mean q75_PM_mean
    ##         <dbl>       <dbl>
    ## 1        75.2        104.

``` r
cancer_sample <- cancer_sample %>%
  mutate(
    perimeter_mean_New = case_when(
      perimeter_mean < 75.17 ~ "low",
      perimeter_mean >=75.17 & perimeter_mean <= 104.1 ~ "medium",
      perimeter_mean >104.1 ~ "high"
    )
  )
```

\#Then we can visualize the above with Task7/8

``` r
ggplot(cancer_sample, aes(x = perimeter_mean_New)) +
  geom_bar(aes(fill = perimeter_mean_New), position = "dodge") +
  geom_density(aes(y = after_stat(count)), fill = "transparent", alpha = 0.5, position = "fill") +
  labs(
    title = "Histogram of 'perimeter_mean_New'",
    x = "Perimeter Mean Category",
    y = "Count"
  )
```

![](MDA_M2_MmZh_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->
\#Comments on result of Question 3: Perimeter is also a good predictor
for tumor. I further visualized the new category perimeter column. We
can see the distribution of the samples based on perimeter catergories.

\#Question 4: How are the values different in benign group? I ask this
question to see whether there will be any features performing
differently in benign group, which may have a different behavior later.
Since we already have a category in perimeter, I can group the data in
that way. I will take Task2 and Task 7 for the practice.

``` r
Answer.4 <- cancer_sample %>%
  filter(diagnosis == "B") %>% 
  group_by(perimeter_mean_New) %>% 
  summarise(
    count = n(),
    Mean_SM = mean(smoothness_mean),
    Std_Dev_SM = sd(smoothness_mean),
    Mean_RD = mean(radius_mean),
    Std_Dev_RD = sd(radius_mean),
    Mean_TT = mean(texture_mean),
    Std_Dev_TT =sd(texture_mean),
    Mean_PM = mean(perimeter_mean),
    Std_Dev_PM =sd(perimeter_mean),
    Mean_AR = mean(area_mean),
    Std_Dev_AR =sd(area_mean),
    Mean_CP = mean(compactness_mean),
    Std_Dev_CP =sd(compactness_mean),
    Mean_CV = mean(concavity_mean),
    Std_Dev_CV =sd(concavity_mean),
    Mean_CVP = mean(concave_points_mean),
    Std_Dev_CVP =sd(concave_points_mean),
    Mean_Sym = mean(symmetry_mean),
    Std_Dev_Sym =sd(symmetry_mean),
    Mean_FRD = mean(fractal_dimension_mean),
    Std_Dev_FRD =sd(fractal_dimension_mean))
print(Answer.4)
```

    ## # A tibble: 3 × 22
    ##   perimeter_mean_New count Mean_SM Std_Dev_SM Mean_RD Std_Dev_RD Mean_TT
    ##   <chr>              <int>   <dbl>      <dbl>   <dbl>      <dbl>   <dbl>
    ## 1 high                   6  0.0896     0.0104    16.6      0.650    16.3
    ## 2 low                  139  0.0948     0.0143    10.4      1.06     17.9
    ## 3 medium               212  0.0910     0.0128    13.2      0.986    18.0
    ## # ℹ 15 more variables: Std_Dev_TT <dbl>, Mean_PM <dbl>, Std_Dev_PM <dbl>,
    ## #   Mean_AR <dbl>, Std_Dev_AR <dbl>, Mean_CP <dbl>, Std_Dev_CP <dbl>,
    ## #   Mean_CV <dbl>, Std_Dev_CV <dbl>, Mean_CVP <dbl>, Std_Dev_CVP <dbl>,
    ## #   Mean_Sym <dbl>, Std_Dev_Sym <dbl>, Mean_FRD <dbl>, Std_Dev_FRD <dbl>

\#visualize Answer4 to see the distribution with Task 7

``` r
ggplot(Answer.4, aes(x = perimeter_mean_New, y = count,fill = perimeter_mean_New)) +
  geom_bar(stat = "identity",alpha = 0.7) +
  labs(title = "Counts of 'B' Group by 'perimeter_mean_New'",
       x = "perimeter_mean_New",
       y = "Count")
```

![](MDA_M2_MmZh_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->
\#Comments on the result of Question 4: We can see most of the samples
in B group (benign tumor) are among the low and medium range of
perimeter.
<!----------------------------------------------------------------------------->

### 1.3 (2 points)

Based on the operations that you’ve completed, how much closer are you
to answering your research questions? Think about what aspects of your
research questions remain unclear. Can your research questions be
refined, now that you’ve investigated your data a bit more? Which
research questions are yielding interesting results?

<!------------------------- Write your answer here ---------------------------->

\##Answer: From the boxplot of different features in cancer_sample
dataset, we can see there are several features that may be significant
preditor to malignant tumor, which is a good sign that most of my
questions can be answered with the tasks. And for some of those
features, they probably have positive correlation in the tumors, which
matches the expectation based on common sense and previous research.

However, I have not tested all the features. Further, I can test the
other interesting features for seeing their capabilities in predicting
the tumors.
<!----------------------------------------------------------------------------->

# Task 2: Tidy your data

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

- Each row is an **observation**
- Each column is a **variable**
- Each cell is a **value**

### 2.1 (2 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

\#My data is tidy. - Each Row is an Observation: each row is a sample
with values in different columns. - Each Column is a Variable:columns
includes the diagnosis, different tumor features and levels of perimeter
I created. - Each Cell is a Value: some of the cells contains M or B for
diagnosis, some of the cells contains the mean value for different
features

<!----------------------------------------------------------------------------->

### 2.2 (4 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

\#I untidy the data by using pivot_wider() to convert each sample to
columns. In this way, we can see different features first with all the
samples in each column.

``` r
my_data_wide <- pivot_wider(
  data = cancer_sample,
  id_cols = diagnosis,
  names_from = ID,
  values_from = radius_mean:fractal_dimension_mean
)
print(my_data_wide)
```

    ## # A tibble: 2 × 5,691
    ##   diagnosis radius_mean_842302 radius_mean_842517 radius_mean_84300903
    ##   <chr>                  <dbl>              <dbl>                <dbl>
    ## 1 M                       18.0               20.6                 19.7
    ## 2 B                       NA                 NA                   NA  
    ## # ℹ 5,687 more variables: radius_mean_84348301 <dbl>,
    ## #   radius_mean_84358402 <dbl>, radius_mean_843786 <dbl>,
    ## #   radius_mean_844359 <dbl>, radius_mean_84458202 <dbl>,
    ## #   radius_mean_844981 <dbl>, radius_mean_84501001 <dbl>,
    ## #   radius_mean_845636 <dbl>, radius_mean_84610002 <dbl>,
    ## #   radius_mean_846226 <dbl>, radius_mean_846381 <dbl>,
    ## #   radius_mean_84667401 <dbl>, radius_mean_84799002 <dbl>, …

<!----------------------------------------------------------------------------->

### 2.3 (4 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the remaining tasks:

<!-------------------------- Start your work below ---------------------------->

1.  *Can the values in each column reflect the tumor status?*
2.  *In spite of smoothness, what else can we select to predict tumors?*

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

1.  I hope to explore every interesting feature to see how they can
    predict the tumor status.
2.  The most important aim for a tumor research is to find the most
    significant biomarker/feature that can discriminate malignant tumor.
    So here I will further explore each of the features as the column,
    testing the values of each features in predicting tumors.
    <!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

(If it makes more sense, then you can make/pick two versions of your
data, one for each research question.)

<!--------------------------- Start your work below --------------------------->

# Task 3: Modelling

## 3.0 (no points)

Pick a research question from 1.2, and pick a variable of interest
(we’ll call it “Y”) that’s relevant to the research question. Indicate
these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: In spite of smoothness, what else can we select
to predict tumors?

**Variable of interest**: radius_mean

<!----------------------------------------------------------------------------->

## 3.1 (3 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

- **Note**: It’s OK if you don’t know how these models/tests work. Here
  are some examples of things you can do here, but the sky’s the limit.

  - You could fit a model that makes predictions on Y using another
    variable, by using the `lm()` function.
  - You could test whether the mean of Y equals 0 using `t.test()`, or
    maybe the mean across two groups are different using `t.test()`, or
    maybe the mean across multiple groups are different using `anova()`
    (you may have to pivot your data for the latter two).
  - You could use `lm()` to test for significance of regression
    coefficients.

<!-------------------------- Start your work below ---------------------------->

``` r
t_test_result <- t.test(cancer_sample$radius_mean[cancer_sample$diagnosis == 'M'],
                        cancer_sample$radius_mean[cancer_sample$diagnosis == 'B'])
print(t_test_result)
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  cancer_sample$radius_mean[cancer_sample$diagnosis == "M"] and cancer_sample$radius_mean[cancer_sample$diagnosis == "B"]
    ## t = 22.209, df = 289.71, p-value < 2.2e-16
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  4.845165 5.787448
    ## sample estimates:
    ## mean of x mean of y 
    ##  17.46283  12.14652

<!----------------------------------------------------------------------------->

## 3.2 (3 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

- Be sure to indicate in writing what you chose to produce.
- Your code should either output a tibble (in which case you should
  indicate the column that contains the thing you’re looking for), or
  the thing you’re looking for itself.
- Obtain your results using the `broom` package if possible. If your
  model is not compatible with the broom function you’re needing, then
  you can obtain your results by some other means, but first indicate
  which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

``` r
tidy_result <- tidy(t_test_result)
(p_value <- tidy_result$p.value)
```

    ## [1] 1.684459e-64

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 4.1 (3 points)

Take a summary table that you made from Task 1, and write it as a csv
file in your `output` folder. Use the `here::here()` function.

- **Robustness criteria**: You should be able to move your Mini Project
  repository / project folder to some other location on your computer,
  or move this very Rmd file to another location within your project
  repository / folder, and your code should still work.
- **Reproducibility criteria**: You should be able to delete the csv
  file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
output_file <- here("output", "summary_stats data.csv")
if (!dir.exists(here("output"))) {
  dir.create(here("output"))
}
write.csv(summary_stats, file = output_file, row.names = FALSE)
```

<!----------------------------------------------------------------------------->

## 4.2 (3 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

- The same robustness and reproducibility criteria as in 4.1 apply here.

<!-------------------------- Start your work below ---------------------------->

``` r
if (!dir.exists("output")) {
  dir.create("output")
}
rds_file <- file.path("output", "model.rds")
saveRDS(t_test_result, rds_file)
```

<!----------------------------------------------------------------------------->

# Overall Reproducibility/Cleanliness/Coherence Checklist

Here are the criteria we’re looking for.

## Coherence (0.5 points)

The document should read sensibly from top to bottom, with no major
continuity errors.

The README file should still satisfy the criteria from the last
milestone, i.e. it has been updated to match the changes to the
repository made in this milestone.

## File and folder structure (1 points)

You should have at least three folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (1 point)

All output is recent and relevant:

- All Rmd files have been `knit`ted to their output md files.
- All knitted md files are viewable without errors on Github. Examples
  of errors: Missing plots, “Sorry about that, but we can’t show files
  that are this big right now” messages, error messages from broken R
  code
- All of these output files are up-to-date – that is, they haven’t
  fallen behind after the source (Rmd) files have been updated.
- There should be no relic output files. For example, if you were
  knitting an Rmd to html, but then changed the output to be only a
  markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

## Tagged release (0.5 point)

You’ve tagged a release for Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
