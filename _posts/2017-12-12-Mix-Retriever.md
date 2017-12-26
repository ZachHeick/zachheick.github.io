---  
layout: post  
title: Mix Retriever&#58; A Hip-Hop Playlist Generator   
---  

I love discovering new music, especially when it comes to hip-hop. Today, music streaming platforms offer thousands of dynamic playlists for finding new music that cover topics such as specific artists, genres, time of year, and many more. *But what if we can't find a playlist built around a particular song that we like*? For the fifth and final project at Metis, we were given the freedom to solve any problem we were interested using any of the data science techniques and technologies we've learned from the bootcamp. My favorite topics covered were NLP and recommendation systems, and I wanted to solve this problem with techniques from these topics.  

![Album Art](https://zachheick.github.io/images/Project_Kojak_images/album_movie.gif){: .center-image }  

## Current Solutions  

If we want a playlist built around a certain song that we like, we could look for a one someone else already made, but a playlist might not exist for that song. Another option is to use an existing playlist generator. These existing playlist generators prioritize artists and genres, as well as looking at similar songs others have listened to. Some things that might not be prioritized or included at all in these playlist generators are the lyrical meanings between songs in a playlist and the overall "mood" of the playlist. Lyrical similarities between songs paired with a similar mood can make playlists much more cohesive and personal.  

## My Solution  

To combine the functionality of individual song-based playlist generators with a focus on making content based recommendations, I created a [web app](http://www.mixretriever.com/) that builds a hip-hop playlist of songs with similar lyrical meaning and mood around a song specified by the user.  

![Album Art](https://zachheick.github.io/images/Project_Kojak_images/mix_retriever_logo.png){: .center-image }  

## Data Collection and Storage  

The data used in this project was collected from multiple sources. I scraped a list of hip-hop artists from Wikipedia, and then used the Spotify API to get metadata for the song of each artist. This metadata included the song's audio preview, album art, as well as other song metrics. These song metrics (energy, tempo, and speechiness) combined with subjectivity and polarity values from sentiment analysis of each song where used as features and quantified what the mood of a song was. To get the song lyrics, I used [PyLyrics](https://pypi.python.org/pypi/PyLyrics/) to scrape song lyrics from the Lyrics Wiki website.  

All of my data was stored in a PostgreSQL database hosted by Amazon Web Services. After removing duplicates and songs with missing data points, I had a corpus of 19831 songs.  

## Training the Model  

Before I could do anything with the lyrics, I cleaned them up, which involved converting words to lowercase, removing punctuation and stopwords, and correcting some slang terminology. I needed to get lyrics into feature space with the mood features collected from before. Using the song lyrics only I trained a neural network with a single hidden layer called Song2vec. This single hidden layer puts all of the songs into feature space based on lyrics. I explored this space to find songs that were similar to one another.   

## Making the Playlist from Feature Space  

Using the data collected and our trained model, how do we get a playlist of similar songs given a single song? For example, lets say we want to make a playlist of similar songs to the song <span class="red">Ivy by Frank Ocean</span>. With a very slow and mellow mood, Frank reflects on a past relationship and his conflicting feelings in this track.  

![Frank Lyrics](https://zachheick.github.io/images/Project_Kojak_images/frank_ocean_lyrics.png){: .center-image }  

Mentioned previously, the Song2vec model's single hidden layer of 100 nodes puts the songs into feature space. As of now, the location of the songs in this space is based on *lyrics only*, where songs with similar lyrical themes to our example appear closer to it.  

![100 Dimensions](https://zachheick.github.io/images/Project_Kojak_images/100_dimensional_space.png){: .center-image }  

Right now its hard to tell which songs have similar lyrics to one another because the distances between them are so large. A technique called "Singular-value Decomposition" reduces the size of the space from 100 features to 25, putting songs with similar lyrical themes closer to each other. The mood features collected from before (energy, speechiness, tempo, subjectivity, and polarity) are added to this feature space to get a final space of 30 features.    

![30 Dimensions](https://zachheick.github.io/images/Project_Kojak_images/30_dimensional_space.png){: .center-image }  

After using SVD and adding the mood features to our feature space, we now have songs with both similar lyrical meaning and mood closer to one another. To get the playlist of similar songs to our example, we use the <span style="color:#fb8c00">Nearest Neighbors</span> algorithm to find the 9 nearest songs, and we now have our playlist!  

![Nearest Neighbors](https://zachheick.github.io/images/Project_Kojak_images/nearest_neighbors.png){: .center-image }  

Below are some similar songs to <span class="red">Ivy by Frank Ocean</span>. These songs are also very slow and mellow in mood, and the artists talk about their struggles with themselves and reflect on past relationships as well.  

![Results Lyrics](https://zachheick.github.io/images/Project_Kojak_images/results_lyrics.png){: .center-image }  

## Testing the App and Analyzing Results  

Music is very subjective and determining what made a playlist a good result was tricky. When testing the app, I focused on songs that had distinct themes throughout; partying, drugs and money, romance, etc. I generated playlists from these songs and analyzed the lyrics first, looking for similar themes. I would then listen to the songs on the playlist and compare them to the original input song. If the lyrical themes of the songs on the playlist weren't quite matching up with the input song, I would go back and tune the model's hyperparameters and features, generate playlists from a specific list of songs, analyze the results, and repeat until I got playlists that I was happy with.    

## Final Thoughts and Future Work  

I'm really proud of my final project at Metis, and had a great time presenting it to potential employers on career day. Trying to quantify something as subjective as music proved to be a challenge, but I'm happy with the results.  

Moving forward, I would like to get more songs, including songs from other genres. The Song2vec model used earlier performs better the more data you feed in, and neural networks need a lot of data! I would also like to incorporate some topic modeling to try and give users a small description or tag about the theme of the playlist they've created.  

## Project Source  

The source code can be found on my [github](https://github.com/ZachHeick/Project_Kojak).
