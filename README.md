# IPL-Data-Analysis-Case-Study-Using-SQL

**Case study on [iThinkData](https://www.youtube.com/@iThinkData) By Vaibhav Chavan**

Subscribe to [iThinkData](https://www.youtube.com/@iThinkData) for more SQL case studies and tutorials.

---

## A. Basic Level Questions

### Q1. Find out each season's winner team.

```sql
SELECT  season, 
        winner 
FROM matches
WHERE match_type = 'Final'
GROUP BY season
ORDER BY season;
```
