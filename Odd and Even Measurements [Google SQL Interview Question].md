# Odd and Even Measurements [Google SQL Interview Question]

Assume you're given a table with measurement values obtained from a Google sensor over multiple days with measurements taken multiple times within each day.A

Write a query to calculate the sum of odd-numbered and even-numbered measurements separately for a particular day and display the results in two different columns. Refer to the Example Output below for the desired format.

Definition:

- Within a day, measurements taken at 1st, 3rd, and 5th times are considered odd-numbered measurements, and measurements taken at 2nd, 4th, and 6th times are considered even-numbered measurements.

*Effective April 15th, 2023, the question and solution for this question have been revised.*

### **`measurements` Table:**

| Column Name | Type |
| --- | --- |
| measurement_id | integer |
| measurement_value | decimal |
| measurement_time | datetime |

### **`measurements` Example Input:**

| measurement_id | measurement_value | measurement_time |
| --- | --- | --- |
| 131233 | 1109.51 | 07/10/2022 09:00:00 |
| 135211 | 1662.74 | 07/10/2022 11:00:00 |
| 523542 | 1246.24 | 07/10/2022 13:15:00 |
| 143562 | 1124.50 | 07/11/2022 15:00:00 |
| 346462 | 1234.14 | 07/11/2022 16:45:00 |

### **Example Output:**

| measurement_day | odd_sum | even_sum |
| --- | --- | --- |
| 07/10/2022 00:00:00 | 2355.75 | 1662.74 |
| 07/11/2022 00:00:00 | 1124.50 | 1234.14 |

**ANSWER**

select measure_date,sum(odd_num_measure) as total_measure_odd,SUM(even_num_measure) as total_measure_even
from(select (case when even_odd % 2 != 0 then measurement_value else 0 end) as odd_num_measure,
(case when even_odd % 2 = 0 then measurement_value else 0 end) as even_num_measure,measure_date
from  (SELECT row_number() OVER(PARTITION BY (date(measurement_time)) order by measurement_time) as even_odd ,
measurement_value,date(measurement_time) as measure_date
FROM measurements) as t) as measure
group by measure_date;