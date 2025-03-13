# The impact of the early-game in League of Legends 

Authors : Stanislas Robineau, Louis Depiesse

## Introduction  
*League of Legends* (*LoL*) is a multiplayer online battle arena (MOBA) game developed by Riot Games. With millions of players worldwide, it has become a cornerstone of esports and one of the most influential games in the industry.  

This project relies on a professional dataset provided by **Oracle’s Elixir**, which includes statistics from competitive *LoL* matches throughout the year 2024. 

 In League of legends, the early game corresponds to the first few minutes of the game. It's a very important period that will define the middle and end of the game, and consequently its overall trajectory. These first few minutes will influence all the strategies implemented by the teams and the final outcome.

So we're going to ask ourselves **how much the early game affects the overall trend of a game, and how much it affects the final result.** The study of the data set will enable us to answer these questions, while allowing us to understand the impact of different parameters over the course of a game.

# Early Game Impact Analysis in League of Legends  

## Description  
This data science project analyzes the impact of **early-game** statistics on match outcomes in *League of Legends*. The focus is on the **first 15 minutes** of professional matches to determine how early advantages influence the final result.  

The analysis includes:  
- Exploratory Data Analysis (EDA)  
- Hypothesis Testing  
- Statistical Modeling  
- Fairness and Bias Evaluation  

## Dataset  
The dataset is sourced from **Oracle’s Elixir** and contains match statistics from professional *LoL* games played in 2022. 

### **Data Cleaning**

By exploring the data, we found out that some values were missing in every quantitative columns. As there are many the raws in every column we are using (even the ones were some data is missing), we simply chose to drop the raws were some values are missing.

### **Relevant Columns and Descriptions** 

The dataset consists of 117,576 rows and captures key gameplay statistics and match outcomes from professional League of Legends (LoL) esports matches. Below is an overview of the most important columns of our study:

- **`gameid`**: A unique identifier for each game.  
- **`side`**: The side of the map the team is playing on (either "Blue" or "Red").  
- **`result`**: The outcome of the game for the team (1 = win, 0 = loss).  
- **`goldat15`**: The total amount of gold earned by the team at the 15-minute mark.  
- **`xpat15`**: The total amount of experience gained by the team at the 15-minute mark.  
- **`csat15`**: The total creep score (CS) of the team at the 15-minute mark (sum of minions and monsters killed).  
- **`golddiffat15`**: The gold difference between the two teams at 15 minutes (positive if the team has more gold than the opponent).  
- **`xpdiffat15`**: The experience difference between the two teams at 15 minutes (positive if the team has more experience than the opponent).  
- **`csdiffat15`**: The creep score difference between the two teams at 15 minutes (positive if the team has a higher CS than the opponent).  
- **`killsat15`**: The number of kills secured by the team at the 15-minute mark.  
- **`assistsat15`**: The number of assists recorded by the team at the 15-minute mark.  
- **`deathsat15`**: The number of deaths suffered by the team at the 15-minute mark.  
- **`opp_killsat15`**: The number of kills secured by the opposing team at the 15-minute mark.  
- **`opp_assistsat15`**: The number of assists recorded by the opposing team at the 15-minute mark.  
- **`opp_deathsat15`**: The number of deaths suffered by the opposing team at the 15-minute mark.

Here is the head of our Dataframe to better understand the data we are using : 

