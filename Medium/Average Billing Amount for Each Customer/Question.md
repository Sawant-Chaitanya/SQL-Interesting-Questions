# Average Billing Amount for Each Customer

A table named `billing` consists of the following columns:

- customer_id
- customer_name
- billing_id
- billing_creation_date
- billed_amount

Display the average billing amount for each customer between 2019 to 2021. Assume a $0 billing amount if nothing is billed for a particular year for that customer.

## Example

Given the following data:

| customer_id | customer_name | billing_id | billing_creation_date | billed_amount |
|-------------|---------------|------------|-----------------------|---------------|
| 1           | A             | id1        | 10-10-2020            | $100          |
| 1           | A             | id2        | 11-11-2020            | $150          |
| 1           | A             | id3        | 12-11-2021            | $100          |
| 2           | B             | id4        | 10-11-2019            | $150          |
| 2           | B             | id5        | 11-11-2020            | $200          |
| 2           | B             | id6        | 12-11-2021            | $250          |
| 3           | C             | id7        | 01-01-2018            | $100          |
| 3           | C             | id8        | 05-01-2019            | $250          |
| 3           | C             | id9        | 06-01-2021            | $300          |

The output should be:

| customer_id | customer_name | avg_billing_amount |
|-------------|---------------|--------------------|
| 1           | A             | $87.50             |
| 2           | B             | $150.00            |
| 3           | C             | $183.33            |

Explanation:
- The average billing amount for customer A is (0 + 100 + 150 + 100) / 4 = $87.50.
- The average billing amount for customer B is (150 + 200 + 250) / 3 = $200.00.
- The average billing amount for customer C is (0 + 250 + 300) / 3 = $183.33.
