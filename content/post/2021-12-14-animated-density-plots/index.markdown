---
title: Animated density plots
author: Jan Savinc
date: '2021-12-14'
slug: animated-density-plots
categories: []
tags: []
description: ''
topics: []
---





# Packages used

* [{ggplot2}](https://ggplot2.tidyverse.org/) for making plots
* [{gganimate}](https://gganimate.com/) for creating animations from {ggplot2} plots
* [{gifski}](https://github.com/r-rust/gifski) for saving a `.gif` from a set of input `.png` files
* [{av}](https://github.com/ropensci/av) for saving an `.mp4` video (other formats are supported!) from a set of input `.png` files
* [{png}](http://www.rforge.net/png/) to load a `.png` file and read the dimensions (width & height)

# 


```r
library(tidyverse)
library(viridis)
library(gganimate)

n <- 1000

data_tbl <-
  map_dfr(
    .x = c(1:10),
    .f =
      ~bind_rows(
        tibble(age = round(100 * rbeta(n, shape1 = 7+.x, shape2 = 20)), category = "A", SIMD = .x),
        tibble(age = round(100 * rbeta(n, shape1 = 1+(.x/10), shape2 = 5)), category = "B", SIMD = .x),
        tibble(age = round(100 * rbeta(n, shape1 = 4-(.x/8), shape2 = 2)), category = "C", SIMD = .x)
      )
  )
  

ggplot(data_tbl, aes(x = age, fill = category)) +
  geom_density(alpha = 0.75) +
  scale_fill_viridis(discrete = TRUE)

ggplot(data_tbl, aes(x = age, fill = category)) +
  geom_density(alpha = 0.75) +
  scale_fill_viridis(discrete = TRUE) +
  facet_grid(SIMD~.)


ggplot(data_tbl, aes(x = age, fill = category)) +
  geom_density(alpha = 0.75) +
  geom_text(aes(label = SIMD), x = 50, y = 0.075) +
  scale_fill_viridis(discrete = TRUE) +
  labs(title = "SIMD: {closest_state}") +
  transition_states(states = SIMD, transition_length = 1)
```

