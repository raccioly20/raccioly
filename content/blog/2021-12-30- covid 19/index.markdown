---
title: "Vamos lá"
subtitle: ""
excerpt: "Avaliando dados de COVID 19"
date: 2021-12-30
author: "Ricardo Accioly"
draft: false
images:
series:
tags:
categories:
layout: single
---


## Usando os dados do pacote covid19BR

Este é um teste do R no blog usando como base os exemplos existentes na vignette do pacote covid19BR.


```r
library(tidyverse)
library(covid19br)
library(forecast)
```


```r
brasil <- downloadCovid19("brazil")
brasil <- brasil %>% mutate(ma_newDeaths = ma(newDeaths, order = 7)
  )
mortes <- brasil %>% select(date, newDeaths, ma_newDeaths) %>%
  pivot_longer(
    cols = c("newDeaths", "ma_newDeaths"),
    values_to = "deaths", names_to = "type"
  ) %>%
  mutate(
    type = recode(type, 
           ma_newDeaths = "moving average",
           newDeaths = "count",
    )
  )
ggplot(mortes, aes(x = date, y=deaths, color = type)) +
  geom_point() +
  geom_path() + 
  theme_light() +
  theme(legend.position="bottom") + 
  scale_color_manual(values = c("#00A08A","#FF0000"))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-2-1.png" width="672" />

