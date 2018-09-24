---
title_meta: 'Chapter One'
title: 'Authoring R Markdown Reports'
description: 'Begin with a real life case study written in R code and then learn to narrate the code, adding interpretations, explanations, and descriptions with Markdown, the text syntax at the heart of R Markdown.'
attachments:
    slides_link: 'https://s3.amazonaws.com/assets.datacamp.com/course/rmarkdown/rmarkdown_ch1_slides.pdf'
free_preview: true
---

## Section 1 - Introduction

```yaml
type: VideoExercise
key: c4aa33a9b7
xp: 50
skills: 1,5
video_link: //player.vimeo.com/video/159981982
video_hls: //videos.datacamp.com/transcoded/639_rmarkdown/v3/hls-ch1_1.master.m3u8
```


---

## The R Markdown Exercise interface

```yaml
type: MarkdownExercise
key: 3c547b4e1b
xp: 100
skills: 1,5
```

For this course, DataCamp has developed a new kind of interface that looks like the R Markdown pane in RStudio. You have a space (my_document.Rmd) to write R Markdown documents, as well as the buttons to compile the R Markdown document. To keep things simple, we'll stick with making html and pdf documents, although it is also possible to create Microsoft Word documents with R Markdown.

When you click "Knit HTML", DataCamp will compile your R Markdown document and display the finished, formatted results in a new pane.

To give you a taste of the things you'll learn in this course, we've prepared two documents in the editor on the right:

- my_document.Rmd containing the actual R Markdown code;
- faded.css, a supplementary file that brands your report.

`@instructions`
- Change the title of the Markdown Document from "Ozone" to "Hello R Markdown".
- Click the "Knit HTML" button to see the compiled version of your sample code.

`@hint`
To change the title, change the value that comes after "title: " in my_document.Rmd on the right. Then, submit your work by clicking the "Knit HTML" button.

`@sample_code`
{{{my_document.Rmd}}}
---
title: "Ozone"
output:
  html_document:
    css: faded.css
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

```{r, echo = FALSE, results = 'hide'}
example_kelvin <- 282.15
```

For example, `r example_kelvin` degrees Kelvin corresponds to `r example_kelvin - 273.15` degrees Celsius.


{{{faded.css}}}

h1{
  color: white;
  padding: 10px;
  background-color: #3399ff
}

ul {
  list-style-type: square;
}

.MathJax_Display {
  padding: 0.5em;
  background-color: #eaeff3
}

`@solution`
{{{solution.Rmd}}}
---
title: "Hello R Markdown"
output:
  html_document:
    css: faded.css
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

```{r, echo = FALSE, results = 'hide'}
example_kelvin <- 282.15
```

For example, `r example_kelvin` degrees Kelvin corresponds to `r example_kelvin - 273.15` degrees Celsius.


{{{faded.css}}}
h1{
  color: white;
  padding: 10px;
  background-color: #3399ff
}

ul {
  list-style-type: square;
}

.MathJax_Display {
  padding: 0.5em;
  background-color: #eaeff3
}

`@sct`
```{r}
test_error()
test_rmd_file({
  test_yaml_header(options = "title",
                   not_called_msg = "Make sure to define the title on top of the R Markdown document in the editor.",
                   incorrect_msg = "Set the title of the document to \"Hello R Markdown\". Beware of typos and caps!")
  test_yaml_header(options = "output.html_document.css",
                   not_called_msg = "Don't change anything in the title apart from the document title!",
                   incorrect_msg = "Don't change anything in the title apart from the document title!")
})
success_msg("Nice job! Cool, right? Continue to the next exercise to get introduced to the basics of R Markdown.")
```

---

## Explore R Markdown

```yaml
type: MarkdownExercise
key: 13a20f7fbe
xp: 100
skills: 1,5
```

The document to the right is a template R Markdown document. It includes the most familiar parts of an R Markdown document:

* A YAML header that contains some metadata
* Narrative text written in Markdown
* R code chunks surrounded by ```` ```{r} ```` and ```` ``` ````; a syntax that comes from the knitr package

`@instructions`
Click the 'Knit HTML' button and compare the document to its compiled form. Then:

- Change the title of the document to "Hello World".
- Change the author of the document to your own name.
- Rewrite the first sentence of the document to say "This is my first R Markdown document.".
- Replace the `cars` data set with the `mtcars` data set in both code blocks.
- Recompile the document; can you see your changes?

