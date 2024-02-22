# Find/delete all the duplicate values
### along with sample data tables for SQL Server,My SQ, PostgreSQL categorized into simple, intermediate, and advanced levels.

## SQL Server:

### Sample Data:
```sql
CREATE TABLE Employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50),
    department_id INT
);

INSERT INTO Employees (employee_id, employee_name, department_id) VALUES
(1, 'John', 101),
(2, 'Alice', 102),
(3, 'Bob', 101),
(4, 'Alice', 102),
(5, 'David', 103),
(6, 'Alice', 102),
(7, 'John', 101),
(8, 'Eva', 104),
(9, 'Alice', 102),
(10, 'John', 101);
```

### Simple Level:
1. **Using GROUP BY and HAVING Clause to Find Duplicate Records:**
   ```sql
   SELECT employee_name, COUNT(*) AS count
   FROM Employees
   GROUP BY employee_name
   HAVING COUNT(*) > 1;
   ```

2. **Using Common Table Expression (CTE) and ROW_NUMBER() Function:**
   ```sql
   WITH DuplicatesCTE AS (
       SELECT employee_id, employee_name, department_id,
              ROW_NUMBER() OVER (PARTITION BY employee_name ORDER BY employee_id) AS rn
       FROM Employees
   )
   SELECT employee_id, employee_name, department_id
   FROM DuplicatesCTE
   WHERE rn > 1;
   ```

### Intermediate Level:
3. **Using Subquery with DELETE Statement to Remove Duplicates:**
   ```sql
   DELETE FROM Employees
   WHERE employee_id NOT IN (
       SELECT MIN(employee_id)
       FROM Employees
       GROUP BY employee_name
   );
   ```

4. **Using Common Table Expression (CTE) and DELETE Statement:**
   ```sql
   WITH DuplicatesCTE AS (
       SELECT employee_id, employee_name, department_id,
              ROW_NUMBER() OVER (PARTITION BY employee_name ORDER BY employee_id) AS rn
       FROM Employees
   )
   DELETE FROM DuplicatesCTE
   WHERE rn > 1;
   ```

### Advanced Level:
5. **Using INNER JOIN to Find Duplicates:**
   ```sql
   SELECT e1.employee_id, e1.employee_name, e1.department_id
   FROM Employees e1
   INNER JOIN Employees e2 ON e1.employee_name = e2.employee_name AND e1.employee_id > e2.employee_id;
   ```

6. **Using Common Table Expression (CTE) and CROSS APPLY:**
   ```sql
   WITH DuplicatesCTE AS (
       SELECT employee_id, employee_name, department_id
       FROM Employees e1
       CROSS APPLY (
           SELECT TOP 1 employee_id
           FROM Employees e2
           WHERE e1.employee_name = e2.employee_name AND e1.employee_id > e2.employee_id
           ORDER BY employee_id
       ) AS ca
   )
   SELECT * FROM DuplicatesCTE;
   ```

## Fastest Way:
The fastest way to find and delete duplicates would be using a simple `GROUP BY` query to identify the duplicate records. This approach is efficient as it leverages the database's built-in aggregation functions to quickly identify duplicate values without the need for complex processing or joins.

## Summary:
- For a simple and efficient solution, the `GROUP BY` approach is recommended.
- For more advanced scenarios or when additional processing is required, techniques like CTEs, window functions, and subqueries can be used.

These solutions provide various approaches to find and delete duplicate values in SQL Server, categorized into simple, intermediate, and advanced levels, with the fastest method being the `GROUP BY` approach due to its efficiency and simplicity.


---

## MySQL:

### Sample Data:
```sql
CREATE TABLE Employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50),
    department_id INT
);

INSERT INTO Employees (employee_id, employee_name, department_id) VALUES
(1, 'John', 101),
(2, 'Alice', 102),
(3, 'Bob', 101),
(4, 'Alice', 102),
(5, 'David', 103),
(6, 'Alice', 102),
(7, 'John', 101),
(8, 'Eva', 104),
(9, 'Alice', 102),
(10, 'John', 101);
```

### Simple Level:
1. **Using GROUP BY and HAVING Clause to Find Duplicate Records:**
   ```sql
   SELECT employee_name, COUNT(*) AS count
   FROM Employees
   GROUP BY employee_name
   HAVING COUNT(*) > 1;
   ```

2. **Using Common Table Expression (CTE) and ROW_NUMBER() Function:**
   ```sql
   WITH DuplicatesCTE AS (
       SELECT employee_id, employee_name, department_id,
              ROW_NUMBER() OVER (PARTITION BY employee_name ORDER BY employee_id) AS rn
       FROM Employees
   )
   SELECT employee_id, employee_name, department_id
   FROM DuplicatesCTE
   WHERE rn > 1;
   ```

### Intermediate Level:
3. **Using Subquery with DELETE Statement to Remove Duplicates:**
   ```sql
   DELETE FROM Employees
   WHERE employee_id NOT IN (
       SELECT MIN(employee_id)
       FROM Employees
       GROUP BY employee_name
   );
   ```

