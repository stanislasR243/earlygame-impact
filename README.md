# The impact of the early-game in League of Legends


## Description  
This data science project, conducted at the University of California, San Diego (UCSD), explores the impact of the **"early-game"** in *League of Legends* matches. The study follows a comprehensive analytical process, including:  

- Exploratory data analysis  
- Hypothesis testing  
- Baseline model creation  
- Fairness analysis  

The primary objective is to evaluate the influence of the early-game on match statistics and outcomes.  

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


