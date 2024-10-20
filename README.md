# IPL-Data-Analysis-Case-Study-Using-SQL

**Case study on [iThinkData](https://www.youtube.com/@iThinkData) By Vaibhav Chavan**

Subscribe to [iThinkData](https://www.youtube.com/@iThinkData) for more SQL case studies and tutorials.

---

## A. Basic Level Questions

### Q1. Winner of the season team
### Question: Find out each seasons winner team.

```sql
SELECT  season, 
        winner 
FROM matches
WHERE match_type = 'Final'
GROUP BY season
ORDER BY season;
```




### Q2. Total matches per season
### Question: How many matches were played in each season from 2008 to 2024?

```sql
SELECT	season,
		COUNT(season) AS 'Total Matches in Season'
FROM matches
GROUP BY season
ORDER BY season;
```


### Q3. City-wise matches
### Question: In which cities were the most IPL matches hosted?

```sql
SELECT 	city,
		COUNT(id) AS 'Total Matches in City'
FROM matches
GROUP BY city
ORDER BY COUNT(id) DESC ;
```





### Q4. Top ‘Player Of the Match’ Awards
### Question: Which player has won the most "Player of the Match" awards in IPL history?

``sql
SELECT  player_of_match,
	COUNT(player_of_match) AS ' Highest POM Awards'
FROM matches
GROUP BY player_of_match
ORDER BY COUNT(player_of_match) DESC
LIMIT 1;
```

### Q5. Most wins by a Team
### Question: Which top 3 teams won the most matches across all seasons?




### Q6. Top scorer (runs) in IPL History
### Question: Identify the top scorer(runs) in IPL history.










## Intermediate Level Questions:

### Q1. Most 4s and 6s in Powerplay by Team
### Question: Which team hit the most 4s and 6s during the powerplay (1st-6th over) in the IPL?

### Q2. Top 3 best economy bowlers in 2024
### Question: Find the top 3 bowlers with the best economy rate (minimum 30 overs bowled) in 2024 IPL season.


### Q3. Best batting strike rate in death overs
### Question: Identify the batsmen with the best strike rate (minimum 100 balls faced) in death overs (16th-20th) across all IPL seasons.


### Q4. Fastest Century in IPL
### Question: Identify the fastest century in IPL (minimum of 50 balls faced). Find the batsman who scored it, the number of balls faced, and the strike rate.


### Q5. Most consistent batsmen across all seasons
### Question: Identify the most consistent batsmen by calculating the average runs per match for batsmen with at least 500 total runs across all matches.



## High Level Questions:
### Q1. Best finisher batsmen in IPL season
### Question: Identify the top 3 finishers (batsmen) in each IPL season final match based on total runs scored in the last 5 overs (overs 16-20) of a match. The query should return the top 3 batsmen for each seasons ranked   by their total runs in the death overs in final match.
.

### Q2. Identify ‘Nervous Nineties’
### Question: Identify all the players who got out in the 90s (between 90 and 99 runs) and how they were dismissed.


### Q3. Death Over Hero bowlers
### Question: Identify bowlers who successfully defended 6 or fewer runs in the last over of a match (over 20) and the matches where they achieved this feat.



### Q4. Biggest match winners
### Question: Find the bowlers with the most 4 or 5-wicket hauls in IPL history.


### Q5. Top 5 Death over specialist bowlers
### Question: Identify the bowlers with the most wickets in the death overs (overs 16-20) for each season.


