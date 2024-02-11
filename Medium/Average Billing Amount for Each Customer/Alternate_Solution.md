# Alternate Solution
```SQL
WITH years AS (
    SELECT 
        generate_series(2019, 2021) AS year
),
customers AS (
    SELECT DISTINCT 
        customer_id, 
        customer_name 
    FROM 
        billing
),
customer_years AS (
    SELECT 
        c.customer_id,
        c.customer_name,
        y.year
    FROM 
        customers c
    CROSS JOIN 
        years y
),
customer_billing AS (
    SELECT 
        c.customer_id,
        c.customer_name,
        b.billing_creation_date,
        EXTRACT(YEAR FROM b.billing_creation_date) AS year,
        COALESCE(SUM(b.billed_amount), 0) AS total_amount
    FROM 
        customer_years c
    LEFT JOIN 
        billing b ON c.customer_id = b.customer_id AND EXTRACT(YEAR FROM b.billing_creation_date) = c.year
    GROUP BY 
        c.customer_id, c.customer_name, b.billing_creation_date -- Include billing_creation_date in the GROUP BY clause
)
SELECT 
    customer_id,
    customer_name,
    ROUND(AVG(total_amount), 2) AS avg_billing_amount
FROM 
    customer_billing
GROUP BY 
    customer_id, customer_name
ORDER BY 
    customer_id;

```

# Step-by-step explanation for alternate solution

## Problem Statement

A table named `billing` consists of the following columns:

- `customer_id`
- `customer_name`
- `billing_id`
- `billing_creation_date`
- `billed_amount`

We need to display the average billing amount for each customer between the years 2019 to 2021. If a customer has no billing records for a particular year, we assume a $0 billing amount for that year.

## Approach

1. **Generate Years:**
   We start by generating a series of years from 2019 to 2021 using the `generate_series` function. This CTE (`years`) will provide us with a list of years to consider in our analysis.

   ```sql
   WITH years AS (
       SELECT 
           generate_series(2019, 2021) AS year
   ),


2. **Distinct Customers:**
   Next, we identify the distinct customers from the `billing` table. This CTE (`customers`) will give us the unique `customer_id` and `customer_name` combinations.

   ```sql
   customers AS (
       SELECT DISTINCT 
           customer_id, 
           customer_name 
       FROM 
           billing
   ),
   ```

3. **Customer Years:**
   We create a Cartesian product of distinct customers and years to ensure that we have all possible combinations of customers and years. This will help us ensure that each customer has an entry for each year.

   ```sql
   customer_years AS (
       SELECT 
           c.customer_id,
           c.customer_name,
           y.year
       FROM 
           customers c
       CROSS JOIN 
           years y
   ),
   ```

4. **Customer Billing:**
   Here, we join the `customer_years` CTE with the `billing` table to calculate the total billed amount for each customer in each year. We use `LEFT JOIN` to include all customer-year combinations, even if there are no corresponding billing records. We also use `COALESCE` to handle cases where there are no billing records for a customer-year combination, setting the total amount to 0.

   ```sql
   customer_billing AS (
       SELECT 
           c.customer_id,
           c.customer_name,
           b.billing_creation_date,
           EXTRACT(YEAR FROM b.billing_creation_date) AS year,
           COALESCE(SUM(b.billed_amount), 0) AS total_amount
       FROM 
           customer_years c
       LEFT JOIN 
           billing b ON c.customer_id = b.customer_id AND EXTRACT(YEAR FROM b.billing_creation_date) = c.year
       GROUP BY 
           c.customer_id, c.customer_name, b.billing_creation_date -- Include billing_creation_date in the GROUP BY clause
   )
   ```

5. **Calculate Average Billing Amount:**
   Finally, we calculate the average billing amount for each customer by taking the average of the total amounts across all years for each customer.

   ```sql
   SELECT 
       customer_id,
       customer_name,
       ROUND(AVG(total_amount), 2) AS avg_billing_amount
   FROM 
       customer_billing
   GROUP BY 
       customer_id, customer_name
   ORDER BY 
       customer_id;
   ```

By following this process, we ensure that we capture all possible combinations of customers and years, calculate the total billed amount for each customer in each year (considering $0 if there are no billing records), and then calculate the average billing amount for each customer across all years. Finally, we order the results by `customer_id` to ensure consistency in the output.
