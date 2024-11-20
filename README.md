![R](https://img.shields.io/badge/R-276DC3?style=for-the-badge&logo=r&logoColor=white)

# Animated-Visualization-of-Intel-s-Stock-Prices-Using-Shiny-App-in-R

# Overview
This project showcases an animated line chart that visualizes Intel's stock prices over time. Using the gganimate package, this Shiny app animates the closing stock prices for Intel, showing how the price fluctuates over time with a smooth animation.

## Features
Animated Line Chart:

A line chart visualizing Intel's closing stock price over time.
Animation reveals the line progressively, enhancing the storytelling of the data.

## Dynamic Visualization:
Users can interact with the app to adjust the time range or view other aspects of the stock price data dynamically.

## Tools and Technologies
- Shiny: For building the interactive web application.
- gganimate: For creating the animated plot.
- gifski: For rendering the animation frames into a gif.
- ggplot2: For creating the line chart.
- dplyr (if needed): For data manipulation (not used in this code, but could be added for data wrangling).

## Dataset
The dataset used in this project consists of Intel's stock price data, including:

- Date: Date of the stock price entry.
- Closing Price: The closing price of Intel stock on that particular day.

Dataset Source: Professor

# Shiny App Highlights
Real-time Animation: Users can view how Intel's stock price changes over time through an animated line chart.
Interactive Visualization: The app could be enhanced to allow users to select specific date ranges or other attributes for further exploration.


# Code

    # Load necessary libraries
    library(shiny)
    library(ggplot2)
    library(gganimate)
    library(gifski)
    
    Intel$Date <- as.Date(Intel$Date, format = "%m/%d/%Y")
    
    # Shiny app UI
    ui <- shinyUI(
      tagList(
        tags$h1("Closing Price Over Time"),
        tags$label("Select Date Range:"),
        sliderInput("dateRange", "",
                    min = min(Intel$Date), max = max(Intel$Date),
                    value = c(min(Intel$Date), max(Intel$Date)),
                    timeFormat = "%Y-%m-%d"),
        imageOutput("animatedPlot")
      )
    )
    
    # Shiny server
    server <- function(input, output) {
      output$animatedPlot <- renderImage({
        # Filter data based on selected date range
        filtered_data <- Intel[Intel$Date >= input$dateRange[1] & Intel$Date <= input$dateRange[2], ]
        
        # Create animated line plot
        p <- ggplot(filtered_data, aes(x = Date, y = Close)) +
          geom_line(color = "blue") +
          geom_point() +
          labs(title = "Closing Price Over Time", x = "Date", y = "Closing Price") +
          theme_minimal() +
          transition_reveal(Date) +
          labs(title = "Closing Price Over Time: {frame_along}")
        
        # Render the animation and save as GIF
        anim <- animate(p, nframes = 100, fps = 10, width = 800, height = 400)
        anim_save("animated_plot.gif", animation = anim)
        
        # Return the GIF to display in the UI
        list(src = "animated_plot.gif", contentType = 'image/gif')
      }, deleteFile = FALSE)
    }
    
    # Run the Shiny app
    shinyApp(ui = ui, server = server)