| gameid            | side | result | goldat15 | xpat15 | csat15 | golddiffat15 | xpdiffat15 | csdiffat15 | killsat15 | assistsat15 | deathsat15 | opp_killsat15 | opp_assistsat15 | opp_deathsat15 |
|-------------------|------|--------|----------|--------|--------|--------------|------------|------------|-----------|-------------|------------|----------------|-----------------|----------------|
| 132542  | Blue | 1      | 5229.0   | 7644.0 | 136.0  | -575.0       | -603.0     | -8.0       | 0.0       | 0.0         | 0.0        | 1.0            | 0.0             | 0.0            |
| 132542  | Blue | 1      | 5366.0   | 5342.0 | 99.0   | 236.0        | -107.0     | -6.0       | 2.0       | 2.0         | 1.0        | 1.0            | 1.0             | 0.0            |
| 132542  | Blue | 1      | 5015.0   | 7406.0 | 133.0  | -409.0       | 39.0       | -4.0       | 0.0       | 1.0         | 1.0        | 0.0            | 2.0             | 0.0            |
| 132542  | Blue | 1      | 7296.0   | 5626.0 | 141.0  | 2811.0       | 1047.0     | 40.0       | 4.0       | 2.0         | 0.0        | 0.0            | 1.0             | 3.0            |
| 132542  | Blue | 1      | 4128.0   | 4003.0 | 21.0   | 230.0        | 573.0      | 1.0        | 0.0       | 5.0         | 1.0        | 1.0            | 1.0             | 3.0            |


### Univariate Analysis

We perform an unvariate analysis to see the distribution of the gold difference a 15 minutes.

<iframe
  src="assets/golddiffat15_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This histogram shows that this difference is logically symetric (depending on the side red/blue we have an identical value, positive or negative). This histogram also highlights that this gold diff at 15 minutes is small most of the time so it will be interesting to see if this gold difference truly has an impact on the final result of the game.


### Bvariate Analysis

We perform a bivariate analysis to see if having a positive gold diff at 15 minutes implies a higher winrate (a result of 1 in our Dataframe).

<iframe
  src="assets/golddiffat15_positive_results.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This pie shows that a positive gold diff at 15 minutes logically implies a higher winrate at the end of the game with more than 60% game won if the gold difference is positive. To quantify how much the early game matters, we will analyse other columns in order to see the most important parameters.

### Interesting Aggregates

After grouping the dataset by the `result` column (where `1` represents a win and `0` represents a loss), we computed the average values for the columns representing the differences at 15 minutes (gold difference, experience difference, and creep score difference). The table below shows the mean values of these differences for each result:

| result | golddiffat15 | xpdiffat15 | csdiffat15 |
|--------|--------------|------------|------------|
| 1      | 505.45       | 330.88     | 5.69       |
| 0      | -505.45      | -330.88    | -5.69      |

