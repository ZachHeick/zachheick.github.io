---  
layout: post  
title: Mix Retriever&#58; A Hip-Hop Playlist Generator   
---  

I love discovering new music, especially when it comes to hip-hop. Today, music streaming platforms offer thousands of dynamic playlists for finding new music that cover topics such as specific artists, genres, time of year, and many more. *What if we can't find a playlist built around a particular song that we like*? For the fifth and final project at Metis, we were given the freedom to solve any problem we were interested using any of the data science techniques and technologies we've learned from the bootcamp. My favorite topics covered were NLP and recommendation systems, and I wanted to solve this problem with techniques from these topics.  

![Album Art](https://zachheick.github.io/images/Project_Kojak_images/album_art.png){: .center-image }  

## Current Solutions  

If we want a playlist built around a certain song that we like, we could look for a playlist someone else already made, but a playlist might not exist for that song. Another option is to use an existing playlist generator. These existing playlist generators prioritize artists and genres, as well as looking at similar songs others have listened to. Some things that might not be prioritized or included at all in these playlist generators are the lyrical meanings between songs in a playlist and the overall "mood" of the playlist. Lyrical similarities between songs paired with a similar mood can make playlists much more cohesive and personal.  

## My Solution  

To combine the functionality of individual song-based playlist generators with a focus on making content based recommendations, I created a web app, [Mix Retriever](http://www.mixretriever.com/), that builds a hip-hop playlist of songs with similar lyrical meaning and mood around a song specified by the user.  

## Data Collection and Storage  

The data used in this project was collected from multiple sources. I scraped a list of hip-hop artists from Wikipedia, and then used the Spotify API to get metadata for the song of each artist. This metadata included the song's audio preview, album art, as well as other song metrics. These song metrics (energy, tempo, and speechiness) combined with subjectivity and polarity values from sentiment analysis of each song where used as features and quantified what the mood of a song was. To get the song lyrics, I used [PyLyrics](https://pypi.python.org/pypi/PyLyrics/), which scrapes song lyrics from the Lyrics Wiki website.  

All of my data was stored in a PostgreSQL database hosted by Amazon Web Services. After removing duplicates and songs with missing data points, I had a corpus of 19831 songs.  

## Training the Model  

Before I could do anything with the lyrics I needed to clean them up, which involved converting words to lowercase, removing punctuation and stopwords, and correcting some slang terminology. I needed to get lyrics into feature space like the mood features collected from before. Using the song lyrics only I trained a neural network with a single hidden layer called Song2vec. This single hidden layer puts all of the songs into feature space based on lyrics.  

## Making the Playlist from the Feature Space  

For example, lets say we want to make a playlist of similar songs to the song "Ivy" by Frank Ocean. "Ivy" is a very slow and mellow song where Frank reflects on a past relationship and his conflicting feelings.  

![Frank Lyrics](https://zachheick.github.io/images/Project_Kojak_images/frank_ocean_lyrics.png){: .center-image }  

Lorem Ipsum.  

![100 Dimensions](https://zachheick.github.io/images/Project_Kojak_images/100_dimensional_space.png){: .center-image }  

Lorem Ipsum.  

![Results Lyrics](https://zachheick.github.io/images/Project_Kojak_images/results_lyrics.png){: .center-image }  

Lorem Ipsum.  

## Project Source  

The source code can be found on my [github](https://github.com/ZachHeick/Project_Kojak).
