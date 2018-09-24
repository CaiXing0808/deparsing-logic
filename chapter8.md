---
title_meta: 'Chapter Two'
title: 'Embedding Code'
description: 'Weave your code and narration into a single document and then render the document to create a finished report that includes both the code and its output. Understand how to customize this process, and open the door for automated, targeted reporting.'
attachments:
    slides_link: 'https://s3.amazonaws.com/assets.datacamp.com/course/rmarkdown/rmarkdown_ch2_slides.pdf'
---

## Section 4 - Knitr

```yaml
type: VideoExercise
key: 7a9881f17b
xp: 50
skills: 1,5
video_link: //player.vimeo.com/video/159981976
video_hls: //videos.datacamp.com/transcoded/639_rmarkdown/v3/hls-ch2_1.master.m3u8
```


---

## R code chunks

```yaml
type: MarkdownExercise
key: 16617a2b3f
xp: 100
skills: 1,5
```

You can embed R code into your R Markdown report with the `knitr` syntax. To do this, surround your code with two lines: one that contains ```` ```{r} ```` and one that contains ```` ``` ````.  The result is a code chunk that looks like this:

<pre><code>```{r}
# some code
```</code></pre>

When you render the report, R will execute the code. If the code returns any results, R will add them to your report.

`@instructions`
The first file in the editor pane on the right contains the next section of your R Markdown report. This section will explain how you cleaned your data. The second file (my_code.R) on the right is an R Script that contains the actual code that we used to clean the data. Use the `knitr` syntax to embed this code into the .Rmd file.

Then render the file to see the results.

`@hint`
The resulting .Rmd file shouldn't contain any reference to the R file; just copy and paste the code from the R script to the .Rmd file.

`@sample_code`
{{{my_document.Rmd}}}
## Cleaning

For the remainder of the report, we will look only at data from the year 1995. We aggregate our data by location, using the *R* code below.

{{{my_code.R}}}
library(nasaweather)
library(dplyr)

year <- 1995

means <- atmos %>%
  filter(year == year) %>%
  group_by(long, lat) %>%
  summarize(temp = mean(temp, na.rm = TRUE),
         pressure = mean(pressure, na.rm = TRUE),
         ozone = mean(ozone, na.rm = TRUE),
         cloudlow = mean(cloudlow, na.rm = TRUE),
         cloudmid = mean(cloudmid, na.rm = TRUE),
         cloudhigh = mean(cloudhigh, na.rm = TRUE)) %>%
  ungroup()

`@solution`
{{{solution.Rmd}}}
## Cleaning

For the remainder of the report, we will look only at data from the year 1995. We aggregate our data by location, using the *R* code below.

```{r}
library(nasaweather)
library(dplyr)

year <- 1995

means <- atmos %>%
  filter(year == year) %>%
  group_by(long, lat) %>%
  summarize(temp = mean(temp, na.rm = TRUE),
         pressure = mean(pressure, na.rm = TRUE),
         ozone = mean(ozone, na.rm = TRUE),
         cloudlow = mean(cloudlow, na.rm = TRUE),
         cloudmid = mean(cloudmid, na.rm = TRUE),
         cloudhigh = mean(cloudhigh, na.rm = TRUE)) %>%
  ungroup()
```

`@sct`
```{r}
test_error()
test_rmd_file({
      test_rmd_group(1, {
        test_text("## Cleaning", not_called_msg = "Have you preserved the title (Cleaning) as a second-level header? Use two hashtags.")
        test_text("For the remainder of the report", not_called_msg = "Make sure to merge both files in the Rmd file: first comes the text, then the code block.")
      })
      test_rmd_group(2, {
        test_library_function("nasaweather")
        test_library_function("dplyr")
        test_object("year", undefined_msg = "The variable `year` is not defined inside the code chunk.",
                    incorrect_msg = "Make sure to set the `year` to 1995.")
        test_object("means", undefined_msg = "The `means` variable has not been defined in the code chunk.",
                    incorrect_msg = "Do not change anything about the code that defines the `means` variable.")
      })
})
success_msg("Good job! Notice that this code does not display any results. It simply saves `means` so we can use it later. Did you notice that we included  `library(nasaweather)` and `library(dplyr)` to be rerun in the last exercise? Each R Markdown document is given a fresh, empty R session to run its code chunks in. This means that you will need to define any R objects that this document uses - and load any packages that it uses - inside the same R Markdown document. The document won't have access to the objects that exist in your current R session.")
```

