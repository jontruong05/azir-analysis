# The Effectiveness of Azir in Pro Play (2024)

The Effectiveness of Azir in Pro Play (2024) is a comprehensive data science project conducted at UCSD. 
It begins with exploratory data analysis and segues into hypothesis testing, the creation of a predictive model, and a fairness analysis. 
This project uses performance stats to reason whether Azir, one of many playable champions in the game League of Legends, is an effective pick or not in the 2024 competitive scene.

## Introduction

League of Legends (LoL) is a popular multiplayer online battle arena (MOBA) developed by Riot Games. 
With a playerbase of millions of people, it is the most popular esport in the world. 
The dataset that this project uses is maintained by Oracle's Elixir. 
It contains a wealth of information from all professional LoL esports games in 2024. 

Azir is a unique, yet difficult champion known for his high damage and team fighting potential. 
Commonly played in the mid lane, he can summon sand soldiers and command them to attack and relocate. 
He can also push a large number of enemies into a direction he desires, which can turn the tides of a fight. 

Because of his unique set of abilities and the influence he can have on games, the overall question I would like to investigate is: **Is Azir an effective pick in pro play?** 

We will first note some key information from the dataset. It contains 115,836 rows, and there are some notable columns that will be used in our analyses:

* `gameid`: A unique identifier for each professional game played.

* `year`: The year when the game was played.

* `side`: The side that a player/team played on. There are two sides: **blue** and **red**.

* `position`: Denotes the position that a player played. There are five: **top**, **jungle**, **mid**, **bot** (or **ADC**), and **support**.

* `champion`: The name of the champion played.

* `gamelength`: The length of the game, in seconds. 

* `kills`: The number of kills a player/team scored.

* `deaths`: The number of times a player/team died. 

* `assists`: The number of assists a player/team scored. An assist is when a player 
contributes to defeating an enemy without dealing the final blow.

* `killsat25`: The number of kills a player scored at 25 minutes. 

* `damagetochampions`: The total damage a player/team dealt to enemy champions.

* `dpm`: Abbreviated for "damage per minute", which is the average damage a player/team dealt 
over the course of the game. 

* `totalgold`: Total gold earned by a player/team.

* `totalcs`: Total score obtained by killing minions and jungle monsters.

* `result`: A binary column where 1 means that the game was won by the player/team and 0 means that the game was lost by the player/team. 

It is important to also note that each game contains twelve rows. For each of the two teams, there are five rows representing player stats. The remaining two rows are overall stats for each team.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

Before we perform any exploratory data analysis, we will keep only the columns previously listed except `killsat25`. 
It is important to note that there are missing values for the columns `champion` and `total cs`. 
The missingness in the `champion` and `total cs` columns can be attributed to the fact that there are rows providing team stats rather than player stats. 
We can simply drop these rows since we are most interested in player stats.
This leaves us with 93,870 rows of player stats to work with.

Below is the head of the cleaned DataFrame, `cleaned`:

| gameid           | year | side | position | champion      | gamelength | kills | deaths | assists | damagetochampions | dpm       | totalgold | total cs | result |
|------------------|------|------|----------|---------------|------------|-------|--------|---------|-------------------|-----------|-----------|----------|--------|
| LOLTMNT99_132542 | 2024 | Blue | top      | Renekton      | 1446       | 4     | 1      | 4       | 11806             | 489.8755  | 10762     | 210.0    | 1      |
| LOLTMNT99_132542 | 2024 | Blue | jng      | Xin Zhao      | 1446       | 5     | 1      | 11      | 12455             | 516.805   | 10895     | 154.0    | 1      |
| LOLTMNT99_132542 | 2024 | Blue | mid      | LeBlanc       | 1446       | 1     | 3      | 8       | 14082             | 584.3154  | 9560      | 208.0    | 1      |
| LOLTMNT99_132542 | 2024 | Blue | bot      | Varus         | 1446       | 8     | 1      | 7       | 17295             | 717.6349  | 13097     | 234.0    | 1      |
| LOLTMNT99_132542 | 2024 | Blue | sup      | Renata Glasc  | 1446       | 2     | 1      | 17      | 5839              | 242.2822  | 8209      | 25.0     | 1      |


### Univariate Analysis

First, let's take a look at the distribution of the players' damage dealt to champions. 

<iframe
  src="assets/dmg_plot.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

The distribution appears to be normal and skewed right, and the mean damage that players dealt to champions is around 16,000. 
One reason for the right skew is that there are champions who are designed to deal high damage, while there are other champions who can be useful without dealing a lot of damage.
Another reason for the right skew is that longer games provide more opportunities for fights, so players may be dealing more damage simply because there are some games that are longer.

Next, let's look at the distribution of total creep score (CS).

<iframe
  src="assets/cs_plot.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

The distribution appears to be bimodal. 
In a typical game of League of Legends, the support player usually forgoes killing minions and monsters, letting their teammates do it instead. 
This results in support players having a much lower CS compared to their teammates. 
Therefore, the left peak is likely representing support players having lower CS, while the right peak is likely representing non-support players having higher CS.

### Bivariate Analysis

Let's take a look at the gold distribution within each position.

<iframe
  src="assets/gold_dist_plot.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

According to the boxplots above, it appears that mid and bot laners generally earn more gold than other positions. 
We also see that support players generally make the least amount of gold compared to players in other positions. 

### Interesting Aggregates

Below is an aggregation that was performed on the cleaned dataset:

