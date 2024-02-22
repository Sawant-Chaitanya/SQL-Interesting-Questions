# Find category-wise top value

### along with sample data tables for SQL Server, MySQL, and PostgreSQL.

## SQL Server:

### Sample Data:
```sql
CREATE TABLE Products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(50),
    category_id INT,
    price DECIMAL(10, 2)
);

INSERT INTO Products (product_id, product_name, category_id, price) VALUES
(1, 'Laptop', 1, 1200.00),
(2, 'Smartphone', 1, 800.00),
(3, 'Tablet', 1, 500.00),
(4, 'TV', 2, 1500.00),
(5, 'Refrigerator', 2, 1000.00),
(6, 'Microwave', 2, 300.00),
(7, 'Headphones', 3, 200.00),
(8, 'Speaker', 3, 150.00),
(9, 'Camera', 3, 700.00);
```

### Simple Level:
1. **Using GROUP BY and MAX():**
   ```sql
   SELECT category_id, MAX(price) AS top_price
   FROM Products
   GROUP BY category_id;
   ```

### Intermediate Level:
2. **Using Common Table Expression (CTE):**
   ```sql
   WITH TopPrices AS (
       SELECT category_id, price,
              ROW_NUMBER() OVER (PARTITION BY category_id ORDER BY price DESC) AS rn
       FROM Products
   )
   SELECT category_id, price AS top_price
   FROM TopPrices
   WHERE rn = 1;
   ```

### Advanced Level:
3. **Using Subquery with JOIN:**
   ```sql
   SELECT p.category_id, p.price AS top_price
   FROM Products p
   INNER JOIN (
       SELECT category_id, MAX(price) AS max_price
       FROM Products
       GROUP BY category_id
   ) AS max_prices ON p.category_id = max_prices.category_id AND p.price = max_prices.max_price;
   ```

## MySQL:

### Sample Data:
```sql
CREATE TABLE Products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(50),
    category_id INT,
    price DECIMAL(10, 2)
);

INSERT INTO Products (product_id, product_name, category_id, price) VALUES
(1, 'Laptop', 1, 1200.00),
(2, 'Smartphone', 1, 800.00),
(3, 'Tablet', 1, 500.00),
(4, 'TV', 2, 1500.00),
(5, 'Refrigerator', 2, 1000.00),
(6, 'Microwave', 2, 300.00),
(7, 'Headphones', 3, 200.00),
(8, 'Speaker', 3, 150.00),
(9, 'Camera', 3, 700.00);
```

### Simple Level:
1. **Using GROUP BY and MAX():**
   ```sql
   SELECT category_id, MAX(price) AS top_price
   FROM Products
   GROUP BY category_id;
   ```

### Intermediate Level:
2. **Using Subquery in WHERE Clause:**
   ```sql
   SELECT category_id, price AS top_price
   FROM Products
   WHERE (category_id, price) IN (
       SELECT category_id, MAX(price) AS top_price
       FROM Products
       GROUP BY category_id
   );
   ```

### Advanced Level:
3. **Using Window Functions:**
   ```sql
   SELECT DISTINCT category_id, FIRST_VALUE(price) OVER (PARTITION BY category_id ORDER BY price DESC) AS top_price
   FROM Products;
   ```

## PostgreSQL:

### Sample Data:
```sql
CREATE TABLE Products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(50),
    category_id INT,
    price DECIMAL(10, 2)
);

INSERT INTO Products (product_name, category_id, price) VALUES
('Laptop', 1, 1200.00),
('Smartphone', 1, 800.00),
('Tablet', 1, 500.00),
('TV', 2, 1500.00),
('Refrigerator', 2, 1000.00),
('Microwave', 2, 300.00),
('Headphones', 3, 200.00),
('Speaker', 3, 150.00),
('Camera', 3, 700.00);
```

### Simple Level:
1. **Using GROUP BY and MAX():**
   ```sql
   SELECT category_id, MAX(price) AS top_price
   FROM Products
   GROUP BY category_id;
   ```

### Intermediate Level:
2. **Using Subquery with JOIN:**
   ```sql
   SELECT p.category_id, p.price AS
   top_price
   FROM Products p
   INNER JOIN (
       SELECT category_id, MAX(price) AS max_price
       FROM Products
       GROUP BY category_id
   ) AS max_prices ON p.category_id = max_prices.category_id AND p.price = max_prices.max_price;
   ```

### Advanced Level:
3. **Using DISTINCT ON:**
   ```sql
   SELECT DISTINCT ON (category_id) category_id, price AS top_price
   FROM Products
   ORDER BY category_id, price DESC;
   ```