`@hint`
The author and the title of the document can be changed in the YAML header, which is located between three hyphens.

`@sample_code`
{{{my_document.Rmd}}}
---
title: "Untitled"
author: "Anonymous"
date: "January 1, 2015"
output: html_document
---

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:

```{r}
summary(cars)
```

You can also embed plots, for example:

```{r, echo=FALSE}
plot(cars)
```

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.

`@solution`
{{{solution.Rmd}}}
---
title: "Hello World"
author: "DataCamp"
date: "January 1, 2015"
output: html_document
---

This is my first R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:

```{r}
summary(mtcars)
```

You can also embed plots, for example:

```{r, echo=FALSE}
plot(mtcars)
```

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.

`@sct`
```{r}
test_error()

test_yaml_header(options = "title")
test_yaml_header(options = "author", check_equality = FALSE)
test_yaml_header(options = "date", check_equality = FALSE)
test_yaml_header(options = "output")

test_rmd_group(1, {
  test_text("This is my first R Markdown document", not_called_msg = "Don't forget to change the first sentence to \"This is my first R Markdown document.\".")
})

test_rmd_group(2, {
  test_function("summary",args = "object", incorrect_msg = "Make sure to change the dataset to summarize to `mtcars`.")
})

test_rmd_group(4, {
  test_function("plot", args = "x", incorrect_msg = "Plot the `mtcars` data set, not `cars`.")
  test_chunk_options(options = "echo")
})

success_msg("You're doing great! During the course we'll look at specific ways to change each individual component of an R Markdown document.")

```

---

## Section 2 - R code for your report

```yaml
type: VideoExercise
key: e37c233ecc
xp: 50
skills: 1,3,4,7
video_link: //player.vimeo.com/video/159981978
video_hls: //videos.datacamp.com/transcoded/639_rmarkdown/v3/hls-ch1_2.master.m3u8
```


---

## Prepare the workspace for preliminary analysis

```yaml
type: NormalExercise
key: 8649eab65d
xp: 100
skills: 1
```

During this course, we will examine a data set that comes in the `nasaweather` package. The data set is called `atmos`, and it contains meteorological data about the western hemisphere.

We'll also use the `dplyr` package to manipulate our data and the `ggvis` package to visualize it.

For the next set of exercises, you will use the traditional DataCamp interface: you have an editor where you can write and submit R code, as well as a console where you can experiment with R code without doing a formal submission.

`@instructions`
- Load the `nasaweather`, `dplyr`, and `ggvis` packages. These packages have already been installed in the DataCamp R session.
- After submitting the correct code, open the help page for the `atmos` data set by executing `?atmos` in the console. Before proceeding to the next exercise, read the help page to familiarize yourself with the data.

`@hint`
Load packages using the `library()` function. For example, the `stats` package can be loaded using `library(stats)`.
You can consult the help page of the`atmos` data set by executing `help(atmos)` or `?atmos` in the console.

`@pre_exercise_code`
```{python}

```

`@sample_code`
```{r}
# Load the nasaweather package


# Load the dplyr package


# Load the ggvis package

```

`@solution`
```{r}
# Load the nasaweather package
library("nasaweather")

# Load the dplyr package
library("dplyr")

# Load the ggvis package
library("ggvis")
```

`@sct`
```{r}
test_error()
general_msg = "Have you called the `library` function correctly to load the `%s` package? Type `?library()` in the console if you have no experience with this function."
test_library_function("nasaweather")
test_library_function("dplyr")
test_library_function("ggvis")

success_msg("Good work. Now that the workspace is ready for some analysis, head over to the next exercise to prepare your data for the report. Don't forget to consult and read the help page on the `atmos` data set.")
```

---

## Prepare your data

```yaml
type: NormalExercise
key: a04468bc61
xp: 100
skills: 1,3
```

We will use some of the data in `atmos` to explore the relationship between ozone and temperature. But before we do, let's transform the data into a more useful form.

The sample code uses `dplyr` functions to aggregate the data. It computes the mean value of `temp`, `pressure`, `ozone`, `cloudlow`, `cloudmid`, and `cloudhigh` for each latitude/longitude grid point.

You can learn more about `dplyr` in DataCamp's [dplyr course](https://www.datacamp.com/courses/dplyr).

Don't get confused by the pipe operator (`%>%`) from the [magrittr](http://cran.r-project.org/web/packages/magrittr/vignettes/magrittr.html) package that is used often in combination with dplyr verbs. It is used to chain your code in case there are several operations you want to do without the need to save intermediate results.

