# The Effectiveness of Azir in Pro Play (2024)

The Effectiveness of Azir in Pro Play (2024) is a comprehensive data science project conducted at UCSD. It begins with exploratory data analysis and segues into hypothesis testing, the creation of a predictive model, and a fairness analysis. This project uses performance stats to reason whether Azir, one of many playable champions in the game League of Legends, is an effective pick or not in the 2024 competitive scene.

## Introduction

League of Legends (LoL) is a popular multiplayer online battle arena (MOBA) developed by Riot Games. With a playerbase of millions of people, it is the most popular esport in the world. The dataset that this project uses is maintained by Oracle’s Elixir. It contains a wealth of information from all professional LoL esports games in 2024. 

Azir is a unique, yet difficult champion known for his high damage and team fighting potential. Commonly played in the mid lane, he can summon sand soldiers and command them to attack and relocate. He can also push a large number of enemies into a direction he desires, which can turn the tides of a fight. 

Because of his unique set of abilities and the influence he can have on games, the overall question I would like to investigate is: **Is Azir an effective pick in pro play?** 

We will first note some key information from the dataset. It contains 115,836 rows, and there are some notable columns that will be used in our analyses:

* `gameid`: A unique identifier for each professional game played.
* `datacompleteness`: Denotes whether a row is partially complete or fully complete with data.
* `league`: Denotes the tournament in which the match took place.
* `year`: The year when the game was played.
* `side`: The side that a player/team played on. There are two: **blue** and **red**.
* `position`: Denotes the position that a player played. There are five: **top**, **jungle**, **mid**, **bot** (or **ADC**), and **support**.
* `playername`: The in-game name of the player that participated in the game.
* `teamname`: The team that participated in the game.
* `champion`: The name of the champion played.
* `gamelength`: The length of the game, in seconds. 
* `kills`: The number of kills a player/team scored.
* `deaths`: The number of times a player/team died. 
* `assists`: The number of assists a player/team scored. An assist is when a player contributes to defeating an enemy without dealing the final blow.
* `firstblood`: A binary column where 1 means that the player's team scored the first kill and 0 means that the player's team did not score the first kill.
* `firstbloodkill`: A binary column where 1 means that the player scored the first kill and 0 means that the player did not score the first kill.
* `damagetochampions`: The total damage a player/team dealt to enemy champions.
* `dpm`: Abbreviated for "damage per minute", which is the average damage a player/team dealt over the course of the game. 
* `totalgold`: Total gold earned by a player/team.
* `totalcs`: Total score obtained by killing minions and jungle monsters
* `result`: A binary column where 1 means that the game was won by the player/team and 0 means that the game was lost by the player/team. 

It is important to also note that each game contains twelve rows. For each of the two teams, there are five rows representing player stats. The remaining two rows are overall stats for each team.

## Data Cleaning and Exploratory Data Analysis

Before we perform any exploratory data analysis, we will keep only the columns previously listed. We will leave missing values alone for now. 

## Assessment of Missingness

### NMAR Analysis

In the dataset, it is believed that the columns `pick1`, `pick2`, `pick3`, `pick4`, and `pick5` are all data that are Not Missing at Random (NMAR). For any game, each team must pick five champions, and they do not have to be in positional order (top, jungle, mid, bottom, support). It would be reasonable to think that no games should have any missing data on the pick order of champions. Given this, it does not seem like these columns follow any trend of missingness. There is also no evidence that the missingness of the data in these columns rely on the data from other columns. 

An additional column that we could add to make these columns' data Missing At Random (MAR) is `pick_order`, which contains integers from 1 to 5 (inclusive), that represent the order that the champion was picked. 

## Hypothesis Testing



## Framing a Prediction Problem



## Baseline Model



## Final Model



## Fairness Analysis

