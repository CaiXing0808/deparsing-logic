---
title: 'Reactive programming'
description: 'This chapter covers the basics of reactive programming. This allows you to create and reuse objects that update dynamically in response to the inputs from the user. Using reactive programming will make your apps more efficient and complex.'
attachments:
    slides_link: 'https://s3.amazonaws.com/assets.datacamp.com/production/course_4850/slides/Chapter3.pdf'
---

## Reactive elements

```yaml
type: VideoExercise
key: 35a09b66b2
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/154783078
video_hls: //videos.datacamp.com/transcoded/4850_building_web_apps_in_r_with_shiny/v1/hls-4850_ch3_1.master.m3u8
```

`@projector_key`
ddf3de107f8b786f6aa4427ca9f33059

---

## Add reactive data frame

```yaml
type: ShinyExercise
key: a58e6f3449
lang: r
xp: 100
skills: 1
```

We ended the previous chapter with an app that allows you to download a data file with selected variables from the `movies` dataset. We will now extend this app by adding a table output of the selected data as well. Given that the same dataset will be used in two outputs, it makes sense to make our code more efficient by using a reactive data frame.

`@instructions`
- With the function `reactive()`, define `movies_selected`: a reactive expression that is a data frame with the selected variables (`input$selected_var`).
- Use the newly constructed `movies_selected()` reactive data frame (instead of `movies`) in the remainder of the app when defining `output$moviestable` and `output$download_data`.

`@hint`
- If `x` is a reactive expression, you should be referring to it as `x()`.
- This exercise involves moving code out of the original creation of `output$moviestable` and using it to define `movies_selected()`.

`@pre_exercise_code`
```{r}
library(shiny)
library(dplyr)
library(readr)
```

`@sample_code`
```{r}
library(shiny)
library(dplyr)
library(readr)
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_4850/datasets/movies.Rdata"))

# UI
ui <- fluidPage(
  sidebarLayout(
    
    # Input(s)
    sidebarPanel(
      
      # Select filetype
      radioButtons(inputId = "filetype",
                   label = "Select filetype:",
                   choices = c("csv", "tsv"),
                   selected = "csv"),
      
      # Select variables to download
      checkboxGroupInput(inputId = "selected_var",
                         label = "Select variables:",
                         choices = names(movies),
                         selected = c("title"))
      
    ),
    
    # Output(s)
    mainPanel(
      HTML("Select filetype and variables, then download and/or view the data."),
      br(), br(),
      downloadButton(outputId = "download_data", label = "Download data"),
      br(), br(),
      DT::dataTableOutput(outputId = "moviestable")
    )
  )
)

# Server
server <- function(input, output) {
  
  # Create reactive data frame
  movies_selected <- reactive({
  ___ # ensure input$selected_var is available
  ___ # select columns of movies
  })
  
  # Create data table
  output$moviestable <- DT::renderDataTable({
    req(input$selected_var)
    DT::datatable(data = movies %>% select(input$selected_var), 
                  options = list(pageLength = 10), 
                  rownames = FALSE)
  })
  
  # Download file
  output$download_data <- downloadHandler(
    filename = function() {
      paste0("movies.", input$filetype)
    },
    content = function(file) { 
      if(input$filetype == "csv"){ 
        write_csv(movies %>% select(input$selected_var), file) 
      }
      if(input$filetype == "tsv"){ 
        write_tsv(movies %>% select(input$selected_var), file) 
      }
    }
  )
  
}

# Create a Shiny app object
shinyApp(ui = ui, server = server)
```

`@solution`
```{r}
library(shiny)
library(dplyr)
library(readr)
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_4850/datasets/movies.Rdata"))

# UI
ui <- fluidPage(
  sidebarLayout(
    
    # Input(s)
    sidebarPanel(
      
      # Select filetype
      radioButtons(inputId = "filetype",
                   label = "Select filetype:",
                   choices = c("csv", "tsv"),
                   selected = "csv"),
      
      # Select variables to download
      checkboxGroupInput(inputId = "selected_var",
                         label = "Select variables:",
                         choices = names(movies),
                         selected = c("title"))
      
    ),

        
    # Output(s)
    mainPanel(
      HTML("Select filetype and variables, then download and/or view the data."),
      br(), br(),
      downloadButton(outputId = "download_data", label = "Download data"),
      br(), br(),
      DT::dataTableOutput(outputId = "moviestable")
    )
  )
)

# Server
server <- function(input, output) {
  
  # Create reactive data frame
  movies_selected <- reactive({
    req(input$selected_var)               # ensure input$selected_var is available
    movies %>% select(input$selected_var) # select columns of movies
  })
  
  # Create data table
  output$moviestable <- DT::renderDataTable({
    DT::datatable(data = movies_selected(), 
                  options = list(pageLength = 10), 
                  rownames = FALSE)
  })
  
  # Download file
  output$download_data <- downloadHandler(
    filename = function() {
      paste0("movies.", input$filetype)
    },
    content = function(file) { 
      if(input$filetype == "csv"){ 
        write_csv(movies_selected(), path = file) 
      }
      if(input$filetype == "tsv"){ 
        write_tsv(movies_selected(), path = file) 
      }
    }
  )
  
}

# Create a Shiny app object
shinyApp(ui = ui, server = server)
```

`@sct`
```{r}
ex() %>% 
    check_error()
    
ex() %>% 
    check_code("movies_selected <- reactive({", fixed = TRUE, missing_msg = "Have you correctly defined `movies_selected` as a reactive dataframe?")
test_or({
    ex() %>% 
        check_code("movies %>% select(input$selected_var)", fixed = TRUE, missing_msg = "Have you correctly selected the right variable using `select()` and `input$selected_var`?")
},{
    ex() %>% 
        check_code("select(movies, input$selected_var)", fixed = TRUE, missing_msg = "Have you correctly selected the right variable using `select()` and `input$selected_var`?")
})

ex() %>% 
    check_code("datatable(data = movies_selected()", fixed = TRUE, missing_msg = "Use the reactive dataframe to reduce duplicated code!")

ex() %>% 
    check_code("write_csv(movies_selected()", fixed = TRUE, missing_msg = "Use the reactive dataframe to reduce duplicated code!")
    
ex() %>% 
    check_code("write_tsv(movies_selected()",  fixed = TRUE, missing_msg = "Use the reactive dataframe to reduce duplicated code!")

ex() %>% 
    check_expr("ui") %>% 
    check_result() %>% 
    check_equal(incorrect_msg = "Your `ui` is wrong. Did you change any of the code that was already given to you?")
# ex() %>% 
#     check_expr("body(server)") %>% 
#     check_result() %>% 
#     check_equal(incorrect_msg = "Your `server` function is wrong. Did you change any of the code that was already given to you?")
success_msg("Well done!")
```