`@instructions`
- Set the `year` variable to 1995. This will cause the code to retain just observations from the year 1995.
- At the end of the sample code, add a command to print the resulting `means` data frame and examine its output.

`@hint`
Simply set the `year` variable equal to 1995. To explain the pipe operator with an example: `x %>% f(y)` corresponds to `f(x,y)`.
Analogously, `x %>% f(y) %>% g(z)` corresponds to `g(f(x,y),z)`. Got the picture?

`@pre_exercise_code`
```{r}
library(nasaweather)
library("dplyr")
```

`@sample_code`
```{r}
# The nasaweather and dplyr packages are available in the workspace

# Set the year variable to 1995
year <- ___

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

# Inspect the means variable

```

`@solution`
```{r}
# The nasaweather and dplyr packages are available in the workspace

# Set the year variable to 1995
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

# Inspect the means variable
means
```

`@sct`
```{r}
test_error()
test_object("year", incorrect_msg = "Have you correctly set the `year` variable? This variable is used to filter the `atmos` data set.")
test_object("means", incorrect_msg = "You shouldn't change anything about the series of data manipulation functions that define the `means` variable.")
test_output_contains("means", incorrect_msg = "Make sure to print the `means` variable.")
success_msg("You're doing great! Can you see that each combination of latitude and longitude only appears once in `means`? `atmos` records multiple values for multiple dates at each location. `means` only records the mean value of all of the dates for each location. Now that we have the data we'll use, let's visualize it!")
```

---

## Experiment with plot generation

```yaml
type: NormalExercise
key: 01edad47e4
xp: 100
skills: 1,4
```

The sample code on the right uses `ggvis` functions to visualize the data. It displays a plot of `pressure` vs. `ozone`.

We'll use `ggvis` to create several graphs for our R Markdown reports.

You can learn more about `ggvis` in DataCamp's [ggvis course](https://www.datacamp.com/courses/ggvis).

`@instructions`
- Run the code and take a look at the graph that it makes. See how straightforward it is to plot the data from the previous exercise?
- Change the code to plot the `temp` variable vs the `ozone` variable, both in the `means` data set. We will write an R Markdown report that analyzes the relationship between `temp` and `ozone`.

`@hint`
You can plot `temp` on the x axis by setting x equal to `~temp`.

`@pre_exercise_code`
```{r}
library(nasaweather)
library("dplyr")
library("ggvis")
```

`@sample_code`
```{r}
# The nasaweather, dplyr and ggvis packages are loaded in the workspace.

# Code for the previous exercise - do not change this
means <- atmos %>%
  filter(year == 1995) %>%
  group_by(long, lat) %>%
  summarize(temp = mean(temp, na.rm = TRUE),
         pressure = mean(pressure, na.rm = TRUE),
         ozone = mean(ozone, na.rm = TRUE),
         cloudlow = mean(cloudlow, na.rm = TRUE),
         cloudmid = mean(cloudmid, na.rm = TRUE),
         cloudhigh = mean(cloudhigh, na.rm = TRUE)) %>%
  ungroup()

# Change the code to plot the temp variable vs the ozone variable
means %>%
  ggvis(x = ~pressure, y = ~ozone) %>%
  layer_points()
```

`@solution`
```{r}
# The nasaweather, dplyr and ggvis packages are loaded in the workspace.

# Code for the previous exercise - do not change this
means <- atmos %>%
  filter(year == 1995) %>%
  group_by(long, lat) %>%
  summarize(temp = mean(temp, na.rm = TRUE),
         pressure = mean(pressure, na.rm = TRUE),
         ozone = mean(ozone, na.rm = TRUE),
         cloudlow = mean(cloudlow, na.rm = TRUE),
         cloudmid = mean(cloudmid, na.rm = TRUE),
         cloudhigh = mean(cloudhigh, na.rm = TRUE)) %>%
  ungroup()

# Change the code to plot the temp variable vs the ozone variable
means %>%
  ggvis(x = ~temp, y = ~ozone) %>%
  layer_points()
```

`@sct`
```{r}
test_error()
test_object("means", incorrect_msg = "Do not change anything about the chained dplyr functions that define the `means` variable.")
test_props(index = 1, funs = c("ggvis","layer_points"), props = c("x","y"), incorrect_msg = "Have you plotted the correct data? Make sure to plot `temp` on the x-axis and `ozone` on the y-axis.")
success_msg("Good job! Don't worry if you don't understand how the code does what it does in this exercise. For this course, we're just going to focus on how to place a graph in a report. All you need to know is that the code will create a nice graph when you run it.")
```

