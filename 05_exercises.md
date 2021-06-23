---
title: 'Weekly Exercises #5'
author: "Rita Liu"
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
library(gardenR)       # for Lisa's garden data
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
library(gifski)        # need the library for creating gifs but don't need to load each time
library(shiny)         # for creating interactive apps
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("data/small_trains.csv") 

# Lisa's garden data
data("garden_harvest")

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("data/mallorca_bike_day7.csv") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("data/panama_swim.csv")

panama_bike <- read_csv("data/panama_bike.csv")

panama_run <- read_csv("data/panama_run.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("data/us-states.csv")
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
g1 <- garden_harvest %>% 
  filter(vegetable == 'lettuce') %>% 
  ggplot(aes(x = weight)) + 
  geom_histogram() + 
  labs(title = 'Distribution of weight of lettuce haversts')
g2 <- garden_harvest %>% 
  filter(vegetable == 'beets') %>% 
  mutate(wt_lbs = weight * 0.00220462) %>% 
  group_by(variety,date) %>% 
  summarise(cum_wt_lbs = sum(wt_lbs)) %>% 
  ggplot(aes(x = date, y = cum_wt_lbs, color = variety)) + 
  geom_line() +
  labs(title = 'Cumulative weight of beets haversts by variety')

ggplotly(g1)
```

```{=html}
<div id="htmlwidget-ddbddcd66d241e97dde2" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-ddbddcd66d241e97dde2">{"x":{"data":[{"orientation":"v","width":[10.8275862068965,10.8275862068966,10.8275862068965,10.8275862068966,10.8275862068966,10.8275862068965,10.8275862068966,10.8275862068966,10.8275862068966,10.8275862068966,10.8275862068965,10.8275862068966,10.8275862068966,10.8275862068965,10.8275862068966,10.8275862068965,10.8275862068965,10.8275862068966,10.8275862068965,10.8275862068966,10.8275862068965,10.8275862068965,10.8275862068966,10.8275862068965,10.8275862068965,10.8275862068965,10.8275862068965,10.8275862068965,10.8275862068965,10.8275862068965],"base":[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],"x":[10.8275862068966,21.6551724137931,32.4827586206897,43.3103448275862,54.1379310344828,64.9655172413793,75.7931034482758,86.6206896551724,97.448275862069,108.275862068966,119.103448275862,129.931034482759,140.758620689655,151.586206896552,162.413793103448,173.241379310345,184.068965517241,194.896551724138,205.724137931034,216.551724137931,227.379310344828,238.206896551724,249.034482758621,259.862068965517,270.689655172414,281.51724137931,292.344827586207,303.172413793103,314,324.827586206897],"y":[7,9,1,6,6,6,6,7,6,3,2,2,3,1,0,0,1,0,0,2,0,0,0,0,0,0,0,0,0,1],"text":["count: 7<br />weight:  10.82759","count: 9<br />weight:  21.65517","count: 1<br />weight:  32.48276","count: 6<br />weight:  43.31034","count: 6<br />weight:  54.13793","count: 6<br />weight:  64.96552","count: 6<br />weight:  75.79310","count: 7<br />weight:  86.62069","count: 6<br />weight:  97.44828","count: 3<br />weight: 108.27586","count: 2<br />weight: 119.10345","count: 2<br />weight: 129.93103","count: 3<br />weight: 140.75862","count: 1<br />weight: 151.58621","count: 0<br />weight: 162.41379","count: 0<br />weight: 173.24138","count: 1<br />weight: 184.06897","count: 0<br />weight: 194.89655","count: 0<br />weight: 205.72414","count: 2<br />weight: 216.55172","count: 0<br />weight: 227.37931","count: 0<br />weight: 238.20690","count: 0<br />weight: 249.03448","count: 0<br />weight: 259.86207","count: 0<br />weight: 270.68966","count: 0<br />weight: 281.51724","count: 0<br />weight: 292.34483","count: 0<br />weight: 303.17241","count: 0<br />weight: 314.00000","count: 1<br />weight: 324.82759"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(89,89,89,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":43.1050228310502},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Distribution of weight of lettuce haversts","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-10.8275862068965,346.48275862069],"tickmode":"array","ticktext":["0","100","200","300"],"tickvals":[0,100,200,300],"categoryorder":"array","categoryarray":["0","100","200","300"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"weight","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.45,9.45],"tickmode":"array","ticktext":["0.0","2.5","5.0","7.5"],"tickvals":[0,2.5,5,7.5],"categoryorder":"array","categoryarray":["0.0","2.5","5.0","7.5"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"count","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"86d34d955356":{"x":{},"type":"bar"}},"cur_data":"86d34d955356","visdat":{"86d34d955356":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```

```r
ggplotly(g2)
```

```{=html}
<div id="htmlwidget-c646c9fe0d79bb72c9d6" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-c646c9fe0d79bb72c9d6">{"x":{"data":[{"x":[18450,18451,18463,18470,18487],"y":[0.13668644,0.18298346,0.23589434,0.32848838,6.13766208],"text":["date: 2020-07-07<br />cum_wt_lbs: 0.13668644<br />variety: Gourmet Golden","date: 2020-07-08<br />cum_wt_lbs: 0.18298346<br />variety: Gourmet Golden","date: 2020-07-20<br />cum_wt_lbs: 0.23589434<br />variety: Gourmet Golden","date: 2020-07-27<br />cum_wt_lbs: 0.32848838<br />variety: Gourmet Golden","date: 2020-08-13<br />cum_wt_lbs: 6.13766208<br />variety: Gourmet Golden"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(248,118,109,1)","dash":"solid"},"hoveron":"points","name":"Gourmet Golden","legendgroup":"Gourmet Golden","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18424,18431,18432,18434],"y":[0.01763696,0.0551155,0.02425082,0.12566334],"text":["date: 2020-06-11<br />cum_wt_lbs: 0.01763696<br />variety: leaves","date: 2020-06-18<br />cum_wt_lbs: 0.05511550<br />variety: leaves","date: 2020-06-19<br />cum_wt_lbs: 0.02425082<br />variety: leaves","date: 2020-06-21<br />cum_wt_lbs: 0.12566334<br />variety: leaves"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(0,186,56,1)","dash":"solid"},"hoveron":"points","name":"leaves","legendgroup":"leaves","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18450,18452,18455,18461,18470,18473,18487],"y":[0.0220462,0.15211878,0.19621118,0.37919464,0.10802638,0.22266662,5.30652034],"text":["date: 2020-07-07<br />cum_wt_lbs: 0.02204620<br />variety: Sweet Merlin","date: 2020-07-09<br />cum_wt_lbs: 0.15211878<br />variety: Sweet Merlin","date: 2020-07-12<br />cum_wt_lbs: 0.19621118<br />variety: Sweet Merlin","date: 2020-07-18<br />cum_wt_lbs: 0.37919464<br />variety: Sweet Merlin","date: 2020-07-27<br />cum_wt_lbs: 0.10802638<br />variety: Sweet Merlin","date: 2020-07-30<br />cum_wt_lbs: 0.22266662<br />variety: Sweet Merlin","date: 2020-08-13<br />cum_wt_lbs: 5.30652034<br />variety: Sweet Merlin"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(97,156,255,1)","dash":"solid"},"hoveron":"points","name":"Sweet Merlin","legendgroup":"Sweet Merlin","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":31.4155251141553},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Cumulative weight of beets haversts by variety","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[18420.85,18490.15],"tickmode":"array","ticktext":["Jun 15","Jul 01","Jul 15","Aug 01","Aug 15"],"tickvals":[18428,18444,18458,18475,18489],"categoryorder":"array","categoryarray":["Jun 15","Jul 01","Jul 15","Aug 01","Aug 15"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"date","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.288364296,6.443663336],"tickmode":"array","ticktext":["0","2","4","6"],"tickvals":[0,2,4,6],"categoryorder":"array","categoryarray":["0","2","4","6"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"cum_wt_lbs","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":0.913385826771654},"annotations":[{"text":"variety","x":1.02,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"left","yanchor":"bottom","legendTitle":true}],"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"86d345cb1a66":{"x":{},"y":{},"colour":{},"type":"scatter"}},"cur_data":"86d345cb1a66","visdat":{"86d345cb1a66":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```
  
  
  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).


```r
samll_trip_gif <-small_trains %>% 
  filter(service == 'National') %>% 
  group_by(year,month) %>% 
  summarise(total_trip = sum(total_num_trips)) %>% 
  ggplot(aes(x = month, y = total_trip,group = year)) + 
  geom_col() + 
  labs(title = "Total National Trips",
       subtitle = "Year: {closest_state}",
       x = "month",
       y = "") +
  transition_states(year)
anim_save("small_trip_gif.gif",animation = samll_trip_gif) 
```

```r
knitr::include_graphics("small_trip_gif.gif")
```

![](small_trip_gif.gif)<!-- -->

> From this animation, we can see that, generally speaking, tomato haverst in 2016 is less comparing to that in 2017 and 2015. The haverst in June, 2016 is abnormally low. 

## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. You should do the following:
  * From the `garden_harvest` data, filter the data to the tomatoes and find the *daily* harvest in pounds for each variety.  
  * Then, for each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each vegetable and arranged (HINT: `fct_reorder()`) from most to least harvested (most on the bottom).  
  * Add animation to reveal the plot over date. 

I have started the code for you below. The `complete()` function creates a row for all unique `date`/`variety` combinations. If a variety is not harvested on one of the harvest dates in the dataset, it is filled with a value of 0.


```r
tcum<-garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(date, variety) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>% 
  ungroup() %>% 
  complete(variety, date, fill = list(daily_harvest_lb = 0)) %>% 
  group_by(variety) %>% 
  mutate(cum_weight = cumsum(daily_harvest_lb),
         variety  = fct_reorder(variety,cum_weight,.desc = FALSE)) %>% 
  ggplot(aes(x = date, y = cum_weight, fill = variety)) + 
  geom_area() + 
  transition_reveal(date) + 
  labs(title = 'Cumulative weight of tomatoes',
       subtitle = 'Date:{frame_along}')
anim_save('tcum.gif',animation = tcum)
```

```r
knitr::include_graphics("tcum.gif")
```

![](tcum.gif)<!-- -->


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
mallorca_map <- get_stamenmap(
    bbox = c(left = min(mallorca_bike_day7$lon)-0.1, 
             bottom = min(mallorca_bike_day7$lat) - 0.03, 
             right = max(mallorca_bike_day7$lon) + 0.1 , 
             top =  max(mallorca_bike_day7$lat)) + 0.03, 
    maptype = "terrain",
    zoom = 11
)
```

```r
bike<-ggmap(mallorca_map) + 
  geom_path(data = mallorca_bike_day7,
             aes(x = lon,
                 y = lat,
                 color = ele)) + 
  geom_point(data = mallorca_bike_day7,
             aes(x = lon,
                 y = lat),color = 'red',size = 3) +
  transition_reveal(time) + 
  
  labs(title = 'Bike ride path',
       subtitle = 'Time : {frame_along}',
       x = 'logitude',
       y = 'lattitude')
anim_save('bike.gif',animition = bike)
```

```r
knitr::include_graphics("bike.gif")
```

![](bike.gif)<!-- -->

> I think this map is better because from it we can kind of see Lisa's speed and where she paused. 
  
  5. In this exercise, you get to meet my sister, Heather! She is a proud Mac grad, currently works as a Data Scientist at 3M where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files (HINT: `bind_rows()`, 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
panama_all <- bind_rows(panama_bike,panama_run,panama_swim)
panama_map <- get_stamenmap(
    bbox = c(left = min(panama_all$lon)-0.04, 
             bottom = min(panama_all$lat) - 0.04, 
             right = max(panama_all$lon) + 0.01 , 
             top =  max(panama_all$lat)) + 0.03, 
    maptype = "terrain",
    zoom = 11
)
```

```r
panama<-ggmap(panama_map) + 
  geom_path(data = panama_all,
            aes(x = lon, 
                y = lat)) +
  geom_point(data = panama_all,
             aes(x = lon, 
                 y = lat,
                 color = event)) + 
  transition_reveal(time) + 
  labs(title = 'Heather\'s triathlete path',
       subtitle = 'Time:{frame_along}') 
anim_save('panama.gif',animation = panama)
```
  

```r
knitr::include_graphics("panama.gif")
```

![](panama.gif)<!-- -->
  
## COVID-19 data

  6. In this exercise, you are going to replicate many of the features in [this](https://aatishb.com/covidtrends/?region=US) visualization by Aitish Bhatia but include all US states. Requirements:
 * Create a new variable that computes the number of new cases in the past week (HINT: use the `lag()` function you've used in a previous set of exercises). Replace missing values with 0's using `replace_na()`.  
  * Filter the data to omit rows where the cumulative case counts are less than 20.  
  * Create a static plot with cumulative cases on the x-axis and new cases in the past 7 days on the y-axis. Connect the points for each state over time. HINTS: use `geom_path()` and add a `group` aesthetic.  Put the x and y axis on the log scale and make the tick labels look nice - `scales::comma` is one option. This plot will look pretty ugly as is.
  * Animate the plot to reveal the pattern by date. Display the date as the subtitle. Add a leading point to each state's line (`geom_point()`) and add the state name as a label (`geom_text()` - you should look at the `check_overlap` argument).  
  * Use the `animate()` function to have 200 frames in your animation and make it 30 seconds long. 
  * Comment on what you observe.

```r
covid19 %>% 
  group_by(state) %>% 
  mutate(cases_week_ago = lag(cases, 7, order_by = date),
         new_cases_week = cases - cases_week_ago) %>% 
  replace_na(list(new_cases_week = 0)) %>% 
  ungroup() %>% 
  filter(cases>=20) %>%  
  ggplot(aes(x = cases, y = new_cases_week,group = state)) + 
  geom_path() + 
  scale_y_log10(label = scales::comma) + 
  scale_x_log10(label = scales::comma) 
```

![](05_exercises_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

```r
covid_gif<-covid19 %>% 
  group_by(state) %>% 
  mutate(cases_week_ago = lag(cases, 7, order_by = date),
         new_cases_week = cases - cases_week_ago) %>% 
  replace_na(list(new_cases_week = 0)) %>% 
  ungroup() %>% 
  filter(cases>=20) %>%  
  ggplot(aes(x = cases, y = new_cases_week,group = state)) + 
  geom_path(color = 'grey') + 
  geom_point(color = 'black') + 
  geom_text(aes(label = state), check_overlap = TRUE) +
  scale_y_log10(label = scales::comma) + 
  scale_x_log10(label = scales::comma) + 
  transition_reveal(along = date) + 
  labs(title = 'Cumulative covid cases vs. new cases in a week',
       subtitle = 'Time:{frame_along}')

animate(covid_gif, nframes = 200, duration = 30)
anim_save("covid.gif")
```
  

```r
knitr::include_graphics("covid.gif")
```

![](covid.gif)<!-- -->
  
> I noticed that relative to its cumulative cases, the new cases in the past week increased quickly for New York at beginning and slowly decreased. It seems that for all states, covid cases increase at a faster rate at the beginning. 

  7. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. The code below gives the population estimates for each state and loads the `states_map` data. Here is a list of details you should include in the plot:
  
  * Put date in the subtitle.   
  * Because there are so many dates, you are going to only do the animation for all Fridays. So, use `wday()` to create a day of week variable and filter to all the Fridays.   
  * Use the `animate()` function to make the animation 200 frames instead of the default 100 and to pause for 10 frames on the end frame.   
  * Use `group = date` in `aes()`.   
  * Comment on what you see.  



```r
census_pop_est_2018 <- read_csv("data/us_census_2018_state_pop_est.csv") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))