---

## Identify reactive objects

```yaml
type: PureMultipleChoiceExercise
key: 9cf8b2a1de
lang: r
xp: 50
skills: 1
```

The `movies_selected()` reactive expression from the video and the previous exercise is a:

`@hint`
Does it have children? Does it have parents? Does it have both?

`@possible_answers`
- Reactive source
- [Reactive conductor]
- Reactive endpoint
- Reactive paradigm

`@feedbacks`
- Is it at the beginning of the reactive flow?
- Yup!
- Is it at the end of the reactive flow?
- Is this a part of the reactive flow?

---

## Using reactives

```yaml
type: VideoExercise
key: fa4c92468a
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/154783078
video_hls: //videos.datacamp.com/transcoded/4850_building_web_apps_in_r_with_shiny/v1/hls-4850_ch3_2.master.m3u8
```

`@projector_key`
87953ce7c9c942a840b595d3f7219ecb

---

## Find missing reactives

```yaml
type: ShinyExercise
key: d5789137cf
lang: r
xp: 100
skills: 1
```

In the following app code there should be two reactive expressions: `movies_subset` and `pretty_plot_title`. Both of these are used in multiple spots in the app so we would like to define them once, and refer to them multiple times. However the sample code provided does not accomplish this goal. One problem is that some reactive expressions are not being referred to properly. And the other problem is that some reactive expressions are not being defined properly.

`@instructions`
- First make sure that all reactive expressions are being defined properly using `reactive()`.
- Then, make sure that all reactive expressions are being referred to properly, i.e. followed by `()`.

Note that you only need to make updates in the server, you should not need to change the code in the UI.

`@hint`
- `pretty_plot_title` is intended to be a reactive expression, is it defined properly?
- Both `pretty_plot_title()` and `movies_subset()` are reactive expressions, are they referred to properly?

`@pre_exercise_code`
```{r}
library(shiny)
library(ggplot2)
library(dplyr)
library(tools)
```

`@sample_code`
```{r}
library(shiny)
library(ggplot2)
library(dplyr)
library(tools)
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_4850/datasets/movies.Rdata"))

# Define UI for application that plots features of movies
ui <- fluidPage(
  
  # Application title
  titlePanel("Movie browser"),
  
  # Sidebar layout with a input and output definitions
  sidebarLayout(
    
    # Inputs(s)
    sidebarPanel(
      
      # Select variable for y-axis
      selectInput(inputId = "y", 
                  label = "Y-axis:",
                  choices = c("IMDB rating" = "imdb_rating", 
                              "IMDB number of votes" = "imdb_num_votes", 
                              "Critics Score" = "critics_score", 
                              "Audience Score" = "audience_score", 
                              "Runtime" = "runtime"), 
                  selected = "audience_score"),
      
      # Select variable for x-axis
      selectInput(inputId = "x", 
                  label = "X-axis:",
                  choices = c("IMDB rating" = "imdb_rating", 
                              "IMDB number of votes" = "imdb_num_votes", 
                              "Critics Score" = "critics_score", 
                              "Audience Score" = "audience_score", 
                              "Runtime" = "runtime"), 
                  selected = "critics_score"),
      
      # Select variable for color
      selectInput(inputId = "z", 
                  label = "Color by:",
                  choices = c("Title Type" = "title_type", 
                              "Genre" = "genre", 
                              "MPAA Rating" = "mpaa_rating", 
                              "Critics Rating" = "critics_rating", 
                              "Audience Rating" = "audience_rating"),
                  selected = "mpaa_rating"),
      
      # Enter text for plot title
      textInput(inputId = "plot_title", 
                label = "Plot title", 
                placeholder = "Enter text for plot title"),
      
      # Select which types of movies to plot
      checkboxGroupInput(inputId = "selected_type",
                         label = "Select movie type(s):",
                         choices = c("Documentary", "Feature Film", "TV Movie"),
                         selected = "Feature Film")
      
    ),
    
    # Output(s)
    mainPanel(
      plotOutput(outputId = "scatterplot"),
      textOutput(outputId = "description")
    )
  )
)

# Server
server <- function(input, output) {
  
  # Create a subset of data filtering for selected title types
  movies_subset <- reactive({
    req(input$selected_type)
    filter(movies, title_type %in% input$selected_type)
  })
  
  # Convert plot_title toTitleCase
  pretty_plot_title <- toTitleCase(input$plot_title)
  
  # Create scatterplot object the plotOutput function is expecting
  output$scatterplot <- renderPlot({
    ggplot(data = movies_subset, 
           aes_string(x = input$x, y = input$y, color = input$z)) +
      geom_point() +
      labs(title = pretty_plot_title)
  })
  
  # Create descriptive text
  output$description <- renderText({
    paste0("The plot above titled '", pretty_plot_title, "' visualizes the relationship between ", 
           input$x, " and ", input$y, ", conditional on ", input$z, ".")
  })
  
}

# Create the Shiny app object
shinyApp(ui = ui, server = server)
```