| position | dpm    | total cs | totalgold | kills | deaths | assists |
|----------|--------|----------|-----------|-------|--------|---------|
| mid      | 682.56 | 277.55   | 13364.12  | 3.91  | 2.69   | 5.94    |
| bot      | 680.88 | 278.90   | 13787.41  | 4.58  | 2.56   | 5.70    |
| top      | 549.95 | 246.91   | 12272.30  | 2.85  | 3.03   | 5.17    |
| jng      | 405.16 | 188.13   | 11018.33  | 2.76  | 3.16   | 7.94    |
| sup      | 204.22 | 43.20    | 8114.54   | 0.91  | 3.61   | 9.83    |

The cleaned dataset was grouped by `position` and aggregated by using the mean function on the columns `dpm`, `total cs`, `totalgold`, `kills`, `deaths`, and `assists`. 
From this aggregated dataset, we can see that mid and bot laners are generally dealing more damage per minute, earning a higher total CS, earning more gold, scoring more kills, and having less deaths and assists. 
This suggests that mid and bot laners generally earn better stats than top, jungle, or support players. 
However, this does not necessarily mean that top, jungle, or support players are less impactful, as each player has a unique role in the game.

## Assessment of Missingness

### NMAR Analysis

In the original dataset, the columns `ban1`, `ban2`, `ban3`, `ban4`, and `ban5` appear to be data that are Not Missing at Random (NMAR).
The missingness of these columns do not appear to be tied to other columns in the dataset, but the missingness does not appear to be random, either.
Before the game starts, each team is allowed to ban at most five champions. 
Most teams would ideally use all five bans so they can avoid having to play against champions they struggle with.
However, some teams may be feeling confident or simply forget to ban. 
Therefore, the missingness of these values actually depend on themselves.
To turn these columns into data that are Missing at Random (MAR), we can craft a binary column called `fully_banned`, where 1 represents that a team used all of their bans and 0 represents that a team did not use all of their bans.
This addition makes the missingness of `ban1`, `ban2`, `ban3`, `ban4`, and `ban5` reliant on `fully_banned`. 

### Missingness Dependency

We will perform some permutation tests to determine whether the missing data in the column `killsat25` depends on other columns.

First, let's see if `killsat25` depends on the column `gamelength`.

**Null Hypothesis**: The distribution of `gamelength` when `killsat25` is missing is the same as the distribution of `gamelength` when `killsat25` is not missing.

**Alternative Hypothesis**: The distribution of `gamelength` when `killsat25` is missing is NOT the same as the distribution of `gamelength` when `killsat25` is not missing.

**Test Statistic**: Absolute difference in means between `gamelength` with missing `killsat25` values and `gamelength` with non-missing `killsat25` values.

**Significance Level**: 5%

Below is a plot that shows the empirical distribution of the absolute differences in means.

<iframe
  src="assets/missingness_1.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

The observed absolute difference in means is **177.12205775476036**. 
This permutation test yielded a **p-value** of **0**, so we reject the null hypothesis.
This means that the missingness of `killsat25` relies on `gamelength`. 

Next, let's see if `killsat25` depends on the column `side`. 

**Null Hypothesis**: The distribution of `side` when `killsat25` is missing is the same as the distribution of `side` when `killsat25` is not missing.

**Alternative Hypothesis**: The distribution of `side` when `killsat25` is missing is NOT the same as the distribution of `side` when `killsat25` is not missing.

**Test Statistic**: Total Variation Distance

**Significance Level**: 5%

Below is a plot that shows the empirical distribution of the TVDs.

<iframe
  src="assets/missingness_2.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

The observed TVD is **0**. 
This permutation test yielded a **p-value** of **1**, so we fail to reject the null hypothesis.
This means that the missingness of `killsat25` does not rely on `side`. 

## Hypothesis Testing

The following hypothesis test aims to assess whether Azir, on average, deals more damage than other mid laners. 
Dealing high damage is one way to be impactful in the game, so this investigation is significant in gauging Azir's effectiveness on the rift. 

**Null Hypothesis**: Azir does just as much damage to champions as other mid laners.

**Alternative Hypothesis**: Azir does more damage to champions as other mid laners.

**Test Statistic**: Difference in means between Azir's average damage and other mid laners' average damage.

**Significance Level**: 5%

A permutation test was run to test the two hypotheses. 
The **observed statistic** for this test is **642.7805324519832**. 
Below is the empirical distribution of the test statistic.

<iframe
  src="assets/dmg_emp_dist.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

The permutation test yielded a **p-value** of **0.01521**, so we **reject the null hypothesis**. 
The results suggest that Azir may be dealing more damage to champions compared to other mid laners in professional games. 
These findings may bring to consideration that the Azir may be one of the more effective mid laners to play professionally. 

## Framing a Prediction Problem

In the previous section, we discovered that Azir may be dealing more damage than many other mid laners in pro play. 
Since dealing damage is one way of being effective in a game of League of Legends, it may be worth considering if Azir's damage dealt to champions is a good indicator of determining his chances of winning. 

Machine learning techniques can be used for the following prediction problem: **Can we predict if Azir wins or loses a game based on his game statistics?**

For this problem, the model will be a binary classifier. 
The response variable will be `result`, which indicates whether Azir's game was won or lost. 
The `result` column of the dataset is used as the response variable because game statistics are representative of performance, and it is reasonable to think that high performance can influence game outcomes. 
We will use model accuracy to evaluate the binary classifier. 
We use model accuracy because we are not working with imbalanced data–the difference between the number of games won and the number of games lost is not very large.

At the time of prediction, we only know the following information about each of Azir's games: `kills`, `deaths`, `assists`, `damagetochampions`, `totalgold`, `totalcs`, and `side`. 
The binary classifier will be trained on these features.

## Baseline Model



## Final Model



## Fairness Analysis

