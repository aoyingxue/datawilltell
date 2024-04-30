---
title: Exploring Cinematic Playlists: Music Analysis of Songs Playing in TV Shows
description: Music is one of the best companions of motion pictures. Songs from movies or TV series can also serve as a key to unlocking the emotions and atmosphere of the scenes, influencing the overall narrative and viewer experience. 
slug: spotify-playlist-music-analysis-songs-from-tv
date: 2024-04-04
image: cover.png
categories:
    - Personal Projects
tags:
    - Tableau
    - Data Visualization
    - Data Collection
    - Spotify
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---

## Dashboards (Tableau Public link [here]())



## Datasets

### IMDb Non-Commercial Datasets

For each TV show's genre and year of release, no dataset is more accessible than IMDb. Subsets of IMDb data are available for access to customers for personal and non-commercial use [here at this link](https://developer.imdb.com/non-commercial-datasets/). The dataset files can be accessed and downloaded from https://datasets.imdbws.com/. The data is refreshed daily.

### Self-Curated Spotify Playlists of Songs From TV Shows I Watched Over the Years

I'm personally a Spotify playlist addict. I love to curate song playlists for different moods, vibes, and seasons, or I make playlists of tracks produced by my favorite artists. Sometimes when I heard great songs playing in TV shows, I would add it into my playlists. And then I developed a habit of making playlists for songs playing in the TV show that I watched. The analysis includes **28 playlists** curated mainly by me, a.k.a. **[RecmRoom](https://open.spotify.com/user/mn2y0ay9emuhncqd3yqdm2x76?si=3f66f5faf03344a5)** on Spotify. You're welcome to check each playlist on [Google Sheet](https://docs.google.com/spreadsheets/d/1csc2dKC11QIZzjmr3jke3FPXEclEBG82bH2e1kGWEbU/edit?usp=sharing). 

With [Spotify Web API](https://developer.spotify.com/web-api/), you can find details of each song in the playlist including genre, mood, artist, date of release, and other musical attributes. If you want to avoid typing codes, there are several free online tools available, such as [Spotify Playlist Analyzer](https://www.chosic.com/spotify-playlist-analyzer/). By copying and pasting your playlist link in the search box and analyze, it would output all kinds of details and you could also export table as csv. 

## Process

### Write a simple Python script to process Spotify data through web api

Thanks to [Spotipy](https://spotipy.readthedocs.io/en/2.19.0/), it's so much easier to access Spotify data through [Spotify's Web API](https://developer.spotify.com/documentation/web-api/).

```python
## import packages
import spotipy
from spotipy.oauth2 import SpotifyClientCredentials
import pandas as pd

## 
CLIENT_ID="INPUT YOUR API ID"
CLIENT_SECRET="INPUT YOUR API SECRET"
SPOTIPY_REDIRECT_URI="http://127.0.0.1/callback" # must match the redirect URI added to your app in your Spotify dashboard.

client_credentials_manager = SpotifyClientCredentials(client_id=CLIENT_ID, client_secret=CLIENT_SECRET)
sp = spotipy.Spotify(client_credentials_manager=client_credentials_manager)
```

*Note: It is important to note that Spotify has set the maximum offset to 1000. For example, if you use sp.search(), it returns a maximum of 50 results per query, which is why a nested for loop is utilized.*

```python
## use a playlist to set an example
playlist_items = sp.playlist_tracks("https://open.spotify.com/playlist/34Vt61ufh30YPJX45e1Om0?si=7b9df5961f8e4308")
playlist_items = pd.DataFrame(playlist_items['items'])
playlist = pd.DataFrame()
playlist['added_at'] = playlist_items['added_at']


```



1. 4*7 matrix of 28 radar maps showing each playlist's musical attributes, or one stacked radar map 
2. A Spotify now playing widget, click the episode and play the playlist

## Inspirations

Thanks to [Emanuela Blaiotta](https://public.tableau.com/app/profile/emanuela8569)'s [A week in music - Spotify API](https://public.tableau.com/app/profile/emanuela8569/viz/Aweekinmusic-SpotifyAPI/Spotify), I get to know that it's even possible to embed Spotify in Tableau dashboard. 