`@solution`
```{r}
library(shiny)
library(ggplot2)
library(dplyr)
library(tools)
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_4850/datasets/movies.Rdata"))

# Define UI for application that plots features of movies
ui <- fluidPage(
  
  # Application title
  titlePanel("Movie browser"),
  
  # Sidebar layout with a input and output definitions
  sidebarLayout(
    
    # Inputs(s)
    sidebarPanel(
      
      # Select variable for y-axis
      selectInput(inputId = "y", 
                  label = "Y-axis:",
                  choices = c("IMDB rating" = "imdb_rating", 
                              "IMDB number of votes" = "imdb_num_votes", 
                              "Critics Score" = "critics_score", 
                              "Audience Score" = "audience_score", 
                              "Runtime" = "runtime"), 
                  selected = "audience_score"),
      
      # Select variable for x-axis
      selectInput(inputId = "x", 
                  label = "X-axis:",
                  choices = c("IMDB rating" = "imdb_rating", 
                              "IMDB number of votes" = "imdb_num_votes", 
                              "Critics Score" = "critics_score", 
                              "Audience Score" = "audience_score", 
                              "Runtime" = "runtime"), 
                  selected = "critics_score"),
      
      # Select variable for color
      selectInput(inputId = "z", 
                  label = "Color by:",
                  choices = c("Title Type" = "title_type", 
                              "Genre" = "genre", 
                              "MPAA Rating" = "mpaa_rating", 
                              "Critics Rating" = "critics_rating", 
                              "Audience Rating" = "audience_rating"),
                  selected = "mpaa_rating"),
      
      # Enter text for plot title
      textInput(inputId = "plot_title", 
                label = "Plot title", 
                placeholder = "Enter text for plot title"),
      
      # Select which types of movies to plot
      checkboxGroupInput(inputId = "selected_type",
                         label = "Select movie type(s):",
                         choices = c("Documentary", "Feature Film", "TV Movie"),
                         selected = "Feature Film")
      
    ),
    
    # Output(s)
    mainPanel(
      plotOutput(outputId = "scatterplot"),
      textOutput(outputId = "description")
    )
  )
)

# Server
server <- function(input, output) {
  
  # Create a subset of data filtering for selected title types
  movies_subset <- reactive({
    req(input$selected_type)
    filter(movies, title_type %in% input$selected_type)
  })
  
  # Convert plot_title toTitleCase
  pretty_plot_title <- reactive({ toTitleCase(input$plot_title) })
  
  # Create scatterplot object the plotOutput function is expecting
  output$scatterplot <- renderPlot({
    ggplot(data = movies_subset(), 
           aes_string(x = input$x, y = input$y, color = input$z)) +
      geom_point() +
      labs(title = pretty_plot_title())
  })
  
  # Create descriptive text
  output$description <- renderText({
    paste0("The plot above titled '", pretty_plot_title(), "' visualizes the relationship between ", 
           input$x, " and ", input$y, ", conditional on ", input$z, ".")
  })
  
}

# Create the Shiny app object
shinyApp(ui = ui, server = server)
```

`@sct`
```{r}
ex() %>% 
    check_code("reactive({ toTitleCase(input$plot_title) })", fixed = TRUE, missing_msg = "It looks like you're still missing a reactive.")

ex() %>% 
    check_code("data = movies_subset()", fixed = TRUE, missing_msg = "It looks like you're still missing some parentheses.")

ex() %>% 
    check_code("title = pretty_plot_title()", fixed = TRUE, missing_msg = "It looks like you're still missing some parentheses.")

ex() %>% 
    check_code("pretty_plot_title()", fixed = TRUE, times = 2, missing_msg = "It looks like you're still missing some parentheses.")
    
ex() %>% 
    check_expr("ui") %>% 
    check_result() %>% 
    check_equal(incorrect_msg = "Your `ui` is wrong. Did you change any of the code that was already given to you?")

success_msg("You're getting the hang of reactives!")
```

---

## Find inconsistencies in what the app is reporting

```yaml
type: ShinyExercise
key: 5d6d22886d
lang: r
xp: 100
skills: 1
```

This exercise features an app where the user can select a random sample of desired title types. The app then reports the frequencies of various title types in the sample and plots user selected variables for these data. There are two new functions of note in the server: `observeEvent()` and `updateNumericInput()`. We use them to update the numeric input widget to display a maximum allowed sample size based on the selected title types. For example, if you only select "TV Movie", the maximum allowed sample size is 5, because there are only 5 TV movies in the sample. We'll learn more about `observeEvent()` later in the course. For now, your task is simple: find the mismatched use of reactives.

`@instructions`
- Run the sample code and view the app. (1) Does the sample size the user inputs match the sample size displayed in the text on top of the app? (2) Does it match the counts displayed in the title type frequency table? (3) How about the number of points plotted?
- If your answers to any of the three questions above is no, debug the app code accordingly.
- Run the app code again to verify that your answer to all three questions above is yes.

`@hint`
You only need to make updates in the server, in the definitions of `output$sample_tt_info` and `output$scatterplot`.

`@pre_exercise_code`
```{r}
library(shiny)
library(ggplot2)
library(dplyr)
```

