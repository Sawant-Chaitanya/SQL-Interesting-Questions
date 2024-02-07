```SQL
--SQL Server/MYSQL
SELECT 
    category, 
    AVG(price) AS average_price_within_last_month
FROM 
    products
WHERE 
    DATEDIFF(DAY, date, GETDATE()) <= 30
GROUP BY 
    category;
```

```SQL
--PostgreSQL
SELECT 
    category, 
    AVG(price) AS average_price_within_last_month
FROM 
    products
WHERE 
    date >= CURRENT_DATE - INTERVAL '1 MONTH'
GROUP BY 
    category;

```
