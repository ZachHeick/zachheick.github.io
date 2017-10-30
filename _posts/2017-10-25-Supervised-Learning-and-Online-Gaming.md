---   
layout: post   
title: Supervised Learning and Online Gaming   
---   

In recent years, the professional video game scene has seen huge growth in both revenue and online stream numbers. Teams are competing for huge cash prizes, and with so much money on the line, these teams are now starting to use data science in order to provide themselves with the best win conditions for the respective game they're playing. The third project at Metis concentrated on exploring multiple supervised learning techniques to see what we could learn from datasets of our choice.  

![League of Legends Crowd](https://zachheick.github.io/images/Project_McNulty_images/lol_crowd.jpg){: .center-image }  

With this in mind, I was interested in exploring match data from the most popular game in the world, *League* *of* *Legends*. I was curious to see if I could predict the winner of a match based on what champions each team picked and banned, as well as what objectives in the game were taken first.  

## What is League of Legends?  

![League of Legends Screenshot](https://zachheick.github.io/images/Project_McNulty_images/lol_screenshot.jpg){: /center-image }  

**League of Legends** is a multiplayer online battle arena (MOBA) video game published by Riot Games. The game consists of two teams of five human players. Each human player controls a champion with unique abilities and fights against the team of other players. The object of the game is destroy the opposing team's nexus, the final structure located in a base surrounded by other defensive structures that must be destroyed prior. Each League of Legends match is discrete.  

#### Terminology  

  * **Champion**: a player controlled character with unique abilities 
  * **Summoner Spell**: extra spells each champion gets, not unique to champion
  * **First Blood**: the first player kill of the game
  * **Tower**: the outermost defensive structure damaging enemies that get too close, protects the inhibitors and nexus
  * **Inhibitor**: defensive structure located before the nexus, doing no damage to enemies
  * **Nexus**: final defensive structure in a team's base, doing no damage to enemies
  * **Baron, Dragon, and Rift Herald**: neutral objectives located around the game map that teams can kill for in-game stat boosts

## Data and Assumptions  

The data I used for this project was downloaded from Kaggle and was in CSV format. The first dataset contained in-game stats and outcomes from 51490 ranked matches played on game version 7.17. These matches are *not* from professional play, but instead matches from regular players. This dataset was also a near perfect 50/50 balance. The other dataset included descriptions about the 138 champions players can choose from. Data was stored in a PostgreSQL database.  

In order to focus on the game itself and not have to take into account the player behind the computer screen, I made a few assumptions:  

  1. The matches are played on the same ranked scale  
  2. Each player is playing their champion to full capacity
  3. Each player knows how to play all 138 champions at equal skill level  

## Exploratory Analysis and Feature Selection  

#### Champions and their Roles  

In League of Legends there are two teams, a <span class="blue">Blue team</span> and a <span class="red">Red team</span>. The team name simply determines the starting point of each team on the game map. There are five players on each team who can pick and control their own champion. Before players pick the champion they want to play, they are allowed to ban one champion away from the other team. This means that if a player on Blue team bans a champion, no one on the Red team can select that champion to play, and vice versa. Deciding between which champions to pick and ban from a pool of 138 champions can give your team distinct strengths and weaknesses in-game.

I wanted to explore the pick and ban rates of champions and their role, and examine their win rates to see if their are certain champions dominating the game.  

![Champion Pick Count and Win Rate](https://zachheick.github.io/images/Project_McNulty_images/champion_pick_count_and_win_rate.png){: .center-image}  

Most champions seem to have a win rate between 48% and 54%, with the exception of a few outliers. It also looks like mages have both the highest variance in win rate and overall lowest pick count, while pick count varies for the other roles.  

![Most Picked Champions](https://zachheick.github.io/images/Project_McNulty_images/most_picked_champions.png){: .center-image}  

Champions like Thresh and Lee Sin are popular because of how easily they fit into any team composition. It was surprising to see how popular the marksman role was.  

![Most Banned Champions](https://zachheick.github.io/images/Project_McNulty_images/most_banned_champions.png){: .center-image}  

All of the top banned champions for version 7.17 have strong survivability when it comes to team fighting and/or the ability to kill other champions very easily.

![Most Picked Champions with Ban Rate](https://zachheick.github.io/images/Project_McNulty_images/most_picked_champions_with_ban_rate.png){: .center-image}  

Janna and Twitch were both so popular and banned so frequently because of how well each of their abilities complimented each others. Also, there were certain items on version 7.17 that increased the strength of their abilities dramatically.  

#### In-game Objectives  

Once a match has started, there are multiple objectives that must be destroyed before reaching the nexus. Players must work together and decide which of these objectives should be taken first. Taking objectives first gives a team huge momentum in planning their next move and can be crucial to winning the match. 

#### Feature Vectors and Target

The feature vector included:  

  * The five picked champions and five banned champions for each team
  * The first team to get a kill  
  * The first team to destroy a turret  
  * The first team to kill the dragon  
  * The first team to kill Rift Herald  

The team champion columns were represented by the numeric ID of the champion. The objective columns contained value 0, 1, or 2. Value 0 represents no team getting this objective, and 1 and 2 for Blue team and Red team. The target value was Blue team or Red Team.  

## Predicting Match Outcomes  

#### Process and Algorithms  

Once the data was cleaned and I had the features selected, I explored multiple classification algorithms to use for my final prediction model. For each algorithm, I selected and tuned the hyperparameter that returned the best accuracy score using Grid Search Cross Validation. I then plotted the learning curve of the algorithm using the best parameter value and recorded the final accuracy score of the algorithm. The algorithms and their hyperparameters:  

  * Support Vector Machine: Budget  
  * Decision Tree: Tree Depth and Criterion
  * Random Forest: Depth and number of trees  
  * Logistic Regression: Regularization Strength  
  * Bernoulli Naive Bayes: None    

#### Algorithm Analysis  

![ROC Curves](https://zachheick.github.io/images/Project_McNulty_images/roc_curve.png){: .center-image}  

I first plotted the ROC cruve of each algorithm, but the curves were *very* similar.  

![Boxplot](https://zachheick.github.io/images/Project_McNulty_images/boxplot.png){: .center-image}  

I then used boxplots to get a better understanding of how the accuracy scores from cross validation were distributed. These scores are from each algorithm's best parameter values in terms of accuracy.    

![Final Dataframe](https://zachheick.github.io/images/Project_McNulty_images/final_dataframe.png){: .center-image}  

I also was curious to see which games each algorithm was classifying incorrectly, in case I could either ensemble algorithms or one was performing completely different from the rest. I created a dataframe where each column represents an algorithm and the cell values are game IDs that were incorrectly classified.  

Each algorithm seems to be nearly identical when it comes to their ROC curve, boxplot, and the games that are classified incorrectly. Because of this, I could not ensemble multiple algorithms for my final model, so how do I even choose one when they're all *so* similar?  It came down to three categores:  

  * Accuracy of the algorithm  
  * Interpretability  
  * Computation Speed  

In terms of accuracy, all algorithms performed identically, give or take 1%. Interpretability and computation speed is where algorithms started to differ. For this project, **Support Vector Machine** was the easiest to interpret and was relatively quick when it came to computing speed.  

![SVM Accuracy for C](https://zachheick.github.io/images/Project_McNulty_images/svm_accuracy.png){: .center-image}  

A graph of the accuracies of varying budget (C) values for SVM. A value of 0.0001 gave an accuracy of 71.7% during cross validation.  

![SVM Learning Curve](https://zachheick.github.io/images/Project_McNulty_images/svm_learning_curve.png){: .center-image}  

The learning curve for SVM. The test accuracy levels off at around 72% with 22000 samples.  

## Final Thoughts, What I learned, and Future Work  



## Project Source  

The source can be on my [github](https://github.com/ZachHeick/Project_McNulty).
