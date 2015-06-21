---
title       : Find the fattest chick
subtitle    : cluck cluck
author      : Jure Bordon
job         : PhD Student
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Overview

1. This is a presentation for a very simple shiny app, ...
2. ... but it is so much fun!
3. Try it out and find the fattest chick around!
4. Dataset used in this app:
    - We used a the ChickWeight dataset to build this app
    - This dataset has weight measurements of different chicks (50) which were on four different diets
    - In order to determine which of the diets cause the most weight gained, we need to find the fattest chick!
5. Link to the app: https://jurebordon.shinyapps.io/09_DevelopingDataProducts

--- .class #id 

## UI component

The UI component of our shiny app is very minimalistic. While the checkboxes are classic input field, we also used the double edged slider which is used to select a range of chicks we want to inspect. The following code gives you the double edged slider:

```
sidebarPanel(
    sliderInput('chick','Select chicks', value=c(1,10),min=1,max=50,step=1),
    ...
)
```

The key difference with the normal slider is the value, which you need to define as `c(1,10)` in order to get two edges. In our main panel we have some text and a plot which is the central part of our shinyapp and which is used to find the fattest chick! Other than that, other UI elements are quite simple!

--- .class #id 

## Server component

Our server component had to parse the input and based on it redraw the plot. We first had to convert the Chick column from the ChickWeight dataset from factor to numeric. We achieved that by running the following code:


```r
data(ChickWeight)
ChickWeight$Chick <- as.numeric(levels(ChickWeight$Chick)[ChickWeight$Chick])
class(ChickWeight$Chick)
```

```
## [1] "numeric"
```

--- .class #id 

## Server component

Once we have it as numeric, we can now subset the data which is to be plotted. We do this with a reactive function:

```
chick_data <- reactive({
    if(input$allChicks == FALSE) {
        chick_data <- ChickWeight[ChickWeight$Chick >= input$chick[1] & ChickWeight$Chick <= input$chick[2],]
    } else {
        chick_data <- ChickWeight
    }
})
```

Notice how we obtain values from both edges of the slider (`input$chick[1]` for lower edge and `input$chick[2]` for higher edge).

--- .class #id 

## Fattest chick around

Thank you for trying out my simple shinyapp which serves as a proof of concept that somethings are easy to learn but difficult to master! Hopefully we can use what we learned here in our future work and especially in the Capstone project!

Btw, did you actually find the fattest chick? **hint: it was the chick #35**