`@sample_code`
```{r}
library(shiny)
library(ggplot2)
library(dplyr)
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_4850/datasets/movies.Rdata"))

# UI
ui <- fluidPage(
  sidebarLayout(
    
    # Input(s)
    sidebarPanel(
      
      # Select which types of movies to plot
      checkboxGroupInput(inputId = "selected_type",
                         label = "Select movie type(s):",
                         choices = c("Documentary", "Feature Film", "TV Movie"),
                         selected = "Feature Film"),
      
      # Select sample size
      numericInput(inputId = "n_samp", 
                   label = "Sample size:", 
                   min = 1, max = nrow(movies), 
                   value = 3),
      
      # Select variable for y-axis
      selectInput(inputId = "y", 
                  label = "Y-axis:",
                  choices = c("IMDB rating" = "imdb_rating", 
                              "IMDB number of votes" = "imdb_num_votes", 
                              "Critics Score" = "critics_score", 
                              "Audience Score" = "audience_score", 
                              "Runtime" = "runtime"), 
                  selected = "audience_score"),
      
      # Select variable for x-axis
      selectInput(inputId = "x", 
                  label = "X-axis:",
                  choices = c("IMDB rating" = "imdb_rating", 
                              "IMDB number of votes" = "imdb_num_votes", 
                              "Critics Score" = "critics_score", 
                              "Audience Score" = "audience_score", 
                              "Runtime" = "runtime"), 
                  selected = "critics_score")

    ),
    
    # Output(s)
    mainPanel(
      htmlOutput(outputId = "sample_info"),
      br(), br(),
      tableOutput(outputId = "sample_tt_info"),
      br(),
      plotOutput(outputId = "scatterplot")
    )
  )
)

# Server
server <- function(input, output, session) {
  
  # Filter movies for selected title types
  movies_selected <- reactive({
    req(input$selected_type)
    filter(movies, title_type %in% input$selected_type)
  })
  
  # Update max allowed sample size in the numeric input widget
  # when input$selected_type changes
  observeEvent(input$selected_type, {
    # Calculate max sample size
    n_max <- nrow(movies_selected())
    # Update max value widget accepts
    updateNumericInput(session, "n_samp", max = n_max)
    # Update label displayed to user
    updateNumericInput(session, "n_samp", 
                       label = paste0("Sample size (max = ", n_max, "):"))
  })
  
  # Sample n_samp movies of selected title types
  movies_sample <- reactive({ 
    req(input$n_samp)
    sample_n(movies_selected(), input$n_samp)
  })
  
  # Display number of sampled movies
  output$sample_info <- renderUI({
    paste(input$n_samp,
          "movies are sampled. The table below shows the frequencies of title 
          types in your sample.")
  })
  
  # Tabulate frequencies of types of sampled movies
  output$sample_tt_info <- renderTable({
    movies_selected() %>% 
      count(title_type) %>%
      rename(`Title type` = title_type, Frequency = n)
  })
  
  # Plot sampled movies
  output$scatterplot <- renderPlot({
    ggplot(data = movies_selected(), aes_string(x = input$x, y = input$y)) +
      geom_point()
  })
  
}

# Create a Shiny app object
shinyApp(ui = ui, server = server)
```

`@solution`
```{r}
library(shiny)
library(ggplot2)
library(dplyr)
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_4850/datasets/movies.Rdata"))

# UI
ui <- fluidPage(
  sidebarLayout(
    
    # Input(s)
    sidebarPanel(
      
      # Select which types of movies to plot
      checkboxGroupInput(inputId = "selected_type",
                         label = "Select movie type(s):",
                         choices = c("Documentary", "Feature Film", "TV Movie"),
                         selected = "Feature Film"),
      
      # Select sample size
      numericInput(inputId = "n_samp", 
                   label = "Sample size:", 
                   min = 1, max = nrow(movies), 
                   value = 3),
      
      # Select variable for y-axis
      selectInput(inputId = "y", 
                  label = "Y-axis:",
                  choices = c("IMDB rating" = "imdb_rating", 
                              "IMDB number of votes" = "imdb_num_votes", 
                              "Critics Score" = "critics_score", 
                              "Audience Score" = "audience_score", 
                              "Runtime" = "runtime"), 
                  selected = "audience_score"),
      
      # Select variable for x-axis
      selectInput(inputId = "x", 
                  label = "X-axis:",
                  choices = c("IMDB rating" = "imdb_rating", 
                              "IMDB number of votes" = "imdb_num_votes", 
                              "Critics Score" = "critics_score", 
                              "Audience Score" = "audience_score", 
                              "Runtime" = "runtime"), 
                  selected = "critics_score")

    ),
    
    # Output(s)
    mainPanel(
      htmlOutput(outputId = "sample_info"),
      br(), br(),
      tableOutput(outputId = "sample_tt_info"),
      br(),
      plotOutput(outputId = "scatterplot")
    )
  )
)

# Server
server <- function(input, output, session) {
  
  # Filter movies for selected title types
  movies_selected <- reactive({
    req(input$selected_type)
    filter(movies, title_type %in% input$selected_type)
  })
  
  # Update max allowed sample size in the numeric input widget
  # when input$selected_type changes
  observeEvent(input$selected_type, {
    # Calculate max sample size
    n_max <- nrow(movies_selected())
    # Update max value widget accepts
    updateNumericInput(session, "n_samp", max = n_max)
    # Update label displayed to user
    updateNumericInput(session, "n_samp", 
                       label = paste0("Sample size (max = ", n_max, "):"))
  })
  
  # Sample n_samp movies of selected title types
  movies_sample <- reactive({ 
    req(input$n_samp)
    sample_n(movies_selected(), input$n_samp)
  })
  
  # Display number of sampled movies
  output$sample_info <- renderUI({
    paste(input$n_samp,
          "movies are sampled. The table below shows the frequencies of title 
          types in your sample.")
  })
  
  # Tabulate frequencies of types of sampled movies
  output$sample_tt_info <- renderTable({
    movies_sample() %>% 
      count(title_type) %>%
      rename(`Title type` = title_type, Frequency = n)
  })
  
  # Plot sampled movies
  output$scatterplot <- renderPlot({
    ggplot(data = movies_sample(), aes_string(x = input$x, y = input$y)) +
      geom_point()
  })
  
}

# Create a Shiny app object
shinyApp(ui = ui, server = server)
```

`@sct`
```{r}
# i can't find this code anywhere in this exercise: types <- 
#ex() %>% 
#    check_code("types <- movies_sample()", fixed = TRUE, missing_msg = "Make sure you're using the right data frame to calculate the number of each type of movie.")

ex() %>% 
    check_expr("ui") %>% 
    check_result() %>% 
    check_equal(incorrect_msg = "Your `ui` is wrong. Did you change any of the code that was already given to you?")
    
ex() %>% 
    check_expr("body(server)") %>% 
    check_result() %>% 
    check_equal(incorrect_msg = "Your `server` function is wrong. Did you change any of the code that was already given to you?")
success_msg("Great job!")
```

---

## Reactives and observers

```yaml
type: VideoExercise
key: 90078af43d
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/154783078
video_hls: //videos.datacamp.com/transcoded/4850_building_web_apps_in_r_with_shiny/v1/hls-4850_ch3_3.master.m3u8
```

`@projector_key`
fc400ae9baf780525781777174bc9160

---

## Does this have a side effect? (1)

```yaml
type: MultipleChoiceExercise
key: ececae42bc
lang: r
xp: 50
skills: 1
```

Which of the following does **not** have a side effect?

