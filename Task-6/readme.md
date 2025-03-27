# Date and Time Functions in SQL

## Objective

The purpose of this task is to manipulate and query data based on date and time values using built-in SQL functions.

## Requirements
- Use built-in date functions (e.g., `DATEDIFF`, `DATEADD`, `DATE_SUB`) to calculate intervals or adjust dates.
- Write queries to filter records based on date ranges (e.g., orders placed within the last 30 days).
- Format date outputs using functions like `CONVERT` or `DATE_FORMAT`.

## Queries and Explanations

### 1. Retrieve All Customers
```sql
SELECT * FROM customers;
```
**Explanation:** This query fetches all records from the `customers` table.

### 2. Retrieve All Orders
```sql
SELECT * FROM orders;
```
**Explanation:** This query retrieves all existing records from the `orders` table.

### 3. Modify the `orderDate` Column Type
```sql
ALTER TABLE orders MODIFY orderDate DATETIME;
```
**Explanation:** This modifies the `orderDate` column in the `orders` table to store both date and time values.

### 4. Insert Current Timestamp in Orders
```sql
INSERT INTO orders(order_id, customer_id, orderDate, amount)
VALUES (1, 1, CURRENT_TIMESTAMP, 10000);
```
**Explanation:** This query inserts a new order with the current date and time.

### 5. Insert an Order 10 Days Ago
```sql
INSERT INTO orders(order_id, customer_id, orderDate, amount)
VALUES (2, 1, DATE_SUB(CURRENT_TIMESTAMP, INTERVAL 10 DAY), 10000);
```
**Explanation:** This inserts an order that was placed 10 days ago.

### 6. Insert an Order 10 Days and 10 Hours Ago
```sql
INSERT INTO orders(order_id, customer_id, orderDate, amount)
VALUES (3, 2, DATE_SUB(DATE_SUB(CURRENT_TIMESTAMP, INTERVAL 10 DAY), INTERVAL 10 HOUR), 10000);
```
**Explanation:** This query records an order placed 10 days and 10 hours before the current time.

### 7. Insert an Order 5 Days Ago
```sql
INSERT INTO orders(order_id, customer_id, orderDate, amount)
VALUES (4, 3, DATE_SUB(CURRENT_TIMESTAMP, INTERVAL 5 DAY), 10000);
```
**Explanation:** This adds an order that was placed 5 days ago.

### 8. Insert an Order 5 Days in the Future
```sql
INSERT INTO orders(order_id, customer_id, orderDate, amount)
VALUES (5, 3, DATE_ADD(CURRENT_TIMESTAMP, INTERVAL 5 DAY), 10000);
```
**Explanation:** This query inserts a future order scheduled for 5 days from now.

### 9. Retrieve Orders Placed in the Last 30 Days
```sql
SELECT t1.name, t1.email, t2.orderDate, t2.amount
FROM customers t1
JOIN (SELECT customer_id, orderDate, amount
      FROM orders
      WHERE DATEDIFF(CURDATE(), DATE(orderDate)) < 30) AS t2
ON t1.customer_id = t2.customer_id;
```
**Explanation:** This query filters orders placed in the last 30 days and joins them with customer details.

### 10. Retrieve Orders for the Current Month
```sql
SELECT t1.name, t1.email, t2.orderDate, t2.amount
FROM customers t1
JOIN (SELECT customer_id, orderDate, amount
      FROM orders
      WHERE MONTH(orderDate) = MONTH(CURDATE())
      AND YEAR(orderDate) = YEAR(CURDATE())) AS t2
ON t1.customer_id = t2.customer_id;
```
**Explanation:** This query retrieves orders placed in the current month.

### 11. Extract Day, Month, and Year of Orders
```sql
SELECT t1.name, t1.email, DAY(t2.orderDate) AS orderedDay,
       MONTH(t2.orderDate) AS orderedMonth, YEAR(t2.orderDate) AS orderedYear,
       t2.amount
FROM customers t1
JOIN (SELECT customer_id, orderDate, amount
      FROM orders
      WHERE MONTH(orderDate) = MONTH(CURDATE())
      AND YEAR(orderDate) = YEAR(CURDATE())) AS t2
ON t1.customer_id = t2.customer_id;
```
**Explanation:** This extracts the day, month, and year from `orderDate`.

### 12. Extract Day Name and Month Name of Orders
```sql
SELECT t1.name, t1.email, DAY(t2.orderDate) AS orderedDay, DAYNAME(t2.orderDate) AS orderedDayName,
       MONTH(t2.orderDate) AS orderedMonth, MONTHNAME(t2.orderDate) AS orderedMonthName,
       YEAR(t2.orderDate) AS orderedYear, t2.amount
FROM customers t1
JOIN (SELECT customer_id, orderDate, amount
      FROM orders
      WHERE MONTH(orderDate) = MONTH(CURDATE())
      AND YEAR(orderDate) = YEAR(CURDATE())) AS t2
ON t1.customer_id = t2.customer_id;
```
**Explanation:** This query extracts and displays the day name (e.g., Monday) and month name (e.g., March) for each order.

### 13. Retrieve Orders Placed in the Last 30 Days Using `DATE_SUB`
```sql
SELECT t1.name, t1.email, t2.orderDate, t2.amount
FROM customers t1
JOIN (SELECT customer_id, orderDate, amount
      FROM orders
      WHERE orderDate >= DATE_SUB(CURRENT_TIMESTAMP, INTERVAL 30 DAY)) t2
ON t1.customer_id = t2.customer_id;
```
**Explanation:** This retrieves orders placed within the last 30 days using `DATE_SUB`.

### 14. Format Order Dates (DD/MM/YYYY)
```sql
SELECT t1.name, t1.email, DATE_FORMAT(t2.orderDate, '%d/%m/%Y') AS changed_dateFormat,
       t2.amount
FROM customers t1
JOIN orders t2
ON t1.customer_id = t2.customer_id;
```
**Explanation:** This formats the `orderDate` column into `DD/MM/YYYY` format.

### 15. Retrieve Orders Placed in the Last Hour
```sql
SELECT t1.name, t1.email, DATE_FORMAT(t2.orderDate, '%d/%m/%Y') AS changed_dateFormat,
       t2.amount
FROM customers t1
JOIN (SELECT customer_id, orderDate, amount
      FROM orders
      WHERE HOUR(TIMEDIFF(CURRENT_TIMESTAMP, orderDate)) < 1) t2
ON t1.customer_id = t2.customer_id;
```
**Explanation:** This query retrieves orders placed in the last hour.

### 16. Format Date and Time (DD/MM/YYYY HH:MM:SS)
```sql
SELECT t1.name, t1.email, DATE_FORMAT(t2.orderDate, '%d/%m/%Y %h:%i:%s') AS changed_dateFormat,
       t2.amount
FROM customers t1
JOIN (SELECT customer_id, orderDate, amount
      FROM orders
      WHERE HOUR(TIMEDIFF(CURRENT_TIMESTAMP, orderDate)) < 1) t2
ON t1.customer_id = t2.customer_id;
```
**Explanation:** This formats `orderDate` to display both date and time.

---

## Conclusion
These queries demonstrate how to manipulate and query date and time values in SQL. They cover various use cases such as filtering records based on time intervals, formatting date outputs, and extracting specific components from date fields.

