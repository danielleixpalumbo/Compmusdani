---
title: "How does today's Hip Hop differ from 90's Hip Hop?"
author: "Daniel Leix Palumbo"
date: "22 February 2019"
output: 
   flexdashboard::flex_dashboard:
       storyboard: true
       theme: lumen
editor_options: 
  chunk_output_type: console
---

`r knitr::opts_chunk$set(cache = TRUE)`

```{r, cache = FALSE}
library(flexdashboard)
library(tidyverse)
library(spotifyr)
library(devtools)
library(ggplot2)
library(dplyr)
library(plotly)
library(Cairo)

# Spotify access variables 

Sys.setenv(SPOTIFY_CLIENT_ID = '8e7bf977224b414d8e6b255352c63b5a')
Sys.setenv(SPOTIFY_CLIENT_SECRET = '1b2f3f287a724da8b6ed562dbf4d99e1')

# Work on playlists with spotifyr

HipHop_90s <- get_playlist_audio_features('hhheroes', '5fWeZI5FLdUkowl4qaglPi')
HipHop_today <- get_playlist_audio_features('mejoresplaylistsspotify', '0OtdLZ5JVyqB4Zp8aylECe')
```

### Introduction 

How does hip-hop from the 1990s differ from hip-hop today? 
With the aim to answer to this question, I decided to look for reliable playlists on Spotify through which I could gain valuable insight into the most remarkable differences within and between the two time periods. In order to do so, I chose as corpus of data two playlists, namely "Hip Hop 90s" from "Hip Hop Heroes" and "Hip Hop 2019: Top100 Rap HITS 2019 / Rap 2019 New Hip Hop Songs" from "Filtr Espana", and I decided to analyse them and to compare their key features. 
These two playlists were chosen since they presented similar features in terms of number of followers and number of tracks, and also they contained tracks from the most famous and representative artists of the two time periods. 

*** 

### A closer look at the two playlists: is Hip Hop become faster in 20 years?  

```{r}
HipHops <-
  HipHop_90s %>% mutate(playlist = "HipHop90") %>%
  bind_rows(HipHop_today %>% mutate(playlist = "HipHopToday"))

# Make our own visualisation 
HipHop_labels<- tibble(
    label = c("Around the Way Girl", "Party Up"),
    playlist = c("HipHop90", "HipHopToday"),
    speechiness = c(0.356, 0.347),
    tempo = c(202.084, 201.936))
HipHopScatterplot <-    
HipHops %>%
  ggplot(aes(x = speechiness, y = tempo, color = mode, size = energy)) +
   geom_point()+
   facet_wrap(~ playlist)+
   geom_text (aes( x = speechiness,y = tempo, label = label), colour = "black", size = 3, data = HipHop_labels, hjust = "right", vjust = "bottom", nudge_y = 6.5)+
   geom_jitter(width = 0.00001, height = 0.00001)+
   theme_light() +              
   labs( x = "speechines", y = "tempo", colour = "Mode")+
   scale_y_continuous(limits = c(0, 240), breaks = c(0, 50, 100, 150, 200), minor_breaks = NULL)+
  scale_x_continuous(limits = c(0, 0.5), breaks = c(0, 0.1,  0.2,  0.3,  0.4,  0.5), minor_breaks = NULL)
ggplotly(HipHopScatterplot)   

```

*** 

After i got the audio features from both playlists using "spotifyr", I decided to combine both the data sets in a scatterplot but by keeping them separated into two different charts with speechiness on the x-axis and tempo on the y-axis. As a result, it could be observed on the scatterplot that overall Hip-Hop got significantly faster in 20 years, since most of the 90's tracks have less than 100 BPM while most of the today's tracks entirely cover the range from 100 BPM to 175. In addition, from the scatterplot could be observed also a couple of outliers, which were two today's hip hop songs, namely "Around the Way Girl" and "Party Up" (the two songs shold be both in the today's chart, I commited an oversight in labelling, I'm sorry for that), that surprisingly were above the 200 BPM, representing the fastest track of both data sets. 

However, after I listened to both the songs, I must say that in this case Spotify's tempo detection was far from the truth. Moreover, the values of speechiness were slightly higher for the 90's tracks, demonstrating a little decrease of the use of spoken words in today's tracks. But here too, there was an outlier represented by today's track "Zone 6 (Remix) (feat. Future & 6LACK)", which had the highest amount of spoken words with the value of 0.6490. Subsequently, I obtained the average danceability and energy for both playlists but I did not find any remarkable difference for these features, the values of which were marginally higher in 90's tracks. However, I wanted anyway to map the energy of the tracks to the size of the dots in the charts, in order to find out something new in the future. 

Finally, as it can be observed in the scatterplot the dots are coloured by "mode", but in both playlist i found a certain balance between songs in major and in minor and there was no remarkable trend to be noticed. The increasing acceleration of today's Hip Hop tracks appear to be at the moment the most interesting findings.