states_map <- map_data("state")
```


```r
covid_map_gif<-covid19 %>% 
  mutate(weekday = wday(date,label = TRUE)) %>% 
  filter(weekday == 'Fri') %>% 
  mutate(state = str_to_lower(state)) %>% 
  left_join(census_pop_est_2018,by = c('state' = 'state')) %>% 
  mutate(cases_10000 = (cases/est_pop_2018)*10000) %>% 
  ggplot(aes(fill = cases_10000,
             group = date)) + 
  geom_map(map = states_map,
           aes(map_id = state)) + 
  expand_limits(x = states_map$long, y = states_map$lat) + 
  theme_map() + 
  transition_states(date,transition_length = 0.3) + 
  labs(title = 'cumulative cases per 10000 people by state',
       subtitle = 'Date: {closest_state}') 
animate(covid_map_gif,nframes = 200,end_pause = 10)
```

```r
anim_save('covid_pop.gif')
```

```r
knitr::include_graphics("covid_pop.gif")
```

![](covid_pop.gif)<!-- -->

> I noticed that cumulative cases per 10000 increased faster for states in the middle at the beiginning. 

## Your first `shiny` app (for next week!)

NOT DUE THIS WEEK! If any of you want to work ahead, this will be on next week's exercises.

  8. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' cumulative number of COVID cases over time. The x-axis will be number of days since 20+ cases and the y-axis will be cumulative cases on the log scale (`scale_y_log10()`). We use number of days since 20+ cases on the x-axis so we can make better comparisons of the curve trajectories. You will have an input box where the user can choose which states to compare (`selectInput()`) and have a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
  
## GitHub link

  9. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 05_exercises.Rmd, provide a link to the 05_exercises.md file, which is the one that will be most readable on GitHub. If that file isn't very readable, then provide a link to your main GitHub page.

[GitHub Link](https://github.com/RitaLiu6/Assignment-5)

**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