`@possible_answers`
- `value <<- 10`
- `source("functions.R")`
- `library(dplyr)`
- `2 * 3`

`@hint`
Any effect of a function that is not the return value is a side effect.

`@pre_exercise_code`
```{r}
library(shiny)
```

`@sct`
```{r}
msg1 <- "This sets a variable in a parent environment"
msg2 <- "This loads into global environment by default"
msg3 <- "This modifies the global search list"
msg4 <- "Yup!"
test_mc(4, feedback_msgs = c(msg1, msg2, msg3, msg4))
```

---

## Does this have a side effect? (2)

```yaml
type: MultipleChoiceExercise
key: 2bb7b355a2
lang: r
xp: 50
skills: 1
```

Which of the following functions has a side effect?

`@possible_answers`
- `function(a, b) { (b - a) / a }`
- `function(values) { hist(values, plot = TRUE) }`
- `function() { readLines("~/data/raw.txt") }`
- `function(x) { summary(x) }`

`@hint`
Any effect of a function that is not the return value is a side effect.

`@pre_exercise_code`
```{r}
library(shiny)
```

`@sct`
```{r}
msg1 <- "Most calculations don't have side effects"
msg2 <- "Creates a plot"
msg3 <- "Reads a file but doesn't save it"
msg4 <- "Most calculations don't have side effects"
test_mc(2, feedback_msgs = c(msg1, msg2, msg3, msg4))
```

---

## Stop - trigger - delay

```yaml
type: VideoExercise
key: 64eabaea12
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/154783078
video_hls: //videos.datacamp.com/transcoded/4850_building_web_apps_in_r_with_shiny/v1/hls-4850_ch3_4.master.m3u8
```

`@projector_key`
c5e688291c7e6571bb03a608498cd5e2

---

## Stop with isolate()

```yaml
type: ShinyExercise
key: e15b4412be
lang: r
xp: 100
skills: 1
```

The `isolate()` function takes one argument, `expr`, which is an expression that can access reactive values or expressions. And this function serves a very precise purpose: it executes `expr` in a scope where reactive values or expression can be read, but they do not trigger an output that depends on them to be re-evaluated. However if that output depends on other reactives as well, when one of those changes, the output is re-evaluated with the new value of the isolated `expr`.

In this app we want to isolate the plot title, such that the plot is updated with the new plot title only when other inputs to the plot change.

`@instructions`
- Run the code and test out the functionality of the plot title input. Is the plot title updated immediately after you're done typing the title?
- Modify the app using `isolate()` so that the plot title *only* gets updated when one of the other inputs is changed. Note that it's best practice to place the argument of the `isolate` function in curly braces.

`@hint`
Isolate only the title, not the `ggplot` layer that the title appears in.

`@pre_exercise_code`
```{r}
library(shiny)
library(ggplot2)
library(tools)
```

`@sample_code`
```{r}
library(shiny)
library(ggplot2)
library(tools)
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_4850/datasets/movies.Rdata"))

# UI
ui <- fluidPage(
  sidebarLayout(
    
    # Input
    sidebarPanel(
      
      # Enter text for plot title
      textInput(inputId = "plot_title", 
                label = "Plot title", 
                placeholder = "Enter text to be used as plot title"),
      
      # Select variable for y-axis
      selectInput(inputId = "y", 
                  label = "Y-axis:",
                  choices = c("IMDB rating" = "imdb_rating", 
                              "IMDB number of votes" = "imdb_num_votes", 
                              "Critics Score" = "critics_score", 
                              "Audience Score" = "audience_score", 
                              "Runtime" = "runtime"), 
                  selected = "audience_score"),
      
      # Select variable for x-axis
      selectInput(inputId = "x", 
                  label = "X-axis:",
                  choices = c("IMDB rating" = "imdb_rating", 
                              "IMDB number of votes" = "imdb_num_votes", 
                              "Critics Score" = "critics_score", 
                              "Audience Score" = "audience_score", 
                              "Runtime" = "runtime"), 
                  selected = "critics_score"),
      
      # Select variable for color
      selectInput(inputId = "z", 
                  label = "Color by:",
                  choices = c("Title Type" = "title_type", 
                              "Genre" = "genre", 
                              "MPAA Rating" = "mpaa_rating", 
                              "Critics Rating" = "critics_rating", 
                              "Audience Rating" = "audience_rating"),
                  selected = "mpaa_rating"),
      
      # Set alpha level
      sliderInput(inputId = "alpha", 
                  label = "Alpha:", 
                  min = 0, max = 1, 
                  value = 0.5),
      
      # Set point size
      sliderInput(inputId = "size", 
                  label = "Size:", 
                  min = 0, max = 5, 
                  value = 2)
      
    ),
    
    # Output:
    mainPanel(
      plotOutput(outputId = "scatterplot")
    )
  )
)

# Define server function required to create the scatterplot-
server <- function(input, output, session) {
  
  # Create scatterplot object the plotOutput function is expecting 
  output$scatterplot <- renderPlot({
    ggplot(data = movies, aes_string(x = input$x, y = input$y, color = input$z)) +
      geom_point(alpha = input$alpha, size = input$size) +
      labs( title = input$plot_title )
  })
  
}

# Create a Shiny app object
shinyApp(ui = ui, server = server)
```

