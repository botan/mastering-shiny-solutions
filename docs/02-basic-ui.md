# Basic UI

## Inputs

**E1:** When space is at a premium, it's useful to label text boxes using a placeholder that appears *inside* the text entry area. How do you call `textInput()` to generate the UI below?

<img src="images/02-basic-ui/placeholder.png" width="%40" />

**Solution:**


```r
textInput("text", "", placeholder = "Your name")
```

**E2:** Carefully read the documentation for `sliderInput()` to figure out how to create a date slider, as shown below.

<img src="images/02-basic-ui/date-slider.png" width="%40" />

**Solution:**


```r
sliderInput(
  "delivery",
  "When should we deliver?",
  min = as.Date("2020-09-16"),
  max = as.Date("2020-09-23"),
  value = as.Date("2021-09-17")
)
```

**E3:** Create a slider input to select values between 0 and 100 where the interval between each selectable value on the slider is 5. Then, add animation to the input widget so when the user presses play the input widget scrolls through the range automatically.

**Solution:**


```r
sliderInput(
  "number", 
  "Select a number:",
  min = 0, 
  max = 100, 
  value = 0, 
  step = 5, 
  animate = TRUE
)
```

**E4:** If you have a moderately long list in a `selectInput()`, it's useful to create sub-headings that break the list up into pieces. Read the documentation to figure out how. (Hint: the underlying HTML is called `<optgroup>`.)

**Solution:**


```r
selectInput(
  "movie",
  "Select your favourite action movie:",
  choices = list(
    "Chose one" = "", # placeholder
    "Guy Ritchie" = list('Lock, Stock, and Two Smoking Barrels', 'Snatch', 'RocknRolla'),
    "Quentin Tarantino" = list('Reservoir Dogs', 'Pulp Fiction', 'Kill Bill')
  )
)
```

## Outputs

**E1:** Which of `textOutput()` and `verbatimTextOutput()` should each of the following render functions be paired with?

a.  `renderPrint(summary(mtcars))`

b.  `renderText("Good morning!")`

c.  `renderPrint(t.test(1:5, 2:6))`

d.  `renderText(str(lm(mpg ~ wt, data = mtcars)))`

**Solution:**


```r
library(shiny)

ui <- fluidPage(
  verbatimTextOutput("summary"),
  textOutput("text"),
  verbatimTextOutput("t_test"),
  verbatimTextOutput("lm")
)

server <- function(input, output, session) {
  output$summary <- renderPrint(summary(mtcars))
  output$text <- renderText("Good morning!")
  output$t_test <- renderPrint(t.test(1:5, 2:6))
  output$lm <- renderPrint(str(lm(mpg ~ wt, data = mtcars)))
}

shinyApp(ui, server)
```


**E2:** Re-create the Shiny app from Section **2.3.3**, this time setting height to 300px and width to 700px. Set the plot “alt” text so that a visually impaired user can tell that its a scatterplot of five random numbers.

**Solution:**


```r
library(shiny)

ui <- fluidPage(
  plotOutput(
    "plot", 
    width = "400px", 
    height = "700px",
    alt = "A scatterplot of five random numbers."
  )
)

server <- function(input, output, session) {
  output$plot <- renderPlot(plot(1:5), res = 96)
}

shinyApp(ui, server)
```

**E3:** Update the options in the call to `renderDataTable()` below so that the data is displayed, but all other controls are suppress (i.e. remove the search, ordering, and filtering commands). You'll need to read `?renderDataTable` and review the options at <https://datatables.net/reference/option/>.


```r
ui <- fluidPage(
  dataTableOutput("table")
)

server <- function(input, output, session) {
  output$table <- renderDataTable(mtcars, options = list(pageLength = 5))
}
```

**Solution:**


```r
ui <- fluidPage(
  dataTableOutput("table")
)

server <- function(input, output, session) {
  output$table <- renderDataTable(
    mtcars, 
    options = list(
      pageLength = 5, 
      searching = FALSE,
      ordering = FALSE,
      filtering = FALSE
    )
  )
}
```

**E4:** Alternatively, read up on [reactable](https://glin.github.io/reactable), and convert the above app to use it instead.

**Solution:**


```r
library(shiny)
library(reactable)

ui <- fluidPage(
  reactableOutput("reactable")
)

server <- function(input, output, session) {
  output$reactable <- renderReactable({
    reactable(
      mtcars,
      searchable = FALSE, # default
      sortable = FALSE,
      filterable = FALSE, # default
      defaultPageSize = 5
    )
  })
}

shinyApp(ui, server)
```