---

## Customize R code chunks

```yaml
type: MarkdownExercise
key: d17fe6de1e
xp: 100
skills: 1,5
```

You can customize each R code chunk in your report by providing optional arguments after the `r` in ```` ```{r} ````, which appears at the start of the code chunk. Let's look at one set of options.

R functions sometimes return messages, warnings, and even error messages. By default, R Markdown will include these messages in your report. You can use the `message`, `warning` and `error` options to prevent R Markdown from displaying these. If any of the options are set to `FALSE` R Markdown will not include the corresponding type of message in the output.

For example, R Markdown would ignore any errors or warnings generated by the chunk below.

<pre><code>```{r warning = FALSE, error = FALSE}
"four" + "five"
```</code></pre>

`@instructions`
- Packages often generate messages when you first load them with `library()`. To make sure that these messages do not appear in your report, separate `library(nasaweather)`, `library(dplyr)`, and `library(ggvis)` into their own code chunk in the document to the right.  Be sure to make this the first code chunk in your document (so other code chunks will have access to the data sets and functions that come in those libraries).
- Arrange for the new code chunk to ignore any messages that are generated when loading the packages.

Then render the file to see the results.

`@hint`
You can look up code chunk options in the [R Markdown Reference Guide](http://www.rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf).

`@sample_code`
{{{my_document.Rmd}}}
## Cleaning

For the remainder of the report, we will look only at data from the year 1995. We aggregate our data by location, using the *R* code below.

```{r}
library(nasaweather)
library(dplyr)
library(ggvis)

year <- 1995

means <- atmos %>%
  filter(year == year) %>%
  group_by(long, lat) %>%
  summarize(temp = mean(temp, na.rm = TRUE),
         pressure = mean(pressure, na.rm = TRUE),
         ozone = mean(ozone, na.rm = TRUE),
         cloudlow = mean(cloudlow, na.rm = TRUE),
         cloudmid = mean(cloudmid, na.rm = TRUE),
         cloudhigh = mean(cloudhigh, na.rm = TRUE)) %>%
  ungroup()
```

`@solution`
{{{solution.Rmd}}}
## Cleaning

For the remainder of the report, we will look only at data from the year 1995. We aggregate our data by location, using the *R* code below.

```{r message = FALSE}
library(nasaweather)
library(dplyr)
```

```{r}
year <- 1995

means <- atmos %>%
  filter(year == year) %>%
  group_by(long, lat) %>%
  summarize(temp = mean(temp, na.rm = TRUE),
         pressure = mean(pressure, na.rm = TRUE),
         ozone = mean(ozone, na.rm = TRUE),
         cloudlow = mean(cloudlow, na.rm = TRUE),
         cloudmid = mean(cloudmid, na.rm = TRUE),
         cloudhigh = mean(cloudhigh, na.rm = TRUE)) %>%
  ungroup()
