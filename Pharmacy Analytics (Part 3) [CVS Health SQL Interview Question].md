# Pharmacy Analytics (Part 3) [CVS Health SQL Interview Question]

CVS Health is trying to better understand its pharmacy sales, and how well different products are selling.

Write a query to find the total drug sales for each manufacturer. Round your answer to the closest million, and report your results in descending order of total sales.

Because this data is being directly fed into a dashboard which is being seen by business stakeholders, format your result like this: "$36 million".

### **`pharmacy_sales` Table:**

| Column Name | Type |
| --- | --- |
| product_id | integer |
| units_sold | integer |
| total_sales | decimal |
| cogs | decimal |
| manufacturer | varchar |
| drug | varchar |

### **`pharmacy_sales` Example Input:**

| product_id | units_sold | total_sales | cogs | manufacturer | drug |
| --- | --- | --- | --- | --- | --- |
| 94 | 132362 | 2041758.41 | 1373721.70 | Biogen | UP and UP |
| 9 | 37410 | 293452.54 | 208876.01 | Eli Lilly | Zyprexa |
| 50 | 90484 | 2521023.73 | 2742445.9 | Eli Lilly | Dermasorb |
| 61 | 77023 | 500101.61 | 419174.97 | Biogen | Varicose Relief |
| 136 | 144814 | 1084258.00 | 1006447.73 | Biogen | Burkhart |

### **Example Output:**

| manufacturer | sale |
| --- | --- |
| Biogen | $4 million |
| Eli Lilly | $3 million |

ANSWER

SELECT manufacturer,CAST(concat('$',round(sum(total_sales)/1000000),' million') as varchar) AS TOTAL_SALE
FROM pharmacy_sales
GROUP BY manufacturer
ORDER BY sum(total_sales) desc;