---

## Prepare a model component

```yaml
type: NormalExercise
key: 4fc1f2573c
xp: 100
skills: 1,7
```

We've now loaded data, cleaned it, and visualized it. Our analysis will have one more component: a model.

The code on the right creates a linear model that predicts `ozone` based on `pressure` and `cloudlow`; all three are variables of the `means` data frame you created earlier.

You can learn more about building models with R in DataCamp's [Introduction to Statistics course](https://www.datacamp.com/tracks/a-hands-on-introduction-to-statistics-with-r).

`@instructions`
- Change the model so that it predicts `ozone` based on `temp` and nothing else.
- Generate a summary of the model using the `summary()` function. Can you interpret the results? Test yourself by looking for the model's estimates for the intercept and `temp` coefficients, as well as the p-value associated with each coefficient and the model's overall Adjusted R-squared.

`@hint`
The tilde operator (`~`) signifies a dependency relation: `y ~ x` specifies that `y` acts as the dependent variable, and `x` as the independent variable. Another example: `y ~ x + z` signifies that `y` depends on both `x` and `z`.

`@pre_exercise_code`
```{r}
library(nasaweather)
library("dplyr")
```

`@sample_code`
```{r}
# The nasaweather and dplyr packages are already at your disposal
means <- atmos %>%
  filter(year == 1995) %>%
  group_by(long, lat) %>%
  summarize(temp = mean(temp, na.rm = TRUE),
         pressure = mean(pressure, na.rm = TRUE),
         ozone = mean(ozone, na.rm = TRUE),
         cloudlow = mean(cloudlow, na.rm = TRUE),
         cloudmid = mean(cloudmid, na.rm = TRUE),
         cloudhigh = mean(cloudhigh, na.rm = TRUE)) %>%
  ungroup()

# Change the model: base prediction only on temp
mod <- lm(ozone ~ pressure + cloudlow, data = means)

# Generate a model summary and interpret the results

```

`@solution`
```{r}
# The nasaweather and dplyr packages are already at your disposal
means <- atmos %>%
  filter(year == 1995) %>%
  group_by(long, lat) %>%
  summarize(temp = mean(temp, na.rm = TRUE),
         pressure = mean(pressure, na.rm = TRUE),
         ozone = mean(ozone, na.rm = TRUE),
         cloudlow = mean(cloudlow, na.rm = TRUE),
         cloudmid = mean(cloudmid, na.rm = TRUE),
         cloudhigh = mean(cloudhigh, na.rm = TRUE)) %>%
  ungroup()

# Change the model: base prediction only on temp
mod <- lm(ozone ~ temp, data = means)

# Generate a model summary and interpret the results
summary(mod)
```

`@sct`
```{r}
test_error()
test_object("means", incorrect_msg = "Do not change anything about the chained function call that specifies the `means` data frame.")
test_function("lm",args = "formula", incorrect_msg = "Make sure to specify the correct formula inside the `lm()` function. Check the hint if you have no experience with the `~` notation.")
test_function("lm", args = "data")
test_function("summary", args = "object", eval = FALSE, not_called_msg = "Don't forget to call the `summary()` function to show information concerning the calculated model.", incorrect_msg = "Have you passed the correct variable to the `summary()` function? You want to summarize `mod`.")

success_msg("Good work! You're now in a familiar position: you've done some preliminary analysis, and you're ready to report your findings. Remember what your code does, as you will work with it again soon. In the next video, Garrett will show you how to write the narrative sections of your report in R Markdown.")
```

---

## Section 3 - Markdown

```yaml
type: VideoExercise
key: 7cb43779cf
xp: 50
skills: 5
video_link: //player.vimeo.com/video/160074284
video_hls: //videos.datacamp.com/transcoded/639_rmarkdown/v3/hls-ch1_3.master.m3u8
```


---

## Styling narrative sections

```yaml
type: MarkdownExercise
key: 2c39e1039f
xp: 100
skills: 5
```

You can use Markdown to embed formatting instructions into your text. For example, you can make a word *italicized* by surrounding it in asterisks, **bold** by surrounding it in two asterisks, and `monospaced` (like code) by surrounding it in backticks:

    *italics*
    **bold**
    `code`

