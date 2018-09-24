---
  title: "Latin Squares, Graeco-Latin Squares, & Factorial experiments"
  description: "Evaluate the NYC SAT scores data and deal with its missing values, then evaluate Latin Square, Graeco-Latin Square, and Factorial experiments."
  attachments: 
    slides_link: "https://s3.amazonaws.com/assets.datacamp.com/production/course_6572/slides/chapter4.pdf"
---

## Latin Squares

```yaml
type: VideoExercise 
lang: r
xp: 50 
skills: 1
key: c49fe48335   
```

`@projector_key`
2792266da48acb28a0dea1a572d227fc
---

## NYC SAT Scores EDA

```yaml
type: BulletExercise 
lang: r
xp: 100 
key: b0c8dfe135   
```


Math is a subject the U.S. is consistently behind the rest of the world on, so our experiments will focus on Math score. While the original dataset is an open dataset downloaded from [Kaggle](https://www.kaggle.com/nycopendata/high-schools/data), throughout this chapter I will add a few variables that will allow you to pretend you are an education researcher conducting experiments ideally aimed at raising students' scores, hopefully increasing the likelihood they will be admitted to colleges.

Before diving into analyzing the experiments, we should do some EDA to make sure we fully understand the `nyc_scores` data. In this lesson, we'll do experiments where we block by `Borough` and `Teacher_Education_Level`, so let's examine math scores by those variables. The `nyc_scores` dataset has been loaded for you.


`@pre_exercise_code`

```{r}
library(dplyr)
nyc_scores <- read.csv("https://assets.datacamp.com/production/repositories/1793/datasets/6eee2fcc47c8c8dbb2e9d4670cf2eabeda52b705/nyc_scores.csv")
nyc_scores$Teacher_Education_Level <- sample(c("College Student", "BA", "Grad Student", "MA", "PhD"),
                                                size = nrow(nyc_scores),
                                                replace = TRUE,
                                                prob = c(0.2, 0.2, 0.2, 0.3, 0.1))
```

`@sample_code`

```{r}
# Mean, var, and median of Math score
nyc_scores %>%
    ___() %>% 
    ___(mean = ___(___, na.rm = TRUE),
              var = ___(___, na.rm = TRUE),
              median = ___(___, na.rm = TRUE))
```

***



```yaml
type: NormalExercise 
xp: 30 
key: c495c18db1   
```





`@instructions`
Find the mean, variance, and median of `Average_Score_SAT_Math` by `Borough` using `dplyr` methods for EDA as we have used them throughout the course.

`@hint`
You should use `group_by()` and `summarise()`, in that order.

`@solution`

```{r}
# Mean, var, and median of Math score by Borough
nyc_scores %>%
    group_by(Borough) %>% 
    summarise(mean = mean(Average_Score_SAT_Math, na.rm = TRUE),
              var = var(Average_Score_SAT_Math, na.rm = TRUE),
              median = median(Average_Score_SAT_Math, na.rm = TRUE))
```

`@sct`

```{r}
#Check for output in the console
ex() %>%
    check_output_expr(expr = "nyc_scores %>% group_by(Borough) %>% 
    summarise(mean = mean(Average_Score_SAT_Math, na.rm = TRUE), var = var(Average_Score_SAT_Math, na.rm = TRUE), median = median(Average_Score_SAT_Math, na.rm = TRUE))",
                      missing_msg = "Be sure to use `group_by()` and `summarise()` to find the mean, variance, and median Math score by Borough.")
```


***



```yaml
type: NormalExercise 
xp: 30 
key: 58fa71b1cd   
```





`@instructions`
Find the mean, variance, and median of `Average_Score_SAT_Math` using `dplyr` EDA methods by `Teacher_Education_Level`.

`@hint`
Be sure not to delete the argument `na.rm = TRUE` throughout, or the output in the console will be full of `NA`s.

`@solution`

```{r}
# Mean, var, and median of Math score by Teacher Education Level
nyc_scores %>%
    group_by(Teacher_Education_Level) %>% 
    summarise(mean = mean(Average_Score_SAT_Math, na.rm = TRUE),
              var = var(Average_Score_SAT_Math, na.rm = TRUE),
              median = median(Average_Score_SAT_Math, na.rm = TRUE))
```

`@sct`

```{r}
#Check for output in the console
ex() %>%
    check_output_expr(expr = "nyc_scores %>% group_by(Teacher_Education_Level) %>% 
    summarise(mean = mean(Average_Score_SAT_Math, na.rm = TRUE), var = var(Average_Score_SAT_Math, na.rm = TRUE), median = median(Average_Score_SAT_Math, na.rm = TRUE))",
                      missing_msg = "Be sure to use `group_by()` and `summarise()` to find the mean, variance, and median Math score by Teacher_Education_Level.")
```


***



```yaml
type: NormalExercise 
xp: 40 
key: 9a8df09bae   
```





`@instructions`
Find the mean, variance, and median of `Average_Score_SAT_Math` using `dplyr` EDA methods by both `Borough` and `Teacher_Education_Level`.

`@hint`
You can group by two variables by entering them into `group_by()` in the desired order.

`@solution`

```{r}
# Mean, var, and median of Math score by both
nyc_scores %>%
    group_by(Borough, Teacher_Education_Level) %>% 
    summarise(mean = mean(Average_Score_SAT_Math, na.rm = TRUE),
              var = var(Average_Score_SAT_Math, na.rm = TRUE),
              median = median(Average_Score_SAT_Math, na.rm = TRUE))
```

`@sct`

```{r}
ex() %>%
    check_output_expr(expr = "nyc_scores %>% group_by(Borough, Teacher_Education_Level) %>% 
    summarise(mean = mean(Average_Score_SAT_Math, na.rm = TRUE), var = var(Average_Score_SAT_Math, na.rm = TRUE), median = median(Average_Score_SAT_Math, na.rm = TRUE))",
                      missing_msg = "Be sure to use `group_by()` and `summarise()` to find the mean, variance, and median Math score by Borough AND Teacher_Education_Level.")

success_msg("Great job! Now that we've examined the data, we can move on to cleaning it, the next important step before analysis.")
```


---

## Dealing with Missing Test Scores

```yaml
type: TabExercise 
lang: r
xp: 100 
skills: 1
key: 37fd8186e2   
```


If we want to use SAT scores as our outcome, we need to examine their missingness. First, look at the pattern of missingness using `md.pattern()` from the `mice` package. There are 60 scores missing in each of the scores. There are many R packages which help with more advanced forms of imputation, such as `MICE`, `Amelia`, `mi`, and more. We will use the `simputation` and`impute_median()` as we did previously. 
  
Create a new dataset, `nyc_scores_2` by imputing Math score by Borough. A note: `impute_median()` returns the imputed variable as type "impute". You'll have to convert the varaible to the type you want it to ultimately be in a separate step.

`mice`, `simputation`, and `dplyr` have been loaded for you.


`@pre_exercise_code`

```{r}
#load for students
library(mice)
library(simputation)
library(dplyr)

#read in data
nyc_scores <- read.csv("https://assets.datacamp.com/production/repositories/1793/datasets/6eee2fcc47c8c8dbb2e9d4670cf2eabeda52b705/nyc_scores.csv")
```

`@sample_code`

```{r}

```


***



```yaml
type: NormalExercise 
xp: 25 
key: 37fd8186e21   
```





`@instructions`
- Examine the missingness of `nyc_scores` with `md.pattern()`.

`@hint`
- The only input the `md_pattern()` should be the dataset name.

`@sample_code`

```{r}
# Examine missingness with md.pattern()
___
```

`@solution`

```{r}
# Examine missingness with md.pattern()
md.pattern(nyc_scores)
```

`@sct`

```{r}
#Check for output in the console
ex() %>%
    check_output_expr(expr = "md.pattern(nyc_scores)",
                      missing_msg = "Be sure to use `md.pattern()` on `nyc_scores` to examine missingness of the variables in the dataset.")
```


***



```yaml
type: NormalExercise 
xp: 25 
key: 37fd8186e22   
```





`@instructions`
Create `nyc_scores_2` by imputing the Math score by Borough (we're only using Math in our experiments.)

`@hint`
The formula input to `impute.median()` should resemble: `variable_to_impute ~ variable_to_impute_by`.

`@sample_code`

```{r}
# Examine missingness with md.pattern()
md.pattern(nyc_scores)

# Impute the Math score by Borough
___
```

`@solution`

```{r}
# Examine missingness with md.pattern()
md.pattern(nyc_scores)

# Impute the Math score by Borough
nyc_scores_2 <- impute_median(nyc_scores, Average_Score_SAT_Math ~ Borough)
```

`@sct`

```{r}
#Check for output in the console
ex() %>%
    check_output_expr(expr = "md.pattern(nyc_scores)",
                      missing_msg = "Be sure to use `md.pattern()` on `nyc_scores` to examine missingness of the variables in the dataset.")

#Checks nyc_scores_2 object, then (if object is wrong) checks function(s) that create object
check_correct({
    ex() %>% 
        #check for the nyc_scores_2 object
        check_object(name = "nyc_scores_2",
                     undefined_msg = "Did you define `nyc_scores_2`?") %>%
        #checks that the nyc_scores_2 object is correct
        check_equal(incorrect_msg = "The contents of `nyc_scores_2` appear to be incorrect.",
                    append = FALSE)
},{
    ex() %>% {
        #checks that impute_median() was called
        check_function(state = ., 
                       name = "impute_median",
                       not_called_msg = "Did you call `impute_median()`?") %>% {
            #checks that the dat argument was included in the impute_median() call
            check_arg(state = ., 
                      arg = "dat",
                      arg_not_specified_msg = "Did you remember to specify the `dat` argument in this call to `impute_median()`?") %>%
            #checks that the dat argument in impute_median() is correct
            check_equal(incorrect_msg = "It looks like the `dat` argument in this call of `impute_median()` is incorrect.",
                        append = FALSE)
            #checks that the formula argument was included in the impute_median() call
            check_arg(state = ., 
                      arg = "formula",
                      arg_not_specified_msg = "Did you remember to specify the `formula` argument in this call to `impute_median()`?") %>%
            #checks that the formula argument in impute_median() is correct
            check_equal(incorrect_msg = "It looks like the `formula` argument in this call of `impute_median()` is incorrect.",
                        append = FALSE)
            }
    }
})
```


***



```yaml
type: NormalExercise 
xp: 25 
key: 37fd8186e23   
```





`@instructions`
Convert `nyc_scores_2$Average_Score_SAT_Math` to numeric.

`@hint`
To convert a variable to numeric, use `as.numeric()`.h

`@sample_code`

```{r}
# Examine missingness with md.pattern()
md.pattern(nyc_scores)

# Impute the Math score by Borough
nyc_scores_2 <- impute_median(nyc_scores, Average_Score_SAT_Math ~ Borough)

# Convert Math score to numeric
___
```

`@solution`

```{r}
# Examine missingness with md.pattern()
md.pattern(nyc_scores)

# Impute the Math score by Borough
nyc_scores_2 <- impute_median(nyc_scores, Average_Score_SAT_Math ~ Borough)

# Convert Math score to numeric
nyc_scores_2$Average_Score_SAT_Math <- as.numeric(nyc_scores_2$Average_Score_SAT_Math)
```

`@sct`

```{r}
#Check for output in the console
ex() %>%
    check_output_expr(expr = "md.pattern(nyc_scores)",
                      missing_msg = "Be sure to use `md.pattern()` on `nyc_scores` to examine missingness of the variables in the dataset.")

#Checks nyc_scores_2 object, then (if object is wrong) checks function(s) that create object
check_correct({
    ex() %>% 
        #check for the nyc_scores_2 object
        check_object(name = "nyc_scores_2",
                     undefined_msg = "Did you define `nyc_scores_2`?") %>%
        #checks that the nyc_scores_2 object is correct
        check_equal(incorrect_msg = "The contents of `nyc_scores_2` appear to be incorrect.",
                    append = FALSE)
},{
    ex() %>% {
        #checks that impute_median() was called
        check_function(state = ., 
                       name = "impute_median",
                       not_called_msg = "Did you call `impute_median()`?") %>% {
            #checks that the dat argument was included in the impute_median() call
            check_arg(state = ., 
                      arg = "dat",
                      arg_not_specified_msg = "Did you remember to specify the `dat` argument in this call to `impute_median()`?") %>%
            #checks that the dat argument in impute_median() is correct
            check_equal(incorrect_msg = "It looks like the `dat` argument in this call of `impute_median()` is incorrect.",
                        append = FALSE)
            #checks that the formula argument was included in the impute_median() call
            check_arg(state = ., 
                      arg = "formula",
                      arg_not_specified_msg = "Did you remember to specify the `formula` argument in this call to `impute_median()`?") %>%
            #checks that the formula argument in impute_median() is correct
            check_equal(incorrect_msg = "It looks like the `formula` argument in this call of `impute_median()` is incorrect.",
                        append = FALSE)
            }
    }
})

#check converting math score to numeric
ex() %>%
    check_object("nyc_scores_2") %>%
    check_column(col = "Average_Score_SAT_Math",
                col_missing_msg = NULL) %>%
    check_equal(incorrect_msg = "The contents of `nyc_scores_2$Average_Score_SAT_Math` appear to be incorrect.",
                append = FALSE)
```


***



```yaml
type: NormalExercise 
xp: 25 
key: 37fd8186e24   
```





`@instructions`
Use `dplyr` to examine the median and mean of math score before and after imputation.

`@hint`
You should use `group_by()` then `summarise()` from `dplyr`, in that order.

`@sample_code`

```{r}
# Examine missingness with md.pattern()
md.pattern(nyc_scores)

# Impute the Math score by Borough
nyc_scores_2 <- impute_median(nyc_scores, Average_Score_SAT_Math ~ Borough)

# Convert Math score to numeric
nyc_scores_2$Average_Score_SAT_Math <- as.numeric(nyc_scores_2$Average_Score_SAT_Math)

# Examine scores by Borough in both datasets, before and after imputation
nyc_scores %>% group_by(___) %>% summarise(median = median(___, na.rm = TRUE), mean = mean(___, na.rm = TRUE))
___
```

`@solution`

```{r}
# Examine missingness with md.pattern()
md.pattern(nyc_scores)

# Impute the Math score by Borough
nyc_scores_2 <- impute_median(nyc_scores, Average_Score_SAT_Math ~ Borough)

# Convert Math score to numeric
nyc_scores_2$Average_Score_SAT_Math <- as.numeric(nyc_scores_2$Average_Score_SAT_Math)

# Examine scores by Borough in both datasets, before and after imputation
nyc_scores %>% group_by(Borough) %>% summarise(median = median(Average_Score_SAT_Math, na.rm = TRUE), mean = mean(Average_Score_SAT_Math, na.rm = TRUE))
nyc_scores_2 %>% group_by(Borough) %>% summarise(median = median(Average_Score_SAT_Math), mean = mean(Average_Score_SAT_Math))
```

`@sct`

```{r}
#Check for output in the console
ex() %>%
    check_output_expr(expr = "md.pattern(nyc_scores)",
                      missing_msg = "Be sure to use `md.pattern()` on `nyc_scores` to examine missingness of the variables in the dataset.")

#Checks nyc_scores_2 object, then (if object is wrong) checks function(s) that create object
check_correct({
    ex() %>% 
        #check for the nyc_scores_2 object
        check_object(name = "nyc_scores_2",
                     undefined_msg = "Did you define `nyc_scores_2`?") %>%
        #checks that the nyc_scores_2 object is correct
        check_equal(incorrect_msg = "The contents of `nyc_scores_2` appear to be incorrect.",
                    append = FALSE)
},{
    ex() %>% {
        #checks that impute_median() was called
        check_function(state = ., 
                       name = "impute_median",
                       not_called_msg = "Did you call `impute_median()`?") %>% {
            #checks that the dat argument was included in the impute_median() call
            check_arg(state = ., 
                      arg = "dat",
                      arg_not_specified_msg = "Did you remember to specify the `dat` argument in this call to `impute_median()`?") %>%
            #checks that the dat argument in impute_median() is correct
            check_equal(incorrect_msg = "It looks like the `dat` argument in this call of `impute_median()` is incorrect.",
                        append = FALSE)
            #checks that the formula argument was included in the impute_median() call
            check_arg(state = ., 
                      arg = "formula",
                      arg_not_specified_msg = "Did you remember to specify the `formula` argument in this call to `impute_median()`?") %>%
            #checks that the formula argument in impute_median() is correct
            check_equal(incorrect_msg = "It looks like the `formula` argument in this call of `impute_median()` is incorrect.",
                        append = FALSE)
            }
    }
})

#check converting math score to numeric
ex() %>%
    check_object("nyc_scores_2") %>%
    check_column(col = "Average_Score_SAT_Math",
                col_missing_msg = NULL) %>%
    check_equal(incorrect_msg = "The contents of `nyc_scores_2$Average_Score_SAT_Math` appear to be incorrect.",
                append = FALSE)

#Check for output in the console
ex() %>%
    check_output_expr(expr = "nyc_scores %>% group_by(Borough) %>% summarise(median = median(Average_Score_SAT_Math, na.rm = TRUE), mean = mean(Average_Score_SAT_Math, na.rm = TRUE))",
                      missing_msg = "Be sure to use `group_by()` and `summarise()` to find the median and mean Math score by Borough before imputation (on `nyc_scores`!)")

#Check for output in the console
ex() %>%
    check_output_expr(expr = "nyc_scores_2 %>% group_by(Borough) %>% summarise(median = median(Average_Score_SAT_Math), mean = mean(Average_Score_SAT_Math))",
                      missing_msg = "Be sure to use `group_by()` and `summarise()` to find the median and mean Math score by Borough after imputation (on `nyc_scores_2`!)")

success_msg("Nice job! Did the median scores change before and after imputation? (Hint: they shouldn't have changed by much, but rounding may have offset them by an integer or two.)")
```


---

## Drawing Latin Squares with agricolae

```yaml
type: NormalExercise 
lang: r
xp: 100 
skills: 1
key: ee659294e3   
```


We return, once again, to the `agricolae` package to examine what a Latin Square design can look like.

Look at the help page for `design.lsd()` by typing `?design.lsd` in the console before designing your Latin Square experiment.


`@instructions`
* Load the `agricolae` package.
* Create a LS design, `my_design_lsd`, using treatments A, B, C, D, & E, a `serie` of 0, and a `seed` of 42.

`@hint`
There are no other inputs besides those listed in the instructions required for `design.lsd()`.

`@sample_code`

```{r}
# Load agricolae
___

# Design a LS with 5 treatments A:E then look at the sketch
___
___
```

`@solution`

```{r}
# Load agricolae
library(agricolae)

# Design a LS with 5 treatments A:E then look at the sketch
my_design_lsd <- design.lsd(trt = LETTERS[1:5], serie = 0, seed = 42)
my_design_lsd$sketch
```

`@sct`

```{r}
#Test for library call
test_library_function(package = "agricolae", 
                      not_called_msg = "Remember to load the `agricolae` package.", 
                      incorrect_msg = "Make sure you've spelled the package name correctly in the `library()` call.")

#checks my_design_lsd object, then (if object is wrong) checks function(s) that create object
ex() %>% check_correct(
    {
        check_object(., "my_design_lsd") %>% check_equal()
    },
    {
        check_function(., "design.lsd") %>% {
            check_arg(., arg = "trt") %>% check_equal()
            check_arg(., arg = "seed") %>% check_equal()
            check_arg(., arg = "serie") %>% check_equal()
        }
    }
)

#Check for output in the console
ex() %>%
    check_output_expr(expr = "my_design_lsd$sketch",
                      missing_msg = "Be sure to print `my_design_lsd$sketch` to the console.")

success_msg("Perhaps you're thinking to yourself 'This looks a lot like a RCBD'...bingo! It does, but as we know from the video, there are now two blocking factors in a LS design.")
```

---

## Latin Square with NYC SAT Scores

```yaml
type: TabExercise 
lang: r
xp: 100 
key: eb602251af   
```


To execute a Latin Square design on this data, suppose we want to know the effect of of our tutoring program, which includes one-on-one tutoring, two small groups, and an in and after school SAT prep class. A new dataset `nyc_scores_ls` is available that represents this experiment. Feel free to explore the dataset in the console.

We'll block by `Borough` and `Teacher_Education_Level` to reduce their known variance on the score outcome. `Borough` is a good blocking factor because schools in America are funded partly based on taxes paid in each city, so it will likely make a difference on quality of education.

_________

**At the 0.05 level, do we have evidence to believe the tutoring program has an effect on math SAT scores, when blocked by `Borough` and `Teacher_Education_Level`?**


`@pre_exercise_code`

```{r}
#libraries for me
library(dplyr)
library(simputation)

nyc_scores <- read.csv("https://assets.datacamp.com/production/repositories/1793/datasets/6eee2fcc47c8c8dbb2e9d4670cf2eabeda52b705/nyc_scores.csv")

#impute the Math scores by Borough
nyc_scores_2 <- impute_median(nyc_scores, Average_Score_SAT_Math ~ Borough)
#sum(is.na(nyc_scores_2$Average_Score_SAT_Math))

#convert Math score to numeric
nyc_scores_2$Average_Score_SAT_Math <- as.numeric(nyc_scores_2$Average_Score_SAT_Math)

nyc_scores <- nyc_scores_2

#latin-square-ize the dataset
nyc_scores_ls <- nyc_scores %>% group_by(Borough) %>%
  sample_n(5)

nyc_scores_ls$Teacher_Education_Level <- rep(c("College Student", "BA", "Grad Student", "MA", "PhD"), times = 5)
nyc_scores_ls$Tutoring_Program <- c(c("One-on-One", "Small Groups (2-3)", "Small Groups (4-6)", "SAT Prep Class (school hours)", "SAT Prep Class (after school)"),
                                        c("Small Groups (2-3)", "Small Groups (4-6)", "SAT Prep Class (school hours)", "SAT Prep Class (after school)", "One-on-One"),
                                        c("Small Groups (4-6)", "SAT Prep Class (school hours)", "SAT Prep Class (after school)","One-on-One", "Small Groups (2-3)"),
                                        c("SAT Prep Class (school hours)", "SAT Prep Class (after school)","One-on-One", "Small Groups (2-3)", "Small Groups (4-6)"),
                                        c("SAT Prep Class (after school)","One-on-One", "Small Groups (2-3)", "Small Groups (4-6)", "SAT Prep Class (school hours)"))


#libraries for students:
library(broom)
```

`@sample_code`

```{r}

```


***



```yaml
type: NormalExercise 
xp: 50 
key: 4835457549   
```





`@instructions`
* Use `lm()` to test the changes in `Average_Score_SAT_Math` using `nyc_scores_ls`.
* Tidy `nyc_scores_ls_lm` with the appropriate `broom` function.
* Examine `nyc_scores_ls_lm` with `anova()`.

`@hint`
* The formula for `lm()` should look something like: `outcome ~ treatment + block1 + block2`.

`@sample_code`

```{r}
# Build nyc_scores_ls_lm
nyc_scores_ls_lm <- ___(___ ~ Tutoring_Program + Borough + Teacher_Education_Level,
                        data=___ )

# Tidy the results with broom
___

# Examine the results with anova
___
```

`@solution`

```{r}
# Build nyc_scores_ls_lm
nyc_scores_ls_lm <- lm(Average_Score_SAT_Math ~ Tutoring_Program + Borough + Teacher_Education_Level,
                       data=nyc_scores_ls)

# Tidy the results with broom
tidy(nyc_scores_ls_lm)

# Examine the results with anova
anova(nyc_scores_ls_lm)
```

`@sct`

```{r}
#Checks nyc_scores_ls_lm object, then (if object is wrong) checks function(s) that create object
ex() %>% check_correct(
    {
        check_object(., name = "nyc_scores_ls_lm") %>% check_equal()
    },
    {
        check_function(., "lm") %>% {
            check_arg(., "formula") %>% check_equal()
            check_arg(., "data") %>% check_equal()
        }
    }
)

#Check for output in the console
ex() %>%
    check_output_expr(expr = "tidy(nyc_scores_ls_lm)",
                      missing_msg = "Be sure to use `tidy()` on `nyc_scores_ls_lm` to view the tidied results.")

#Check for output in the console
ex() %>%
    check_output_expr(expr = "anova(nyc_scores_ls_lm)",
                      missing_msg = "Be sure to use `anova()` on `nyc_scores_ls_lm` to view the results.")
```


***



```yaml
type: MultipleChoiceExercise 
xp: 50 
key: fdabd3ddbe   
```





`@instructions`
* Nope! Given the p-value, we have no reason to reject the null hypothesis.
* Yes! Given the p-value, we have reason to believe that the tutoring program had an effect on the Math score.

`@hint`
* Be sure you're looking at the p-value for `Tutoring_Program`.

`@sample_code`

```{r}
# Build nyc_scores_ls_lm
nyc_scores_ls_lm <- lm(Average_Score_SAT_Math ~ Tutoring_Program + Borough + Teacher_Education_Level, data=nyc_scores_ls)

# Tidy the results with broom
tidy(nyc_scores_ls_lm)

# Examine the results with anova
anova(nyc_scores_ls_lm)
```

`@sct`

```{r}
#feedback/success messages for each possible answer; provided by instructor
msg1 <- "Great! Because the p-value is greater than 0.05, statisticians usually say we fail to reject the null hypothesis, which was that the `Tutoring_Program` has no effect on Math scores."
msg2 <- "Not quite! Be sure you're looking at the p-value for `Tutoring_Program`."

#tests that correct answer was chosen; returns feedback message for selected answer
test_mc(correct = 1, feedback_msgs = c(msg1, msg2))

success_msg("Excellent! It seems that when we block for `Borough` of the school and `Teacher_Education_Level`, our `Tutoring_Program` isn't having a statistically significant effect on the Math SAT score.")
```


---

## Graeco-Latin Squares

```yaml
type: VideoExercise 
lang: r
xp: 50 
skills: 1
key: 92797f11d9   
```

`@projector_key`
eea381c3b1efa8ceba34c68871ad3833
---

## NYC SAT Scores Data Viz

```yaml
type: NormalExercise 
lang: r
xp: 100 
skills: 1
key: dcab1a283e   
```


In the last lesson, when discussing Latin Squares, we did mostly numerical EDA in the form of looking at means, variances, and medians of the math scores. As we've covered, another crucial part of the EDA is data visualization, as it often helps in spotting outliers plus gives you a visual representation of the distribution of your variables. 

`ggplot2` has been loaded for you and the `nyc_scores` dataset is available. Create and examine the requested boxplot. How do the medians differ by Borough? How many outliers are present, and where are they mostly present?


`@instructions`
* Create a boxplot of Math SAT scores by `Borough`.
* Include a title: `"Average SAT Math Scores by Borough, NYC"`. Use the title-specific `ggplot2` function to add it.
* Change the x- and y-axis labels to read `"Borough (NYC)"` and `"Average SAT Math Scores (2014-15)"`, respectively, using the label-specific `ggplot2` functions.

`@hint`
* You can add a title to a plot by passing the title as a character string to `ggtitle()`. You add this on as an argument as you do the type of plot.
* x- and y-axis labels can be created with `xlab()` and `ylab()`, respectively.
* Don't use `labs()` to create any of the titles or labels!

`@pre_exercise_code`

```{r}
#library for pre_ex_code
library(simputation)

nyc_scores <- read.csv("https://assets.datacamp.com/production/repositories/1793/datasets/6eee2fcc47c8c8dbb2e9d4670cf2eabeda52b705/nyc_scores.csv")

#impute the Math scores by Borough
nyc_scores_2 <- simputation::impute_median(nyc_scores, Average_Score_SAT_Math ~ Borough)
#sum(is.na(nyc_scores_2$Average_Score_SAT_Math))

#convert Math score to numeric
nyc_scores_2$Average_Score_SAT_Math <- as.numeric(nyc_scores_2$Average_Score_SAT_Math)

nyc_scores <- nyc_scores_2

#variables needed
nyc_scores$Teacher_Education_Level <- sample(c("College Student", "BA", "Grad Student", "MA", "PhD"), size = nrow(nyc_scores), replace = TRUE, prob = c(0.2, 0.2,0.2, 0.3, 0.1))

nyc_scores$Tutoring_Program <- sample(c("One-on-One", "Small Groups (2-3)", "Small Groups (4-6)", "SAT Prep Class (school hours)", "SAT Prep Class (after school)"), size = nrow(nyc_scores), replace = TRUE, prob = c(0.2, 0.2, 0.2, 0.2, 0.2))

#load for students
library(ggplot2)
```

`@sample_code`

```{r}
# Create a boxplot of Math scores by Borough, with a title and x/y axis labels
___
```

`@solution`

```{r}
# Create a boxplot of Math scores by Borough, with a title and x/y axis labels
ggplot(nyc_scores, aes(Borough, Average_Score_SAT_Math)) +
  geom_boxplot() + 
  ggtitle("Average SAT Math Scores by Borough, NYC") +
  xlab("Borough (NYC)") +
  ylab("Average SAT Math Score (2014-15)")
```

`@sct`

```{r}
#checks ggplot() logic
ex() %>% {
  #checks that ggplot() was called
  check_function(., "ggplot") %>% {
    check_arg(., arg = "data") %>% check_equal()
    check_arg(., arg = "mapping") %>% check_equal()
  }
  check_function(., "aes") %>% {
    check_arg(., arg = "x") %>% check_equal(eval = FALSE)
    check_arg(., arg = "y") %>% check_equal(eval = FALSE)
  }
  check_function(., "geom_boxplot")
}

success_msg("Beautiful! It's interesting to see the different distribution of scores by Borough and to see that Manhattan and Brooklyn have quite a few outliers.")
```

---

## Drawing Graeco-Latin Squares with agricolae

```yaml
type: NormalExercise 
lang: r
xp: 100 
skills: 1
key: c5c7b99395   
```


As we've seen, `agricolae` provides us the ability to draw all of the experimental designs we've used so far, and they can also draw Graeco-Latin squares. One difference in the input to `design.graeco()` that we haven't seen before is that we'll need to input 2 vectors, `trt1` and `trt2`, which must be of equal length. You can think of `trt1` as your actual treatment and `trt2` as one of your blocking factors.


`@instructions`
* `agricolae` has been loaded for you.
* Create `trt1` with `LETTERS` A through E and `trt2` with numbers 1 through 5.
* Make `my_graeco_design` with `design.graeco()`, using `serie = 0` and `seed = 42`.
* Examine the `parameters` and `sketch` of `my_design_graeco`.

`@hint`
* Remember that you can index `LETTERS` to create a vector of capital letters A-Z. 
* You don't have to use `rep()` or `seq()` to get a sequence of numbers in R, you can use, for e.g.: `x <- 1:10` and it creates a vector of the numbers 1 through 10.
* You can access the `parameters` and `sketch` using, for e.g., `my_design$sketch`.

`@pre_exercise_code`

```{r}
library(agricolae)
```

`@sample_code`

```{r}
# Create trt1 and trt2
___
___

# Create my_graeco_design
___

# Examine the parameters and sketch
___
___
```

`@solution`

```{r}
# Create trt1 and trt2
trt1 <- LETTERS[1:5]
trt2 <- 1:5

# Create my_graeco_design
my_graeco_design <- design.graeco(trt1, trt2, serie = 0, seed = 42)

# Examine the parameters and sketch
my_graeco_design$parameters
my_graeco_design$sketch
```

`@sct`

```{r}
#check trt1
ex() %>% check_object(name = "trt1") %>% check_equal()

#check trt2
ex() %>% check_object("trt2") %>% check_equal()

#checks my_graeco_design object, then (if object is wrong) checks function(s) that create object
ex() %>% check_correct(
    {
        check_object(., "my_graeco_design") %>% check_equal()
    },{
        check_function(., "design.graeco") %>% {
            check_arg(., "trt1") %>% check_equal()
            check_arg(., "trt2") %>% check_equal()
            check_arg(., "seed") %>% check_equal()
            check_arg(., "serie") %>% check_equal()
        }
    }
)

#Check for output in the console
ex() %>%
    check_output_expr(expr = "my_graeco_design$parameters",
                      missing_msg = "Be sure to print `my_graeco_design$parameters` to the console.")

#Check for output in the console
ex() %>%
    check_output_expr(expr = "my_graeco_design$sketch",
                      missing_msg = "Be sure to print `my_graeco_design$sketch` to the console.")

success_msg("Superb! You can see that this time the sketch object includes your treatment (the capital letter) and a blocking factor (the number.)")
```

---

## Graeco-Latin Square with NYC SAT Scores

```yaml
type: TabExercise 
lang: r
xp: 100 
skills: 1
key: 453fa151eb   
```


Recall that our Latin Square exercise in this chapter tested the effect of our tutoring program, blocked by `Borough` and `Teacher_Education_Level`. 

For our Graeco-Latin Square, say we also want to block out the known effect of `Homework_Type`, which indicates what kind of homework the student was given: individual only, small or large group homework, or some combination. We can add this as another blocking factor to create a Graeco-Latin Square experiment.

_________

**At the 0.05 level, do we have evidence to believe the tutoring program has an effect on math SAT scores, when blocked by `Borough`, `Teacher_Education_Level`, and `Homework_Type`?**


`@pre_exercise_code`

```{r}
#libraries for me
library(dplyr)
library(simputation)

nyc_scores <- read.csv("https://assets.datacamp.com/production/repositories/1793/datasets/6eee2fcc47c8c8dbb2e9d4670cf2eabeda52b705/nyc_scores.csv")

#impute the Math scores by Borough
nyc_scores_2 <- impute_median(nyc_scores, Average_Score_SAT_Math ~ Borough)
#sum(is.na(nyc_scores_2$Average_Score_SAT_Math))

#convert Math score to numeric
nyc_scores_2$Average_Score_SAT_Math <- as.numeric(nyc_scores_2$Average_Score_SAT_Math)

nyc_scores <- nyc_scores_2

#gls-ize the data:
nyc_scores_gls <- nyc_scores %>% 
  group_by(Borough) %>%
  sample_n(5)
  
nyc_scores_gls$Teacher_Education_Level <- rep(c("College Student", "BA", "Grad Student", "MA", "PhD"), times = 5)

A <- "One-on-One"
B <- "Small Groups (2-3)"
C <- "Small Groups (4-6)"
D <- "SAT Prep Class (school hours)"
E <- "SAT Prep Class (after school)"
nyc_scores_gls$Tutoring_Program <- c(c(D, E, A, C, B),
                                    c(E, A, C, B, D),
                                    c(A, C, B, D, E),
                                    c(C, B, D, E, A),
                                    c(B, D, E, A, C)
)

a <- "Individual"
b <- "Small Group"
c <- "Large Group"
d <- "Mix of Small Group/Individual"
e <- "Mix of Large Group/Individual"
nyc_scores_gls$Homework_Type <-  c(c(b, c, a, e, d),
                                   c(a, e, d, b, c),
                                   c(d, b, c, a, e),
                                   c(c, a, e, d, b),
                                   c(e, d, b, c, a))


#loaded for students:
library(broom)
```

***



```yaml
type: NormalExercise 
xp: 50 
key: ca8048832b   
```





`@instructions`
* Use `lm()` to test the changes in `Average_Score_SAT_Math` using the `nyc_scores_gls` data.
* Tidy `nyc_scores_gls_lm` with the appropriate `broom` function.
* Examine `nyc_scores_gls_lm` with `anova()`.

`@hint`
* The formula for `lm()` should look something like: `outcome ~ treatment + block1 + block2 + block3`.

`@sample_code`

```{r}
# Build nyc_scores_gls_lm
nyc_scores_gls_lm <- ___(___ ~ Tutoring_Program + Borough + Teacher_Education_Level + Homework_Type,
                        data=___ )

# Tidy the results with broom
___

# Examine the results with anova
___
```

`@solution`

```{r}
# Build nyc_scores_gls_lm
nyc_scores_gls_lm <- lm(Average_Score_SAT_Math ~ Tutoring_Program + Borough + Teacher_Education_Level + Homework_Type,
                        data=nyc_scores_gls)

# Tidy the results with broom
tidy(nyc_scores_gls_lm)

# Examine the results with anova
anova(nyc_scores_gls_lm)
```

`@sct`

```{r}
#Checks nyc_scores_gls_lm object, then (if object is wrong) checks function(s) that create object
check_correct({
    ex() %>% 
        #check for the nyc_scores_gls_lm object
        check_object(name = "nyc_scores_gls_lm",
                     undefined_msg = "Did you define `nyc_scores_gls_lm`?") %>%
        #checks that the nyc_scores_gls_lm object is correct
        check_equal(incorrect_msg = "The contents of `nyc_scores_gls_lm` appear to be incorrect.",
                    append = FALSE)
},{
    ex() %>% {
        #checks that lm() was called
        check_function(state = ., 
                       name = "lm",
                       not_called_msg = "Did you call `lm()`?") %>% {
            #checks that the formula argument was included in the lm() call
            check_arg(state = ., 
                      arg = "formula",
                      arg_not_specified_msg = "Did you remember to specify the `formula` argument in this call to `lm()`?") %>%
            #checks that the formula argument in lm() is correct
            check_equal(incorrect_msg = "It looks like the `formula` argument in this call of `lm()` is incorrect.",
                        append = FALSE)
            #checks that the data argument was included in the lm() call
            check_arg(state = ., 
                      arg = "data",
                      arg_not_specified_msg = "Did you remember to specify the `data` argument in this call to `lm()`?") %>%
            #checks that the data argument in lm() is correct
            check_equal(incorrect_msg = "It looks like the `data` argument in this call of `lm()` is incorrect.",
                        append = FALSE)
            }
    }
})

#Check for output in the console
ex() %>%
    check_output_expr(expr = "tidy(nyc_scores_gls_lm)",
                      missing_msg = "Be sure to use `tidy()` on `nyc_scores_gls_lm` to view the tidied results.")

#Check for output in the console
ex() %>%
    check_output_expr(expr = "anova(nyc_scores_gls_lm)",
                      missing_msg = "Be sure to use `anova()` on `nyc_scores_gls_lm` to view the results.")
```


***



```yaml
type: MultipleChoiceExercise 
xp: 50 
key: 65f37599eb   
```





`@instructions`
* Nope! Given the p-value, we have no reason to reject the null hypothesis.
* Yes! Given the p-value, we have reason to believe that the tutoring program had an effect on the Math score.

`@hint`
* Be sure you're looking at the p-value for `Tutoring_Program`.

`@sample_code`

```{r}
# Build nyc_scores_gls_lm
nyc_scores_gls_lm <- lm(Average_Score_SAT_Math ~ Tutoring_Program + Borough + Teacher_Education_Level + Homework_Type,
                        data=nyc_scores_gls)

# Tidy the results with broom
tidy(nyc_scores_gls_lm)

# Examine the results with anova
anova(nyc_scores_gls_lm)
```

`@sct`

```{r}
#feedback/success messages for each possible answer; provided by instructor
msg1 <- "Great! Because the p-value is greater than 0.05, statisticians usually say we fail to reject the null hypothesis, which was that the `Tutoring_Program` has no effect on Math scores."
msg2 <- "Not quite! Be sure you're looking at the p-value for `Tutoring_Program`."

#tests that correct answer was chosen; returns feedback message for selected answer
test_mc(correct = 1, feedback_msgs = c(msg1, msg2))

success_msg("Excellent! It seems that when we block for `Borough` of the school and `Teacher_Education_Level`, our `Tutoring_Program` isn't having a statistically significant effect on the Math SAT score.")

success_msg("Bravo! It seems that here, when blocked out by all the other factors, our Tutoring program has no effect on the Math score.")
```


---

## Factorial Experiments

```yaml
type: VideoExercise 
lang: r
xp: 50 
skills: 1
key: 89de737cf3   
```

`@projector_key`
eb47cc481394422db9d5abddf5971e70
---

## NYC SAT Scores Factorial EDA

```yaml
type: BulletExercise 
lang: r
xp: 100 
key: 8f44110ac7   
```


Let's do some more EDA before we dive into analysis of our factorial experiment. 

Here, we plan to test the effect of `Percent_Black_HL`, `Percent_Tested_HL`, and `Tutoring_Program` on our outcome, `Average_Score_SAT_Math`. The `HL` stands for high-low, where a 1 indicates less than 50% Black students or overall students tested at a school, and a 2 indicates greater than 50% of both. We should build a boxplot of each factor vs. the outcome to have an idea of which ones have a difference in median by factor level (though ultimately, mean difference is what's tested.)


`@pre_exercise_code`

```{r}
#load libraries
library(dplyr)
library(simputation)

#load the data
nyc_scores <- read.csv("https://assets.datacamp.com/production/repositories/1793/datasets/6eee2fcc47c8c8dbb2e9d4670cf2eabeda52b705/nyc_scores.csv")
#clean the data
nyc_scores_2 <- impute_median(nyc_scores, Average_Score_SAT_Math ~ Borough)
nyc_scores_2$Percent_Black[which(is.na(nyc_scores_2$Percent_Black))] <- 0.5
nyc_scores_2$Percent_Tested[which(is.na(nyc_scores_2$Percent_Tested))] <- 0.5

#convert Math score to numeric
nyc_scores_2$Average_Score_SAT_Math <- as.numeric(nyc_scores_2$Average_Score_SAT_Math)
#nyc_scores_2$Percent_Black <- as.numeric(nyc_scores_2$Percent_Black)
#nyc_scores_2$Percent_Tested <- as.numeric(nyc_scores_2$Percent_Tested)

nyc_scores <- nyc_scores_2

nyc_scores$Percent_Tested_HL <- ntile(nyc_scores$Percent_Tested, 2)
nyc_scores$Percent_Black_HL <- ntile(nyc_scores$Percent_Black, 2)

tutor_vec <- rep(c("Yes", "No"), times = 217)
tutor_vec[435] <- "No"
nyc_scores$Tutoring_Program <- tutor_vec

nyc_scores$Tutoring_Program <- as.factor(nyc_scores$Tutoring_Program)
nyc_scores$Percent_Tested_HL <- as.factor(nyc_scores$Percent_Tested_HL)
nyc_scores$Percent_Black_HL <- as.factor(nyc_scores$Percent_Black_HL)

#for steps 2 & 3
library(ggplot2)
```

***



```yaml
type: NormalExercise 
xp: 40 
key: bd275d7493   
```





`@instructions`
Load `ggplot2`. Create a boxplot of the outcome versus `Tutoring_Program`.

`@hint`
For `geom_boxplot()`, the associated `aes()` takes an `x` and `y` argument. `x` should be your factor variable and `y` the outcome.

`@sample_code`

```{r}
# Load ggplot2
___

# Build the boxplot for the tutoring program vs. Math SAT score
ggplot(___,
       aes(___, ___)) + 
    geom_boxplot()
```

`@solution`

```{r}
# Load ggplot2
library(ggplot2)

# Build the boxplot for the tutoring program vs. Math SAT score
ggplot(nyc_scores,
       aes(Tutoring_Program, Average_Score_SAT_Math)) + 
       geom_boxplot()
```

`@sct`

```{r}
#Test for library call
test_library_function(package = "ggplot2", 
                      not_called_msg = "Remember to load the `ggplot2` package.", 
                      incorrect_msg = "Make sure you've spelled the package name correctly in the `library()` call.")

#checks ggplot() logic
ex() %>% {
  #checks that ggplot() was called
  check_function(state = .,
                 name = "ggplot") %>% {
        #checks for the data argument in the ggplot()
        check_arg(state = ., 
                  arg = "data") %>%
        #checks that the data argument in ggplot() is correct
        check_equal()
        #checks for the mapping argument in the ggplot()
        check_arg(state = ., 
                  arg = "mapping") %>%
        #checks that the mapping argument in ggplot() is correct
        check_equal()
    }
  #checks that aes() was called
  check_function(., "aes") %>% {
        #checks for the x argument in the aes()
        check_arg(., arg = "x") %>%
        #checks that the x argument in aes() is correct
        check_equal(eval = FALSE)
        #checks for the y argument in the aes()
        check_arg(., arg = "y") %>%
        #checks that the y argument in aes() is correct
        check_equal(eval = FALSE)
    }
  #checks that geom_boxplot() was called
  check_function(state = .,
                 name = "geom_boxplot")
}
```


***



```yaml
type: NormalExercise 
xp: 30 
key: dfb7d1f6ff   
```





`@instructions`
Using `ggplot2`, create a boxplot of the outcome versus `Percent_Black_HL`.

`@hint`
For `geom_boxplot()`, the associated `aes()` takes an `x` and `y` argument. `x` should be your group variable and `y` the outcome.

`@sample_code`

```{r}
# Build the boxplot for the percent black vs. Math SAT score
ggplot(___,
       aes(___, ___)) + 
    geom_boxplot()
```

`@solution`

```{r}
# Build the boxplot for percent black vs. Math SAT score
ggplot(nyc_scores,
       aes(Percent_Black_HL, Average_Score_SAT_Math)) + 
    geom_boxplot()
```

`@sct`

```{r}
#checks ggplot() logic
ex() %>% {
  #checks that ggplot() was called
  check_function(state = .,
                 name = "ggplot") %>% {
        #checks for the data argument in the ggplot()
        check_arg(state = ., 
                  arg = "data") %>%
        #checks that the data argument in ggplot() is correct
        check_equal()
        #checks for the mapping argument in the ggplot()
        check_arg(state = ., 
                  arg = "mapping") %>%
        #checks that the mapping argument in ggplot() is correct
        check_equal()
    }
  #checks that aes() was called
  check_function(., "aes") %>% {
        #checks for the x argument in the aes()
        check_arg(., arg = "x") %>%
        #checks that the x argument in aes() is correct
        check_equal(eval = FALSE)
        #checks for the y argument in the aes()
        check_arg(., arg = "y") %>%
        #checks that the y argument in aes() is correct
        check_equal(eval = FALSE)
    }
  #checks that geom_boxplot() was called
  check_function(state = .,
                 name = "geom_boxplot")
}
```


***



```yaml
type: NormalExercise 
xp: 30 
key: e23fca2d71   
```





`@instructions`
Using `ggplot2`, create a boxplot of the outcome versus `Percent_Tested_HL`.

`@hint`
For `geom_boxplot()`, the associated `aes()` takes an `x` and `y` argument. `x` should be your group variable and `y` the outcome.

`@sample_code`

```{r}
# Build the boxplot for percent tested vs. Math SAT score
ggplot(___,
       aes(___, ___)) + 
    geom_boxplot()
```

`@solution`

```{r}
# Build the boxplot for percent tested vs. Math SAT score
ggplot(nyc_scores,
       aes(Percent_Tested_HL, Average_Score_SAT_Math)) + 
    geom_boxplot()
```

`@sct`

```{r}
#checks ggplot() logic
ex() %>% {
  #checks that ggplot() was called
  check_function(state = .,
                 name = "ggplot") %>% {
        #checks for the data argument in the ggplot()
        check_arg(state = ., 
                  arg = "data") %>%
        #checks that the data argument in ggplot() is correct
        check_equal()
        #checks for the mapping argument in the ggplot()
        check_arg(state = ., 
                  arg = "mapping") %>%
        #checks that the mapping argument in ggplot() is correct
        check_equal()
    }
  #checks that aes() was called
  check_function(., "aes") %>% {
        #checks for the x argument in the aes()
        check_arg(., arg = "x") %>%
        #checks that the x argument in aes() is correct
        check_equal(eval = FALSE)
        #checks for the y argument in the aes()
        check_arg(., arg = "y") %>%
        #checks that the y argument in aes() is correct
        check_equal(eval = FALSE)
    }
  #checks that geom_boxplot() was called
  check_function(state = .,
                 name = "geom_boxplot")
}

success_msg("Excellent! Now, let's move on to the analysis of these factors on the score.")
```


---

## Factorial Experiment with NYC SAT Scores

```yaml
type: NormalExercise 
lang: r
xp: 100 
skills: 1
key: 7eee45a518   
```


Now we want to examine the effect of tutoring programs on the NYC schools' SAT Math score. As noted in the last exercise: the variable `Tutoring_Program` is simply `yes` or `no`, depending on if a school got a tutoring program implemented. For `Percent_Black_HL` and `Percent_Tested_HL`, `HL` stands for high/low, a 1 indicates less than 50% Black students or overall students tested, and a 2 indicates greater than 50% of both.

Remember that because we intend to test all of the possible combinations of factor levels, we need to write the formula like: `outcome ~ factor1 * factor2 * factor3`.


`@instructions`
* Use `aov()` to create a model to test how `Percent_Tested_HL`, `Percent_Black_HL`, and `Tutoring_Program` affect the outcome `Average_Score_SAT_Math`. Save as a model object `nyc_scores_factorial` and examine the outcome with `tidy()`.

`@hint`
Your formula should look like: `Average_Score_SAT_Math ~ Percent_Tested_HL * Percent_Black_HL * Tutoring_Program`.

`@pre_exercise_code`

```{r}
library(dplyr)
nyc_scores <- read.csv("https://assets.datacamp.com/production/repositories/1793/datasets/6eee2fcc47c8c8dbb2e9d4670cf2eabeda52b705/nyc_scores.csv")
nyc_scores$Percent_Tested_HL <- ntile(nyc_scores$Percent_Tested, 2)
nyc_scores$Percent_Black_HL <- ntile(nyc_scores$Percent_Black, 2)
tutor_vec <- rep(c("Yes", "No"), times = 217)
tutor_vec[435] <- "No"
nyc_scores$Tutoring_Program <- tutor_vec

nyc_scores$Tutoring_Program <- as.factor(nyc_scores$Tutoring_Program)
nyc_scores$Percent_Tested_HL <- as.factor(nyc_scores$Percent_Tested_HL)
nyc_scores$Percent_Black_HL <- as.factor(nyc_scores$Percent_Black_HL)
#for students
library(broom)
```

`@sample_code`

```{r}
# Create nyc_scores_factorial and examine the results
___
___
```

`@solution`

```{r}
# Create nyc_scores_factorial and examine the results
nyc_scores_factorial <- aov(Average_Score_SAT_Math ~ Percent_Tested_HL * Percent_Black_HL * Tutoring_Program, data = nyc_scores)
tidy(nyc_scores_factorial)
```

`@sct`

```{r}
#Checks nyc_scores_factorial object, then (if object is wrong) checks function(s) that create object
check_correct({
    ex() %>% 
        #check for the nyc_scores_factorial object
        check_object(name = "nyc_scores_factorial",
                     undefined_msg = "Did you define `nyc_scores_factorial`?") %>%
        #checks that the nyc_scores_factorial object is correct
        check_equal(incorrect_msg = "The contents of `nyc_scores_factorial` appear to be incorrect.",
                    append = FALSE)
},{
    ex() %>% {
        #checks that aov() was called
        check_function(state = ., 
                       name = "aov",
                       not_called_msg = "Did you call `aov()`?") %>% {
            #checks that the formula argument was included in the aov() call
            check_arg(state = ., 
                      arg = "formula",
                      arg_not_specified_msg = "Did you remember to specify the `formula` argument in this call to `aov()`?") %>%
            #checks that the formula argument in aov() is correct
            check_equal(incorrect_msg = "It looks like the `formula` argument in this call of `aov()` is incorrect.",
                        append = FALSE)
            #checks that the data argument was included in the aov() call
            check_arg(state = ., 
                      arg = "data",
                      arg_not_specified_msg = "Did you remember to specify the `data` argument in this call to `aov()`?") %>%
            #checks that the data argument in aov() is correct
            check_equal(incorrect_msg = "It looks like the `data` argument in this call of `aov()` is incorrect.",
                        append = FALSE)
            }
    }
})

#Check for output in the console
ex() %>%
    check_output_expr(expr = "tidy(nyc_scores_factorial)",
                      missing_msg = "Be sure to call `tidy()` on `nyc_scores_factorial` to view the tidied results of the model.")

success_msg("Whoo! As is best practice, let's go to the next exercise and do some model checking.")
```

---

## Evaluating the NYC SAT Scores Factorial Model

```yaml
type: NormalExercise 
lang: r
xp: 100 
skills: 1
key: aae74b9c8b   
```


We've built our model, so we know what's next: model checking! We need to examine both if our outcome and our model residuals are normally distributed. We'll check the normality assumption using `shapiro.test()`. A low p-value means we can reject the null hypothesis that the sample came from a normally distributed population.

Let's carry out the requisite model checks for our 2^k factorial model, `nyc_scores_factorial`, which as been loaded for you.


`@instructions`
* Test the outcome `Average_Score_SAT_Math` for normality using `shapiro.test()`.
* Set up a 2 by 2 grid for plots and plot the `nyc_scores_factorial` model object to create the residual plots.

`@hint`
* The only argument to `shapiro.test()` should be the outcome from the `nyc_scores` dataset.

`@pre_exercise_code`

```{r}
#libraries
library(dplyr)

#load the data
nyc_scores <- read.csv("https://assets.datacamp.com/production/repositories/1793/datasets/6eee2fcc47c8c8dbb2e9d4670cf2eabeda52b705/nyc_scores.csv")
#modify it
nyc_scores$Percent_Tested_HL <- ntile(nyc_scores$Percent_Tested, 2)
nyc_scores$Percent_Black_HL <- ntile(nyc_scores$Percent_Black, 2)
tutor_vec <- rep(c("Yes", "No"), times = 217)
tutor_vec[435] <- "No"
nyc_scores$Tutoring_Program <- tutor_vec

nyc_scores$Tutoring_Program <- as.factor(nyc_scores$Tutoring_Program)
nyc_scores$Percent_Tested_HL <- as.factor(nyc_scores$Percent_Tested_HL)
nyc_scores$Percent_Black_HL <- as.factor(nyc_scores$Percent_Black_HL)

#nyc_scores_factorial
nyc_scores_factorial <- aov(Average_Score_SAT_Math ~ Percent_Tested_HL * Percent_Black_HL * Tutoring_Program, data = nyc_scores)

#for students
library(broom)
```

`@sample_code`

```{r}
# Use shapiro.test() to test the outcome
___

# Plot nyc_scores_factorial to examine residuals
par(mfrow = c(2,2))
___
```

`@solution`

```{r}
# Use shapiro.test() to test the outcome
shapiro.test(nyc_scores$Average_Score_SAT_Math)

# Plot nyc_scores_factorial to examine residuals
par(mfrow = c(2,2))
plot(nyc_scores_factorial)
```

`@sct`

```{r}
#check shapiro.test() output
ex() %>%
    check_output_expr(expr = "shapiro.test(nyc_scores$Average_Score_SAT_Math)",
                      missing_msg = "Be sure to call `shapiro.test()` on `nyc_scores$Average_Score_SAT_Math` to view the results.")

#check 2x2 grid code
ex() %>% 
    check_code(state = .,
               regex = 'par(mfrow = c(2, 2))',
               fixed = TRUE,
               times = 1)

#check plot
ex() %>% 
    check_function("plot")

success_msg("Brilliant! The model appears to be fairly well fit, though our evidence indicates the score may not be from a normally distributed population. If we had more time, we might consider a transformation on the outcome to move towards normality.")
```

---

## What's next in Experimental Design

```yaml
type: VideoExercise 
lang: r
xp: 50 
skills: 1
key: 77f35681ef   
```

`@projector_key`
d2fedb3e142b978e9eae111ed680dd00
