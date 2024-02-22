# Find managers of all employees
### along with sample data tables for SQL Server, MySQL, and PostgreSQL.

## SQL Server:

### Sample Data:
```sql
CREATE TABLE Employees (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(50),
    manager_id INT
);

INSERT INTO Employees (emp_id, emp_name, manager_id) VALUES
(1, 'John', NULL),
(2, 'Alice', 1),
(3, 'Bob', 1),
(4, 'Carol', 2),
(5, 'David', 3);
```

### Simple Level:
1. **Using a Self Join:**
   ```sql
   SELECT e.emp_name AS Employee, m.emp_name AS Manager
   FROM Employees e
   LEFT JOIN Employees m ON e.manager_id = m.emp_id;
   ```

### Intermediate Level:
2. **Using Common Table Expression (CTE):**
   ```sql
   WITH EmployeeHierarchy AS (
       SELECT emp_id, emp_name, manager_id
       FROM Employees
       UNION ALL
       SELECT e.emp_id, e.emp_name, eh.manager_id
       FROM Employees e
       JOIN EmployeeHierarchy eh ON e.manager_id = eh.emp_id
   )
   SELECT e.emp_name AS Employee, m.emp_name AS Manager
   FROM EmployeeHierarchy e
   LEFT JOIN Employees m ON e.manager_id = m.emp_id;
   ```

### Advanced Level:
3. **Using Recursive CTE:**
   ```sql
   WITH EmployeeHierarchy AS (
       SELECT emp_id, emp_name, manager_id
       FROM Employees
       WHERE manager_id IS NULL
       UNION ALL
       SELECT e.emp_id, e.emp_name, eh.manager_id
       FROM Employees e
       JOIN EmployeeHierarchy eh ON e.manager_id = eh.emp_id
   )
   SELECT e.emp_name AS Employee, m.emp_name AS Manager
   FROM EmployeeHierarchy e
   LEFT JOIN Employees m ON e.manager_id = m.emp_id;
   ```

## MySQL:

### Sample Data:
```sql
CREATE TABLE Employees (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(50),
    manager_id INT
);

INSERT INTO Employees (emp_id, emp_name, manager_id) VALUES
(1, 'John', NULL),
(2, 'Alice', 1),
(3, 'Bob', 1),
(4, 'Carol', 2),
(5, 'David', 3);
```

### Simple Level:
1. **Using a Self Join:**
   ```sql
   SELECT e.emp_name AS Employee, m.emp_name AS Manager
   FROM Employees e
   LEFT JOIN Employees m ON e.manager_id = m.emp_id;
   ```

### Intermediate Level:
2. **Using Recursive Common Table Expression (CTE) - Not Supported:**
   MySQL does not support recursive CTEs.

### Advanced Level:
3. **Using Stored Procedure:**
   ```sql
   DELIMITER //
   CREATE PROCEDURE GetEmployeeManagers()
   BEGIN
       DECLARE done INT DEFAULT FALSE;
       DECLARE emp_name VARCHAR(50);
       DECLARE manager_name VARCHAR(50);
       DECLARE cur CURSOR FOR
           SELECT e.emp_name, m.emp_name
           FROM Employees e
           LEFT JOIN Employees m ON e.manager_id = m.emp_id;
       DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
       OPEN cur;
       read_loop: LOOP
           FETCH cur INTO emp_name, manager_name;
           IF done THEN
               LEAVE read_loop;
           END IF;
           SELECT emp_name, manager_name;
       END LOOP;
       CLOSE cur;
   END//
   DELIMITER ;
   CALL GetEmployeeManagers();
   ```

## PostgreSQL:

### Sample Data:
```sql
CREATE TABLE Employees (
    emp_id SERIAL PRIMARY KEY,
    emp_name VARCHAR(50),
    manager_id INT
);

INSERT INTO Employees (emp_name, manager_id) VALUES
('John', NULL),
('Alice', 1),
('Bob', 1),
('Carol', 2),
('David', 3);
```

### Simple Level:
1. **Using a Self Join:**
   ```sql
   SELECT e.emp_name AS Employee, m.emp_name AS Manager
   FROM Employees e
   LEFT JOIN Employees m ON e.manager_id = m.emp_id;
   ```

### Intermediate Level:
2. **Using Recursive Common Table Expression (CTE):**
   ```sql
   WITH RECURSIVE EmployeeHierarchy AS (
       SELECT emp_id, emp_name, manager_id
       FROM Employees
       UNION ALL
       SELECT e.emp_id, e.emp_name, eh.manager_id
       FROM Employees e
       JOIN EmployeeHierarchy eh ON e.manager_id = eh.emp_id
   )
   SELECT e.emp_name AS Employee, m.emp_name AS Manager
   FROM EmployeeHierarchy e
   LEFT JOIN Employees m ON e.manager_id = m.emp_id;
   ```

### Advanced Level:
3. **Using pg_proc System Catalog:**
   ```sql
   SELECT
       e.emp_name AS Employee,
       (SELECT emp_name FROM Employees WHERE emp_id = e.manager_id) AS Manager
   FROM Employees e;
   ```

These solutions allow you to find the managers of all employees in SQL Server, MySQL, and PostgreSQL databases, categorized into simple, intermediate, and advanced levels.
