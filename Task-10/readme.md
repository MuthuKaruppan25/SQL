# Database Design, Optimization, and Advanced Features

## Overview
This project involves designing a normalized database schema for a complex business scenario (e.g., an eCommerce platform) and implementing advanced SQL features to ensure performance, data integrity, and automation. It covers schema design, indexing strategies, triggers, transactions, and views to enhance database efficiency and maintainability.

## Objectives
- **Schema Design:** Create multiple related tables with proper constraints.
- **Indexing and Performance:** Optimize query execution using indexing.
- **Triggers:** Automate business rule enforcement.
- **Transactions:** Ensure data consistency in multi-step operations.
- **Views and Materialized Views:** Simplify query complexity and optimize performance.
- **Documentation and Testing:** Provide clear documentation and test queries to validate constraints and functionalities.

---

## Schema Design
The database follows normalization principles:

1. **First Normal Form (1NF):** Ensures atomicity of columns.
2. **Second Normal Form (2NF):** Eliminates partial dependency.
3. **Third Normal Form (3NF):** Removes transitive dependencies.
4. **Boyce-Codd Normal Form (BCNF):** Ensures every determinant is a candidate key.

### Tables
#### Customers
```sql
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    phone_no VARCHAR(10) UNIQUE
);
```

#### Products
```sql
CREATE TABLE Products (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    product_name VARCHAR(50),
    price DECIMAL(10,2),
    stock INT
);
```

#### Orders
```sql
CREATE TABLE Orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT,
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);
```

#### OrderDetails
```sql
CREATE TABLE OrderDetails (
    order_detail_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT,
    product_id INT,
    quantity INT,
    price DECIMAL(10,2),
    FOREIGN KEY (order_id) REFERENCES Orders(order_id),
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);
```

---

## Indexing
Indexes are created to speed up queries:
```sql
CREATE INDEX pdt_index ON Products(product_name);
```
### Query Performance Before Indexing
```sql
EXPLAIN SELECT * FROM Products WHERE product_name = 'laptop';
```
Result: Full table scan, leading to slow performance.

### Query Performance After Indexing
```sql
EXPLAIN SELECT * FROM Products WHERE product_name = 'laptop';
```
Result: Index is used, significantly improving query performance.

---

## Triggers
Triggers automate specific business rules.

### Trigger to Reduce Stock After Order Placement
```sql
CREATE TRIGGER reduce_stock AFTER INSERT ON OrderDetails
FOR EACH ROW
BEGIN
    UPDATE Products
    SET stock = stock - NEW.quantity
    WHERE product_id = NEW.product_id;
END;
```
#### Example:
- Before Order: Stock = 10
- Insert Order: `INSERT INTO OrderDetails (order_id, product_id, quantity, price) VALUES (20, 1, 5, 5000);`
- After Order: Stock = 5

### Trigger to Prevent Over-Ordering
```sql
CREATE TRIGGER prevent_over_ordering BEFORE INSERT ON OrderDetails
FOR EACH ROW
BEGIN
    DECLARE available_stock INT;
    SELECT stock INTO available_stock FROM Products WHERE product_id = NEW.product_id;
    IF available_stock < NEW.quantity THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Not enough stock available';
    END IF;
END;
```
#### Example:
- Attempting to order more than available stock results in an error.

---

## Transactions
Ensuring consistency during multi-step operations:

```sql
START TRANSACTION;
UPDATE Products SET stock = stock - 5 WHERE product_id = 1;
INSERT INTO Orders (customer_id, order_date) VALUES (2, CURRENT_TIMESTAMP());
COMMIT;
```
If an error occurs, rollback ensures no partial updates:
```sql
ROLLBACK;
```

---

## Views
Views simplify complex queries:
```sql
CREATE VIEW OrderSummary AS
SELECT o.order_id, c.name, o.order_date, SUM(od.quantity * od.price) AS total_amount
FROM Orders o
JOIN Customers c ON o.customer_id = c.customer_id
JOIN OrderDetails od ON o.order_id = od.order_id
GROUP BY o.order_id, c.name, o.order_date;
```
Querying view:
```sql
SELECT * FROM OrderSummary;
```

---

## Conclusion
This project implements a structured and optimized relational database using MySQL. It follows normalization principles, indexing for query performance, triggers for automation, transactions for consistency, and views for simplified reporting.

### Key Takeaways:
- **Normalization** reduces redundancy and improves integrity.
- **Indexing** speeds up data retrieval.
- **Triggers** enforce business rules automatically.
- **Transactions** ensure consistency.
- **Views** simplify reporting and complex queries.

---

## Author
Developed by [Your Name]