```

`@sct`
```{r}
test_error()
test_rmd_group(1, {
  test_text("## Cleaning", not_called_msg = "Have you preserved the title (Cleaning) as a second-level header? Use two hashtags.")
  test_text("For the remainder of the report", not_called_msg = "Don't touch the text that precedes the code blocks.")
})
test_rmd_group(2, {
  test_chunk_options("message", not_called_msg = "Make sure to specify the `message` option in the code chunk that is responsible for loading the packages.",
                     incorrect_msg = "Make sure to set the `message` option to `FALSE`. That way, no warning messages will be visible in the resulting output file.")
  test_library_function("nasaweather")
  test_library_function("dplyr")
})
test_rmd_group(3, {
  test_object("year", undefined_msg = "The variable `year` is not defined inside the code chunk.", incorrect_msg = "Make sure to set the `year` to 1995.")
  test_object("means", undefined_msg = "The `means` variable has not been defined in the code chunk.", incorrect_msg = "Do not change anything about the code that defines the `means` variable.")
})
success_msg("Nice job! Notice that splitting your code in different chunks does not change anything about the availability of the results. Although the `library()` functions have been executed in another chunk, the packages are still available in the next chunk.")
```

---

## Popular chunk options

```yaml
type: MarkdownExercise
key: f9c09dd92b
xp: 100
skills: 1,5
```

Three of the most popular chunk options are `echo`, `eval` and `results`.

If `echo = FALSE`,  R Markdown will not display the code in the final document (but it will still run the code and display its results unless told otherwise).

If `eval = FALSE`, R Markdown will not run the code or include its results, (but it will still display the code unless told otherwise).

If `results = 'hide'`, R Markdown will not display the results of the code (but it will still run the code and display the code itself unless told otherwise).

`@instructions`
The R Markdown file to the right contains a complete report with two figures. It is common to display figures without the code that generates them (the code is a distraction). Modify each code chunk that generates a graph so that it does not display the code that makes the graph. Notice how the document controls the size of the figures with the `fig.height` and `fig.width` arguments.

`@hint`
In this case you don't want the code to be shown in the output document, so you work with the `echo` option.
Don't be overwhelmed by the amount of sample code; you just have to add some options here and there.

`@sample_code`
{{{my_document.Rmd}}}
## Data

The `atmos` data set resides in the `nasaweather` package of the *R* programming language. It contains a collection of atmospheric variables measured between 1995 and 2000 on a grid of 576 coordinates in the western hemisphere. The data set comes from the [2006 ASA Data Expo](http://stat-computing.org/dataexpo/2006/).

Some of the variables in the `atmos` data set are:

* **temp** - The mean monthly air temperature near the surface of the Earth (measured in kelvins (*K*))

* **pressure** - The mean monthly air pressure at the surface of the Earth (measured in millibars (*mb*))

* **ozone** - The mean monthly abundance of atmospheric ozone (measured in Dobson units (*DU*))

You can convert the temperature unit from Kelvin to Celsius with the formula

$$ celsius = kelvin - 273.15 $$

And you can convert the result to Fahrenheit with the formula

$$ fahrenheit = celsius \times \frac{9}{5} + 32 $$

## Cleaning

For the remainder of the report, we will look only at data from the year 1995. We aggregate our data by location, using the *R* code below.

```{r message = FALSE}
load(url("http://assets.datacamp.com/course/rmarkdown/atmos.RData")) # working with a subset
library(dplyr)
library(ggvis)
```

```{r}
year <- 1995
means <- atmos %>%
  filter(year == year) %>%
  group_by(long, lat) %>%
  summarize(temp = mean(temp, na.rm = TRUE),
         pressure = mean(pressure, na.rm = TRUE),
         ozone = mean(ozone, na.rm = TRUE),
         cloudlow = mean(cloudlow, na.rm = TRUE),
         cloudmid = mean(cloudmid, na.rm = TRUE),
         cloudhigh = mean(cloudhigh, na.rm = TRUE)) %>%
  ungroup()
```

## Ozone and temperature

Is the relationship between ozone and temperature useful for understanding fluctuations in ozone? A scatterplot of the variables shows a strong, but unusual relationship.

```{r fig.height = 4, fig.width = 5}
means %>%
  ggvis(~temp, ~ozone) %>%
  layer_points()
```

We suspect that group level effects are caused by environmental conditions that vary by locale. To test this idea, we sort each data point into one of four geographic regions:

```{r}
means$locale <- "north america"
means$locale[means$lat < 10] <- "south pacific"
means$locale[means$long > -80 & means$lat < 10] <- "south america"
means$locale[means$long > -80 & means$lat > 10] <- "north atlantic"
```

### Model

We suggest that ozone is highly correlated with temperature, but that a different relationship exists for each geographic region. We capture this relationship with a second order linear model of the form

$$ ozone = \alpha + \beta_{1} temperature + \sum_{locales} \beta_{i} locale_{i} + \sum_{locales} \beta_{j} interaction_{j} + \epsilon$$

This yields the following coefficients and model lines.

```{r}
lm(ozone ~ temp + locale + temp:locale, data = means)
```

```{r fig.height = 4, fig.width = 5}
means %>%
  group_by(locale) %>%
  ggvis(~temp, ~ozone) %>%
  layer_points(fill = ~locale) %>%
  layer_model_predictions(model = "lm", stroke = ~locale) %>%
  hide_legend("stroke") %>%
  scale_nominal("stroke", range = c("darkorange", "darkred", "darkgreen", "darkblue"))
