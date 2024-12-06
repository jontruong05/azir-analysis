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

Below is the head of the cleaned DataFrame, `cleaned`:

| gameid           | league | year | side | position | teamname           | playername | champion      | gamelength | kills | deaths | assists | damagetochampions | dpm       | totalgold | total cs | result |
|------------------|--------|------|------|----------|--------------------|------------|---------------|------------|-------|--------|---------|-------------------|-----------|-----------|----------|--------|
| LOLTMNT99_132542 | TSC    | 2024 | Blue | top      | BoostGate Esports | Taegyeong  | Renekton      | 1446       | 4     | 1      | 4       | 11806             | 489.8755  | 10762     | 210.0    | 1      |
| LOLTMNT99_132542 | TSC    | 2024 | Blue | jng      | BoostGate Esports | CHEF       | Xin Zhao      | 1446       | 5     | 1      | 11      | 12455             | 516.805   | 10895     | 154.0    | 1      |
| LOLTMNT99_132542 | TSC    | 2024 | Blue | mid      | BoostGate Esports | Nakyung    | LeBlanc       | 1446       | 1     | 3      | 8       | 14082             | 584.3154  | 9560      | 208.0    | 1      |
| LOLTMNT99_132542 | TSC    | 2024 | Blue | bot      | BoostGate Esports | Ruep       | Varus         | 1446       | 8     | 1      | 7       | 17295             | 717.6349  | 13097     | 234.0    | 1      |
| LOLTMNT99_132542 | TSC    | 2024 | Blue | sup      | BoostGate Esports | Carry      | Renata Glasc  | 1446       | 2     | 1      | 17      | 5839              | 242.2822  | 8209      | 25.0     | 1      |


### Univariate Analysis

First, let's take a look at the distribution of the players' damage dealt to champions. 

<iframe
  src="assets/dmg_plot.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

The distribution appears to be normal and skewed right, and the mean damage that players dealt to champions is around 16,000. A reason for the right skew is that there are champions who are designed to deal high damage, while there are other champions who can be useful without dealing a lot of damage.

Next, let's look at the distribution of total creep score (CS).

<iframe
  src="assets/cs_plot.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

The distribution appears to be bimodal. In a typical game of League of Legends, the support player usually forgoes killing minions and monsters, letting their teammates do it instead. This results in support players having a much lower CS compared to their teammates. Therefore, the left peak is likely representing the many support players having low CS, while the right peak is likely representing the distribution of CS of non-support players.

### Bivariate Analysis

Let's take a look at the gold distribution within each position.

<iframe
  src="assets/gold_dist_plot.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

According to the boxplots above, it appears that mid and bot laners generally earn more gold than other positions. We also see that support players generally make the least amount of gold compared to players in other positions. 

### Interesting Aggregates

Below is an aggregation that was performed on the cleaned dataset:

| position | dpm    | total cs | totalgold | kills | deaths | assists |
|----------|--------|----------|-----------|-------|--------|---------|
| mid      | 682.56 | 277.55   | 13364.12  | 3.91  | 2.69   | 5.94    |
| bot      | 680.96 | 278.89   | 13787.34  | 4.58  | 2.56   | 5.70    |
| top      | 549.95 | 246.91   | 12272.30  | 2.85  | 3.03   | 5.17    |
| jng      | 405.14 | 188.13   | 11018.00  | 2.76  | 3.16   | 7.94    |
| sup      | 204.22 | 43.19    | 811.34    | 0.91  | 3.61   | 9.83    |

The cleaned dataset was grouped by `position` and aggregated by using the mean function on the columns `dpm`, `total cs`, `totalgold`, `kills`, `deaths`, and `assists`. From this aggregated dataset, we can see that mid and bot laners are generally dealing more damage per minute, earning a higher total CS, earning more gold, scoring more kills, and having less deaths and assists. This suggests that mid and bot laners generally earn better stats than top, jungle, or support players. However, this does not necessarily mean that top, jungle, or support players are less impactful, as each player has a unique role in the game.

## Assessment of Missingness

### NMAR Analysis

In the dataset, it is believed that the columns `pick1`, `pick2`, `pick3`, `pick4`, and `pick5` are all data that are Not Missing at Random (NMAR). For any game, each team must pick five champions, and they do not have to be in positional order (top, jungle, mid, bottom, support). It would be reasonable to think that no games should have any missing data on the pick order of champions. Given this, it does not seem like these columns follow any trend of missingness. There is also no evidence that the missingness of the data in these columns rely on the data from other columns. Since Oracle's Elixir is widely consulted to conduct analyses, it is plausible that certain teams do not want to have their pick orders to be easily obtainable online. Pick orders can allow analysts from other teams to figure out the strengths, weaknesses, and strategies of certain teams. This is crucial information for opponents in the middle of a tournament, so teams may consider it beneficial to keep it as hidden as possible.

An additional column that we could add to make these columns' data Missing At Random (MAR) is `pick_order`, which contains integers from 1 to 5 (inclusive), that represent the order that the champion was picked. 

## Hypothesis Testing

The following hypothesis test aims to assess whether Azir, on average, deals more damage than other mid laners. Dealing high damage can have a positive influence in the game, so this investigation is significant in gauging Azir's effectiveness on the rift. 

**Null Hypothesis**: On average, Azir does just as much damage to champions as other mid laners.

**Alternative Hypothesis**: On average, Azir does more damage to champions as other mid laners.

**Test Statistic**: Difference in means between Azir's average damage and other mid laner's average damage.

**Significance Level**: 5%

A permutation test was run to test the two hypotheses. The **observed statistic** for this test is **642.7805324519832**. Below is the empirical distribution of the test statistic.

<iframe
  src="assets/dmg_emp_dist.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

The permutation test yielded a **p-value** of **0.01521**, so we **reject the null hypothesis**. The results suggest that, on average, Azir does more damage to champions compared to other mid laners. This finding may bring to consideration that for mid laners, dealing high damage is one of many ways to be effective in a game. 

## Framing a Prediction Problem



## Baseline Model



## Final Model



## Fairness Analysis

