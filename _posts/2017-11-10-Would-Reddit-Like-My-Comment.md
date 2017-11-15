---  
layout: post  
title: Would Reddit Like My Comment?  
---  

In today’s age of information, social media has become the most popular medium for sharing ideas and expressing one’s opinion, whether that be on politics or what the best genre of music is. But are all opinions treated equally on the internet? The fourth project at Metis focused on utilizing unsupervised and supervised learning techniques on datasets of our choice. One of my favorite social media platforms is <span style="color:#ff5700">Reddit</span>. For this project, I was specifically interested in using Natural Language Processing and classification techniques and seeing what information I could draw from looking at Reddit comments and their content.  

## Reddit's Comment Scoring System  

![Colbert](https://zachheick.github.io/images/Project_Fletcher_images/colbert.png){: .center-image }  

When a user makes a comment on a submission, they are given an intial score of 1. From there, other users can either <span style="color:#ff8b60">upvote (+1)</span> or <span style="color:#9494ff">downvote (-1)</span> the comment score. The intention of this scoring system was that users would upvote comments they found funny or the comment was relevant and contributed well to discussion, while downvoting comments that were racist, sexist, or just not nice in general. Reddit sorts comment scores from greatest to least by default on all submissions, and the idea was that this upvote-downvote system would help hide nasty internet comments from discussion and promote positive comments. It does in fact do a really good job at this.  

## The Problem with the Comment Scoring System  

![Recall Scores](https://zachheick.github.io/images/Project_Fletcher_images/echo_chamber.jpeg){: .center-image }

While the comment scoring system on Reddit is working as designed, users are also taking advantage of it as well. If someone posts a genuine comment giving an opinion that does not agree with the majority of Reddit users' opinions, that comment will be downvoted out of disagreement, driving the comment score down and eventually pushing the comment out of default visability. This results in many subreddits and submissions becoming "echo chambers"; a place where the same opinion or viewpoint is being iterated over and over again without any discussion from opposing sides.  

With this in mind, I wanted to see if I could predict how different subreddits view comments based on the context of the comment itself.  

## Data Collection and Storage  

I used PRAW (Python Reddit API Wrapper) to collect about 200,000 comments from various subreddits:

  * /r/atheism
  * /r/hiphopheads
  * /r/politics
  * /r/science
  * /r/worldnews

  * Apples
  * Oranges
  * Potatoes
  * Milk

  1. Mow the lawn
  2. Feed the dog
  3. Dance

Continue...  

## Modeling  

![Recall Scores](https://zachheick.github.io/images/Project_Fletcher_images/score_vs_recall.png){: .center-image }

## Final Thoughts, What I Learned, and Future Work  

## Project Source  

The source can be found on my [github](https://github.com/ZachHeick/Project_Fletcher).
