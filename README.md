 [ 1] SELECT DATE_DIFF(MAX(date), MIN(date), DAY) FROM `sql-sandbox-422308.Final_Exercise.match` 
  

 
[ 2]  SELECT season, name, MIN(home_team_goal) AS min_home_goal, AVG(home_team_goal) AS avg_home_goal,
  ((MAX(home_team_goal) + MIN(home_team_goal))/2) AS mid_range_goal, MAX(home_team_goal) AS max_home_goal,
  SUM(home_team_goal) AS sum_home_goal
  FROM `sql-sandbox-422308.Final_Exercise.match` AS m
   LEFT JOIN  `sql-sandbox-422308.Final_Exercise.leagues` AS l
    ON m.country_id = l.country_id
    GROUP BY season, name
    ORDER BY max_home_goal desc


 [ 3]  SELECT season, name, count(m.id) AS count
  FROM `sql-sandbox-422308.Final_Exercise.match` AS m
  LEFT JOIN  `sql-sandbox-422308.Final_Exercise.leagues` AS l
  ON m.country_id = l.country_id
  GROUP BY season, name
 ORDER BY count 

 [4] CREATE TABLE `sql-sandbox-422308.Final_Exercise.PlayerBMI`
    AS SELECT (weight/2.205) AS kg_weight, (height/100) AS m_height,
      (weight/2.205)/POW((height/100), 2) AS BMI
      FROM `sql-sandbox-422308.Final_Exercise.player`

   SELECT COUNT(*) FROM `sql-sandbox-422308.Final_Exercise.PlayerBMI` 
   WHERE BMI BETWEEN 18.5 AND 24.9

   SELECT COUNT(*) FROM `sql-sandbox-422308.Final_Exercise.PlayerBMI` 
   WHERE BMI NOT BETWEEN 18.5 AND 24.9


[ 5] SELECT team_long_name, season, (SUM(home_team_goal) + SUM(away_team_goal)) AS total_goal
  FROM `sql-sandbox-422308.Final_Exercise.match` AS m
LEFT JOIN  `sql-sandbox-422308.Final_Exercise.team` AS t
ON m.home_team_api_id = t.team_api_id
GROUP BY team_long_name, season
ORDER BY season DESC, total_goal DESC




[ 6] SELECT season, team_long_name, 
  FROM (SELECT season, team_long_name, RANK() OVER (PARTITION BY season ORDER BY (SUM(home_team_goal) + SUM--(away_team_goal)) DESC) as position FROM `sql-sandbox-422308.Final_Exercise.match` AS m
 LEFT JOIN  `sql-sandbox-422308.Final_Exercise.team` AS t
 ON m.home_team_api_id = t.team_api_id
 ROUP BY season, team_long_name
 ORDER BY season)
 WHERE position = 1;



[7]  CREATE TABLE `sql-sandbox-422308.Final_Exercise.TopScorer`
  AS SELECT t.id AS team_id, (SUM(home_team_goal) + SUM(away_team_goal)) AS total_goal
  FROM `sql-sandbox-422308.Final_Exercise.match` AS m
  LEFT JOIN  `sql-sandbox-422308.Final_Exercise.team` AS t
  ON m.home_team_api_id = t.team_api_id
  GROUP BY t.id
  ORDER BY total_goal DESC
  LIMIT 10

[8] SELECT t1.team_long_name AS team_1, t2.team_long_name AS team_2
  FROM `sql-sandbox-422308.Final_Exercise.TopScorer` AS t1
  JOIN `sql-sandbox-422308.Final_Exercise.TopScorer` AS t2
  ON t1.team_id < t2.team_id




