# IPL-Data-Analysis-Case-Study-Using-SQL

**Case study on [iThinkData](https://www.youtube.com/@iThinkData) By Vaibhav Chavan**

Subscribe to [iThinkData](https://www.youtube.com/@iThinkData) for more SQL case studies and tutorials.

---

## A. Basic Level Questions

### Q1. Winner of the season team
###  Question:   Find out each seasons winner team.

```sql
SELECT  season, 
        winner 
FROM matches
WHERE match_type = 'Final'
GROUP BY season
ORDER BY season;
```




### Q2. Total matches per season
### Question:  How many matches were played in each season from 2008 to 2024?

```sql
SELECT	season,
		COUNT(season) AS 'Total Matches in Season'
FROM matches
GROUP BY season
ORDER BY season;
```


### Q3. City-wise matches
### Question:  In which cities were the most IPL matches hosted?

```sql
SELECT 	city,
		COUNT(id) AS 'Total Matches in City'
FROM matches
GROUP BY city
ORDER BY COUNT(id) DESC ;
```





### Q4. Top ‘Player Of the Match’ Awards
### Question:   Which player has won the most "Player of the Match" awards in IPL history?

```sql
SELECT  player_of_match,
	COUNT(player_of_match) AS ' Highest POM Awards'
FROM matches
GROUP BY player_of_match
ORDER BY COUNT(player_of_match) DESC
LIMIT 1;
```

### Q5. Most wins by a Team
### Question:   Which top 3 teams won the most matches across all seasons?
```sql
SELECT  winner,
		COUNT(winner) AS 'Most Matches winner Team'
FROM matches
GROUP BY winner
ORDER BY COUNT(winner) DESC limit 3;
```

### Q6. Top scorer (runs) in IPL History
### Question:  Identify the top scorer(runs) in IPL history.
```sql
SELECT  batter, 
		SUM(batsman_runs) AS total_runs
FROM deliveries
GROUP BY batter
ORDER BY total_runs DESC
LIMIT 1;
```





## Intermediate Level Questions:

### Q1. Most 4s and 6s in Powerplay by Team
### Question:   Which team hit the most 4s and 6s during the powerplay (1st-6th over) in the IPL?
```sql
SELECT d.batting_team, 
       SUM(CASE WHEN d.batsman_runs = 4 THEN 1 ELSE 0 END) AS total_4s,
       SUM(CASE WHEN d.batsman_runs = 6 THEN 1 ELSE 0 END) AS total_6s
FROM deliveries d
JOIN matches m ON d.match_id = m.id
WHERE d.overs BETWEEN 0 AND 5
GROUP BY d.batting_team
ORDER BY (total_4s + total_6s) DESC;
```

### Q2. Top 3 best economy bowlers in 2024
### Question:   Find the top 3 bowlers with the best economy rate (minimum 30 overs bowled) in 2024 IPL season.
```sql
SELECT  b.bowler, 
		ROUND(SUM(b.total_runs) / (COUNT(b.bowler) / 6.0),2) AS economy_rate
FROM deliveries b
JOIN matches m ON b.match_id = m.id
WHERE m.season = 2024
GROUP BY b.bowler
HAVING COUNT(b.bowler) >= 180 -- At least 30 overs (30*6=total balls)
ORDER BY economy_rate DESC
LIMIT 3;
```

### Q3. Best batting strike rate in death overs
### Question:   Identify the batsmen with the best strike rate (minimum 100 balls faced) in death overs (16th-20th) across all IPL seasons.
```sql
SELECT d.batter, 
       ROUND(SUM(d.batsman_runs) * 100.0 / COUNT(*),2) AS strike_rate, 
       COUNT(*) AS balls_faced
FROM deliveries d
JOIN matches m ON d.match_id = m.id
WHERE d.overs BETWEEN 15 AND 19
-- AND m.season = 2023
GROUP BY d.batter
HAVING COUNT(*) >= 100     -- Minimum 100 balls faced
ORDER BY strike_rate DESC;
```

### Q4. Fastest Century in IPL
### Question:   Identify the fastest century in IPL (minimum of 50 balls faced). Find the batsman who scored it, the number of balls faced, and the strike rate.
```sql
SELECT  d.batter, 
		COUNT(*) AS balls_faced, 
		SUM(d.batsman_runs) AS total_runs,
        SUM(d.batsman_runs) * 100.0 / COUNT(*) AS strike_rate
FROM deliveries d
JOIN matches m ON d.match_id = m.id
GROUP BY d.batter
HAVING total_runs >= 100 AND COUNT(*) >= 50
ORDER BY balls_faced ASC
LIMIT 1;
```

### Q5. Most consistent batsmen across all seasons
### Question:   Identify the most consistent batsmen by calculating the average runs per match for batsmen with at least 500 total runs across all matches.
```sql
WITH TotalBatsmanRuns AS (
    SELECT b.batter, 
           COUNT(DISTINCT b.match_id) AS total_matches, 
           SUM(b.batsman_runs) AS total_runs
    FROM deliveries b
    JOIN matches m ON b.match_id = m.id
    GROUP BY b.batter
    HAVING SUM(b.batsman_runs) >= 500 -- Minimum 500 total runs
)
SELECT batter, 
       total_runs, 
       total_matches, 
       (total_runs * 1.0 / total_matches) AS avg_runs_per_match
FROM TotalBatsmanRuns
ORDER BY avg_runs_per_match DESC;
```




## High Level Questions:

### Q1. Best finisher batsmen in IPL season
### Question:   Identify the top 3 finishers (batsmen) in each IPL season final match based on total runs scored in the last 5 overs (overs 16-20) of a match. The query should return the top 3 batsmen for each seasons ranked   by their total runs in the death overs in final match.
.
```sql
WITH DeathOverRuns AS (
    SELECT d.batter, 
           m.season, 
           SUM(d.batsman_runs) AS runs_in_death_overs
    FROM deliveries d
    JOIN matches m ON d.match_id = m.id
    WHERE d.overs BETWEEN 15 AND 19 -- Death overs
    AND m.match_type = 'final'
    GROUP BY d.batter, m.season
),
RankedFinishers AS (
    SELECT batter, 
           season, 
           runs_in_death_overs,
           ROW_NUMBER() OVER (PARTITION BY season ORDER BY runs_in_death_overs DESC) AS ranks
    FROM DeathOverRuns
)
SELECT  batter, 
		season, 
        runs_in_death_overs
FROM RankedFinishers
WHERE ranks <= 3
ORDER BY season, 
		 ranks;
```


### Q2. Identify ‘Nervous Nineties’
### Question:   Identify all the players who got out in the 90s (between 90 and 99 runs) and how they were dismissed.
```sql
WITH batsman_totals AS (
    SELECT  match_id, 
			batter, 
            SUM(batsman_runs) AS total_runs
    FROM deliveries
    GROUP BY match_id, batter
)
SELECT  d.match_id, 
		d.batter, 
		d.dismissal_kind, 
        bt.total_runs
FROM deliveries d
JOIN batsman_totals bt ON d.batter = bt.batter AND d.match_id = bt.match_id
WHERE d.is_wicket = 1
AND bt.total_runs BETWEEN 90 AND 99
ORDER BY bt.total_runs DESC;
```

### Q3. Death Over Hero bowlers
### Question:   Identify bowlers who successfully defended 6 or fewer runs in the last over of a match (over 20) and the matches where they achieved this feat.
```sql
WITH final_over_defenses AS (
    SELECT  m.season,
			d.match_id,
            m.team1,
            m.team2,
			d.bowling_team, 
            d.bowler, 
            SUM(d.total_runs) AS runs_conceded
    FROM deliveries d
    JOIN matches m ON d.match_id = m.id
    WHERE d.overs = 19
    GROUP BY d.match_id, 
			 d.bowling_team, 
             d.bowler
)
SELECT  season,
		match_id,
        team1,
        team2,
        bowling_team,
		bowler, 
        runs_conceded
FROM final_over_defenses
WHERE runs_conceded <= 6
ORDER BY runs_conceded ASC;
```



### Q4. Biggest match winners
### Question:   Find the bowlers with the most 4 or 5-wicket hauls in IPL history.
```sql
WITH WicketHauls AS (
    SELECT b.bowler, 
           b.match_id, 
           COUNT(*) AS wickets_in_match
    FROM deliveries b
    JOIN matches m ON b.match_id = m.id
    WHERE b.is_wicket = 1
    AND b.dismissal_kind NOT IN ('run out', 'retired hurt', 'obstructing the field') -- Only count proper wickets
    GROUP BY b.bowler, b.match_id
    HAVING COUNT(*) >= 4 -- 4 or more wickets in a match
)
SELECT bowler, 
       COUNT(*) AS four_five_wicket_hauls
FROM WicketHauls
GROUP BY bowler
ORDER BY four_five_wicket_hauls DESC;
```

### Q5. Top 5 Death over specialist bowlers
### Question:   Identify the bowlers with the most wickets in the death overs (overs 16-20) for each season.

```sql
WITH DeathOverWickets AS (
    SELECT b.bowler, 
           m.season, 
           COUNT(b.is_wicket) AS total_wickets
    FROM deliveries b
    JOIN matches m ON b.match_id = m.id
    WHERE b.overs BETWEEN 15 AND 19 
      AND b.is_wicket = 1
    GROUP BY b.bowler, m.season
),
RankedDeathOverBowlers AS (
    SELECT bowler, 
           season, 
           total_wickets,
           ROW_NUMBER() OVER (PARTITION BY season ORDER BY total_wickets DESC) AS ranks
    FROM DeathOverWickets
)
SELECT  season,bowler, total_wickets
FROM RankedDeathOverBowlers
WHERE ranks <= 5
ORDER BY season,
	 ranks;
```