4. **Using Common Table Expression (CTE) and DELETE Statement:**
   ```sql
   WITH DuplicatesCTE AS (
       SELECT employee_id, employee_name, department_id,
              ROW_NUMBER() OVER (PARTITION BY employee_name ORDER BY employee_id) AS rn
       FROM Employees
   )
   DELETE FROM DuplicatesCTE
   WHERE rn > 1;
   ```

### Advanced Level:
5. **Using INNER JOIN to Find Duplicates:**
   ```sql
   SELECT e1.employee_id, e1.employee_name, e1.department_id
   FROM Employees e1
   INNER JOIN Employees e2 ON e1.employee_name = e2.employee_name AND e1.employee_id > e2.employee_id;
   ```

6. **Using Common Table Expression (CTE) and CROSS APPLY:**
   ```sql
   WITH DuplicatesCTE AS (
       SELECT employee_id, employee_name, department_id
       FROM Employees e1
       CROSS APPLY (
           SELECT employee_id
           FROM Employees e2
           WHERE e1.employee_name = e2.employee_name AND e1.employee_id > e2.employee_id
           ORDER BY employee_id
           LIMIT 1
       ) AS ca
   )
   SELECT * FROM DuplicatesCTE;
   ```

## Fastest Way:
The fastest way to find and delete duplicates would be using a simple `GROUP BY` query to identify the duplicate records. This approach is efficient as it leverages the database's built-in aggregation functions to quickly identify duplicate values without the need for complex processing or joins.

## Summary:
- For a simple and efficient solution, the `GROUP BY` approach is recommended.
- For more advanced scenarios or when additional processing is required, techniques like CTEs, window functions, and subqueries can be used.

These solutions provide various approaches to find and delete duplicate values in MySQL, categorized into simple, intermediate, and advanced levels, with the fastest method being the `GROUP BY` approach due to its efficiency and simplicity.

----


## PostgreSQL:

### Sample Data:
```sql
CREATE TABLE Employees (
    employee_id SERIAL PRIMARY KEY,
    employee_name VARCHAR(50),
    department_id INT
);

INSERT INTO Employees (employee_name, department_id) VALUES
('John', 101),
('Alice', 102),
('Bob', 101),
('Alice', 102),
('David', 103),
('Alice', 102),
('John', 101),
('Eva', 104),
('Alice', 102),
('John', 101);
```

### Simple Level:
1. **Using GROUP BY and HAVING Clause to Find Duplicate Records:**
   ```sql
   SELECT employee_name, COUNT(*) AS count
   FROM Employees
   GROUP BY employee_name
   HAVING COUNT(*) > 1;
   ```

2. **Using Common Table Expression (CTE) and ROW_NUMBER() Function:**
   ```sql
   WITH DuplicatesCTE AS (
       SELECT employee_id, employee_name, department_id,
              ROW_NUMBER() OVER (PARTITION BY employee_name ORDER BY employee_id) AS rn
       FROM Employees
   )
   SELECT employee_id, employee_name, department_id
   FROM DuplicatesCTE
   WHERE rn > 1;
   ```

### Intermediate Level:
3. **Using Subquery with DELETE Statement to Remove Duplicates:**
   ```sql
   DELETE FROM Employees
   WHERE employee_id NOT IN (
       SELECT MIN(employee_id)
       FROM Employees
       GROUP BY employee_name
   );
   ```

4. **Using Common Table Expression (CTE) and DELETE Statement:**
   ```sql
   WITH DuplicatesCTE AS (
       SELECT employee_id, employee_name, department_id,
              ROW_NUMBER() OVER (PARTITION BY employee_name ORDER BY employee_id) AS rn
       FROM Employees
   )
   DELETE FROM DuplicatesCTE
   WHERE rn > 1;
   ```

### Advanced Level:
5. **Using INNER JOIN to Find Duplicates:**
   ```sql
   SELECT e1.employee_id, e1.employee_name, e1.department_id
   FROM Employees e1
   INNER JOIN Employees e2 ON e1.employee_name = e2.employee_name AND e1.employee_id > e2.employee_id;
   ```

6. **Using Common Table Expression (CTE) and CROSS JOIN LATERAL:**
   ```sql
   WITH DuplicatesCTE AS (
       SELECT e1.employee_id, e1.employee_name, e1.department_id
       FROM Employees e1
       CROSS JOIN LATERAL (
           SELECT employee_id
           FROM Employees e2
           WHERE e1.employee_name = e2.employee_name AND e1.employee_id > e2.employee_id
           ORDER BY employee_id
           LIMIT 1
       ) AS ca
   )
   SELECT * FROM DuplicatesCTE;
   ```

## Fastest Way:
The fastest way to find and delete duplicates would be using a simple `GROUP BY` query to identify the duplicate records. This approach is efficient as it leverages the database's built-in aggregation functions to quickly identify duplicate values without the need for complex processing or joins.

## Summary:
- For a simple and efficient solution, the `GROUP BY` approach is recommended.
- For more advanced scenarios or when additional processing is required, techniques like CTEs, window functions, and subqueries can be used.

These solutions provide various approaches to find and delete duplicate values in PostgreSQL, categorized into simple, intermediate, and advanced levels, with the fastest method being the `GROUP BY` approach due to its efficiency and simplicity.
