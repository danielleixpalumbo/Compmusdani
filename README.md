## Load libraries

library(tidyverse)
library(spotifyr)
library(devtools)
library(ggplot2)
library(dplyr)

## Spotify access variables 

Sys.setenv(SPOTIFY_CLIENT_ID = '8e7bf977224b414d8e6b255352c63b5a')
Sys.setenv(SPOTIFY_CLIENT_SECRET = '1b2f3f287a724da8b6ed562dbf4d99e1')

## Work on playlists with spotifyr

HipHop_90s <- get_playlist_audio_features('hhheroes', '5fWeZI5FLdUkowl4qaglPi')
HipHop_today <- get_playlist_audio_features('mejoresplaylistsspotify', '0OtdLZ5JVyqB4Zp8aylECe')

## Creation of a Scatterplot combining both the data set, with speechiness on the x-axis and tempo on the y-axis

ggplot() +
  geom_point(data=HipHop_90s, aes(x = speechiness, y = tempo), color='red') +
  geom_point(data=HipHop_today, aes(x = speechiness, y = tempo)) 

## Summarization of Danceability and Energy 

HipHop_90s %>%
  summarize(mean_energy90s = mean(energy))
HipHop_today %>%
  summarize(mean_energy_Today = mean(energy))
HipHop_90s %>%
  summarize(mean_danceability90s = mean(danceability))
HipHop_today %>%
  summarize(mean_danceability_Today = mean(danceability))

## Creation of a Scatterplot combining both the data set, with instrumentalness on the x-axis and acousticness on the y-axis

ggplot() + scale_x_log10 ()+
  geom_point(data=HipHop_90s, aes(x = instrumentalness, y = acousticness), color='red') +
  geom_point(data=HipHop_today, aes(x = instrumentalness, y = acousticness)) 



![Visualisation](Rplot.png)

# Description of the Project 

How does hip-hop from the 1990s differ from hip-hop today? With the aim to answer to this question, I decided to look for reliable playlists on Spotify through which I could gain valuable insight into the most remarkable differences within and between the two time periods. In order to do so, I chose as corpus of data two playlists, namely "Hip Hop 90s" from "Hip Hop Heroes" and "Hip Hop 2019: Top100 Rap HITS 2019 / Rap 2019 New Hip Hop Songs" from "Filtr Espana", and I decided to analyse them and to compare their key features. These two playlists were chosen since they presented similar features in terms of number of followers and number of tracks, and also they contained tracks from the most famous and representative artists of the two time periods.
After i got the audio features from both playlists using "spotifyr", I decided to combine both the data sets in a scatterplot with speechiness on the x-axis and tempo on the y-axis. As a result, it could be observed on the scatterplot that overall Hip-Hop got significantly faster in 20 years, since most of the 90's tracks have less than 100 BPM while most of the today's tracks entirely cover the range from 100 BPM to 175. In addition, from the scatterplot could be observed also a couple of outliers, which were two today's hip hop songs, namely "Around the Way Girl" and "Party Up", that surprisingly were above the 200 BPM, representing the fastest track of both data sets. However, after I listened to both the songs, I must say that in this case Spotify's tempo detection was far from the truth. Moreover, the values of speechiness were slightly higher for the 90's tracks, demonstrating a little decrease of the use of spoken words in today's tracks. But here too, there was an outlier represented by today's track "Zone 6 (Remix) (feat. Future & 6LACK)", which had the highest amount of spoken words with the value of 0.6490. 
Subsequently, I obtained the average danceability and energy for both playlists but I did not find any remarkable difference for these features, the values of which were marginally higher in 90's tracks. 
Finally, I created a new scatterplot combining again both playlists, but this time with instrumentalness (x axis) and acousticness (y axis) as variables. From this scatterplot, in which I also decided to show instrumentalness on a logarithmic scale, I did not find remarkable differences in terms of instrumentalness but today's tracks showed overall much greater values in terms of acousticness, as demonstrated by the today's song "New Shapes" with an amount of 0.82600 (maximum value of the entire data set). Furthermore, also the two most "instrumental" songs represented by the scatterplot were belonging to the today's playlist, with respectively the values of 3.91e-01 and 3.77e-01.
The increasing acceleration and acousticness of today's Hip Hop tracks appear to be at the moment the most interesting findings. 