```

### Diagnostics

An anova test suggests that both locale and the interaction effect of locale and temperature are useful for predicting ozone (i.e., the p-value that compares the full model to the reduced models is statistically significant).

```{r}
mod <- lm(ozone ~ temp, data = means)
mod2 <- lm(ozone ~ temp + locale, data = means)
mod3 <- lm(ozone ~ temp + locale + temp:locale, data = means)

anova(mod, mod2, mod3)
```

`@solution`
{{{solution.Rmd}}}

## Data

The `atmos` data set resides in the `nasaweather` package of the *R* programming language. It contains a collection of atmospheric variables measured between 1995 and 2000 on a grid of 576 coordinates in the western hemisphere. The data set comes from the [2006 ASA Data Expo](http://stat-computing.org/dataexpo/2006/).

Some of the variables in the `atmos` data set are:

* **temp** - The mean monthly air temperature near the surface of the Earth (measured in kelvins (*K*))

* **pressure** - The mean monthly air pressure at the surface of the Earth (measured in millibars (*mb*))

* **ozone** - The mean monthly abundance of atmospheric ozone (measured in Dobson units (*DU*))

You can convert the temperature unit from Kelvin to Celsius with the formula

$$ celsius = kelvin - 273.15 $$

And you can convert the result to Fahrenheit with the formula

$$ fahrenheit = celsius \times \frac{9}{5} + 32 $$

## Cleaning

For the remainder of the report, we will look only at data from the year 1995. We aggregate our data by location, using the *R* code below.

```{r message = FALSE}
load(url("http://assets.datacamp.com/course/rmarkdown/atmos.RData")) # working with a subset
library(dplyr)
library(ggvis)
```

```{r}
year <- 1995
means <- atmos %>%
  filter(year == year) %>%
  group_by(long, lat) %>%
  summarize(temp = mean(temp, na.rm = TRUE),
         pressure = mean(pressure, na.rm = TRUE),
         ozone = mean(ozone, na.rm = TRUE),
         cloudlow = mean(cloudlow, na.rm = TRUE),
         cloudmid = mean(cloudmid, na.rm = TRUE),
         cloudhigh = mean(cloudhigh, na.rm = TRUE)) %>%
  ungroup()
```

## Ozone and temperature

Is the relationship between ozone and temperature useful for understanding fluctuations in ozone? A scatterplot of the variables shows a strong, but unusual relationship.

```{r echo = FALSE, fig.height = 4, fig.width = 5}
means %>%
  ggvis(~temp, ~ozone) %>%
  layer_points()
```

We suspect that group level effects are caused by environmental conditions that vary by locale. To test this idea, we sort each data point into one of four geographic regions:

```{r}
means$locale <- "north america"
means$locale[means$lat < 10] <- "south pacific"
means$locale[means$long > -80 & means$lat < 10] <- "south america"
means$locale[means$long > -80 & means$lat > 10] <- "north atlantic"
```

### Model

We suggest that ozone is highly correlated with temperature, but that a different relationship exists for each geographic region. We capture this relationship with a second order linear model of the form

$$ ozone = \alpha + \beta_{1} temperature + \sum_{locales} \beta_{i} locale_{i} + \sum_{locales} \beta_{j} interaction_{j} + \epsilon$$

This yields the following coefficients and model lines.

```{r}
lm(ozone ~ temp + locale + temp:locale, data = means)
```

```{r echo = FALSE, fig.height = 4, fig.width = 5}
means %>%
  group_by(locale) %>%
  ggvis(~temp, ~ozone) %>%
  layer_points(fill = ~locale) %>%
  layer_model_predictions(model = "lm", stroke = ~locale) %>%
  hide_legend("stroke") %>%
  scale_nominal("stroke", range = c("darkorange", "darkred", "darkgreen", "darkblue"))
