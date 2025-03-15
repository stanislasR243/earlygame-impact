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

## Baseline Model Description

### Model Overview

In this step, we built a simple baseline model to predict match outcomes based solely on the differences in team statistics at 15 minutes. We used a scikit-learn pipeline that first standardizes the features using `StandardScaler` and then applies a logistic regression classifier (`LogisticRegression`) for binary classification.

### Features and Their Types

**Features Used:**  
- `golddiffat15`  
- `xpdiffat15`  
- `csdiffat15`

#### Feature Categorization

- **Quantitative Features:**  
  - All three features (`golddiffat15`, `xpdiffat15`, and `csdiffat15`) are quantitative. They represent continuous numerical values and were directly standardized.
  
- **Ordinal and Nominal Features:**  
  - No ordinal or nominal features are used in this model, so no additional encoding (such as ordinal encoding or one-hot encoding) was necessary.

### Model Pipeline

1. **Data Cleaning:**  
   - Rows with missing values were dropped to ensure the data quality.
  
2. **Data Splitting:**  
   - The dataset was split into a training set (80%) and a testing set (20%).
  
3. **Standardization:**  
   - `StandardScaler` was applied to the quantitative features so that each has a mean of 0 and a standard deviation of 1.
  
4. **Classification:**  
   - A logistic regression classifier was used for its simplicity and interpretability in a binary classification setting.

### Model Performance

We evaluated the model using two key performance metrics:

- **F1-Score:** 0.65  
  - This metric indicates a balanced trade-off between precision and recall. An F1-score of 0.65 suggests that while the model performs reasonably well in predicting wins and losses, it still misses some cases.
  
- **AUC-ROC:** 0.71  
  - The AUC-ROC score of 0.71 implies that the model has a moderate ability to distinguish between winning and losing matches. In other words, given a random win and a random loss, there is a 71% chance that the model will correctly rank the winning match higher.

### Interpretation and Model Quality

- **Strengths:**
  - The model’s simplicity allows for easy interpretation of results.
  - Standardizing the quantitative features ensures that all inputs contribute equally to the model’s performance.
  - The performance metrics (F1-score of 0.65 and AUC-ROC of 0.71) indicate that the model is moderately effective at predicting match outcomes.

- **Limitations and Areas for Improvement:**
  - Since the model relies only on three early-game statistics, it might not capture the full complexity of match dynamics.
  - Further improvements could be achieved by incorporating additional features, engaging in more advanced feature engineering, and performing hyperparameter tuning.

### Conclusion

This baseline model provides a solid starting point for predicting match outcomes based on early-game statistics. While the current performance is acceptable for an initial model, there is significant room for enhancement by expanding the feature set and optimizing the modeling approach. Overall, the model is considered "good" as a preliminary approach, yet future developments are expected to further refine its predictive power.

## Optimized Final Model Performance

- **F1-score:** 0.66  
- **AUC-ROC:** 0.73

### Interpretation

- **F1-score Improvement:**  
  Compared to the baseline model (F1-score = 0.65), our final model’s F1-score has increased to 0.66. Although the improvement is modest, it indicates that our enhanced features and fine-tuning have helped the model achieve a slightly better balance between precision and recall. This means the model is marginally better at correctly predicting wins and losses, reducing misclassifications.

- **AUC-ROC Improvement:**  
  The AUC-ROC improved from 0.71 in the baseline to 0.73 in the optimized model. This suggests that the model’s discriminative power has increased; it can more reliably distinguish between winning and losing matches by assigning higher probabilities to wins over losses.

- **Overall Impact:**  
  While the numerical gains are moderate, these improvements confirm that our targeted feature engineering—such as adding the **gold/XP ratio**, **CS/Gold ratio**, **kill advantage**, **assist advantage**, and the combined **advantage index**—coupled with hyperparameter tuning, has enhanced the model’s predictive performance. This is a clear step up from the baseline, as we now leverage both raw statistics and engineered insights to capture the nuances of early-game performance.

### Engineered Features Table

Below is a sample of the newly engineered features for a few games:

|   gold_xp_ratio |   cs_gold_ratio |   kill_advantage |   assist_advantage |   advantage_index |
|----------------:|----------------:|-----------------:|-------------------:|------------------:|
|        0.684066 |      0.0260088  |               -1 |                  0 |          -395.333 |
|        1.00449  |      0.0184495  |                1 |                  1 |            41     |
|        0.677154 |      0.0265204  |                0 |                 -1 |          -124.667 |
|        1.29684  |      0.0193257  |                4 |                  1 |          1299.33  |
|        1.03123  |      0.00508721 |               -1 |                  4 |           268     |

### Explanation of Choices and Improvements

- **Feature Engineering Enhancements:**  
  In contrast to Step 6 where only the basic difference features were used, we now created several new engineered features:
  - **gold_xp_ratio:** Measures the efficiency of converting experience into gold.
  - **cs_gold_ratio:** Indicates how effectively a team translates gold into creep score.
  - **kill_advantage & assist_advantage:** Quantify the difference in kills and assists between the team and its opponent.
  - **advantage_index:** Provides an aggregated measure of the team’s early-game advantage by averaging the differences in gold, XP, and CS.

- **Hyperparameter Optimization:**  
  By tuning parameters such as the number of trees, maximum depth, and minimum samples per leaf, we tailored the Random Forest model to better fit our data. This fine-tuning contributes to the slight improvements seen in both F1-score and AUC-ROC.

- **Overall Improvement:**  
  The combination of richer features and optimized hyperparameters has yielded a model that, while only moderately better in metrics, demonstrates the effectiveness of leveraging additional statistical insights. This approach shows promise for further refinements, indicating that additional feature engineering or a more expansive hyperparameter search might yield even greater gains.

In summary, our final model builds upon the baseline by integrating both raw and engineered features that capture multiple dimensions of early-game performance, resulting in improved predictive capabilities.

## Fairness Analysis of the Final Model

In this analysis, we investigate whether our final model performs differently for teams on the Blue side (Group X) compared to those on the Red side (Group Y) using **precision** as our evaluation metric. Our goal is to determine if the model is fair—that is, if its precision is roughly the same for both groups—or if there is a statistically significant difference.

### Groups:
- **Blue:** Matches where the team is on the Blue side.
- **Red:** Matches where the team is on the Red side.

### Evaluation Metric:
- **Precision:** The fraction of predicted wins that are actual wins.

### Hypotheses:
- **Null Hypothesis (H₀):** The model is fair; any difference in precision between Blue and Red is due to random chance.
- **Alternative Hypothesis (H₁):** The model is unfair; specifically, the precision for one group is significantly lower than the other.

### Observed Results:
- **Observed precision (Blue):** 0.69044  
- **Observed precision (Red):** 0.64611  
- **Observed precision difference (Blue - Red):** 0.04433

### Permutation Test:
We performed a permutation test by randomly shuffling the 'side' labels among the test samples 1,000 times and recalculating the precision difference for each permutation. The p-value is computed as the proportion of permuted differences that are as extreme as (or more extreme than) the observed difference.

- **Permutation test p-value:** 1.0

### Interpretation:
The observed precision for Blue (0.69044) is slightly higher than for Red (0.64611), with an observed difference of 0.04433. However, the permutation test yielded a p-value of 1.0. This high p-value indicates that such a difference (or a more extreme one) is very common under the null hypothesis of fairness. In other words, the observed difference in precision between Blue and Red is not statistically significant.

**Conclusion:**  
We fail to reject the null hypothesis. There is no evidence from the permutation test to suggest that the model performs significantly differently for Blue side matches compared to Red side matches in terms of precision.

