# Sending vs. Opening Snaps [Snapchat SQL Interview Question]

Assume you're given tables with information on Snapchat users, including their ages and time spent sending and opening snaps.

Write a query to obtain a breakdown of the time spent sending vs. opening snaps as a percentage of total time spent on these activities grouped by age group. Round the percentage to 2 decimal places in the output.

Notes:

- Calculate the following percentages:
    - time spent sending / (Time spent sending + Time spent opening)
    - Time spent opening / (Time spent sending + Time spent opening)
- To avoid integer division in percentages, multiply by 100.0 and not 100.

*Effective April 15th, 2023, the solution has been updated and optimised.*

### **`activities` Table**

| Column Name | Type |
| --- | --- |
| activity_id | integer |
| user_id | integer |
| activity_type | string ('send', 'open', 'chat') |
| time_spent | float |
| activity_date | datetime |

### **`activities` Example Input**

| activity_id | user_id | activity_type | time_spent | activity_date |
| --- | --- | --- | --- | --- |
| 7274 | 123 | open | 4.50 | 06/22/2022 12:00:00 |
| 2425 | 123 | send | 3.50 | 06/22/2022 12:00:00 |
| 1413 | 456 | send | 5.67 | 06/23/2022 12:00:00 |
| 1414 | 789 | chat | 11.00 | 06/25/2022 12:00:00 |
| 2536 | 456 | open | 3.00 | 06/25/2022 12:00:00 |

### **`age_breakdown` Table**

| Column Name | Type |
| --- | --- |
| user_id | integer |
| age_bucket | string ('21-25', '26-30', '31-25') |

### **`age_breakdown` Example Input**

| user_id | age_bucket |
| --- | --- |
| 123 | 31-35 |
| 456 | 26-30 |
| 789 | 21-25 |

### **Example Output**

| age_bucket | send_perc | open_perc |
| --- | --- | --- |
| 26-30 | 65.40 | 34.60 |
| 31-35 | 43.75 | 56.25 |

**ANSWER**

WITH snapchat
AS (
SELECT ag.age_bucket as age_bucket ,sum(CASE when activity_type ='open' then act.time_spent else 0 END) AS opening_time,
sum(CASE when activity_type ='send' then act.time_spent else 0 END) AS time_spending,
sum( act.time_spent) AS total_time
FROM activities as act
LEFT JOIN age_breakdown as ag ON act.user_id = ag.user_id
where  act.activity_type IN ('open','send')
group by ag.age_bucket
order by ag.age_bucket ASC )
select age_bucket,ROUND((time_spending*100/total_time),2) as time_spent_snap,ROUND((opening_time*100/total_time),2) as opening_snap from snapchat;