'''

# Load libraries

library(tidyverse)
library(spotifyr)
library(devtools)
library(ggplot2)
library(dplyr)

# Spotify access variables 

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

 '''