```

### Diagnostics

An anova test suggests that both locale and the interaction effect of locale and temperature are useful for predicting ozone (i.e., the p-value that compares the full model to the reduced models is statistically significant).

```{r}
mod <- lm(ozone ~ temp, data = means)
mod2 <- lm(ozone ~ temp + locale, data = means)
mod3 <- lm(ozone ~ temp + locale + temp:locale, data = means)

anova(mod, mod2, mod3)
```

`@sct`
```{r}
test_error()
not_called = "For the code chunk that %s, specify the `echo` option."
incorrect = "For the code chunk that %s, make sure to specify `echo` correctly."
test_rmd_group(5,{
  mess = "plots `temp` versus `ozone`"
  test_chunk_options("echo", not_called_msg = sprintf(not_called,mess), incorrect_msg = sprintf(incorrect,mess))
})
test_rmd_group(10,{
  mess = "reveals clusters in the latitude versus temperature graph"
  test_chunk_options("echo", not_called_msg = sprintf(not_called,mess), incorrect_msg = sprintf(incorrect,mess))
})
success_msg("Nice work! Apart from adding code chunks to your report, you can also include inline code. Find out more about it in the next exercise.")
```

---

## Inline R code

```yaml
type: MarkdownExercise
key: 008bee65f6
xp: 100
skills: 1,5
```

You can embed R code into the text of your document with the `` `r ` `` syntax. Be sure to include the lower case `r` in order for this to work properly. R Markdown will run the code and replace it with its result, which should be a piece of text, such as a character string or a number.

For example, the line below uses embedded R code to create a complete sentence:

```{r}
The factorial of four is `r factorial(4)`.
```

When you render the document the result will appear as:

```
The factorial of four is 24.
```

Inline code provides a useful way to make your reports completely automatable.

`@instructions`
The report to the right has been reorganized to make it more automatable.

- Change the value of `year` to 2000 in line 28.
- Complete lines 31 and 52 so that the blank space, `___`, shows the value of the `year` object when the report is rendered.
- Render the document and notice how everything updates to use the new year's worth of data. Even the sentences in lines 31 and 52 update to reflect the new year.

`@hint`
You can add the actual value of the `year` variable by adding `r year` at the appropriate location in your inline text.

`@sample_code`
{{{my_document.Rmd}}}
---
output: html_document
---

## Data

The `atmos` data set resides in the `nasaweather` package of the *R* programming language. It contains a collection of atmospheric variables measured between 1995 and 2000 on a grid of 576 coordinates in the western hemisphere. The data set comes from the [2006 ASA Data Expo](http://stat-computing.org/dataexpo/2006/).

Some of the variables in the `atmos` data set are:

* **temp** - The mean monthly air temperature near the surface of the Earth (measured in kelvins (*K*))

* **pressure** - The mean monthly air pressure at the surface of the Earth (measured in millibars (*mb*))

* **ozone** - The mean monthly abundance of atmospheric ozone (measured in Dobson units (*DU*))

You can convert the temperature unit from Kelvin to Celsius with the formula

$$ celsius = kelvin - 273.15 $$

And you can convert the result to Fahrenheit with the formula

$$ fahrenheit = celsius \times \frac{9}{5} + 32 $$

## Cleaning

```{r echo = FALSE}
year <- 1995
```

For the remainder of the report, we will look only at data from the year ___. We aggregate our data by location, using the *R* code below.

```{r message = FALSE}
library(nasaweather)
library(dplyr)
library(ggvis)
```

```{r}
means <- atmos %>%
  filter(year == year) %>%
  group_by(long, lat) %>%
  summarize(temp = mean(temp, na.rm = TRUE),
         pressure = mean(pressure, na.rm = TRUE),
         ozone = mean(ozone, na.rm = TRUE),
         cloudlow = mean(cloudlow, na.rm = TRUE),
         cloudmid = mean(cloudmid, na.rm = TRUE),
         cloudhigh = mean(cloudhigh, na.rm = TRUE)) %>%
  ungroup()
```

where the `year` object equals ___.


## Ozone and temperature

Is the relationship between ozone and temperature useful for understanding fluctuations in ozone? A scatterplot of the variables shows a strong, but unusual relationship.

```{r echo = FALSE, fig.height = 4, fig.width = 5}
means %>%
  ggvis(~temp, ~ozone) %>%
  layer_points()
