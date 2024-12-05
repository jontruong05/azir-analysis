# The Effectiveness of Azir in Pro Play (2024)

The Effectiveness of Azir in Pro Play (2024) is a comprehensive data science project conducted at UCSD. It begins with exploratory data analysis and segues into hypothesis testing, the creation of a predictive model, and a fairness analysis. This project uses performance stats to reason whether Azir, one of many playable champions in the game League of Legends, is an effective pick or not in the 2024 competitive scene.

## Introduction

League of Legends (LoL) is a popular multiplayer online battle arena (MOBA) developed by Riot Games. With a playerbase of millions of people, it is the most popular esport in the world. The dataset that this project uses is maintained by Oracle’s Elixir. It contains a wealth of information from all professional LoL esports games in 2024. 

Azir is a unique, yet difficult champion known for his high damage and team fighting potential. Commonly played in the mid lane, he can summon sand soldiers and command them to attack and relocate. He can also push a large number of enemies into a direction he desires, which can turn the tides of a fight. 

Because of his unique set of abilities and the influence he can have on games, the overall question I would like to investigate is: **Is Azir an effective pick in pro play?** 

We will first note some key information from the dataset. It contains 115,836 rows, and there are some notable columns that will be used in our analyses:

* `gameid`: A unique identifier for each professional game played.

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

* `assists`: The number of assists a player/team scored. An assist is when a player 
contributes to defeating an enemy without dealing the final blow.

* `firstblood`: A binary column where 1 means that the player's team scored the first kill 
and 0 means that the player's team did not score the first kill.

* `firstbloodkill`: A binary column where 1 means that the player scored the first kill and 0 
means that the player did not score the first kill.

* `damagetochampions`: The total damage a player/team dealt to enemy champions.

* `dpm`: Abbreviated for "damage per minute", which is the average damage a player/team dealt 
over the course of the game. 

* `totalgold`: Total gold earned by a player/team.

* `totalcs`: Total score obtained by killing minions and jungle monsters

* `result`: A binary column where 1 means that the game was won by the player/team and 0 means that the game was lost by the player/team. 

It is important to also note that each game contains twelve rows. For each of the two teams, there are five rows representing player stats. The remaining two rows are overall stats for each team.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

Before we perform any exploratory data analysis, we will keep only the columns previously listed. If necessary, we will add other columns later on in the analysis and process it accordingly. There are missing values for the columns `playername`, `champion`, and `total cs`. The missingness in the `champion` and `total cs` columns can be attributed to the rows that provide team stats rather than player stats. The majority of the missingness of `playername` can also be attributed to the same reason, but there appears to be some rows representing player stats that has the player's name missing. There are only 18 such rows, so we will drop all rows that have any missing data. This leaves us with 93,852 rows of player stats to work with.

### Univariate Analysis

First, let's take a look at the distribution of the players' damage dealt to champions. 

<iframe
  src="assets/dmg_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/cs_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

<iframe
  src="assets/gold_dist_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Interesting Aggregates

## Assessment of Missingness

### NMAR Analysis

In the dataset, it is believed that the columns `pick1`, `pick2`, `pick3`, `pick4`, and `pick5` are all data that are Not Missing at Random (NMAR). For any game, each team must pick five champions, and they do not have to be in positional order (top, jungle, mid, bottom, support). It would be reasonable to think that no games should have any missing data on the pick order of champions. Given this, it does not seem like these columns follow any trend of missingness. There is also no evidence that the missingness of the data in these columns rely on the data from other columns. Since Oracle's Elixir is widely consulted to conduct analyses, it is plausible that certain teams do not want to have their pick orders to be easily obtainable online. Pick orders can allow analysts from other teams to figure out the strengths, weaknesses, and strategies of certain teams. This is crucial information for opponents in the middle of a tournament, so teams may consider it beneficial to keep it as hidden as possible.

An additional column that we could add to make these columns' data Missing At Random (MAR) is `pick_order`, which contains integers from 1 to 5 (inclusive), that represent the order that the champion was picked. 

## Hypothesis Testing



## Framing a Prediction Problem



## Baseline Model



## Final Model



## Fairness Analysis