`@solution`
```{r}
library(shiny)
library(ggplot2)
library(tools)
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_4850/datasets/movies.Rdata"))

# UI
ui <- fluidPage(
  sidebarLayout(
    
    # Input
    sidebarPanel(
      
      # Enter text for plot title
      textInput(inputId = "plot_title", 
                label = "Plot title", 
                placeholder = "Enter text to be used as plot title"),
      
      # Select variable for y-axis
      selectInput(inputId = "y", 
                  label = "Y-axis:",
                  choices = c("IMDB rating" = "imdb_rating", 
                              "IMDB number of votes" = "imdb_num_votes", 
                              "Critics Score" = "critics_score", 
                              "Audience Score" = "audience_score", 
                              "Runtime" = "runtime"), 
                  selected = "audience_score"),
      
      # Select variable for x-axis
      selectInput(inputId = "x", 
                  label = "X-axis:",
                  choices = c("IMDB rating" = "imdb_rating", 
                              "IMDB number of votes" = "imdb_num_votes", 
                              "Critics Score" = "critics_score", 
                              "Audience Score" = "audience_score", 
                              "Runtime" = "runtime"), 
                  selected = "critics_score"),
      
      # Select variable for color
      selectInput(inputId = "z", 
                  label = "Color by:",
                  choices = c("Title Type" = "title_type", 
                              "Genre" = "genre", 
                              "MPAA Rating" = "mpaa_rating", 
                              "Critics Rating" = "critics_rating", 
                              "Audience Rating" = "audience_rating"),
                  selected = "mpaa_rating"),
      
      # Set alpha level
      sliderInput(inputId = "alpha", 
                  label = "Alpha:", 
                  min = 0, max = 1, 
                  value = 0.5),
      
      # Set point size
      sliderInput(inputId = "size", 
                  label = "Size:", 
                  min = 0, max = 5, 
                  value = 2)
      
    ),
    
    # Output:
    mainPanel(
      plotOutput(outputId = "scatterplot")
    )
  )
)

# Define server function required to create the scatterplot-
server <- function(input, output, session) {
  
  # Create scatterplot object the plotOutput function is expecting 
  output$scatterplot <- renderPlot({
    ggplot(data = movies, aes_string(x = input$x, y = input$y, color = input$z)) +
      geom_point(alpha = input$alpha, size = input$size) +
      labs(title = isolate({ input$plot_title }) )
  })
  
}

# Create a Shiny app object
shinyApp(ui = ui, server = server)
```

`@sct`
```{r}
ex() %>% 
    check_fun_def("server") %>% 
    check_body() %>% 
    check_function("isolate") %>% 
    check_arg("expr")

ex() %>%
    check_code("title = isolate({ input$plot_title })", fixed = TRUE, missing_msg = "Make sure you're correctly `isolate()`-ing the output of the function `toTitleCase()`.")
    
ex() %>% 
    check_expr("ui") %>% 
    check_result() %>% 
    check_equal(incorrect_msg = "Your `ui` is wrong. Did you change any of the code that was already given to you?")
    
ex() %>% 
    check_expr("body(server)") %>% 
    check_result() %>% 
    check_equal(incorrect_msg = "Your `server` function is wrong. Did you change any of the code that was already given to you?")
```

---

## Delay with eventReactive();

```yaml
type: ShinyExercise
key: bd8479db4f
lang: r
xp: 100
skills: 1
```

In this exercise we introduce an action button that will be used to update the plot title when the button is clicked. This is an example of delaying a reaction (until the button is clicked), which we can accomplish with the `eventReactive()` function.

`eventReactive()` takes two main arguments: The first (`eventExpr`) is what to condition on, and the second (`valueExpr`) is what should happen when the event expression is  happens.

`@instructions`
- In the UI: Add an `actionButton`, with input ID `"update_plot_title"` and label `"Update plot title"` to the UI that will be used to update the title only when the button is clicked.
- In the server: Use an `eventReactive()` to create a new reactive expression called `new_plot_title`. This expression should get created when the action button is clicked (`input$update_plot_title` is `TRUE`), and when that happens, the plot title will be reformatted to Title Case (first letter of each word capitalized) with the `toTitleCase()` function.
- Run the code, and try out the action button functionality. You should note that the plot doesn't show up when the app first launches.
- Take a look at the help for `eventReactive()`, specifically the argument `ignoreNULL`. Set `ignoreNULL` to the appropriate value (`TRUE` or `FALSE`) so that the app displays the plot upon launch.

`@hint`
Your final answer should include a call to `eventReactive` of the following form:

```{r}
new_plot_title <- eventReactive(
  eventExpr = ___, 
  valueExpr = { ___ },
  ignoreNULL = ___
  )
```

and for the `ignoreNULL` argument, use the full world `FALSE`.

`@pre_exercise_code`
```{r}
library(shiny)
library(ggplot2)
library(tools)
```

`@sample_code`
```{r}
library(shiny)
library(ggplot2)
library(tools)
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_4850/datasets/movies.Rdata"))

# UI
ui <- fluidPage(
  sidebarLayout(
    
    # Input
    sidebarPanel(
      
      # Enter text for plot title
      textInput(inputId = "plot_title", 
                label = "Plot title", 
                placeholder = "Enter text to be used as plot title"),
      
      # Action button for plot title
      ___(inputId = ___, 
          label = ___),
      
      # Visual separation
      hr(),
      
      # Select variable for y-axis
      selectInput(inputId = "y", 
                  label = "Y-axis:",
                  choices = c("IMDB rating" = "imdb_rating", 
                              "IMDB number of votes" = "imdb_num_votes", 
                              "Critics Score" = "critics_score", 
                              "Audience Score" = "audience_score", 
                              "Runtime" = "runtime"), 
                  selected = "audience_score"),
      
      # Select variable for x-axis
      selectInput(inputId = "x", 
                  label = "X-axis:",
                  choices = c("IMDB rating" = "imdb_rating", 
                              "IMDB number of votes" = "imdb_num_votes", 
                              "Critics Score" = "critics_score", 
                              "Audience Score" = "audience_score", 
                              "Runtime" = "runtime"), 
                  selected = "critics_score"),
      
      # Select variable for color
      selectInput(inputId = "z", 
                  label = "Color by:",
                  choices = c("Title Type" = "title_type", 
                              "Genre" = "genre", 
                              "MPAA Rating" = "mpaa_rating", 
                              "Critics Rating" = "critics_rating", 
                              "Audience Rating" = "audience_rating"),
                  selected = "mpaa_rating"),
      
      # Set alpha level
      sliderInput(inputId = "alpha", 
                  label = "Alpha:", 
                  min = 0, max = 1, 
                  value = 0.5),
      
      # Set point size
      sliderInput(inputId = "size", 
                  label = "Size:", 
                  min = 0, max = 5, 
                  value = 2)
      
    ),
    
    # Output:
    mainPanel(
      plotOutput(outputId = "scatterplot")
    )
  )
)

# Define server function required to create the scatterplot-
server <- function(input, output, session) {
  
  # New plot title
  new_plot_title <- ___(
    eventExpr = ___, 
    valueExpr = { ___ }
    )
  
  # Create scatterplot object the plotOutput function is expecting 
  output$scatterplot <- renderPlot({
    ggplot(data = movies, aes_string(x = input$x, y = input$y, color = input$z)) +
      geom_point(alpha = input$alpha, size = input$size) +
      labs(title = new_plot_title())
  })
  
}

# Create a Shiny app object
shinyApp(ui = ui, server = server)
```