```

We suspect that group level effects are caused by environmental conditions that vary by locale. To test this idea, we sort each data point into one of four geographic regions:

```{r}
means$locale <- "north america"
means$locale[means$lat < 10] <- "south pacific"
means$locale[means$long > -80 & means$lat < 10] <- "south america"
means$locale[means$long > -80 & means$lat > 10] <- "north atlantic"
```

### Model

We suggest that ozone is highly correlated with temperature, but that a different relationship exists for each geographic region. We capture this relationship with a second order linear model of the form

$$ ozone = \alpha + \beta_{1} temperature + \sum_{locales} \beta_{i} locale_{i} + \sum_{locales} \beta_{j} interaction_{j} + \epsilon$$

This yields the following coefficients and model lines.

```{r}
lm(ozone ~ temp + locale + temp:locale, data = means)
```

```{r echo = FALSE, fig.height = 4, fig.width = 5}
means %>%
  group_by(locale) %>%
  ggvis(~temp, ~ozone) %>%
  layer_points(fill = ~locale) %>%
  layer_model_predictions(model = "lm", stroke = ~locale) %>%
  hide_legend("stroke") %>%
  scale_nominal("stroke", range = c("darkorange", "darkred", "darkgreen", "darkblue"))
```

### Diagnostics

An anova test suggests that both locale and the interaction effect of locale and temperature are useful for predicting ozone (i.e., the p-value that compares the full model to the reduced models is statistically significant).

```{r}
mod <- lm(ozone ~ temp, data = means)
mod2 <- lm(ozone ~ temp + locale, data = means)
mod3 <- lm(ozone ~ temp + locale + temp:locale, data = means)

anova(mod, mod2, mod3)
```

`@solution`
{{{solution.Rmd}}}
---
output: html_document
---

## Data

The `atmos` data set resides in the `nasaweather` package of the *R* programming language. It contains a collection of atmospheric variables measured between 1995 and 2000 on a grid of 576 coordinates in the western hemisphere. The data set comes from the [2006 ASA Data Expo](http://stat-computing.org/dataexpo/2006/).

Some of the variables in the `atmos` data set are:

* **temp** - The mean monthly air temperature near the surface of the Earth (measured in kelvins (*K*))

* **pressure** - The mean monthly air pressure at the surface of the Earth (measured in millibars (*mb*))

* **ozone** - The mean monthly abundance of atmospheric ozone (measured in Dobson units (*DU*))

You can convert the temperature unit from Kelvin to Celsius with the formula

$$ celsius = kelvin - 273.15 $$

And you can convert the result to Fahrenheit with the formula

$$ fahrenheit = celsius \times \frac{9}{5} + 32 $$

## Cleaning

```{r echo = FALSE}
year <- 2000
```

For the remainder of the report, we will look only at data from the year `r year`. We aggregate our data by location, using the *R* code below.

```{r message = FALSE}
library(nasaweather)
library(dplyr)
library(ggvis)
```

```{r}
means <- atmos %>%
  filter(year == year) %>%
  group_by(long, lat) %>%
  summarize(temp = mean(temp, na.rm = TRUE),
         pressure = mean(pressure, na.rm = TRUE),
         ozone = mean(ozone, na.rm = TRUE),
         cloudlow = mean(cloudlow, na.rm = TRUE),
         cloudmid = mean(cloudmid, na.rm = TRUE),
         cloudhigh = mean(cloudhigh, na.rm = TRUE)) %>%
  ungroup()
```

where the `year` object equals `r year`.


## Ozone and temperature

Is the relationship between ozone and temperature useful for understanding fluctuations in ozone? A scatterplot of the variables shows a strong, but unusual relationship.

```{r echo = FALSE, fig.height = 4, fig.width = 5}
means %>%
  ggvis(~temp, ~ozone) %>%
  layer_points()
