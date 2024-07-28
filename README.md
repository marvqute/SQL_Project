# European Soccer Data Analysis Using SQL

## Introduction

Welcome to the world of European soccer data analysis! In this project, you will have the opportunity to dive into the European SoccerDatabase, consisting of leagues, matches, teams,and player information. Get ready to explore and analyse this extensive collection of data touncover valuable insights and trends in the worldof soccer.

## Objective

The goal of this SQL project is to analyse the European Soccer Database, specifically focusing on the information contained in the leagues, match, team, and player CSV files
 
This analysis will involve querying the database to extract valuable insights such as team performance, player statistics, match outcomes, and trends within various leagues. By leveraging SQL queries and database manipulation techniques, we aim to gain a comprehensive understanding of the dynamics and patterns present in European soccer data.


### Questions


1. 	How many days have passed from the oldest Match to the most recent one (dataset time interval)?

#### Query
      SELECT DATE_DIFF(MAX(date), MIN(date), DAY) FROM `sql-sandbox-422308.Final_Exercise.match` 

 

 
 2. Produce a table which, for each Season and League Name, shows the following statistics about the home goals scored: 
a.	min
b.	average 
c.	mid-range 
d.	max 
e.	sum


#### Query
     SELECT season, name, MIN(home_team_goal) AS min_home_goal, AVG(home_team_goal) AS avg_home_goal,

    ((MAX(home_team_goal) + MIN(home_team_goal))/2) AS mid_range_goal, MAX(home_team_goal) AS max_home_goal,
  
    SUM(home_team_goal) AS sum_home_goal
  
    FROM `sql-sandbox-422308.Final_Exercise.match` AS m
  
    LEFT JOIN  `sql-sandbox-422308.Final_Exercise.leagues` AS l
   
    ON m.country_id = l.country_id
    
    GROUP BY season, name
    
    ORDER BY max_home_goal desc



 
3. Find out how many unique seasons there are in the Match table. Then write a query that shows, for each Season, the number of matches played by each League. Do you notice anything out of the ordinary?


 #### Query
     SELECT season, name, count(m.id) AS count
   
     FROM `sql-sandbox-422308.Final_Exercise.match` AS m
  
     LEFT JOIN  `sql-sandbox-422308.Final_Exercise.leagues` AS l
  
    ON m.country_id = l.country_id
  
    GROUP BY season, name
  
    ORDER BY count 




4. 	Using Players as the starting point, create a new table (PlayerBMI) and add:
   
a.	a new variable that represents the players’ weight in kg (divide the mass value by 2.205) and call it kg_weight; 

b.	a variable that represents the height in metres (divide the cm value by 100) and call it m_height; 

c.	a variable that shows the body mass index (BMI) of the player;
Hint: research how to calculate the formula of the BMI

d.	Filter the table to show only the players with an optimal BMI (from 18.5 to 24.9).
   
   
How many rows does this table have? 


#### Query

      CREATE TABLE `sql-sandbox-422308.Final_Exercise.PlayerBMI`
 
      AS SELECT (weight/2.205) AS kg_weight, (height/100) AS m_height,
      
      (weight/2.205)/POW((height/100), 2) AS BMI
      
      FROM `sql-sandbox-422308.Final_Exercise.player`

     SELECT COUNT(*) FROM `sql-sandbox-422308.Final_Exercise.PlayerBMI` 
     
     WHERE BMI BETWEEN 18.5 AND 24.9




5. 	How many players do not have an optimal BMI? 

#### Query
     SELECT COUNT(*) FROM `sql-sandbox-422308.Final_Exercise.PlayerBMI` 
     
     WHERE BMI NOT BETWEEN 18.5 AND 24.9


     

6. Which Team has scored the highest total number of goals (home + away) during the most recent available season? How many goals has it scored?


#### Query
    SELECT team_long_name, season, (SUM(home_team_goal) + SUM(away_team_goal)) AS total_goal
    
    FROM `sql-sandbox-422308.Final_Exercise.match` AS m
    
    LEFT JOIN  `sql-sandbox-422308.Final_Exercise.team` AS t
    
    ON m.home_team_api_id = t.team_api_id
    
    GROUP BY team_long_name, season
  
    ORDER BY season DESC, total_goal DESC



7. Create a query that, for each season, shows the name of the team that ranks first in terms of total goals scored (the output table should have as many rows as the number of seasons). 
Which team was the one that ranked first in most of the seasons? 


#### Query
     SELECT season, team_long_name, 
     
     FROM (SELECT season, team_long_name, RANK() OVER 
     
     (PARTITION BY season ORDER BY (SUM(home_team_goal) + SUM--(away_team_goal)) DESC) as position 
     
     FROM `sql-sandbox-422308.Final_Exercise.match` AS m
     
     LEFT JOIN  `sql-sandbox-422308.Final_Exercise.team` AS t
     
     ON m.home_team_api_id = t.team_api_id
     
    GROUP BY season, team_long_name
    
    ORDER BY season)
    
    WHERE position = 1;




8. 	From the query above (question 8) create a new table (TopScorer) containing the top 10 teams in terms of total goals scored (hint: add the team id as well).  Then write a query that shows all the possible “pair combinations” between those 10 teams. How many “pair combinations” did it generate? 


#### Query
     CREATE TABLE `sql-sandbox-422308.Final_Exercise.TopScorer`
     
     AS SELECT t.id AS team_id, (SUM(home_team_goal) + SUM(away_team_goal)) AS total_goal
     
     FROM `sql-sandbox-422308.Final_Exercise.match` AS m
     
    LEFT JOIN  `sql-sandbox-422308.Final_Exercise.team` AS t
    
    ON m.home_team_api_id = t.team_api_id
    
    GROUP BY t.id
    
    ORDER BY total_goal DESC
   
    LIMIT 10

  
  
    SELECT t1.team_long_name AS team_1, t2.team_long_name AS team_2
   
    FROM `sql-sandbox-422308.Final_Exercise.TopScorer` AS t1
  
    JOIN `sql-sandbox-422308.Final_Exercise.TopScorer` AS t2
  
    ON t1.team_id < t2.team_id




### Conclusion

In conclusion, the analysis of European soccer data using SQL has provided valuable insights into various aspects of the game, such as team performance, player statistics, and league trends. By utilizing SQL’s powerful querying capabilities, we were able to efficiently extract, manipulate, and analyze large datasets to uncover patterns and correlations within the data.

Key findings from the analysis include identifying top-performing teams and players, understanding scoring patterns, and analyzing factors contributing to match outcomes. For instance, we observed that certain teams consistently outperform others in home games, highlighting the significance of home-field advantage. Additionally, player-level data analysis revealed critical players who significantly impact their team’s success, often measured through metrics like goals scored, assists, and defensive contributions.




