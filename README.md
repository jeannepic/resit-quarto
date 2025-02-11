# resit-quarto
Assignment resit for Data Analysis MT5000
---
title: "Resit exam Quarto"
author: "Jeanne Pic"
date: Sunday, July 14th, 2024
format: html
editor: visual
---

***Covid-19 hurt every country in the world, and Ireland was not an exception.***

***How was the crisis managed in Ireland, what were its effects ?***

***This document aims at analyzing how Ireland was affected by the pandemic and how the country managed the crisis compared to nine other countries : Belize, Dominican Republic, Eritrea, Ghana, Iran, Nigeria, Saint Barthelemy, the French part of Saint Martin and Zimbabwe.***

## Total tests in each country

```{r}
install.packages("maps")
library(maps)
install.packages("tidyverse")
library(tidyverse)
country_data <- read.csv("/cloud/project/country_data.csv", sep=";")
country_data_filtered <- country_data %>%
  filter(date=="01/01/2021")
map_world <- map_data("world") 
map_cases <- full_join(map_world, country_data_filtered, by = c("region" = "location"))
ggplot(data = map_cases)+
aes(x = long, y = lat, group = group, fill = total_tests) +
  geom_polygon()+
  labs (x=NULL, y=NULL)
```

This map chart displays the total number of tests in each country, on January 1st of 2021, that is to say in the middle of the crisis.

We can notice that Ireland, Zimbabwe and Dominican Republic had realized an equivalent, quite low number of tests as the countries appear in dark blue.

Iran realized a lot more tests as the blue color is much lighter.

That result can be linked to the population factor : there are much more people living in Iran (89M) than people living in Ireland (5M), Zimbabwe (16M) or Dominican Republic (11M).

**In conclusion, we may assert that Ireland had not tested its population so much in the very beginning of 2021.**

## Proportion of total cases in each country

```{r message=FALSE, warning=FALSE, paged.print=FALSE}
install.packages("tidyverse")
library(tidyverse)
country_data_mid_crisis <- country_data %>%
filter (date== "12/06/2021")
ggplot(data = country_data_mid_crisis) +
  aes(x = reorder (total_cases_per_million, location), y = total_cases_per_million, fill = location) +
  geom_col()+
  labs (x="Location", y="Total cases per million")
```

This bar chart displays the proportional distribution of cases in each country we study, on June 12th, 2021, that is to say in the middle of the crisis.

We can see that Zimbabwe, Eritrea, Dominican Republic and Ghana were not much affected by the crisis with less than 3000 cases per million, that is to say less than 0,3% of the population was affected by the pandemic at that moment.

The four countries which were most affected were Ireland with approx. 53000 cases per million (5.3% of population), Saint Martin, Nigeria and Saint Barthelemy. In these countries, 5 to 10% of the population was affected by the pandemic.

**In conclusion, we can assert that Ireland was quite a lot affected by the Covid-19 compared to the nine other countries.**

## Relationship between the number of vaccinations and the excess mortality

```{r warning=FALSE}
install.packages("tidyverse")
library(tidyverse)
country_data <- read.csv("/cloud/project/country_data.csv", sep=";")
ggplot(data = country_data) +
  aes(x = people_vaccinated_per_hundred, y = excess_mortality_cumulative_absolute, color=location) +
  geom_point()+
  geom_smooth(method="lm", se=FALSE, color="black")+
  theme_bw()+
  labs (x="People vaccinated per hundred", y="Excess mortality cumulative absolute")
```

This scatter plot displays the relationship between the vaccination and the excess mortality.

Ireland is represented by a green blue. The plots which represent Ireland are more on the right and lower than the spots which represent Dominican Republic. This means that Ireland vaccinated more, and the excess mortality cumulative was lower, than Dominican Republic. We have to keep in mind that Ireland is less populated than Dominican Republic ; that can explain the lower excess mortality cumulative in Ireland.

Spots which represent Ireland are quite on the right side of the graph, and most of them are on top, the rest is in the middle. The fact that the dots are on the right side means that the vaccination rate was high. The fact that the plots are mostly on top of the graph means that the excess mortality was high in Ireland.

The Ireland plots that are most on the right are also most on the top : when the country was vaccinating a lot, the excess mortality was at the highest point. That means that vaccination happened after the pandemic had killed a lot of people.

Iran plots follow quite much the plots of Ireland, but there is no Iran plot that is as much top-right as Ireland plots. This means that in Iran, the management of the crisis in terms of vaccination was quite the same as in Ireland, but Iran did not reach the same vaccination rate as Ireland.

**In a word, we can say that Ireland was a country which vaccinated its population a lot compared to Dominican republic and Iran. We can interpret it with the graph saying that the high rate of vaccination was a reaction to the high excess mortality.**

## Evolution of the number of tests realized in each country

```{r warning=FALSE}
install.packages("tidyverse")
library(tidyverse)
country_data <- read.csv("/cloud/project/country_data.csv", sep=";")
country_data %>%
  mutate(date = as.Date(date, format = "%d/%m/%Y")) %>%
  ggplot() +
  aes(x = date, y = new_tests_smoothed, color = location) +
  geom_line(aes(color=location), alpha = 0.4)+
  geom_line(data=.%>% 
              filter (location=="Ireland"),
            color="red", size=1.2)+
  theme_bw()+
  labs (x="Date", y="Smoothed number of new tests realized")

```

This line chart presents the evolution of the number of new tests realized in each country. Ireland is highlighted in red.

Ireland is the second line which is most on the top almost all the time, above Iran. We can observe some peaks on the line representing Ireland, namely in the beginning of 2021 and 2022. The 2021 peak is also a peak in Iran, while the 2022 peak is a low peak in Iran.

We can thus hypothesize that the 2021 peak in Ireland and Iran means that there was a global expansion of the pandemic in the beginning of 2021 : a variant Covid-19 for example. We can also hypothesize that the 2022 peak in Ireland and 2022 low peak in Iran means that Ireland was particularly affected by the pandemic at that moment, while the rest of the world was facing the crisis as before or even less.

**In conclusion, Ireland tested a lot more its population than the other countries except Iran, while being one of the least populated countries we study. This can be linked to the fact that the GDP per capita is one of the highest in Ireland among the countries we study.**

***In conclusion, we can say that Ireland was affected a lot by the pandemic. The proportion of cases per million was very high, as well as the excess mortality compared to the other countries. The government answered by vaccinating the population a lot : Ireland has the higher rate of vaccinated population. Another answer was the implementation of a lot of tests on the population : after Iran, Ireland was the second country which tested its population the most.***