```

We suspect that group level effects are caused by environmental conditions that vary by locale. To test this idea, we sort each data point into one of four geographic regions:

```{r}
means$locale <- "north america"
means$locale[means$lat < 10] <- "south pacific"
means$locale[means$long > -80 & means$lat < 10] <- "south america"
means$locale[means$long > -80 & means$lat > 10] <- "north atlantic"
```

### Model

We suggest that ozone is highly correlated with temperature, but that a different relationship exists for each geographic region. We capture this relationship with a second order linear model of the form

$$ ozone = \alpha + \beta_{1} temperature + \sum_{locales} \beta_{i} locale_{i} + \sum_{locales} \beta_{j} interaction_{j} + \epsilon$$

This yields the following coefficients and model lines.

```{r}
lm(ozone ~ temp + locale + temp:locale, data = means)
```

```{r echo = FALSE, fig.height = 4, fig.width = 5}
means %>%
  group_by(locale) %>%
  ggvis(~temp, ~ozone) %>%
  layer_points(fill = ~locale) %>%
  layer_model_predictions(model = "lm", stroke = ~locale) %>%
  hide_legend("stroke") %>%
  scale_nominal("stroke", range = c("darkorange", "darkred", "darkgreen", "darkblue"))
```

### Diagnostics

An anova test suggests that both locale and the interaction effect of locale and temperature are useful for predicting ozone (i.e., the p-value that compares the full model to the reduced models is statistically significant).

```{r}
mod <- lm(ozone ~ temp, data = means)
mod2 <- lm(ozone ~ temp + locale, data = means)
mod3 <- lm(ozone ~ temp + locale + temp:locale, data = means)

anova(mod, mod2, mod3)
```

`@sct`
```{r}
test_error()
test_rmd_group(2, {
  test_object("year", incorrect_msg = "Make sure to correctly set the `year` variable inside the small code chunk that follows the section title \"Data\".")
})
test_rmd_group(3, {
  test_text("year", format = "inline_code", incorrect_msg = "Have you correctly written the inline R code in the text following the code chunk where you set `year`?")
})
test_rmd_group(6, {
  test_text("year", format = "code")
  test_text("year", format = "inline_code", incorrect_msg = "Have you correctly written the inline R code in the text following the code chunk where `means` is defined?")
})
success_msg("Great job! Note that you cannot customize inline code the way you can customize code chunks.")
```

---

## Know your options

```yaml
type: PureMultipleChoiceExercise
key: 2d56a13765
xp: 50
skills: 1,5
```

Which of the following code chunks would you use to display example code that should not be run?

Option A:

    ```{r echo = FALSE}
    # For example, you could look up today's date:
    Sys.Date()
    ```

Option B:

    ```{r eval = FALSE}
    # For example, you could look up today's date:
    Sys.Date()
    ```

Option C:

    ```{r results = FALSE}
    # For example, you could look up today's date:
    Sys.Date()
    ```

Option D:

    ```{r results = 'hide'}
    # For example, you could look up today's date:
    Sys.Date()
    ```

`@hint`
The code chunks that use the `results` option are not correct, because it determines whether the _results_ of your code have to be displayed. The code is still executed in the background.

`@possible_answers`
- A
- [B]
- C
- D

`@feedbacks`
- The chunk option `echo = FALSE` will not display the code in the final document, but the code is still executed and results will be shown. Try again.
- Great work! Proceed to the next exercise.
- Setting the option `results = FALSE` will not do anything since the syntax is not correct. You have to assign a character to the `results` option.
- If `results = 'hide'` R Markdown will not display the results of the code, but it will still run the code and display the code itself.

---

## Labeling and reusing code chunks

```yaml
type: MarkdownExercise
key: 916767a3e9
xp: 100
skills: 1,5
```

Apart from the popular code chunk options you have learned by now, you can define even more things in the curly braces that follow the triple backticks.

An interesting feature available in `knitr` is the labeling of code snippets. The code chunk below would be assigned the label `simple_sum`:

    ```{r simple_sum, results = 'hide'}
    2 + 2
    ```

However, because the `results` option is equal to `hide`, no output is shown. This is what appears in the output document:

```
2 + 2
```

What purpose do these labels serve? `knitr` provides the option `ref.label` to refer to previously defined and labeled code chunks. If used correctly, `knitr` will copy the code of the chunk you referred to and repeat it in the current code chunk. This feature enables you to separate R code and R output in the output document, without code duplication.

Let's continue the example; the following code chunk:

    ```{r ref.label='simple_sum', echo = FALSE}
    ```

produces the output you would expect:

```
 ## [1] 4
