## Detailed explanation for solution for the query provided below [techTFQ](https://youtu.be/jS5_hjFgfzA?si=UI8hwMlcBwr5h3QG)

```SQL
-- Common Table Expression (CTE) to calculate billing sums and counts for each customer between 2019 to 2021
WITH cte AS (
    SELECT 
        customer_id,
        customer_name,
        SUM(CASE WHEN EXTRACT(YEAR FROM billing_creation_date) = 2019 THEN billed_amount ELSE 0 END)::DECIMAL AS bill_2019_sum,
        SUM(CASE WHEN EXTRACT(YEAR FROM billing_creation_date) = 2020 THEN billed_amount ELSE 0 END)::DECIMAL AS bill_2020_sum,
        SUM(CASE WHEN EXTRACT(YEAR FROM billing_creation_date) = 2021 THEN billed_amount ELSE 0 END)::DECIMAL AS bill_2021_sum,
        COUNT(CASE WHEN EXTRACT(YEAR FROM billing_creation_date) = 2019 THEN billed_amount ELSE NULL END) AS bill_2019_cnt,
        COUNT(CASE WHEN EXTRACT(YEAR FROM billing_creation_date) = 2020 THEN billed_amount ELSE NULL END) AS bill_2020_cnt,
        COUNT(CASE WHEN EXTRACT(YEAR FROM billing_creation_date) = 2021 THEN billed_amount ELSE NULL END) AS bill_2021_cnt
    FROM 
        billing
    GROUP BY 
        customer_id, customer_name
)
-- Main query to calculate the average billing amount for each customer
SELECT 
    customer_id, 
    customer_name,
    ROUND((bill_2019_sum + bill_2020_sum + bill_2021_sum) / 
          (CASE WHEN bill_2019_cnt = 0 THEN 1 ELSE bill_2019_cnt END +
           CASE WHEN bill_2020_cnt = 0 THEN 1 ELSE bill_2020_cnt END +
           CASE WHEN bill_2021_cnt = 0 THEN 1 ELSE bill_2021_cnt END), 2) AS avg_billing_amount
FROM 
    cte
ORDER BY 
    customer_id;
```