`@solution`
```{r}
library(shiny)
library(ggplot2)
library(tools)
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_4850/datasets/movies.Rdata"))

# UI
ui <- fluidPage(
  sidebarLayout(
    
    # Input
    sidebarPanel(
      
      # Enter text for plot title
      textInput(inputId = "plot_title", 
                label = "Plot title", 
                placeholder = "Enter text to be used as plot title"),
      
      # Action button for plot title
      actionButton(inputId = "update_plot_title", 
                   label = "Update plot title"),
      
      # Visual separation
      hr(),
      
      # Select variable for y-axis
      selectInput(inputId = "y", 
                  label = "Y-axis:",
                  choices = c("IMDB rating" = "imdb_rating", 
                              "IMDB number of votes" = "imdb_num_votes", 
                              "Critics Score" = "critics_score", 
                              "Audience Score" = "audience_score", 
                              "Runtime" = "runtime"), 
                  selected = "audience_score"),
      
      # Select variable for x-axis
      selectInput(inputId = "x", 
                  label = "X-axis:",
                  choices = c("IMDB rating" = "imdb_rating", 
                              "IMDB number of votes" = "imdb_num_votes", 
                              "Critics Score" = "critics_score", 
                              "Audience Score" = "audience_score", 
                              "Runtime" = "runtime"), 
                  selected = "critics_score"),
      
      # Select variable for color
      selectInput(inputId = "z", 
                  label = "Color by:",
                  choices = c("Title Type" = "title_type", 
                              "Genre" = "genre", 
                              "MPAA Rating" = "mpaa_rating", 
                              "Critics Rating" = "critics_rating", 
                              "Audience Rating" = "audience_rating"),
                  selected = "mpaa_rating"),
      
      # Set alpha level
      sliderInput(inputId = "alpha", 
                  label = "Alpha:", 
                  min = 0, max = 1, 
                  value = 0.5),
      
      # Set point size
      sliderInput(inputId = "size", 
                  label = "Size:", 
                  min = 0, max = 5, 
                  value = 2)
      
    ),
    
    # Output:
    mainPanel(
      plotOutput(outputId = "scatterplot")
    )
  )
)

# Define server function required to create the scatterplot-
server <- function(input, output, session) {
  
  # New plot title
  new_plot_title <- eventReactive(
    eventExpr = input$update_plot_title, 
    valueExpr = { toTitleCase(input$plot_title) },
    ignoreNULL = FALSE
  )
  
  # Create scatterplot object the plotOutput function is expecting 
  output$scatterplot <- renderPlot({
    ggplot(data = movies, aes_string(x = input$x, y = input$y, color = input$z)) +
      geom_point(alpha = input$alpha, size = input$size) +
      labs(title = new_plot_title())
  })
  
}

# Create a Shiny app object
shinyApp(ui = ui, server = server)
```

`@sct`
```{r}
ex() %>% 
    check_error()
    
server_body <- ex() %>% 
    check_fun_def("server") %>% 
    check_body()

server_body  %>% 
    check_function("eventReactive") %>% 
    {
    check_arg(., "eventExpr") %>% 
    check_equal(eval = FALSE)
    
    check_arg(., "valueExpr") %>% 
    check_equal(eval = FALSE)
    }

ex() %>% 
    check_expr("ui") %>% 
    check_result() %>% 
    check_equal(incorrect_msg = "Your `ui` is wrong. Did you change any of the code that was already given to you?")
    
ex() %>%
    check_expr("body(server)") %>%
    check_result() %>%
    check_equal(incorrect_msg = "Your `server` function is wrong. Did you change any of the code that was already given to you?")

#check that server is defined
# ex() %>% check_fun_def("server")

# ex() %>% check_function("eventReactive") %>% {
#     check_arg(., "eventExpr") %>% check_equal(eval = FALSE)
#     check_arg(., "valueExpr") %>% check_equal(eval = FALSE)
#     check_arg(., "ignoreNULL") %>% check_equal()
# }
```

---

## Trigger with observeEvent()

```yaml
type: ShinyExercise
key: 84adc94f29
lang: r
xp: 100
skills: 1
```

In this app we want two things to happen when an action button is clicked: 

- A message printed to the console stating how many records are shown. 
- A table output of those records.

While `observeEvent()` will print a message to the console when the action button is clicked on your local machine, it will not currently work on the DataCamp platform. It is important, however, to learn how to do this for your future Shiny apps!

`@instructions`
- Use `observeEvent()` to print a message to the console when the action button is clicked.
- Set up a table output that will print only when action button is clicked, but not when other inputs that go into the creation of that output changes. Note that the corresponding render function for `tableOutput()` is `renderTable()`.

`@hint`
`observeEvent()` takes two arguments:

- First argument is the event to condition on.
- The second argument is the action that should happen when the event in the first argument happens. Place this in curly braces.

`@pre_exercise_code`
```{r}
library(shiny)
options("shiny.sanitize.errors" = FALSE) # Turn off error sanitization
```

`@sample_code`
```{r}
library(shiny)
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_4850/datasets/movies.Rdata"))

# UI
ui <- fluidPage(
  sidebarLayout(
    
    # Input
    sidebarPanel(
      
      # Numeric input for number of rows to show
      numericInput(inputId = "n_rows",
                   label = "How many rows do you want to see?",
                   value = 10),
      
      # Action button to show
      actionButton(inputId = "button", 
                   label = "Show")
      
    ),
    
    # Output:
    mainPanel(
      tableOutput(outputId = "datatable")
    )
  )
)

# Define server function required to create the scatterplot-
server <- function(input, output, session) {

  # Print a message to the console every time button is pressed
  ___(input$___, {
    cat("Showing", input$n_rows, "rows\n")
  })
  
  # Take a reactive dependency on input$button, but not on any other inputs
  df <- ___(input$___, {
    head(movies, input$n_rows)
  })
  output$___ <- ___({
    df()
  })
  
}

# Create a Shiny app object
shinyApp(ui = ui, server = server)
```

