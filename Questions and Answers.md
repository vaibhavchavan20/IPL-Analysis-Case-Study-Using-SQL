# IPL-Data-Analysis-Case-Study-Using-SQL

**Case study on [iThinkData](https://www.youtube.com/@iThinkData) By Vaibhav Chavan**

Subscribe to [iThinkData](https://www.youtube.com/@iThinkData) for more SQL case studies and tutorials.

---

## A. Basic Level Questions

### Q1. Winner of the season team
####  Question:   Find out each seasons winner team.

```sql
SELECT  season, 
        winner 
FROM matches
WHERE match_type = 'Final'
GROUP BY season
ORDER BY season;
```
| season | winner                |
|--------|-----------------------|
| 2008   | Rajasthan Royals       |
| 2009   | Deccan Chargers        |
| 2010   | Chennai Super Kings    |
| 2011   | Chennai Super Kings    |
| 2012   | Kolkata Knight Riders  |
| 2013   | Mumbai Indians         |
| 2014   | Kolkata Knight Riders  |
| 2015   | Mumbai Indians         |
| 2016   | Sunrisers Hyderabad    |
| 2017   | Mumbai Indians         |
| 2018   | Chennai Super Kings    |
| 2019   | Mumbai Indians         |
| 2020   | Mumbai Indians         |
| 2021   | Chennai Super Kings    |
| 2022   | Gujarat Titans         |
| 2023   | Chennai Super Kings    |
| 2024   | Kolkata Knight Riders  |


### Q2. Total matches per season
#### Question:  How many matches were played in each season from 2008 to 2024?

```sql
SELECT	season,
	COUNT(season) AS 'Total Matches in Season'
FROM matches
GROUP BY season
ORDER BY season;
```
| season | Total Matches in Season |
|--------|-------------------------|
| 2008   | 58                      |
| 2009   | 57                      |
| 2010   | 60                      |
| 2011   | 73                      |
| 2012   | 74                      |
| 2013   | 76                      |
| 2014   | 60                      |
| 2015   | 59                      |
| 2016   | 60                      |
| 2017   | 59                      |
| 2018   | 60                      |
| 2019   | 60                      |
| 2020   | 60                      |
| 2021   | 60                      |
| 2022   | 74                      |
| 2023   | 74                      |
| 2024   | 71                      |


### Q3. City-wise matches
#### Question:  In which cities were the most IPL matches hosted?

```sql
SELECT 	city,
	COUNT(id) AS 'Total Matches in City'
FROM matches
GROUP BY city
ORDER BY COUNT(id) DESC ;
```
| city             | Total Matches in City |
|------------------|-----------------------|
| Mumbai           | 173                   |
| Kolkata          | 93                    |
| Delhi            | 90                    |
| Chennai          | 85                    |
| Hyderabad        | 77                    |
| Bangalore        | 65                    |
| Chandigarh       | 61                    |
| Jaipur           | 57                    |
| Pune             | 51                    |
| Dubai            | 46                    |
| Abu Dhabi        | 37                    |
| Ahmedabad        | 36                    |
| Bengaluru        | 29                    |
| Sharjah          | 28                    |
| Durban           | 15                    |
| Visakhapatnam    | 15                    |
| Lucknow          | 14                    |
| Dharamsala       | 13                    |
| Centurion        | 12                    |
| Rajkot           | 10                    |
| Indore           | 9                     |
| Navi Mumbai      | 9                     |
| Johannesburg     | 8                     |
| Cape Town        | 7                     |
| Port Elizabeth   | 7                     |
| Cuttack          | 7                     |
| Ranchi           | 7                     |
| Raipur           | 6                     |
| Mohali           | 5                     |
| Kochi            | 5                     |
| Kanpur           | 4                     |
| Kimberley        | 3                     |
| East London      | 3                     |
| Guwahati         | 3                     |
| Nagpur           | 3                     |
| Bloemfontein     | 2                     |





### Q4. Top ‘Player Of the Match’ Awards
#### Question:   Which player has won the most "Player of the Match" awards in IPL history?

```sql
SELECT  player_of_match,
	COUNT(player_of_match) AS ' Highest POM Awards'
FROM matches
GROUP BY player_of_match
ORDER BY COUNT(player_of_match) DESC
LIMIT 1;
```
| player_of_match   | Highest POM Awards |
|-------------------|--------------------|
| AB de Villiers    | 25                 |

### Q5. Most wins by a Team
#### Question:   Which top 3 teams won the most matches across all seasons?
```sql
SELECT  winner,
	COUNT(winner) AS 'Most Matches winner Team'
FROM matches
GROUP BY winner
ORDER BY COUNT(winner) DESC limit 3;
```
| winner                | Most Matches winner Team |
|-----------------------|--------------------------|
| Mumbai Indians        | 144                      |
| Chennai Super Kings   | 138                      |
| Kolkata Knight Riders | 131                      |

### Q6. Top scorer (runs) in IPL History
#### Question:  Identify the top scorer(runs) in IPL history.
```sql
SELECT  batter, 
	SUM(batsman_runs) AS total_runs
FROM deliveries
GROUP BY batter
ORDER BY total_runs DESC
LIMIT 1;
```

| batter  | total_runs |
|---------|------------|
| V Kohli | 8014       |




## Intermediate Level Questions:

### Q1. Most 4s and 6s in Powerplay by Team
#### Question:   Which team hit the most 4s and 6s during the powerplay (1st-6th over) in the IPL?
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
| batting_team                | total_4s | total_6s |
|-----------------------------|----------|----------|
| Mumbai Indians               | 1435     | 374      |
| Kolkata Knight Riders         | 1465     | 340      |
| Royal Challengers Bangalore   | 1409     | 353      |
| Chennai Super Kings           | 1283     | 343      |
| Rajasthan Royals              | 1319     | 272      |
| Kings XI Punjab               | 1104     | 249      |
| Sunrisers Hyderabad           | 1066     | 241      |
| Delhi Daredevils              | 912      | 182      |
| Delhi Capitals                | 606      | 150      |
| Deccan Chargers               | 401      | 101      |
| Punjab Kings                  | 345      | 94       |
| Gujarat Titans                | 281      | 44       |
| Lucknow Super Giants          | 227      | 73       |
| Pune Warriors                 | 235      | 34       |
| Gujarat Lions                 | 187      | 56       |
| Rising Pune Supergiants       | 172      | 36       |
| Kochi Tuskers Kerala          | 85       | 18       |

### Q2. Top 3 best economy bowlers in 2024
#### Question:   Find the top 3 bowlers with the best economy rate (minimum 30 overs bowled) in 2024 IPL season.
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
| bowler         | economy_rate   |
|----------------|-------|
| Yash Thakur    | 10.8  |
| MM Sharma      | 10.48 |
| HH Pandya      | 10.33 |

### Q3. Best batting strike rate in death overs
#### Question:   Identify the batsmen with the best strike rate (minimum 100 balls faced) in death overs (16th-20th) across all IPL seasons.
```sql
SELECT d.batter, 
       ROUND(SUM(d.batsman_runs) * 100.0 / COUNT(*),2) AS strike_rate, 
       COUNT(*) AS balls_faced
FROM deliveries d
JOIN matches m ON d.match_id = m.id
WHERE d.overs BETWEEN 15 AND 19
GROUP BY d.batter
HAVING COUNT(*) >= 100     -- Minimum 100 balls faced
ORDER BY strike_rate DESC LIMIT 20;
```
| batter            | strike_rate | balls_faced |
|-------------------|-------------|--------------|
| T Stubbs          | 234.78      | 115          |
| AB de Villiers    | 215.46      | 867          |
| MA Agarwal        | 207.56      | 119          |
| LS Livingstone     | 198.60      | 143          |
| C Green           | 192.45      | 106          |
| RR Pant           | 192.29      | 480          |
| CH Gayle          | 191.12      | 304          |
| MEK Hussey        | 189.13      | 138          |
| R Powell          | 187.70      | 122          |
| JC Buttler        | 186.83      | 319          |
| H Klaasen         | 184.02      | 219          |
| CL White          | 182.41      | 199          |
| SV Samson         | 181.40      | 430          |
| BB McCullum       | 181.00      | 100          |
| SR Watson         | 180.48      | 251          |
| AD Russell        | 179.16      | 739          |
| V Kohli           | 178.28      | 824          |
| JM Sharma         | 176.00      | 150          |
| TH David          | 175.32      | 308          |
| DJ Hussey         | 175.21      | 234          |

### Q4. Fastest Century in IPL
#### Question:   Identify the fastest century in IPL (minimum of 50 balls faced). Find the batsman who scored it, the number of balls faced, and the strike rate.
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
| batter       | balls_faced | total_runs | strike_rate |
|--------------|------|-------|-------------|
| LJ Wright    | 63   | 106   | 168.25      |


### Q5. Most consistent batsmen across all seasons
#### Question:   Identify the most consistent batsmen by calculating the average runs per match for batsmen with at least 500 total runs across all matches.
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
       ROUND((total_runs * 1.0 / total_matches),2) AS avg_runs_per_match
FROM TotalBatsmanRuns
ORDER BY avg_runs_per_match DESC LIMIT 20;
```
| batter            | total_runs | total_matches | avg_runs_per_match |
|-------------------|------|-------|-------------|
| DP Conway         | 924  | 22    | 42.00          |
| B Sai Sudharsan   | 1034 | 25    | 41.36       |
| KL Rahul          | 4689 | 122   | 38.43       |
| LMP Simmons       | 1079 | 29    | 37.21       |
| RD Gaikwad        | 2380 | 65    | 36.62       |
| SE Marsh          | 2489 | 69    | 36.07       |
| HM Amla           | 577  | 16    | 36.06       |
| DA Warner         | 6567 | 184   | 35.69       |
| CH Gayle          | 4997 | 141   | 35.44       |
| ML Hayden         | 1107 | 32    | 34.59       |
| MEK Hussey        | 1977 | 58    | 34.09       |
| JC Buttler        | 3583 | 106   | 33.80       |
| RM Patidar        | 799  | 24    | 33.29       |
| F du Plessis      | 4571 | 138   | 33.12       |
| V Kohli           | 8014 | 244   | 32.84       |
| Shubman Gill      | 3216 | 99    | 32.48       |
| JM Bairstow       | 1589 | 50    | 31.78       |
| CA Lynn           | 1329 | 42    | 31.64       |
| PD Salt           | 653  | 21    | 31.10       |
| H Klaasen         | 993  | 32    | 31.03       |




## High Level Questions:

### Q1. Best finisher batsmen in IPL season
#### Question:   Identify the top 3 finishers (batsmen) in each IPL season final match based on total runs scored in the last 5 overs (overs 16-20) of a match. The query should return the top 3 batsmen for each seasons ranked   by their total runs in the death overs in final match.
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
| batter              | season | runs_in_death_overs |
|---------------------|------|------|
| YK Pathan           | 2008 | 21   |
| MS Dhoni            | 2008 | 20   |
| SK Raina            | 2008 | 9    |
| HH Gibbs            | 2009 | 24   |
| RV Uthappa          | 2009 | 14   |
| RJ Harris           | 2009 | 9    |
| SK Raina            | 2010 | 29   |
| KA Pollard          | 2010 | 27   |
| JA Morkel           | 2010 | 15   |
| MS Dhoni            | 2011 | 20   |
| SS Tiwary           | 2011 | 19   |
| Z Khan              | 2011 | 18   |
| SK Raina            | 2012 | 37   |
| JH Kallis           | 2012 | 27   |
| MS Dhoni            | 2012 | 14   |
| MS Dhoni            | 2013 | 33   |
| KA Pollard          | 2013 | 28   |
| Harbhajan Singh     | 2013 | 14   |
| WP Saha             | 2014 | 55   |
| MK Pandey           | 2014 | 19   |
| PP Chawla           | 2014 | 13   |
| KA Pollard          | 2015 | 28   |
| MM Sharma           | 2015 | 21   |
| AT Rayudu           | 2015 | 18   |
| BCJ Cutting         | 2016 | 39   |
| Sachin Baby         | 2016 | 18   |
| STR Binny           | 2016 | 9    |
| KH Pandya           | 2017 | 31   |
| SPD Smith           | 2017 | 26   |
| MG Johnson          | 2017 | 12   |
| YK Pathan           | 2018 | 24   |
| CR Brathwaite       | 2018 | 21   |
| SR Watson           | 2018 | 20   |
| SR Watson           | 2019 | 38   |
| KA Pollard          | 2019 | 31   |
| HH Pandya           | 2019 | 16   |
| SS Iyer             | 2020 | 23   |
| Ishan Kishan        | 2020 | 16   |
| AR Patel            | 2020 | 9    |
| MM Ali              | 2021 | 35   |
| F du Plessis        | 2021 | 25   |
| Shivam Mavi         | 2021 | 20   |
| DA Miller           | 2022 | 23   |
| R Parag             | 2022 | 15   |
| Shubman Gill        | 2022 | 12   |
| B Sai Sudharsan     | 2023 | 48   |
| HH Pandya           | 2023 | 21   |
| Rashid Khan         | 2023 | 0    |
| PJ Cummins          | 2024 | 15   |
| JD Unadkat          | 2024 | 4    |
| B Kumar             | 2024 | 0    |


### Q2. Identify ‘Nervous Nineties’
#### Question:   Identify all the players who got out in the 90s (between 90 and 99 runs) and how they were dismissed.
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
| match_id | batter               | dismissal_kind       | total_runs |
|-----------|----------------------|------------------|------|
| 1304092   | RD Gaikwad           | caught           | 99   |
| 1175365   | PP Shaw              | caught           | 99   |
| 1254086   | MA Agarwal           | run out          | 99   |
| 598054    | V Kohli              | run out          | 99   |
| 1216547   | Ishan Kishan         | caught           | 99   |
| 1216537   | CH Gayle             | bowled           | 99   |
| 1426284   | RD Gaikwad           | caught           | 98   |
| 392202    | SK Raina             | caught           | 98   |
| 548309    | AM Rahane            | bowled           | 98   |
| 1082632   | RR Pant              | caught           | 97   |
| 1216542   | JM Bairstow          | lbw              | 97   |
| 1178418   | KD Karthik           | run out          | 97   |
| 980991    | HM Amla              | caught           | 96   |
| 1370353   | B Sai Sudharsan      | lbw              | 96   |
| 1304077   | F du Plessis         | caught           | 96   |
| 1304077   | F du Plessis         | run out          | 96   |
| 1082640   | SS Iyer              | bowled           | 96   |
| 1082640   | SS Iyer              | run out          | 96   |
| 1304062   | Shubman Gill         | caught           | 96   |
| 1178430   | F du Plessis         | bowled           | 96   |
| 829713    | CH Gayle             | run out          | 96   |
| 829713    | CH Gayle             | run out          | 96   |
| 1178416   | SR Watson            | caught           | 96   |
| 501223    | SE Marsh             | caught           | 95   |
| 501271    | M Vijay              | caught           | 95   |
| 598034    | MEK Hussey           | caught           | 95   |
| 729295    | GJ Maxwell           | caught           | 95   |
| 1082609   | M Vohra              | lbw              | 95   |
| 729283    | GJ Maxwell           | bowled           | 95   |
| 1359526   | JC Buttler           | lbw              | 95   |
| 1136574   | RG Sharma            | caught           | 94   |
| 1136610   | KL Rahul             | caught           | 94   |
| 734049    | MK Pandey            | caught           | 94   |
| 335991    | KC Sangakkara        | run out          | 94   |
| 1136574   | RG Sharma            | run out          | 94   |
| 1359538   | LS Livingstone       | retired out      | 94   |
| 1359538   | LS Livingstone       | caught           | 94   |
| 335991    | KC Sangakkara        | caught           | 94   |
| 1136586   | SS Iyer              | run out          | 93   |
| 1082631   | RA Tripathi          | caught           | 93   |
| 1304114   | MM Ali               | caught           | 93   |
| 548344    | G Gambhir            | caught           | 93   |
| 419116    | ML Hayden            | caught           | 93   |
| 980953    | DA Warner            | caught           | 92   |
| 1254068   | S Dhawan             | bowled           | 92   |
| 598034    | MS Bisla             | run out          | 92   |
| 1426296   | V Kohli              | caught           | 92   |
| 1359475   | RD Gaikwad           | caught           | 92   |
| 829743    | DA Warner            | caught           | 91   |
| 1178422   | HH Pandya            | caught           | 91   |
| 336033    | GC Smith             | caught           | 91   |
| 336033    | GC Smith             | run out          | 91   |
| 336014    | SC Ganguly           | caught           | 91   |
| 1254061   | KL Rahul             | caught           | 91   |
| 734029    | DA Warner            | bowled           | 90   |
| 733987    | GJ Maxwell           | caught           | 90   |


### Q3. Death Over Hero bowlers
#### Question:   Identify bowlers who successfully defended 6 or fewer runs in the last over of a match (over 20) and the matches where they achieved this feat.
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
| season | match_id | team1                     | team2                   | bowling_team                  | bowler          | runs_conceded |
|------|-----------|----------------------------------|-------------------------------|---------------------------------|------------------|---------|
| 2008 | 335991    | Kings XI Punjab                  | Mumbai Indians                | Kings XI Punjab                 | IK Pathan        | 0       |
| 2008 | 336000    | Rajasthan Royals                 | Kolkata Knight Riders         | Rajasthan Royals                | SR Watson        | 0       |
| 2008 | 336006    | Royal Challengers Bangalore      | Kings XI Punjab               | Kings XI Punjab                 | IK Pathan        | 0       |
| 2008 | 336002    | Deccan Chargers                  | Royal Challengers Bangalore   | Royal Challengers Bangalore     | DW Steyn         | 1       |
| 2008 | 336029    | Chennai Super Kings              | Royal Challengers Bangalore   | Royal Challengers Bangalore     | P Kumar          | 1       |
| 2008 | 336019    | Kings XI Punjab                  | Rajasthan Royals              | Kings XI Punjab                 | IK Pathan        | 2       |
| 2008 | 336020    | Delhi Daredevils                 | Deccan Chargers               | Delhi Daredevils                | A Mishra         | 2       |
| 2008 | 335985    | Mumbai Indians                   | Royal Challengers Bangalore   | Mumbai Indians                  | DS Kulkarni      | 3       |
| 2008 | 335996    | Royal Challengers Bangalore      | Chennai Super Kings           | Chennai Super Kings             | Joginder Sharma   | 3       |
| 2008 | 335995    | Kings XI Punjab                  | Delhi Daredevils              | Delhi Daredevils                | Shoaib Malik     | 4       |
| 2008 | 336015    | Rajasthan Royals                 | Delhi Daredevils              | Delhi Daredevils                | VY Mahesh        | 4       |
| 2008 | 336017    | Kolkata Knight Riders            | Delhi Daredevils              | Delhi Daredevils                | MF Maharoof      | 4       |
| 2008 | 336029    | Chennai Super Kings              | Royal Challengers Bangalore   | Chennai Super Kings             | JA Morkel        | 4       |
| 2008 | 336033    | Chennai Super Kings              | Rajasthan Royals              | Rajasthan Royals                | Sohail Tanvir    | 4       |
| 2008 | 336035    | Kolkata Knight Riders            | Kings XI Punjab               | Kolkata Knight Riders           | Umar Gul         | 4       |
| 2008 | 336038    | Delhi Daredevils                 | Rajasthan Royals              | Delhi Daredevils                | Mohammad Asif    | 4       |
| 2008 | 335992    | Royal Challengers Bangalore      | Rajasthan Royals              | Rajasthan Royals                | SR Watson        | 5       |
| 2008 | 336013    | Chennai Super Kings              | Kings XI Punjab               | Kings XI Punjab                 | IK Pathan        | 5       |
| 2008 | 335995    | Kings XI Punjab                  | Delhi Daredevils              | Kings XI Punjab                 | IK Pathan        | 6       |
| 2008 | 336004    | Mumbai Indians                   | Delhi Daredevils              | Delhi Daredevils                | VY Mahesh        | 6       |
| 2008 | 336024    | Deccan Chargers                  | Mumbai Indians                | Mumbai Indians                  | A Nehra          | 6       |

- similarly for all other seasons also , it will give the output. Due to space and size constraints, I have omitted the other seasons output.

### Q4. Biggest match winners
#### Question:   Find the bowlers with the most 4 or 5-wicket hauls in IPL history.
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
| bowler     | four_five_wicket_hauls |
|------------------|---------|
| SP Narine        | 8       |
| SL Malinga       | 7       |
| YS Chahal        | 7       |
| K Rabada         | 7       |
| A Mishra         | 5       |
| JJ Bumrah        | 5       |
| L Balaji         | 4       |
| RA Jadeja        | 4       |
| MM Sharma        | 4       |
| B Kumar          | 4       |
| CH Morris        | 4       |
| AJ Tye           | 4       |
| Kuldeep Yadav    | 4       |
| A Kumble         | 3       |
| MM Patel         | 3       |
| UT Yadav         | 3       |
| JP Faulkner      | 3       |
| Imran Tahir      | 3       |
| Sandeep Sharma   | 3       |
| AD Russell       | 3       |

- here output contains some other players also, due to space, I limited it to 20 only.

### Q5. Top 5 Death over specialist bowlers
#### Question:   Identify the bowlers with the most wickets in the death overs (overs 16-20) for each season.

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
| season | bowler         | total_wickets |
|------|---------------------|---------|
| 2008 | Sohail Tanvir       | 18      |
| 2008 | JA Morkel           | 13      |
| 2008 | RP Singh            | 11      |
| 2008 | MF Maharoof         | 10      |
| 2008 | VY Mahesh           | 10      |
| 2009 | SL Malinga          | 16      |
| 2009 | RP Singh            | 15      |
| 2009 | A Nehra             | 13      |
| 2009 | IK Pathan           | 13      |
| 2009 | DP Nannes           | 11      |
| 2010 | SL Malinga          | 10      |
| 2010 | IK Pathan           | 10      |
| 2010 | SW Tait             | 9       |
| 2010 | Z Khan              | 9       |
| 2010 | RJ Harris           | 9       |
| 2011 | SL Malinga          | 16      |
| 2011 | DE Bollinger        | 14      |
| 2011 | RJ Harris           | 11      |
| 2011 | S Aravind           | 11      |
| 2011 | SK Trivedi          | 9       |
| 2012 | SP Narine           | 21      |
| 2012 | SL Malinga          | 18      |
| 2012 | M Morkel            | 16      |
| 2012 | Z Khan              | 13      |
| 2012 | R Vinay Kumar       | 12      |
| 2013 | DJ Bravo            | 25      |
| 2013 | JP Faulkner         | 21      |
| 2013 | SP Narine           | 16      |
| 2013 | R Vinay Kumar       | 16      |
| 2013 | DW Steyn            | 16      |
| 2014 | MM Sharma           | 16      |
| 2014 | SP Narine           | 15      |
| 2014 | SL Malinga          | 14      |
| 2014 | B Kumar             | 12      |
| 2014 | MG Johnson          | 11      |
| 2015 | DJ Bravo            | 24      |
| 2015 | SL Malinga          | 17      |
| 2015 | Imran Tahir         | 13      |
| 2015 | B Kumar             | 12      |
| 2015 | MA Starc            | 12      |
| 2016 | SR Watson           | 16      |
| 2016 | B Kumar             | 14      |
| 2016 | MM Sharma           | 13      |
| 2016 | DJ Bravo            | 12      |
| 2016 | MJ McClenaghan      | 11      |
| 2017 | B Kumar             | 19      |
| 2017 | JD Unadkat          | 18      |
| 2017 | JJ Bumrah           | 13      |
| 2017 | S Kaul              | 10      |
| 2017 | Basil Thampi        | 10      |
| 2018 | AJ Tye              | 16      |
| 2018 | JJ Bumrah           | 13      |
| 2018 | S Kaul              | 13      |
| 2018 | M Prasidh Krishna   | 12      |
| 2018 | TA Boult            | 11      |
| 2019 | K Rabada            | 21      |
| 2019 | Mohammed Shami      | 17      |
| 2019 | JJ Bumrah           | 14      |
| 2019 | SL Malinga          | 14      |
| 2019 | DJ Bravo            | 12      |
| 2020 | K Rabada            | 20      |
| 2020 | JJ Bumrah           | 15      |
| 2020 | T Natarajan         | 12      |
| 2020 | Mohammed Shami      | 12      |
| 2020 | A Nortje            | 11      |
| 2021 | HV Patel            | 24      |
| 2021 | Avesh Khan          | 15      |
| 2021 | JJ Bumrah           | 13      |
| 2021 | Mohammed Shami      | 13      |
| 2021 | SN Thakur           | 10      |
| 2022 | YS Chahal           | 14      |
| 2022 | DJ Bravo            | 12      |
| 2022 | T Natarajan         | 12      |
| 2022 | AD Russell          | 11      |
| 2022 | JR Hazlewood        | 11      |
| 2023 | M Pathirana         | 22      |
| 2023 | MM Sharma           | 18      |
| 2023 | TU Deshpande        | 13      |
| 2023 | YS Chahal           | 12      |
| 2023 | Sandeep Sharma      | 12      |
| 2024 | HV Patel            | 22      |
| 2024 | Mukesh Kumar        | 14      |
| 2024 | TU Deshpande        | 11      |
| 2024 | Arshdeep Singh      | 11      |
| 2024 | T Natarajan         | 11      |

- -----------------------------------------------------------------------

### 🙋‍♂️ About Me

I’m **Vaibhav Chavan**, the creator of [iThinkData](https://www.youtube.com/@iThinkData?sub_confirmation=1), a platform dedicated to making data analytics and SQL easy to understand for everyone. Follow along as I dive deep into SQL tutorials, case studies, and data projects on YouTube.

**🌟 Stay Connected with iThinkData ! 🌟**

**🎥 Subscribe to [iThinkData](https://www.youtube.com/@iThinkData?sub_confirmation=1) :**  🔔 Don't miss out on weekly data challenges, tutorials, and expert insights! 💡📈

**👨‍💼 [LinkedIn](https://linkedin.com/in/vaibhav-chavan) :**  📊 Let’s connect professionally and build a powerful data-driven network! 💼🌐

**💬 Join My [WhatsApp Channel](https://whatsapp.com/channel/0029VaoircxInlqLbopDNS2K) :** 📱 Be the first to get exclusive content, project updates, and new videos! 🚀📊

