

# Finding Second Highest or Second Lowest Value in SQL Databases

## SQL Server:

### Simple Level:
1. **Using ORDER BY and OFFSET-FETCH:**
   - For the second highest:
     ```sql
     SELECT TOP 1 column_name
     FROM table_name
     ORDER BY column_name DESC OFFSET 1 ROW FETCH NEXT 1 ROW ONLY;
     ```
   - For the second lowest:
     ```sql
     SELECT TOP 1 column_name
     FROM table_name
     ORDER BY column_name ASC OFFSET 1 ROW FETCH NEXT 1 ROW ONLY;
     ```

### Intermediate Level:
2. **Using Common Table Expressions (CTEs):**
   - For the second highest:
     ```sql
     WITH RankedValues AS (
         SELECT column_name, ROW_NUMBER() OVER (ORDER BY column_name DESC) AS RowNum
         FROM table_name
     )
     SELECT column_name
     FROM RankedValues
     WHERE RowNum = 2;
     ```
   - For the second lowest:
     ```sql
     WITH RankedValues AS (
         SELECT column_name, ROW_NUMBER() OVER (ORDER BY column_name ASC) AS RowNum
         FROM table_name
     )
     SELECT column_name
     FROM RankedValues
     WHERE RowNum = 2;
     ```

### Advanced Level:
3. **Using Common Table Expressions (CTEs) with DENSE_RANK:**
   - For the second highest:
     ```sql
     WITH RankedValues AS (
         SELECT column_name, DENSE_RANK() OVER (ORDER BY column_name DESC) AS Rank
         FROM table_name
     )
     SELECT column_name
     FROM RankedValues
     WHERE Rank = 2;
     ```
   - For the second lowest:
     ```sql
     WITH RankedValues AS (
         SELECT column_name, DENSE_RANK() OVER (ORDER BY column_name ASC) AS Rank
         FROM table_name
     )
     SELECT column_name
     FROM RankedValues
     WHERE Rank = 2;
     ```

4. **Using Subqueries:**
   - For the second highest:
     ```sql
     SELECT MIN(column_name)
     FROM (
         SELECT TOP 2 column_name
         FROM table_name
         ORDER BY column_name DESC
     ) AS SubQuery;
     ```
   - For the second lowest:
     ```sql
     SELECT MIN(column_name)
     FROM (
         SELECT TOP 2 column_name
         FROM table_name
         ORDER BY column_name ASC
     ) AS SubQuery;
     ```

## MySQL:

### Simple Level:
1. **Using ORDER BY and LIMIT:**
   - For the second highest:
     ```sql
     SELECT column_name
     FROM table_name
     ORDER BY column_name DESC
     LIMIT 1, 1;
     ```
   - For the second lowest:
     ```sql
     SELECT column_name
     FROM table_name
     ORDER BY column_name ASC
     LIMIT 1, 1;
     ```

### Intermediate Level:
2. **Using Subqueries with LIMIT and OFFSET:**
   - For the second highest:
     ```sql
     SELECT column_name
     FROM table_name
     ORDER BY column_name DESC
     LIMIT 1 OFFSET 1;
     ```
   - For the second lowest:
     ```sql
     SELECT column_name
     FROM table_name
     ORDER BY column_name ASC
     LIMIT 1 OFFSET 1;
     ```

### Advanced Level:
3. **Using Variables:**
   - For the second highest:
     ```sql
     SELECT column_name
     FROM (
         SELECT column_name, @rownum := @rownum + 1 AS rank
         FROM table_name, (SELECT @rownum := 0) AS r
         ORDER BY column_name DESC
     ) AS ranked
     WHERE rank = 2;
     ```
   - For the second lowest:
     ```sql
     SELECT column_name
     FROM (
         SELECT column_name, @rownum := @rownum + 1 AS rank
         FROM table_name, (SELECT @rownum := 0) AS r
         ORDER BY column_name ASC
     ) AS ranked
     WHERE rank = 2;
     ```

## PostgreSQL:

### Simple Level:
1. **Using ORDER BY and LIMIT:**
   - For the second highest:
     ```sql
     SELECT column_name
     FROM table_name
     ORDER BY column_name DESC
     LIMIT 1 OFFSET 1;
     ```
   - For the second lowest:
     ```sql
     SELECT column_name
     FROM table_name
     ORDER BY column_name ASC
     LIMIT 1 OFFSET 1;
     ```

### Intermediate Level:
2. **Using Subqueries with LIMIT and OFFSET:**
   - For the second highest:
     ```sql
     SELECT column_name
     FROM table_name
     ORDER BY column_name DESC
     LIMIT 1 OFFSET

 1;
     ```
   - For the second lowest:
     ```sql
     SELECT column_name
     FROM table_name
     ORDER BY column_name ASC
     LIMIT 1 OFFSET 1;
     ```

### Advanced Level:
3. **Using Window Functions:**
   - For the second highest:
     ```sql
     SELECT column_name
     FROM (
         SELECT column_name, ROW_NUMBER() OVER (ORDER BY column_name DESC) AS rn
         FROM table_name
     ) AS ranked
     WHERE rn = 2;
     ```
   - For the second lowest:
     ```sql
     SELECT column_name
     FROM (
         SELECT column_name, ROW_NUMBER() OVER (ORDER BY column_name ASC) AS rn
         FROM table_name
     ) AS ranked
     WHERE rn = 2;
     ```

These methods allow you to find the second highest or second lowest value in your SQL database according to your preference and the capabilities of your database system.