```

Notice that the `echo` option was explicitly set to `FALSE`, suppressing the R code that generated the output.

In the sample code on the right, you see a rather large code chunk that contains R code to load packages `dplyr` and `ggvis` and functions to create a ggvis graph

`@instructions`
- Separate the code chunks into two: one for the `library()` calls, one for the `ggvis` and `dplyr` functions.
- Edit the first chunk's header so no messages are shown in the output document.
- Edit the second chunk's header so output is hidden; give this code chunk the label `chained`.
- Move the sentence "The `ggvis` plot gives us a nice visualization of the `mtcars` data set:" after the second chunk.
- Add a third chunk at the end containing no code, showing the output of the second chunk, without the echoing code that generated it. Use the `ref.label` option.

`@hint`
- `message = FALSE` suppresses messages generated by the R code when it is executed.
- `results = 'hide'` shows the R code in the output document but does not print the results of the R code.
- Write the chunk label straight after ````{r `.
- `echo = FALSE` does not display the R code for the code chunk, but still displays its results.

`@sample_code`
{{{my_document.Rmd}}}
## Exploring the mtcars data set

Have you ever wondered whether there is a clear correlation between the gas consumption of a car and its weight?
To answer this question, we first have to load the `dplyr` and `ggvis` packages. The `ggvis` plot gives us a nice visualization of the `mtcars` data set:

```{r}
library(dplyr)
library(ggvis)

mtcars %>%
  group_by(factor(cyl)) %>%
  ggvis(~mpg, ~wt, fill = ~cyl) %>%
  layer_points()
```

`@solution`
{{{solution.Rmd}}}

## Exploring the mtcars data set

Have you ever wondered whether there is a clear correlation between the gas consumption of a car and its weight?
To answer this question, we first have to load the `dplyr` and `ggvis` packages.

```{r message = FALSE}
library(dplyr)
library(ggvis)
```

```{r chained, results = 'hide'}
mtcars %>%
  group_by(factor(cyl)) %>%
  ggvis(~mpg, ~wt, fill = ~cyl) %>%
  layer_points()
```

The `ggvis` plot gives us a nice visualization of the `mtcars` data set:

```{r ref.label='chained', echo = FALSE}
```

`@sct`
```{r}
test_error()
test_rmd_group(1, {
  test_text("Have you ever wondered", not_called_msg = "Keep the first block of text in there, do not remove it.")
})
test_rmd_group(2, {
  test_chunk_options(options = "message", not_called_msg = "Make sure to specify the `message` option in the chunk containing the `library()` calls.", incorrect_msg = "Make sure to set the `message` option in the first code chunk correctly.")
})
test_rmd_group(3, {
  test_chunk_options(options = "label", incorrect_msg = "Have you specified the label of the second code chunk correctly?")
  test_chunk_options(options = "results", incorrect_msg = "Make sure to hide the results of the second code chunk using the `results` option!")
  test_props(index = 1, funs = c("ggvis","layer_points"))
  test_props(index = 1, funs = c("ggvis","layer_points"), props = "fill")
})
test_rmd_group(4, {
  test_text("The `ggvis` plot gives us a nice visualization", not_called_msg = "Make sure to move the last sentence to between the second and third code chunk.")
})
test_rmd_group(5, {
  test_chunk_options(options = "ref.label", not_called_msg = "Make sure to specify the `ref.label` option in the last code chunk!", incorrect_msg = "Have another look at the `ref.label` option in the last code chunk; make sure to refer to the correct code chunk!")
  test_chunk_options(options = "echo", not_called_msg = "In the last code chunk, specify the `echo` option to hide the code that generates the `ggvis` plot.", incorrect_msg = "Make sure to set the `echo` option in the last code chunk correctly.")
})
success_msg("Nice job! There is a myriad of options available for code chunks, you can discover them at <a target=_blank href=http://yihui.name/knitr/options/>Yihui Xie's website</a>. So far, we have been compiling your R Markdown reports into HTML, but you can also compile your reports into pdf documents, Microsoft word documents, and slideshows. You can also customize the output of your documents. The next video will show you how.")
```
