---
title       : Examining statistical distributions
subtitle    : An interactive approach
author      : Georges BODIONG
job         : Communication Manager/Data Analyst
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
logo        : wellness.jpg
---

## Overview

This application is a dynamic and interactive way of examining statistical distributions. It is made to be extended, but this basic one deals with:

1- The normal distribution

2- The exponential distribution


--- .class #id 

## Reminder

In statistics, a distribution involves the relative number of times each outcomes will occur in a number of trials. 

The normal or gaussian distribution is a very common continuous probability distribution where distributions of real-valued random variables are not known. This distribution is useful because of the central limit theorem.

The exponential distribution describes the time between events in a poisson process (events occur continuously and independently at an average rate).

This application takes input and graphs the distribution accordingly.

--- .class #id

## Code for user interface


```r
library(shiny)
shinyServer(
        pageWithSidebar(
                headerPanel("Application for visualizing distributions"),
                sidebarPanel(
                selectInput("Distribution", "Please select distribution type",
                             choices = c("Normal", "Exponential")),
                sliderInput("SampleSize", "Please select sample size:",
                            min=100, max=10000, value=1000, step=100),
                conditionalPanel(condition= "input.Distribution=='Normal'",
                                 textInput("mean", "Please select the mean", 10),
                                 textInput("sd", "Please enter the standard deviation", 3)),
                conditionalPanel(condition = "input.Distribution=='Exponential'",
                                 textInput("Lambda", "Please enter exponential lambda:",1))
        ),
        #Create a space for the histogram
        mainPanel(
                plotOutput("myPlot")    
        )
)  
)
```

--- .class #id

## server.R


```r
library(shiny)
shinyServer(
  function(input, output, session){
          
          output$myPlot <- renderPlot({
                  
                  distType <- input$Distribution
                  
                  size <- input$SampleSize
                  
                  if (distType=="Normal"){
                          randomVec <- rnorm(size, mean = as.numeric(input$mean), sd= as.numeric(input$sd))
                  }
                  
                  else{
                          randomVec <- rexp(size, rate = 1/as.numeric(input$Lambda))
                          
                  }
                  hist(randomVec, col = "skyblue")
          })
  })
```

