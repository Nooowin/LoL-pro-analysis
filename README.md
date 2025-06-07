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
We will make a new dataframe called **final_LoL** with the columns mentioned above. We will remove games labeled with "partial" in the `datacompleteness` column. These rows have missing data in multiple columns, so we will just remove them entirely. In the original dataset, each `gameid` corresponds to 12 rows - one for each of the 5 players of each team and 2 with team-level summary statistics. For the majority of this project, we will be working with primarily team-level statistics, so any rows corresponding to individual players or champions will be dropped. 
When inspecting the dataframe, we can see that `towers`, `dragons`, and `heralds` have similar missing mechanisms. Each of the statistics corresponding to a individual for these columns are missing and are just filled in the team-level summaries. To achieve a dataframe with only team-level summary statistics, we will any rows where the data for these columns are missing.

The head of the **final_LoL** dataframe is below:
'| gameid           | side   | position   |   result |   kills |   deaths |   assists |   damageshare |   towers |   dragons |   heralds |   barons |   earnedgold |   goldat25 |   gamelength |   side_number |\n|:-----------------|:-------|:-----------|---------:|--------:|---------:|----------:|--------------:|---------:|----------:|----------:|---------:|-------------:|-----------:|-------------:|--------------:|\n| LOLTMNT03_179647 | Blue   | team       |        0 |       3 |       13 |         5 |           nan |        3 |         0 |         0 |        0 |        24639 |      39226 |         1592 |             1 |\n| LOLTMNT03_179647 | Red    | team       |        1 |      13 |        3 |        36 |           nan |        9 |         2 |         1 |        1 |        36320 |      46192 |         1592 |             0 |\n| LOLTMNT06_96134  | Blue   | team       |        1 |      21 |       11 |        53 |           nan |       11 |         3 |         1 |        1 |        43687 |      47876 |         1922 |             1 |\n| LOLTMNT06_96134  | Red    | team       |        0 |      10 |       21 |        22 |           nan |        2 |         2 |         0 |        0 |        29697 |      39499 |         1922 |             0 |\n| LOLTMNT06_95160  | Blue   | team       |        0 |      18 |       22 |        30 |           nan |        3 |         0 |         0 |        0 |        31835 |      42735 |         1782 |             1 |'

### Assessment of Missingness

### Hypothesis Testing

### Framing a Prediction Problem

### Baseline Model

### Final Model

### Fairness Analysis