You can turn a word into a link by surrounding it in hard brackets and then placing the link behind it in parentheses, like this:

    [RStudio](www.rstudio.com)

To create titles and headers, use leading hastags. The number of hashtags determines the header's level:

    # First level header
    ## Second level header
    ### Third level header

`@instructions`
The paragraph to the right describes the data that we'll use in our report.

- Turn the line that begins with "Data" into a second level header.
- Change the words atmos and nasaweather into a monospaced font suitable for code snippets.
- Make the letter R italicized.
- Change "2006 ASA Data Expo" to a link that points to http://stat-computing.org/dataexpo/2006/

The paragraph to the right describes the data that you'll use in your report. Try rendering it both before and after you make the changes below.

`@hint`
You can look up Markdown syntax in the [R Markdown Reference Guide](http://www.rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf).

`@sample_code`
{{{my_document.Rmd}}}
# Data

The atmos data set resides in the nasaweather package of the R programming language. It contains a collection of atmospheric variables measured between 1995 and 2000 on a grid of 576 coordinates in the western hemisphere. The data set comes from the 2006 ASA Data Expo.

`@solution`
{{{solution.Rmd}}}
## Data

The `atmos` data set resides in the `nasaweather` package of the *R* programming language. It contains a collection of atmospheric variables measured between 1995 and 2000 on a grid of 576 coordinates in the western hemisphere. The data set comes from the [2006 ASA Data Expo](http://stat-computing.org/dataexpo/2006/).

`@sct`
```{r}
test_error()
test_rmd_group(1, {
  test_text("## Data", format = "any", not_called_msg = "To make a second level header, use two leading hashtags.")
  test_text("atmos", format = "code", incorrect_msg = "Make sure to change the word \"atmos\" into a monospaced font using backticks.")
  test_text("nasaweather", format = "code", incorrect_msg = "Make sure to change the word \"nasaweather\" into a monospaced font using backticks.")
  test_text("R", format = "italics", incorrect_msg = "Don't forget to italicize the word \"R\".")
  test_text("2006 ASA Data Expo", format = "brackets", incorrect_msg = "Surround text with brackets `[]` to add a link to it.")
  test_text("http://stat-computing.org/dataexpo/2006/", format = "parentheses", not_called_msg = "Don't forget to add the link.", incorrect_msg = "To specify the link, use parentheses: `()`.")
})
success_msg("Good job! You're learning how to write in Markdown, the syntax language that the R Markdown package uses to create text. Head over to the next exercise to find out how to generate lists in Markdown syntax.")
```

---

## Lists in R Markdown

```yaml
type: MarkdownExercise
key: 363006d050
xp: 100
skills: 5
```

To make a bulleted list in Markdown, place each item on a new line after an asterisk and a space, like this:

    * item 1
    * item 2
    * item 3

You can make an ordered list by placing each item on a new line after a number followed by a period followed by a space, like this

    1. item 1
    2. item 2
    3. item 3

In each case, you need to place a blank line between the list and any paragraphs that come before it.

`@instructions`
We've added some text to your description on the right.

- Turn the text into a bulleted list with three bullets. Temp, pressure, and ozone should each get their own entry.
- Make temp, pressure, and ozone bold at the start of each entry.
- Make K, mb, and DU italicized at the end of each entry.

Then render your results to see the final format.

`@hint`
You can look up Markdown syntax in the [R Markdown Reference Guide](http://www.rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf).

`@sample_code`
{{{my_document.Rmd}}}
## Data

The `atmos` data set resides in the `nasaweather` package of the *R* programming language. It contains a collection of atmospheric variables measured between 1995 and 2000 on a grid of 576 coordinates in the western hemisphere. The data set comes from the [2006 ASA Data Expo](http://stat-computing.org/dataexpo/2006/).

Some of the variables in the `atmos` data set are:
temp - The mean monthly air temperature near the surface of the Earth (measured in kelvins (K))
pressure - The mean monthly air pressure at the surface of the Earth (measured in millibars (mb))
ozone - The mean monthly abundance of atmospheric ozone (measured in Dobson units (DU))

`@solution`
{{{solution.Rmd}}}
## Data

The `atmos` data set resides in the `nasaweather` package of the *R* programming language. It contains a collection of atmospheric variables measured between 1995 and 2000 on a grid of 576 coordinates in the western hemisphere. The data set comes from the [2006 ASA Data Expo](http://stat-computing.org/dataexpo/2006/).

Some of the variables in the `atmos` data set are:

* **temp** - The mean monthly air temperature near the surface of the Earth (measured in kelvins (*K*))
* **pressure** - The mean monthly air pressure at the surface of the Earth (measured in millibars (*mb*))
* **ozone** - The mean monthly abundance of atmospheric ozone (measured in Dobson units (*DU*))

`@sct`
```{r}
test_error()
test_rmd_group(1, {
  test_text("temp", format = "bold", incorrect_msg = "Make \"temp\" bold.")
  test_text("pressure", format = "bold", incorrect_msg = "Make \"temp\" bold.")
  test_text("ozone", format = "bold", incorrect_msg = "Make \"temp\" bold.")
  test_text("temp", format = c("bold","list"), incorrect_msg = "Make a list of the line starting with \"temp\".")
  test_text("pressure", format = c("bold","list"), incorrect_msg = "Make a list of the line starting with \"pressure\".")
  test_text("ozone", format = c("bold","list"), incorrect_msg = "Make a list of the line starting with \"ozone\".")
  test_text("in the `atmos` data set are:\\s*\n\n", not_called_msg = "Don't forget to leave a blank line before staring the summation. Otherwise the list will not render correctly.")
  test_text("K",format = "italics", incorrect_msg = "Italicize \"K\".")
  test_text("mb",format = "italics", incorrect_msg = "Italicize \"mb\".")
  test_text("DU",format = "italics", incorrect_msg = "Italicize \"DU\".")
})
success_msg("Great job! You can see that Markdown formatting sometimes makes the raw text easier to read as well.")

```

---

## LaTeX equations

```yaml
type: MarkdownExercise
key: 3ef910ea23
xp: 100
skills: 5
```

You can also use the Markdown syntax to embed latex math equations into your reports. To embed an equation in its own centered equation block, surround the equation with two pairs of dollar signs like this,

```
$$1 + 1 = 2$$
```

To embed an equation inline, surround it with a single pair of dollar signs, like this: `$1 + 1 = 2$`.

You can use all of the [standard latex math symbols](http://en.wikibooks.org/wiki/LaTeX/Mathematics) to create attractive equations.

`@instructions`
The text on the right contains a formula that converts degrees Celsius to degrees Fahrenheit. Where the comment is, write another formula that converts degrees Kelvin to degrees Celsius. You can convert any temperature in degrees Kelvin to a temperature in degrees Celsius by subtracting 273.15 from it. Do not capitalize Kelvin or Celsius when writing the formula. Then render your results to see the final format.

`@hint`
You can look up Markdown syntax in the [R Markdown Reference Guide](http://www.rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf).

`@sample_code`
{{{my_document.Rmd}}}
## Data

The `atmos` data set resides in the `nasaweather` package of the *R* programming language. It contains a collection of atmospheric variables measured between 1995 and 2000 on a grid of 576 coordinates in the western hemisphere. The data set comes from the [2006 ASA Data Expo](http://stat-computing.org/dataexpo/2006/).

Some of the variables in the `atmos` data set are:

* **temp** - The mean monthly air temperature near the surface of the Earth (measured in kelvins (*K*))

* **pressure** - The mean monthly air pressure at the surface of the Earth (measured in millibars (*mb*))

* **ozone** - The mean monthly abundance of atmospheric ozone (measured in Dobson units (*DU*))

You can convert the temperature unit from Kelvin to Celsius with the formula

<!-- Insert the conversion formula here -->

And you can convert the result to Fahrenheit with the formula

$$ fahrenheit = celsius \times \frac{9}{5} + 32 $$

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

`@sct`
```{r}
test_error()
test_rmd_group(1, {
  test_text("\\$\\$", freq = 4, not_called_msg = "Surround your equation with double dollar signs, `$$` to generate a LaTeX equation.")
  test_text("celsius", freq = 2, not_called_msg = "Have you used \"celsius\" in the equation? Don't capitalize the \"c\".")
  test_text("kelvin", freq = 2, not_called_msg = "Have you used \"kelvin\" in the equation? Don't capitalize the \"k\".")
  test_text("273.15", not_called_msg = "Have you used \"273.15\" in the equation?")
})
success_msg("You're getting the hang of this! You've also written a useful introduction to your report. In the next video, you'll learn how to embed R code into your report. This gives you the best of both worlds: formatted text for narration, and precise R code for reproducible analysis.")
```
