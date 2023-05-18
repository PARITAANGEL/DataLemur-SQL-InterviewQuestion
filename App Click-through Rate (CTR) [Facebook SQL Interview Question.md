# App Click-through Rate (CTR) [Facebook SQL Interview Question]

Assume you have an events table on Facebook app analytics. Write a query to calculate the click-through rate (CTR) for the app in 2022 and round the results to 2 decimal places.

Definition and note:

- Percentage of click-through rate (CTR) = 100.0 * Number of clicks / Number of impressions
- To avoid integer division, multiply the CTR by 100.0, not 100.

### **`events` Table:**

| Column Name | Type |
| --- | --- |
| app_id | integer |
| event_type | string |
| timestamp | datetime |

### **`events` Example Input:**

| app_id | event_type | timestamp |
| --- | --- | --- |
| 123 | impression | 07/18/2022 11:36:12 |
| 123 | impression | 07/18/2022 11:37:12 |
| 123 | click | 07/18/2022 11:37:42 |
| 234 | impression | 07/18/2022 14:15:12 |
| 234 | click | 07/18/2022 14:16:12 |

### **Example Output:**

| app_id | ctr |
| --- | --- |
| 123 | 50.00 |
| 234 | 100.00 |

**ANSWER**

select app_id,round((100.0*num_click/num_impre),2) as ctc
from(SELECT app_id,sum(case when event_type = 'impression' then 1 end )as num_impre,sum(case when event_type ='click' then 1 end) as num_click
FROM events
where date_part('year',timestamp) = 2022
group by app_id
) as t;