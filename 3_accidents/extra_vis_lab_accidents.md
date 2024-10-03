# Road traffic accidents in Edinburgh

Data vis challenge! Your task is to recreate the graph below.

The data can be found in the data directory, and it’s called `accidents` It covers all recorded accidents in Edinburgh in 2018 and some of the variables were modified for the purposes of this assignment. 

![Road traffic accidents in Edinburgh](./edi-accidents-1.png "Road traffic accidents in Edinburgh")

<cite>
    This exercise has been adapted from Mine Çetinkaya-Rundel, CC-BY-NC.
</cite>

#1. read in data
#2. filter the info we want
#3. mutate to add new variable (weekend or weekday)
#4. plot

#load tidyverse
library(tidyverse)

#import RDS file
accidents <- readRDS(here("3_accidents", "data", "accidents.rds"))

#filter columns we need
filtered_accidents = accidents %>% 
  select(severity, time, day_of_week, id)

#add new variable 
 
 update = filtered_accidents %>% 
    mutate(weekday_or_weekend = if_else(day_of_week %in% c("Saturday", "Sunday"), "Weekend", "Weekday"))
    
##plot

density_plot = update %>% 
    ggplot(aes(x = time, fill = severity)) +
    geom_density(alpha = 0.5) +
    facet_wrap(~weekday_or_weekend, ncol = 1) +
    labs(title = "Number of accidents throught the day",
         subtitile = "By day of week and severity",
         x = "Time of Day",
         y = "Density") +
         theme_minimal() +
         scale_fill_manual(
         values = c("Fatal" = "#D55E00", "Serious" = "009E73", "Slight" = "#F0E442"),
         name = "Severity")
      

print(density_plot)

