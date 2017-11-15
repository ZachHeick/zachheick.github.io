---  
layout: post  
title: Would Reddit Like My Comment?  
---  

In today’s age of information, social media has become the most popular medium for sharing ideas and expressing one’s opinion, whether that be on politics or what the best genre of music is. But are all opinions treated equally on the internet? The fourth project at Metis focused on utilizing unsupervised and supervised learning techniques on datasets of our choice. One of my favorite social media platforms is <span style="color:#ff5700">Reddit</span>. For this project, I was specifically interested in using Natural Language Processing and classification techniques to see what information I could draw from looking at Reddit comments and their content.  

## Reddit's Comment Scoring System  

![Colbert](https://zachheick.github.io/images/Project_Fletcher_images/colbert.png){: .center-image }  

When a user makes a comment on a submission, the comment is given an intial score of 1. From there, other users can either <span style="color:#ff8b60">upvote (+1)</span> or <span style="color:#9494ff">downvote (-1)</span> the comment score. The intention of this scoring system was that users would upvote comments they found funny or the comment was relevant and contributed well to discussion, while downvote comments that were racist, sexist, or just not nice in general. Reddit sorts comment scores from greatest to least by default on all submissions, and the idea was that this upvote-downvote system would help hide nasty internet comments from discussion. It does in fact do a really good job at this.  

## The Problem with the Comment Scoring System  

![Recall Scores](https://zachheick.github.io/images/Project_Fletcher_images/echo_chamber.jpeg){: .center-image }

While the comment scoring system on Reddit is working as designed, users are also taking advantage of it as well. If someone posts a genuine comment giving an opinion that does not agree with the majority of Reddit users' opinions, that comment will be downvoted out of disagreement, driving the comment score down and eventually pushing the comment out of default visability. This results in many subreddits and submissions becoming "echo chambers"; a place where the same opinion or viewpoint is being iterated over and over again without any discussion from opposing sides.  

Note: a **subreddit** is similar to a discussion board or forum focused on a certain topic. Reddit is an aggregation of subreddits.    

With this in mind, I wanted to see if I could predict how different subreddits view comments based on the context of the comment itself.  

## Data Collection, Storage, and Cleaning    

I used PRAW (Python Reddit API Wrapper) to collect about 200,000 total comments from various subreddits:

   * /r/atheism
   * /r/hiphopheads
   * /r/politics
   * /r/science
   * /r/worldnews

Comments from each subreddit were stored into their own collection in a MongoDB database hosted by AWS. Before the comments could be vectorized for modeling, they needed to be cleaned. Raw Reddit comments are very similar to [markdown](https://help.github.com/articles/basic-writing-and-formatting-syntax/), so cleaning involved removing extra symbols, punctuation, emojis, and hyperlinks. Stop words, such as *the*, *is*, *a*, and *on* are removed as well. Many Reddit comments also reference either the submission article or quote another user. The Reddit comment notation was very inconsistent when it came to making these references within comments, so these comments ended up getting tossed out.    

Comments are going to use different forms of a word, such as *swim*, *swims*, and *swimming*. Also, slightly different words belong to families of similar meaning, like *democracy*, *democratic*, and *democratization*.  

![Lemmatization Example](https://zachheick.github.io/images/Project_Fletcher_images/lemma_example.png){: .center-image }  

The best way to handle this is a technique called **lemmatization**, the process of grouping together the inflected forms of a word so they can be analysed as a single base item. After the cleaned comments were lemmatized, they could now be vectorized. I used **tf-idf (term frequency–inverse document frequency)** to create the final matrix of feature vectors.  

## Other Features  

## Modeling  

![Recall Scores](https://zachheick.github.io/images/Project_Fletcher_images/score_vs_recall.png){: .center-image }

## Flask App  

## Final Thoughts, What I Learned, and Future Work  

## Project Source  

The source can be found on my [github](https://github.com/ZachHeick/Project_Fletcher).
