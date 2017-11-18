---  
layout: post  
title: Would Reddit Like My Comment?  
---  

In today’s age of information, social media has become the most popular medium for sharing ideas and expressing one’s opinion, whether that be on politics or what the best genre of music is. But are all opinions treated equally on the internet? The fourth project at Metis focused on utilizing unsupervised and supervised learning techniques on datasets of our choice. One of my favorite social media platforms is <span style="color:#ff5700">Reddit</span>. For this project, I was specifically interested in using Natural Language Processing and classification techniques to see what information I could draw from looking at Reddit comments and their content.  

## Reddit's Comment Scoring System  

![Colbert](https://zachheick.github.io/images/Project_Fletcher_images/colbert.png){: .center-image }  

Reddit is an aggregation of **subreddits**, which are similar to discussion boards or forums focused on a certain topic. When a user makes a comment on a subreddit submission, the comment is given an intial score of 1. From there, other users can either <span style="color:#ff8b60">upvote (+1)</span> or <span style="color:#9494ff">downvote (-1)</span> the comment score.  

The intention of this scoring system was that users would upvote comments they found funny or the comment was relevant and contributed well to discussion, while downvote comments that were racist, sexist, or just not nice in general. Reddit sorts comment scores from greatest to least by default on all submissions, and the idea was that this upvote-downvote system would help hide nasty internet comments from discussion. It does in fact do a really good job at this.  

## The Problem with the Comment Scoring System  

![Recall Scores](https://zachheick.github.io/images/Project_Fletcher_images/echo_chamber.jpeg){: .center-image }

While the comment scoring system on Reddit is working as designed, users are also taking advantage of it as well. If someone posts a genuine comment giving an opinion that does not agree with the majority of Reddit users' opinions, that comment will be downvoted out of disagreement by these users, driving the comment score down and eventually pushing the comment out of default visability. This results in many subreddits and submissions becoming "echo chambers"; a place where the same opinion or viewpoint is being iterated over and over again without any discussion from opposing sides.  

With this in mind, I wanted to see if I could predict how different subreddits view comments based on the context of the comment itself.  

## Data Collection, Storage, and Cleaning    

I used PRAW (Python Reddit API Wrapper) to collect about 200,000 total comments from various subreddits:

   * /r/atheism
   * /r/hiphopheads
   * /r/politics
   * /r/science
   * /r/worldnews

These were subreddits that I found to be very popular, but also very opinionated. Comments from each subreddit were stored into their own collection in a MongoDB database hosted by AWS. Before the comments could be vectorized for modeling, they needed to be cleaned. Raw Reddit comments are very similar to [markdown](https://help.github.com/articles/basic-writing-and-formatting-syntax/), so cleaning involved removing extra symbols, punctuation, emojis, and hyperlinks. Stop words, such as *the*, *is*, *a*, and *on* were removed as well. Many Reddit comments also reference either the submission article or quote another user. The Reddit comment notation was very inconsistent when it came to making these references within comments, so these type of comments ended up getting tossed out.    

Comments are going to use different forms of a word, such as *swim*, *swims*, and *swimming*. Also, slightly different words belong to families of similar meaning, like *democracy*, *democratic*, and *democratization*.  

![Lemmatization Example](https://zachheick.github.io/images/Project_Fletcher_images/lemma_example.png){: .center-image }  

The best way to handle these words and meanings is a technique called **lemmatization**, the process of grouping together the inflected forms of a word so they can be analysed as a single base item. After the cleaned comments were lemmatized, they could now be vectorized. Finally, I used **tf-idf (term frequency–inverse document frequency)** to create the final matrix of feature vectors.  

## Sentiment Analysis and Other Features  

I used the [TextBlob](https://textblob.readthedocs.io/en/dev/) library for sentiment analysis. For each Reddit comment, I looked at the objectivity and subjectivity of each comment, where a value of 0.0 is very objective and 1.0 is very subjective. I also looked at the polarity of the comment was by sentence, where -1.0 is very negative and 1.0 is very positive. Other comment metadata I included was comment character count and word count. I combined the values from sentiment analysis and comment metadata with the matrix created previously to get my final data matrix for modeling.   

## Modeling  

Before I could begin modeling, I needed to bin and classify the comment scores. To do this, I set a score threshold of 50 points, and said that if a comment score was 50 or below, the subreddit did not like the comment. If the comment was greater than 50, the subreddit did like the comment. The classified comment scores were unbalanced, so I used a Decision Tree to handle classification. I used recall because I wanted to minimize false negatives, meaning I did not want to misclassify as "not liked" when the comment should have been "liked".  

![Recall Scores](https://zachheick.github.io/images/Project_Fletcher_images/score_vs_recall.png){: .center-image }  

Once I got my pipeline down, it was easy to feed in comments from a subreddits and create a Decision Tree model for that subreddit with the best parameters. A dummy model was also created for comparison, where the dummy guessed "not liked" for every comment. 

## Final Thoughts, What I Learned, and Future Work  

I really enjoyed working with some NLP techniques, and seeing how words and language could be converted to numerical values with meaning. The models for each subreddit all out-performed their respective dummy model, which was great, but the final recall scoresvaried greatly.  

One idea for improving recall score would be to analyze and include the title of the submission or article that comments were discussing. For this project I generalized comments with subreddit, and adding in the title of posts could have made a positive difference. Comments on Reddit could have replies to them, and those replies could also have replies, and so on and so forth. In this project I only collected comments that were *not* replies, just general comments. Including comment replies might also help improve recall score.  

There is definitely room for improvements in the cleaning and sentiment analysis parts of the project. Reddit and social media comments are filled with all sorts of slang, abbreviations, and weird punctuation. Focusing more on cleaning comments and using a different sentiment analysis library could handle these comments better. Anomaly detection algorithms would also be appropriate for this project, as highly scored comments on a submission are rare compared to the total number of comments.  

## Project Source  

The source can be found on my [github](https://github.com/ZachHeick/Project_Fletcher).
