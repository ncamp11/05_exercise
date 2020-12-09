---
title: 'Weekly Exercises #5'
author: "Nick Camp"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(googlesheets4) # for reading googlesheet data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(transformr)    # for "tweening" (gganimate)
library(shiny)         # for creating interactive apps
gs4_deauth()           # To not have to authorize each time you knit.
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
garden_harvest <- read_sheet("https://docs.google.com/spreadsheets/d/1DekSazCzKqPS2jnGhKue7tLxRU3GVL1oxi-4bEM5IWw/edit?usp=sharing") %>% 
  mutate(date = ymd(date))

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.

```r
garden_graph_plotly_graph <- garden_harvest %>%
mutate(daily_weight_lb = weight*0.00220462,
       capital_vegetable = str_to_title(vegetable)) %>%
ggplot(aes(y = fct_reorder(capital_vegetable, weight, median),
           x = daily_weight_lb)) +
  geom_boxplot() +
  labs(title= "Vegetable Harvests: Type and Weight in Pounds",
       x = "", 
       y = "") +
  theme(panel.grid.major.y = element_blank(),
        plot.background = element_rect(fill = "lightblue"),
        panel.background = element_rect(fill = "lightblue"))

ggplotly(garden_graph_plotly_graph)
```

<!--html_preserve--><div id="htmlwidget-ea603346f84aa35f34c9" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-ea603346f84aa35f34c9">{"x":{"data":[{"x":[0.0440924,0.11464024,0.0330693,0.0220462,0.17857422,0.02645544,0.30203294,0.41667318,0.24471282,0.04188778,0.14770954,0.23589434,0.10582176,0.1873927,0.0661386,0.02866006,0.18077884,0.11464024,0.10361714,0.13448182,0.70988764,0.03747854,0.08598018,0.1763696,0.08377556,0.04850164,0.26896364,0.03968316,0.21605276,0.0992079,0.16093726,0.20282504,0.1322772,0.31746528,0.03527392,0.2094389,0.1433003,0.47840254,0.12345872,0.05070626,0.01763696,0.18298346,0.27116826,0.32407914,0.17416498,0.03968316,0.29541908,0.16093726,0.47619792,0.21825738,0.20062042,0.21825738,0.14770954,0.20723428,0.19621118,0.05070626,0.12786796,0.12345872,0.19180194,0.24471282,0.04188778,0.13448182,0.2866006,0.08598018,0.30644218,0.2094389,0.11684486,0.03086468,0.1653465,0.07936632,0.14770954,0.08598018,0.19400656,0.08157094,0.03527392,0.110231,0.11684486,0.09479866,0.04629702,0.21825738,0.1322772,0.1763696,0.08598018,0.12786796,0.11243562,0.09700328,0.09038942,0.12786796,0.01984158,0.08157094,0.03527392,0.0551155,0.06834322,0.15652802,0.04850164,0.03086468,0.08598018,0.13007258,0.04188778,0.0661386,0.37919464,0.23589434,0.10802638,0.19621118,0.12566334,0.0220462,0.32848838,5.45863912,0.18298346,0.22266662,0.43651476,0.15211878,0.13668644,4.87000558,0.01763696,0.0551155,0.67902296,0.02425082,0.6724091,0.08598018,0.28219136,0.25794054,0.23809896,0.13448182,0.26675902,0.30203294,0.1322772,0.3196699,0.35935306,0.06172936,0.6172936,0.38139926,0.84436946,0.15652802,0.0220462,0.27998674,0.28219136,0.24912206,0.04188778,0.26675902,0.6283167,0.74075232,0.95460046,0.15652802,1.00751134,1.15963012,0.55556424,0.1653465,0.10582176,1.3778875,0.32628376,1.74826366,0.0881848,1.23679182,0.3637623,0.9369635,0.0881848,0.3086468,0.45635634,0.73413846,0.07495708,0.0881848,1.63803266,1.75928676,0.01763696,0.01763696,0.16975574,0.1873927,0.20502966,0.04188778,0.05070626,0.03747854,0.08157094,0.19400656,0.24912206,0.0881848,0.0440924,0.05070626,0.56438272,1.02735292,0.50265336,0.6834322,0.21164352,0.39242236,1.13978854,0.66579524,0.04188778,0.06393398,0.19400656,0.07054784,0.05291088,0.68122758,0.51147184,0.02866006,0.07275246,0.00440924,0.03747854,0.01543234,0.03747854,0.30203294,0.02425082,0.0110231,0.330693,0.00661386,0.05952474,0.0551155,0.03527392,0.02645544,0.07495708,0.05291088,0.01984158,0.02866006,0.1322772,0.0661386,0.13448182,0.33510224,0.16975574,0.28880522,0.06393398,0.2314851,0.06393398,0.30203294,0.07054784,0.81791402,0.3858085,7.936632,0.16755112,3.39952404,1.61157722,0.17857422,1.4550492,2.91230302,0.3196699,0.75838928,2.866006,12.566334,0.2425082,2.68743178,8.377556,0.97664666,3.91099588,1.00751134,0.55556424,2.53751762,7.23997208,2.6786133,1.08467304,0.86641566,0.29541908,6.24127922,2.47358364,0.9810559,2.5904285,7.15178728,0.36155768,5.37045432,1.89376858,1.83865308,2.0282504,0.94137274,0.5180857,1.14419778,0.72311536,1.60496336,0.220462,0.6724091,0.04629702,0.20723428,0.771617,0.26896364,0.41005932,0.4960395,0.63272594,1.4550492,1.26104264,1.52780166,0.39242236,0.5401319,0.77382162,0.1433003,0.28439598,1.63803266,0.2314851,0.09038942,0.26896364,0.7054784,0.97664666,1.2015179,0.17196036,1.30513504,0.3527392,0.51588108,1.54543862,0.23809896,0.3086468,1.03176216,0.17857422,1.6644881,1.64685114,2.33028334,2.92553074,1.32056738,6.36694256,1.13317468,0.17196036,1.4440261,0.64815828,2.4912206,1.38009212,0.39462698,1.7306267,0.38360388,1.98636262,0.10361714,0.27998674,2.19800614,1.17065322,0.76500314,1.1574255,0.39462698,0.2866006,0.33510224,0.16755112,0.1543234,0.39903622,0.77382162,2.26855398,3.74124014,2.5463361,0.51367646,0.43431014,0.67902296,0.0881848,0.69886454,0.52469956,1.39552446,0.64154442,1.32056738,1.2345872,2.27737246,0.46517482,0.87523414,0.35714844,0.29982832,1.19710866,2.42949124,0.75838928,0.67902296,0.32628376,1.24340568,0.68784144,1.30293042,0.17857422,0.26014516,0.4850164,2.2266662,0.79145858,0.66579524,0.67681834,3.52959662,0.32628376,0.33730686,1.24120106,0.3527392,1.12215158,0.67681834,1.55205248,0.96121432,1.27647498,0.220462,0.86641566,0.98326052,0.6393398,0.74736618,1.60275874,0.78043548,0.9590097,0.5070626,1.36465978,0.21384814,0.11904948,1.34702282,0.14770954,0.40565008,1.23899644,0.48060716,2.18918766,4.35853374,0.52469956,1.1574255,0.46076558,0.74075232,0.34392072,0.27778212,0.22487124,1.09569614,1.08687766,0.6944553,1.56748482,2.64995324,1.23899644,1.02073906,0.50926722,0.16093726,0.74736618,0.8377556,1.62480494,2.19800614,2.41846814,0.58201968,1.4770954,2.3038279,0.30203294,0.6724091,0.73413846,1.29631656,1.68432968,0.7054784,0.6393398,1.72180822,0.49163026,0.84216484,0.47840254,1.11553772,0.18959732,1.83644846,4.42246772,0.67681834,0.3858085,0.06834322,0.5291088,0.54454114,1.48591388,1.85629004,3.39070556,0.2314851,0.79145858,0.86641566,0.80248168,0.05291088,0.56879196,1.76810524,0.64374904,0.23368972,1.92242864,2.49342522,1.3558413,0.46737944,0.7385477,0.59304278,4.7619792,0.67461372,0.80248168,0.73193384,6.46835508,1.06483146,1.39331984,0.96121432,0.55997348,0.80027706,1.5763033,5.24038174,0.14109568,1.29852118,3.63541838,1.3558413,0.28880522,3.086468,1.45284458,0.47619792,1.88935934,1.52559704,0.58642892,1.0802638,0.47619792,0.94357736,0.53572266,0.7275246,0.20062042,1.70417126,3.38850094,1.5983495,3.39952404,1.76590062,0.72311536,1.11553772,3.46786726,1.3448182,1.57409868,0.1543234,0.50265336,0.99428362,0.97444204,2.72050108,1.76590062,0.1653465,1.41536604,0.91050806,1.92022402,0.2535313,1.38670598,0.96121432,0.37037616,1.07585456,1.42418452,4.01902226,1.65787424,1.80558378,3.03576174,2.26194012,4.15791332,0.39903622,1.34040896,0.440924,3.06883104,0.68122758,1.7747191,0.57540582,1.80558378,0.5842243,0.5070626,0.92814502,0.85318794,1.05160374,0.76279852,1.89817782,3.19008514,6.46835508,2.7888443,1.49473236,1.81219764,0.72091074,2.31926024,3.59573522,0.44312862,0.5621781,1.24781492,0.66799986,0.3086468,0.67461372,2.29721404,1.32056738,0.3417161,1.66228348,0.52029032,2.90348454,6.25671156,0.39242236,0.85098332,4.30562286,0.9479866,2.33248796,5.46304836,1.89376858,1.30733966,0.69665992,0.59965664,0.59745202,0.78484472,0.51367646,6.03624956,1.15963012,2.59704236,1.74385442,2.73152418,0.39462698,1.46827692,0.44753786,1.08908228,0.8267325,1.5652802,1.27427036,6.39119338,0.67020448,0.14109568,0.69665992,0.7936632,2.5904285,0.97444204,0.46958406,0.91050806,1.12215158,1.57409868,1.97974876,4.36073836,0.53131342,1.0141252,0.92153116,1.06703608,0.48281178,0.55556424,0.2094389,1.05160374,0.27778212,0.24912206,0.30203294,0.22487124,0.16975574,0.40124084,0.04188778,0.20062042,0.4299009,0.63713518,0.07275246,0.28439598,0.26014516,0.40344546,0.110231,5.11912764,0.16314188,0.35714844,0.09479866,0.0440924,0.56879196,0.12786796,0.26234978,0.43431014,0.77602624,1.12215158,0.19180194,0.3858085,0.22487124,0.0881848,0.06834322,1.23238258,0.02645544,0.05291088,0.98987438,2.25532626,1.95770256,1.94667946,0.25573592,0.48722102,1.00751134,0.39242236,0.3527392,0.23589434,0.5621781,0.38360388,3.30693,0.1763696,0.37258078,0.97444204,0.37037616,0.3527392,0.12345872,0.36155768,0.2535313,0.8157094,0.24691744,1.38229674,0.1543234,0.72311536,0.31746528,0.14991416,0.42328704,0.55556424,0.5952474,0.1322772,0.18518808,0.20282504,2.27296322,0.17857422,0.37258078,0.18518808,0.19621118,0.82011864,0.96341894,0.29541908,0.1653465,0.48281178,0.22487124,3.78753716,0.59965664,1.57850792,0.6503629,0.22266662,1.38670598,3.51857352,0.59965664,0.61288436,0.96782818,3.36645474,0.69886454,0.71209226,0.37037616,0.57761044,0.82011864,0.96121432,1.85849466,0.64595366,1.06483146,0.24030358,1.16183474,3.62439528,0.7275246,1.45725382,3.54282434,3.44802568,0.47178868,1.75928676,0.75838928,0.84436946,15.542571,2.44492358,7.58609742,2.87041524,3.4612534,16.203957,3.54502896,5.01991974,3.84265266,6.46174122,6.35371484,2.66318096,5.92822318,2.54413148,2.26634936,2.49342522,13.778875,6.5918138,2.99607858,2.89025682,11.0231,10.48958196,5.16322004,2.866006,9.63859864,1.55646172,4.6737944,5.1257415,7.11430874,11.353793,1.56307558,4.72450066,4.299009,2.84616442,3.58691674,5.9745202,0.67681834,0.68784144,1.18388094,0.69225068,1.08908228,1.06703608,1.00089748,1.0582176,0.55556424,0.64815828,0.96341894,4.04327308,3.6486461,4.24830274,0.87523414,2.60806546,2.59704236,3.71698932,3.9352467,4.23948426,2.58381464,0.65256752,3.43479796,0.42108242,0.34392072,1.6314188,1.94667946,2.42728662,2.05470584,2.41626352,4.41144462,1.48370926,0.31746528,0.80689092,3.07103566,1.99077186,0.92373578,2.26194012,2.976237,0.65477214,0.11464024,0.25132668],"y":[10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,7,7,7,7,7,7,7,7,7,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,1,8,8,8,8,8,8,8,8,8,8,4,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,3,3,3,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,9,9,9,9,9,9,9,9,9,9,9,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,15,15,15,15,15,15,15,15,15,15,15,15,15,15,5,5,5,5,5,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,18,18,18,18,18,18,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,25,25,25,25,27,27,27,27,27,27,27,27,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,20,16,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29],"hoverinfo":"y","type":"box","fillcolor":"rgba(255,255,255,1)","marker":{"opacity":null,"outliercolor":"rgba(0,0,0,1)","line":{"width":1.88976377952756,"color":"rgba(0,0,0,1)"},"size":5.66929133858268},"line":{"color":"rgba(51,51,51,1)","width":1.88976377952756},"showlegend":false,"xaxis":"x","yaxis":"y","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":25.5707762557078,"l":81.0958904109589},"plot_bgcolor":"rgba(173,216,230,1)","paper_bgcolor":"rgba(173,216,230,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Vegetable Harvests: Type and Weight in Pounds","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.805568148,17.013934388],"tickmode":"array","ticktext":["0","5","10","15"],"tickvals":[0,5,10,15],"categoryorder":"array","categoryarray":["0","5","10","15"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,31.6],"tickmode":"array","ticktext":["Chives","Basil","Cilantro","Asparagus","Hot Peppers","Spinach","Radish","Strawberries","Raspberries","Lettuce","Beets","Peppers","Onions","Kale","Jalapeño","Apple","Carrots","Broccoli","Swiss Chard","Kohlrabi","Peas","Beans","Potatoes","Tomatoes","Edamame","Cucumbers","Corn","Zucchini","Rutabaga","Squash","Pumpkins"],"tickvals":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31],"categoryorder":"array","categoryarray":["Chives","Basil","Cilantro","Asparagus","Hot Peppers","Spinach","Radish","Strawberries","Raspberries","Lettuce","Beets","Peppers","Onions","Kale","Jalapeño","Apple","Carrots","Broccoli","Swiss Chard","Kohlrabi","Peas","Beans","Potatoes","Tomatoes","Edamame","Cucumbers","Corn","Zucchini","Rutabaga","Squash","Pumpkins"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"2db3301f9f50":{"x":{},"y":{},"type":"box"}},"cur_data":"2db3301f9f50","visdat":{"2db3301f9f50":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# Read in the data for the week
mobile <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-11-10/mobile.csv')
landline <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-11-10/landline.csv')
```



```r
Canadian_cell_graph <- mobile %>%
  filter(entity == "Canada") %>%
  ggplot(aes(x = year, y = mobile_subs))+
  labs(title = "Canadian Mobile Phone Subscriptions per Hundred People",
       x = "",
       y = "")+
  geom_line(color = "white")+
  theme_minimal()+
  theme(panel.background = element_rect(fill = 'red', colour = 'white'))+
  theme(plot.background = element_rect(fill = 'red', colour = 'red'))+
  theme(plot.title = element_text(color = "White", 
                                  size = 12, 
                                  face = "bold.italic"))+
  labs(subtitle = "Nick Camp")

ggplotly(Canadian_cell_graph)
```

<!--html_preserve--><div id="htmlwidget-aca53411cf38a5e864ea" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-aca53411cf38a5e864ea">{"x":{"data":[{"x":[1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017],"y":[2.10801554779097,2.76641789868611,3.61682053666897,4.64270813484153,6.42791605980089,8.82977686417623,11.8079920783555,14.0286224026566,17.7148129834668,22.6934534852763,28.393624588521,34.3242288120329,37.9044158037788,42.023148870965,47.0118124534166,52.7025846770222,57.4324478519739,61.4095752831956,66.1362039683938,70.470609929952,75.5821093172259,77.7101066742037,79.4253296602461,80.4413609850039,80.8572389599494,82.796219574406,84.7400133293572,85.8956669605252],"text":["year: 1990<br />mobile_subs:  2.108016","year: 1991<br />mobile_subs:  2.766418","year: 1992<br />mobile_subs:  3.616821","year: 1993<br />mobile_subs:  4.642708","year: 1994<br />mobile_subs:  6.427916","year: 1995<br />mobile_subs:  8.829777","year: 1996<br />mobile_subs: 11.807992","year: 1997<br />mobile_subs: 14.028622","year: 1998<br />mobile_subs: 17.714813","year: 1999<br />mobile_subs: 22.693453","year: 2000<br />mobile_subs: 28.393625","year: 2001<br />mobile_subs: 34.324229","year: 2002<br />mobile_subs: 37.904416","year: 2003<br />mobile_subs: 42.023149","year: 2004<br />mobile_subs: 47.011812","year: 2005<br />mobile_subs: 52.702585","year: 2006<br />mobile_subs: 57.432448","year: 2007<br />mobile_subs: 61.409575","year: 2008<br />mobile_subs: 66.136204","year: 2009<br />mobile_subs: 70.470610","year: 2010<br />mobile_subs: 75.582109","year: 2011<br />mobile_subs: 77.710107","year: 2012<br />mobile_subs: 79.425330","year: 2013<br />mobile_subs: 80.441361","year: 2014<br />mobile_subs: 80.857239","year: 2015<br />mobile_subs: 82.796220","year: 2016<br />mobile_subs: 84.740013","year: 2017<br />mobile_subs: 85.895667"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(255,255,255,1)","dash":"solid"},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":42.1685346616853,"r":7.30593607305936,"b":25.5707762557078,"l":22.648401826484},"plot_bgcolor":"rgba(255,0,0,1)","paper_bgcolor":"rgba(255,0,0,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"<b> <i> Canadian Mobile Phone Subscriptions per Hundred People <\/i> <\/b>","font":{"color":"rgba(255,255,255,1)","family":"","size":15.9402241594022},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[1988.65,2018.35],"tickmode":"array","ticktext":["1990","2000","2010"],"tickvals":[1990,2000,2010],"categoryorder":"array","categoryarray":["1990","2000","2010"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-2.08136702284574,90.0850495311619],"tickmode":"array","ticktext":["0","25","50","75"],"tickvals":[0,25,50,75],"categoryorder":"array","categoryarray":["0","25","50","75"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"2db323e05de8":{"x":{},"y":{},"type":"scatter"}},"cur_data":"2db323e05de8","visdat":{"2db323e05de8":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).


```r
train_anim <- small_trains %>%
  mutate(month = factor(month.abb[month], levels = month.abb)) %>%
  ggplot(aes(x = month, y = num_late_at_departure)) +
  geom_col() +
  transition_states(year) +
  labs(title = "Number of Late Train Departures Monthly",
       x = "",
       y = "") +
  theme_set(theme_dark())
anim_save("small_trains.gif", train_anim)
```

```r
knitr::include_graphics("small_trains.gif")
```

![](small_trains.gif)<!-- -->

## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. You should do the following:
  * From the `garden_harvest` data, filter the data to the tomatoes and find the *daily* harvest in pounds for each variety.  
  * Then, for each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each vegetable and arranged (HINT: `fct_reorder()`) from most to least harvested (most on the bottom).  
  * Add animation to reveal the plot over date. 

I have started the code for you below. The `complete()` function creates a row for all unique `date`/`variety` combinations. If a variety is not harvested on one of the harvest dates in the dataset, it is filled with a value of 0.


```r
tomato_harvest <- garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(date, variety) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>% 
  ungroup() %>% 
  complete(variety, date, fill = list(daily_harvest_lb = 0)) %>%
  group_by(variety) %>%
  mutate(total = cumsum(daily_harvest_lb)) %>%
  ungroup() %>%
  mutate(variety = fct_reorder(variety, total, max)) %>%
  ggplot(aes(x = date,
             y = total,
             fill = variety)) +
  geom_area() +
  labs(title = "Cumulative Tomato Harvest in Pounds",
       x = "",
       y = "") +
  transition_reveal(date) 
anim_save("tomato_harvest.gif", tomato_harvest, renderer = gifski_renderer())
```


```r
knitr::include_graphics("tomato_harvest.gif")
```

![](tomato_harvest.gif)<!-- -->


## Maps, animation, and movement!

  4. Map my `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle.  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.

```r
mallorca <- get_stamenmap(bbox = c(left = 2.35, bottom = 39.5, right = 2.62, top = 39.7),
                              maptype = "terrain",
                              zoom = 10)
```



```r
bike_image_link <- 
  "https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png"
ggmap(mallorca) +
  geom_path(data = mallorca_bike_day7, aes(x = lon, y = lat, color = ele), size = 1) +
  geom_image(data = mallorca_bike_day7, aes(x = lon, y = lat, image = bike_image_link), size = .1) +
  labs(title = "Mallorca Bike Path", 
       subtitle = "Time: {frame_along}", 
       x = "Longitude", 
       y = "Latitude", 
       color = "Elevation") +
  transition_reveal(time)

anim_save("mallorca.gif", renderer = gifski_renderer())
```
  

```r
knitr::include_graphics("mallorca.gif")
```

![](mallorca.gif)<!-- -->
  
  I prefer the animated map because it gives a vizualization to the viewer. The animation is more interesting and attracts a person to check out the map. It also gives a better perspective on how the journey was by showing the path of elevation and how it changed. Whereas, a static maps path would show elevation but this shows it as the bike is traveling just like in real life.
  
  5. In this exercise, you get to meet my sister, Heather! She is a proud Mac grad, currently works as a Data Scientist at 3M where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files (HINT: `bind_rows()`, 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
new_data <- bind_rows(panama_swim, panama_bike, panama_run)
panama <- get_stamenmap(bbox = c(left = min(new_data$lon), bottom = min(new_data$lat), right = max(new_data$lon), top = max(new_data$lat)), maptype = "terrain", zoom = 15)

ggmap(panama) +
  geom_path(data = new_data, aes(x = lon, y = lat)) +
  geom_point(data = new_data, aes(x = lon, y =lat, color = event), size = 4) +
  labs(title = "Heather's Ironman Path: Panama", subtitle = "Time: {frame_along}", color = "Event") +
  theme_map() +
  transition_reveal(time)
anim_save("panama.gif", renderer = gifski_renderer())
```




```r
knitr::include_graphics("panama.gif")
```

![](panama.gif)<!-- -->


  
## COVID-19 data

  6. In this exercise, you are going to replicate many of the features in [this](https://aatishb.com/covidtrends/?region=US) visualization by Aitish Bhatia but include all US states. Requirements:
 * Create a new variable that computes the number of new cases in the past week (HINT: use the `lag()` function you've used in a previous set of exercises). Replace missing values with 0's using `replace_na()`.  
  * Filter the data to omit rows where the cumulative case counts are less than 20.  
  * Create a static plot with cumulative cases on the x-axis and new cases in the past 7 days on the x-axis. Connect the points for each state over time. HINTS: use `geom_path()` and add a `group` aesthetic.  Put the x and y axis on the log scale and make the tick labels look nice - `scales::comma` is one option. This plot will look pretty ugly as is.
  * Animate the plot to reveal the pattern by date. Display the date as the subtitle. Add a leading point to each state's line (`geom_point()`) and add the state name as a label (`geom_text()` - you should look at the `check_overlap` argument).  
  * Use the `animate()` function to have 200 frames in your animation and make it 30 seconds long. 
  * Comment on what you observe.
  

```r
covid19 %>%
  group_by(state) %>%
  mutate(cases_last_week = lag(cases, 7, order_by = date)) %>%
  replace_na(list(cases_last_week = 0)) %>%
  mutate(new_cases = cases - cases_last_week) %>%
  filter(cases >= 20) %>%
  
  ggplot(aes(x = cases,
             y = new_cases,
             group = state)) +
  geom_path() +
  geom_point(size = .5, color = "green") +
  geom_text(aes(label = state), check_overlap = TRUE) +
  labs(title = "Coronavirus Cases: Cumulative and Current",
       subtitle = "Time: {frame_along}",
       x = "Cumulative Cases",
       y = "New Cases in past 7 Days") +
  scale_x_log10(label = scales::comma) +
  scale_y_log10(label = scales::comma) +
  transition_reveal(date)
anim_save("covid19.gif", renderer = gifski_renderer())
```
  

```r
knitr::include_graphics("covid19.gif")
```

![](covid19.gif)<!-- -->
  
  I observe that the cumulative cases and new cases in past seven days appears to be linear and then drop off. This is likely due to not being able to control the virus at first. Once states locked down cumulative cases did not change but new cases lowered and therefore the dots drop. Overall, it appears that the states that have a more dense population, like New Jersey, New York, and California soared at first. However, this graph does take into account the cases per person.
  
  
  7. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. Put date in the subtitle. Comment on what you see. The code below gives the population estimates for each state. Because there are so many dates, you are going to only do the animation for all Fridays. So, use `wday()` to create a day of week variable and filter to all the Fridays. HINT: use `group = date` in `aes()`.


```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))

