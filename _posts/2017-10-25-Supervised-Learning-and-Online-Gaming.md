---   
layout: post   
title: Supervised Learning and Online Gaming   
---   

In recent years, the professional video game scene has seen huge growth in both revenue and online stream numbers. Teams are competing for huge cash prizes, and with so much money on the line, these teams are now starting to use data science in order to provide themselves with the best win conditions for the respective game they're playing. The third project at Metis concentrated on exploring multiple supervised learning techniques to see what we could learn from datasets of our choice.  

![League of Legends Crowd](https://zachheick.github.io/images/Project_McNulty_images/lol_crowd.jpg){: .center-image }  

With this in mind, I was interested in exploring match data from the most popular game in the world, *League* *of* *Legends*. I was curious to see if I could predict the winner of a match based on what champions each team picked and banned, as well as what objectives in the game were taken first.  

## What is League of Legends?  

![League of Legends Screenshot](https://zachheick.github.io/images/Project_McNulty_images/lol_screenshot.jpg){: /center-image }  

League of Legends is a multiplayer online battle arena (MOBA) video game published by Riot Games. The game consists of two teams of five human players. Each human player controls a champion with unique abilities and fights against the team of other players. Players can also ban a champion away from the other team, preventing that champion from being picked. The object of the game is destroy the opposing team's nexus, the final structure located in a base surrounded by other defensive structures that must be destroyed prior. Each League of Legends match is discrete.  

#### Terminology  

  * **Champion**: a player controlled character with unique abilities 
  * **Summoner Spell**: extra spells each champion gets, not unique to champion
  * **First Blood**: the first player kill of the game
  * **Tower**: the outermost defensive structure damaging enemies that get too close, protects the inhibitors and nexus
  * **Inhibitor**: defensive structure located before the nexus, doing no damage to enemies
  * **Nexus**: final defensive structure in a team's base, doing no damage to enemies
  * **Baron, Dragon, and Rift Herald**: neutral objectives located around the game map that teams can kill for in-game stat boosts

## Data and Assumptions  

The data I used for this project was downloaded from Kaggle. All datasets were in CSV format and contained in-game stats and outcomes from 51490 ranked matches played on game version 7.17. The datasets also included descriptions about the 138 champions players can choose from. Data was stored in a PostgreSQL database.  

In order to focus on the game itself and not have to take into account the player behind the computer screen, I made a few assumptions:  

  1. The matches are played on the same ranked scale  
  2. Each player is playing their champion to full capacity
  3. Each player knows how to play all 138 champions at equal skill level  

## Feature Selection  

## Exploratory Analysis  

![Picks and Bans by Champion Role](https://zachheick.github.io/images/Project_McNulty_images/picks_and_bans_by_role.png){: .center-image}  

![Champion Pick Count and Win Rate](https://zachheick.github.io/images/Project_McNulty_images/champion_pick_count_and_win_rate.png){: .center-image}  

![Most Picked Champions](https://zachheick.github.io/images/Project_McNulty_images/most_picked_champions.png){: .center-image}  

![Most Banned Champions](https://zachheick.github.io/images/Project_McNulty_images/most_banned_champions.png){: .center-image}  

![Most Picked Champions with Ban Rate](https://zachheick.github.io/images/Project_McNulty_images/most_picked_champions_with_bane_rate.png){: .center-image}  

![Summoner Spell Usage](https://zachheick.github.io/images/Project_McNulty_images/summoner_spell_usage.png){: .center-image}  