With this Dataframe, we understand that several datas have a real impact on the result of the game and we understand which of them are the most important to predict the result of a game. As we could have predict, a team that has a positive xpdiffat15 or csdiffat15 (as cs provides gold this column is likely related to the golddiffat15 one so it isn't surprising) is more likely to win a game.

## Assessment of Missingness

### NMAR Analysis

In our columns, we believe the **`goldat15`** column is NMAR (Not Missing At Random) as it would be possible for the team to surrend if the team is playing poorly in the early game. Therefore, there would not be any gold at 15 minutes for some games. The missingness would depend on other factors of the game such as the team overall performance. We could add a **`team_surrended_at_15`** with a value of 1 if the team surrended after 15 minutes and 0 otherwise to make our column MAR (Missing At Random).

### Missingness Dependancy

We wanted to observe if there was a missingness dependancy between **`csdiffat15`** and **`xpdiffat15`** so we first represented the distribution of **`xpdiffat15`** when **`csdiffat15`** was and was not missing and we could observe that there was not any value for **`xpdiffat15`** when **`csdiffat15`** was missing.


<iframe
  src="assets/csdiffat15_present.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

To ensure that the values were missing in the same raws for out two columns, we counted if there was the same number of missing values in each columns.

| result | csdiffat15 | xpdiffat15 |
|--------|------------|------------|
| 0      | 8370       | 8370       |
| 1      | 8346       | 8346       |

We can confirm that there is a missingness dependancy between our two columns as the values are missing in the same raws for both.

## Hypothesis Testing 

**Null Hypothesis (H₀):**  
There is no significant difference in the average gold difference at 15 minutes between games that resulted in a win and those that resulted in a loss.

**Alternative Hypothesis (H₁):**  
There is a significant difference in the average gold difference at 15 minutes between games that resulted in a win and those that resulted in a loss (a positive gold difference implies a higher probability of winning).

### Test Statistic:  
The test statistic chosen is the **difference in means** between the games that resulted in a win (result = 1) and the games that resulted in a loss (result = 0).

### Significance Level:  
The significance level chosen for this hypothesis test is typically **α = 0.05**.

### Statistical Test and p-value:  
To test the hypotheses, we will use a **difference in means** between the two groups (games won vs. games lost). This test will compare the average gold difference at 15 minutes for games with result = 1 (win) and result = 0 (loss).

<iframe
  src="assets/golddiffat15_by_result.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Based on the hypothesis test performed, with a **p-value of 0.0**, we **reject** the null hypothesis. This indicates that there is a statistically significant difference in the average gold difference at 15 minutes between games that resulted in a win and those that resulted in a loss. Specifically, a positive gold difference at 15 minutes is associated with a higher probability of winning the game.

## Framing a Prediction Problem (Team Differences Only)

For this step, we define a prediction problem that focuses solely on the relative advantage between teams at the 15-minute mark.

**Objective:** Predict the outcome of a League of Legends match using the differences in early-game statistics between the two teams.  
**Target Variable:** `result` (1 = win, 0 = loss).  
**Features Used:**  
- `golddiffat15`: Gold difference between teams.  
- `xpdiffat15`: Experience difference between teams.  
- `csdiffat15`: Creep score (CS) difference between teams.

**Problem Type:** Binary classification.  
**Evaluation Metric:** We will use the F1-score (or AUC-ROC) to measure model performance, ensuring that both precision and recall are well-balanced.

This choice is justified by our preliminary analyses, which indicate that an early-game advantage—as measured by these differences—is strongly correlated with a win. By focusing solely on these relative indicators, we aim to quantify the net impact of an early advantage or disadvantage on the final match outcome.

## Baseline Model

In this step, we build a simple baseline model to predict match outcomes using only the differences in team statistics at 15 minutes (`golddiffat15`, `xpdiffat15`, and `csdiffat15`). We started by cleaning the data—dropping rows with missing values—and then split the dataset into a training set (80%) and a testing set (20%). Our model uses a scikit-learn pipeline that first standardizes the features with `StandardScaler` and then applies a `LogisticRegression` classifier.

**Features and Target:**  
- **Features:** `golddiffat15`, `xpdiffat15`, `csdiffat15`  
- **Target:** `result` (1 = win, 0 = loss)

**Model Pipeline:**  
- The features are standardized so that they have a mean of 0 and a standard deviation of 1.  
- A Logistic Regression classifier is used for its simplicity and interpretability in binary classification.

**Evaluation Metrics:**  
We evaluated our model using two key metrics: F1-score and AUC-ROC.

- **F1-score (0.65):**  
  The F1-score balances precision and recall. A score of 0.65 suggests that the model does a fair job overall in predicting wins and losses, though it does miss some cases. This indicates that while the model is reasonably good at both identifying true positives and avoiding false positives, there is still room for improvement.

- **AUC-ROC (0.71):**  
  The AUC-ROC score measures how well the model distinguishes between the two classes. A value of 0.71 means that, given a random win and a random loss, there’s a 71% chance that the model will correctly rank the winning match higher than the losing one. This is a decent performance, showing moderate discriminative ability.

**Overall Interpretation:**  
- The **F1-score** of 0.65 indicates that our model maintains a reasonable balance between precision and recall, making it fairly competent in predicting the correct match outcome, although some misclassifications occur.
- The **AUC-ROC** of 0.71 demonstrates that the model has a moderate ability to differentiate between winning and losing matches.
- Together, these metrics show that while early-game differences provide a meaningful signal for predicting match outcomes, the baseline model still has significant room for improvement. This gives us a solid starting point to explore further feature engineering and hyperparameter tuning in subsequent steps.
