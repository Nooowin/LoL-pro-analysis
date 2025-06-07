## League of Legends: Analyzing Pro Side Selection
League of Legends: Analyzing Pro Side Selection is a data science project done at UCSD that goes through the complete data science process - from question formulation and data cleaning to EDA (Exploratory Data Analysis) and predictive modeling. We look at starting side - blue and red - and how this can influence gameplay, match statistics, and outcomes.

Authors: Vinson Nguyen

### Introduction
**League of Legends** is a Multiplayer Online Battle Arena (MOBA) developed by Riot games where two teams of 5 fight to destroy each other's base, or "Nexus." It is a widely known game and there are many professional leagues that compete in this esport, leading to the creation of the data set that we will be working with from Oracle's Elixer. We will specifically be looking at the 2025 data set, which can be found [here](https://oracleselixir.com/tools/downloads)

The primary question we are looking at: **Is there a statistically significant blue side advantage for pro League of Legends players in 2025, and what factors contribute to this?**
Side selection may seem like a small detail, but at the highest level, these finer details such as improved camera perspective, first pick selection, and easier access to objectives can have massive impacts on pro play. This project seeks to truly understand if there is any statistically significant advantage to blue side. 
The data and statistics here will segway into our predictive model in which we will predict if an Attack Damage Carry (ADC) has the in-game statistics to "carry" his team to victory.
These questions highlight the metrics that truly go into success in a LoL game and has the potential to improve team-strategy and offer up a better understanding of fairness and balance in the game.

#### Dataset
The dataset proves team-level and player-level metrics for pro players. In the 2025 dataset, there are 62292 rows and 161 columns. Here are some of the key columns that we will need for our project:
- `gameid`: unique identifer for a LoL match
- `side`: which side the team started on - either "blue" or "red"
- `position`: in-game role of a player: "top", "mid", "bot"/ADC, "jng", and "sup"
- `result`: whether a team won (1) or lost (0)
- `kills`: number of enemy champions a player killed/eliminated
- `deaths`: number of times a player was killed/eliminated
- `assists`: number of times a player contributed to killing/eliminating an enemy champion without getting the kill themselves
- `damageshare`: percentage of the team's total damage dealt done by a single player
- `towers`: number of enemy turrets destroyed by a team
- `dragons`: number of dragons secured by a team
- `heralds`: number of heralds (Rift Heralds) secured by a team, usually a good early-game objective
- `barons`: number of barons (Baron Nashors) secured by a team, usually a good late-game objective
- `earnedgold`: total gold earned by a player
- `goldat25`: total gold a player had at the 25-minute mark
- `gamelength`: how long the match was

### Data Cleaning and Exploratory Data Analysis
#### Data Cleaning
We will make a new dataframe called **final_LoL** with the columns mentioned above. We will remove games labeled with "partial" in the `datacompleteness` column. These rows have missing data in multiple columns, so we will just remove them entirely. In the original dataset, each `gameid` corresponds to 12 rows - one for each of the 5 players of each team and 2 with team-level summary statistics. For the majority of this project, we will be working with primarily team-level statistics, so any rows corresponding to individual players or champions will be dropped. 
When inspecting the dataframe, we can see that `towers`, `dragons`, and `heralds` have similar missing mechanisms. Each of the statistics corresponding to a individual for these columns are missing and are just filled in the team-level summaries. To achieve a dataframe with only team-level summary statistics, we will any rows where the data for these columns are missing.

The head of the **final_LoL** dataframe is below:
```markdown
| gameid           | side   | position   |   result |   kills |   deaths |   assists |   damageshare |   towers |   dragons |   heralds |   barons |   earnedgold |   goldat25 |   gamelength |   side_number |
|:-----------------|:-------|:-----------|---------:|--------:|---------:|----------:|--------------:|---------:|----------:|----------:|---------:|-------------:|-----------:|-------------:|--------------:|
| LOLTMNT03_179647 | Blue   | team       |        0 |       3 |       13 |         5 |           nan |        3 |         0 |         0 |        0 |        24639 |      39226 |         1592 |             1 |
| LOLTMNT03_179647 | Red    | team       |        1 |      13 |        3 |        36 |           nan |        9 |         2 |         1 |        1 |        36320 |      46192 |         1592 |             0 |
| LOLTMNT06_96134  | Blue   | team       |        1 |      21 |       11 |        53 |           nan |       11 |         3 |         1 |        1 |        43687 |      47876 |         1922 |             1 |
| LOLTMNT06_96134  | Red    | team       |        0 |      10 |       21 |        22 |           nan |        2 |         2 |         0 |        0 |        29697 |      39499 |         1922 |             0 |
| LOLTMNT06_95160  | Blue   | team       |        0 |      18 |       22 |        30 |           nan |        3 |         0 |         0 |        0 |        31835 |      42735 |         1782 |             1 |
```
#### Univariate Analysis
We will perform univariate analysis on `kills` statistic.

<iframe
  src="assets/plot1.html"
  width="800"
  height="425"
  frameborder="0"
></iframe>

The distribution of the histogram of `kills` is nearly normal and slightly skewed to the right. This shows that `kills` is well-balanced and is fit for our statistical analysis.

#### Bivariate Analysis
We will perform bivariate analysis on `result` (win/loss) and `side` (blue/red)
<iframe
  src="assets/plot2.html"
  width="800"
  height="425"
  frameborder="0"
></iframe>

We see that blue side wins slightly more often than red side. This suggests that there may be a potential advantage to being on blue side. 

#### Interesting Aggregates
Below in the pivot table, we can see some interesting aggregates when we index our pivot table **LoL_aggregates** by `side`
```markdown
| side   |   assists |   barons |   deaths |   dragons |   earnedgold |    goldat25 |   heralds |   kills |
|:-------|----------:|---------:|---------:|----------:|-------------:|------------:|----------:|--------:|
| Blue   |    180060 |     2567 |    76081 |      9628 |  1.82683e+08 | 1.99899e+08 |      2863 |   79438 |
| Red    |    170348 |     2407 |    79644 |     11368 |  1.78826e+08 | 1.9741e+08  |      1864 |   75879 |
```

We created a pivot table showing total stats and objectives for blue and red side. Surprisingly, contrary to in-game logic, red seems to have better dragon control. However, blue outperforms red side in every other category: more barons, less deaths, more gold (overall and at 25), more heralds, more assists, and more kills.
This is important because it suggests that being on blue side may have an association with better in-game statistics.

### Assessment of Missingness
#### NMAR Analysis
Columns we believe to be NMAR, Not Missing at Random, in my dataset (original LoL dataset) are the ban columns: **ban1**, **ban2**, **ban3**, **ban4**, and **ban5**. Some players may just choose not to ban any champions, so there are no visible trends in the dataset that indicate whether or not a value in one of these columns will be missing. There are unobserved reasons why these values are missing such as being forgetful, player intent, or maybe even strategy. 
To make this MAR, Missing At Random, we could add a column named **used_bans** that records the number of bans a team uses (between 0-5). The missingness in the ban columns could then be explained by this newly introduced column, making it MAR.

#### Missingness Dependency
We are going to test if the missingness of `goldat25` depends on other columns. Namely, we will be looking at `gamelength` and `side`.
We choose a significance level, or alpha, of .05 when we conduct the following missingness permutation tests.

First, we will look at `goldat25` and `gamelength`

**Null:**
The mean of `gamelength` when `goldat25` is missing is the same as the mean of `gamelength` when `goldat25` is not missing

**Alternate:**
The mean of `gamelength` when `goldat25` is missing is NOT the same as the mean of `gamelength` when `goldat25` is not missing

After performing the permutation test, the observed statistic is around 610. We get p-value = 0.0. This is visualized below.

<iframe
  src="assets/plot3.html"
  width="800"
  height="425"
  frameborder="0"
></iframe>

Against our significance level of .05, we reject the null hypothesis. There is statistically signifant evidence that the mean of `gamelength` when `goldat25` is missing is NOT the same as when `goldat25` is not missing.


Now, we will look at `goldat25` and `side`

**Null:**
The distribution of `side` when `goldat25` is missing is the same as the distribution of `side` when `goldat25` is not missing

**Alternate:**
The distribution of `side` when `goldat25` is missing is NOT the same as the distribution of `side` when `goldat25` is not missing

**Note:** We will create a new column, **side_number** and encode blue side as integer 1, and red side as integer 0

After performing our second permutation test, we get a p-value = 1.0.  This is visualized below.

<iframe
  src="assets/plot4.html"
  width="800"
  height="425"
  frameborder="0"
></iframe>

Against our significance level of .05, we fail to reject the null hypothesis. There is not enough evidence to conclude that the distribution of `side` is from when `goldat25` is missing and when it is not missing. 

### Hypothesis Testing
We will see if there is a difference in win rate for pro teams starting on blue side and pro teams starting on red side. Understanding whether a side offers a consistent advantage is important for balancing the game and team strategy. 

**Null:** The true win rate for teams on blue side is the same as the true win rate for teams on the red side

**Alternate:** The true win rate for teams on blue side is greater than the true win rate for teams on the red side

**Test Statistic:** The sample proportion of wins on blue side minus the sample proportion of wins on the red side

**Significance Level/Alpha:** .05

A histogram of the permutation test is pictured below, alongside a dotted vertical line of the observed difference in true win rate in our dataset. 

<iframe
  src="assets/plot5.html"
  width="800"
  height="425"
  frameborder="0"
></iframe>

My observed statistic was around .07. After simulating null 1000 times and comparing the observed statistic to the calculated difference in proportions for each simulation, I got a **p-value = 0.0**. We **reject the null hypothesis** and accept the alternate. There is statistically significant evidence that the true win rate for teams on blue side is greater than the true win rate for for teams on red side.

### Framing a Prediction Problem
In a LoL game, the ADC, attack damage carry, is supposed to be, well, the "carry." They are supposed to be the most impactful role in the game, usually outputting the most damage. Their stats can serve as a strong indicator of a team's overall performance, which leads us into the following prediction problem.
**Prediction Problem: Can we predict win/loss based on the in-game stats of the ADC (attack damage carry)?**
This is a binary classification problem. We aim to predict the outcome of a match (win or lose) using the player-level game statistics for a team's ADC. 
Our response variable here is `result`, which is either 1 (win) or 0 (lose). This column directly matches what we are looking for here. 

We will use the following features for this problem (in our new dataframe described shortly below): `kills`, `deaths`, `earnedgold`, `damageshare`, `assists`

The dataset could be imbalanced, so we will opt to look at the F1-score as a suitable metric here. F1-score is able to balance precision and recall well. 

**Precision:** The proportion of predicted wins that are actually wins.

**Recall:** The proportion of actual wins that were correctly predicted.

We will look at the complete_LoL, the intermediary dataframe, made before creating another dataframe, **prediction_LoL**, to use for our prediction problem as we need access to player-level statistics instead of team-level statistics now.

**Note:** We will NOT be using final_LoL here

### Baseline Model
For the baseline model, we will be using a logistic regression model. The features we will be using here are: `kills`, `deaths`, and `earnedgold`.
These features are all quantitative, but depending on the length of a game, these numbers could differ drastically. We are going to use **StandardScaler** Transformer on these features to standardize them. 
We split the data into training and test data, 20% going to test data. We did this by using the train_test_split function in sklearn. After fitting the model, we got an **F1-Score of .8389.** Our logistic regression model performed quite well, able to balance and precision and recall at a high level. 

**But can we improve on our base model's performance?**

### Final Model
We added on two more quantitative variables: `damageshare` and `assists`.
On top of `kills` and `deaths`, `assists` are also an important statistic for ADCs. 
Even if players can't successfully get kills, assists showcase higher participation in team fights and can indicate sustained damage. 
`damageshare` is the percentage of the total team's damage by a single player. ADCs with higher damageshares generally have better influence and impact on a game, being able to "carry" a better offensive load and potentially lead a team to victory. Sometimes, when you have a high kill count, your damage output could still be low if you are consistently **kill-stealing (KS)**, landing the final blow on an enemy champion even if another teammate did most of the damage. 
Similar to the features in the base model, we will use **StandardScaler** Transformer on `assists` for similar reasons. However, we will leave `damageshare` alone as it is already on a fixed scale (0 to 1).

Our final model instead will be Random Forest Classifier. Not only did we choose another model, we performed **GridSearchCV** to find the best hyperparameters for this model. We choose 3 hyperparameters: **max depth**, **minimum number of samples required to split a node**, and  **criterion (gini or entropy)**.
After running the grid search, with a cv (cross-validation) of 5, we found the best hyperparameters were a max depth of 30, a minimum sample split of 20, and an entropy criterion when splitting nodes. 

The final model now has an **F1-Score of .8892**, an improvement on our base model. 

**Note:** Base model's accuracy was .8403. Our final model's accuracy has improved to .8914!

### Fairness Analysis
We will see if our final model is fair for different groups. In particular, we will be looking at:

**Group X:** ADCs with low kills (less than 7) 

**Group Y:** ADCs with higher kills (7 or more)

We will run a permutation test to see if there is a difference here. Our evaluation metric will be **precision**.

These are our hypotheses below:

**Null:** Our final model is fair. The model's precision is the same for ADCs with less than 7 kills and 7 or more kills.

**Alternate:** Our final model is unfair. The model's precision is not the same for ADCs with less than 7 kills and 7 or more kills.

**Significance Level/Alpha:** .05

**Test-Statistic:** Difference in precision between ADCs with less than 7 kills and 7 or more kills.

Below is a visualization of our permutation test:

<iframe
  src="assets/plot6.html"
  width="800"
  height="425"
  frameborder="0"
></iframe>

We got a p-value of .044, which is less than our significance level of .05. As a result, we reject the null hypothesis. There is statisically signifcant evidence that our model's performance differs for ADCs with less than 7 kills and ADCs with 7 or more kills. Higher-kill ADCs have more potential to take over and dominate a game. Games where ADCs have lower kills will have more variability where macro-level decisions, teammate performance, and objective control may become more important. 