`@solution`
```{r}
library(shiny)
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_4850/datasets/movies.Rdata"))

# UI
ui <- fluidPage(
  sidebarLayout(
    
    # Input
    sidebarPanel(
      
      # Numeric input for number of rows to show
      numericInput(inputId = "n_rows",
                   label = "How many rows do you want to see?",
                   value = 10),
      
      # Action button to show
      actionButton(inputId = "button", 
                   label = "Show")
      
    ),
    
    # Output:
    mainPanel(
      tableOutput(outputId = "datatable")
    )
  )
)

# Define server function required to create the scatterplot-
server <- function(input, output, session) {

  # Print a message to the console every time button is pressed
  observeEvent(input$button, {
    cat("Showing", input$n_rows, "rows\n")
  })
  # Take a reactive dependency on input$button, 
  # but not on any of the stuff inside the function
  df <- eventReactive(input$button, {
    head(movies, input$n_rows)
  })
  output$datatable <- renderTable({
    df()
  })
  
}

# Create a Shiny app object
shinyApp(ui = ui, server = server)
```

`@sct`
```{r}
server_body <- ex() %>% 
    check_fun_def("server") %>% 
    check_body()
    
server_body %>% 
    check_function("observeEvent") %>% 
    {
    check_arg(., "eventExpr") %>% 
    check_equal(eval = FALSE)
    
    check_arg(., "handlerExpr") %>% 
    check_equal(eval = FALSE)
    }

    
server_body %>% 
    check_function("eventReactive") %>% 
    {
    check_arg(., "eventExpr") %>% 
    check_equal(eval = FALSE)
    
    check_arg(., "valueExpr") %>% 
    check_equal(eval = FALSE)
    }
    
server_body %>% 
    check_code("output$datatable", missing_msg = "Make sure you've correctly defined the datatable output.", fixed = TRUE)

server_body %>% 
    check_code("renderTable({df()})", fixed = TRUE, missing_msg = "Make sure you've correctly defined the datatable output.")
    
ex() %>% 
    check_expr("ui") %>% 
    check_result() %>% 
    check_equal(incorrect_msg = "Your `ui` is wrong. Did you change any of the code that was already given to you?")

ex() %>% 
    check_expr("body(server)") %>% 
    check_result() %>% 
    check_equal(incorrect_msg = "Your `server` function is wrong. Did you change any of the code that was already given to you?")
```

---

## eventReactive() vs observeEvent()

```yaml
type: MultipleChoiceExercise
key: 37f9800b98
lang: r
xp: 50
skills: 1
```

Which of the following is false?

See the help for `eventReactive()` and `observeEvent()`, especially the Details section.

`@possible_answers`
- `observeEvent()` is used to perform an action in response to an event
- `isolate()` is used to trigger a reaction
- `eventReactive()` is used to create a calculated value that only updates in response to an event
- Recalculating a value does not generally count as performing an action

`@hint`
See the help documentation for more info on each of these functions.

`@pre_exercise_code`
```{r}
library(shiny)
```

`@sct`
```{r}
test_mc(2)
```

---

## Reactivity recap

```yaml
type: VideoExercise
key: 58817ab0e8
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/154783078
video_hls: //videos.datacamp.com/transcoded/4850_building_web_apps_in_r_with_shiny/v1/hls-4850_ch3_5.master.m3u8
```

`@projector_key`
8ab9ea795caf064d0e4bc96b7f2df312

---

## What's wrong?

```yaml
type: ShinyExercise
key: 2ccaefafe1
lang: r
xp: 100
skills: 1
```

This app adds 2 to the value provided by the user via the slider. However, currently it's buggy and not working as intended.

`@instructions`
- Run the sample code provided, and review the error message.
- Implement the fix in the definition of `output$x_updated`
- Run the app code again to verify that the app works as expected.

`@hint`
`current_x` should be reactive.

`@pre_exercise_code`
```{r}
library(shiny)
options("shiny.sanitize.errors" = FALSE) # Turn off error sanitization
```

`@sample_code`
```{r}
library(shiny)

add_2 <- function(x) { x + 2 }

ui <- fluidPage(
  titlePanel("Add 2"),
  sidebarLayout(
    sidebarPanel( sliderInput("x", "Select x", min = 1, max = 50, value = 30) ),
    mainPanel( textOutput("x_updated") )
  )
)

server <- function(input, output) {
  # Add 2 to input$x, save as a reactive expression
  current_x        <- reactive({ add_2(input$x) })
  
  # Render the current result of input$x + 2
  output$x_updated <- renderText({ current_x })
}

shinyApp(ui, server)
```

`@solution`
```{r}
library(shiny)

add_2 <- function(x) { x + 2 }

ui <- fluidPage(
  titlePanel("Add 2"),
  sidebarLayout(
    sidebarPanel( sliderInput("x", "Select x", min = 1, max = 50, value = 30) ),
    mainPanel( textOutput("x_updated") )
  )
)

server <- function(input, output) {
  # Add 2 to input$x, save as a reactive expression
  current_x        <- reactive({ add_2(input$x) })
  
  # Render the current result of input$x + 2
  output$x_updated <- renderText({ current_x() })
}

shinyApp(ui, server)
```

`@sct`
```{r}
ex() %>% 
    check_error()
    
server_body <- ex() %>% 
    check_fun_def("server") %>% 
    check_body()
    
server_body %>% 
    check_function("reactive")

server_body %>% 
    check_code("current_x <- reactive({ add_2(input$x) })", fixed = TRUE, missing_msg = "Double check the argument to your `reactive()` call")

server_body %>% 
    check_code("current_x()", fixed = TRUE, missing_msg = "You seem to be missing some parentheses.")

success_msg("Great job on reactivity, now on to customizing the appearance of your app!")
```
