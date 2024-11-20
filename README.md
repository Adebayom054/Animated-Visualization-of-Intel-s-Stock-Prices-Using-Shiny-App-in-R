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

    library(gganimate)
    library(gifski)
    library(gapminder)
    library(ggplot2)

    # Assuming 'Intel' dataset is loaded
    Intel$Date <- as.Date(Intel$Date, format = "%m/%d/%Y")

    # Create basic line plot
    p <- ggplot(Intel, aes(x = Date, y = Close)) +
    geom_line(color = "blue") +
    geom_point() +
    labs(title = "Closing Price Over Time", x = "Date", y = "Closing Price") +
    theme_minimal()

    # Add animation to reveal the line over time
    p_animated <- p + 
    transition_reveal(Date) +
    labs(title = "Closing Price Over Time: {frame_along}")

    # Render the animation
    animate(p_animated, nframes = 100, fps = 10, width = 800, height = 400)