states_map <- map_data("state")
covid19 %>%
  mutate(state = str_to_lower(state)) %>%
  left_join(census_pop_est_2018,
            by = c("state" = "state")) %>% 
  mutate(number_cases_per_10000 = (cases/est_pop_2018)*10000,
         week_day = wday(date, label = TRUE)) %>%
  filter(week_day == "Fri") %>%
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = state,
               fill = number_cases_per_10000,
               group = date)) +
  expand_limits(x = states_map$long, y = states_map$lat) +
  labs(title = "Cumulative Recent Coronavirus Cases per 10000 People",
       subtitle = "Time: {closest_state}",
       fill = " Number of Cases per 10,000 People") +
  theme_map() +
  transition_states(date)
```

![](05_exercises_files/figure-html/unnamed-chunk-15-1.gif)<!-- -->

```r
anim_save("covid19_map.gif", renderer = gifski_renderer())
```

```r
knitr::include_graphics("covid19_map.gif")
```

![](covid19_map.gif)<!-- -->

It appears that with time the graph gets a lighter blue meaning more cases are happening. However, these are cumulative cases. Overall, it appears that the midwest and southeast struggle the most with cumulative cases per person.

## Your first `shiny` app (for next week!)

NOT DUE THIS WEEK! If any of you want to work ahead, this will be on next week's exercises.

  8. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' cumulative number of COVID cases over time. The x-axis will be number of days since 20+ cases and the y-axis will be cumulative cases on the log scale (`scale_y_log10()`). We use number of days since 20+ cases on the x-axis so we can make better comparisons of the curve trajectories. You will have an input box where the user can choose which states to compare (`selectInput()`) and have a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
  
## GitHub link

  9. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 05_exercises.Rmd, provide a link to the 05_exercises.md file, which is the one that will be most readable on GitHub. If that file isn't very readable, then provide a link to your main GitHub page.


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
