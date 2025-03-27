# Multi-Table JOINs

## Objective

- Combine data from two related tables using JOIN operations.

## Requirements

- Create two related tables (e.g., Customers and Orders) with a foreign key relationship.
- Write an INNER JOIN query to retrieve combined information (e.g., customer names along with their order details).
- Experiment with other types of joins such as LEFT JOIN to understand how missing matches are handled.

## Tables Creation

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY, 
    name VARCHAR(100), 
    email VARCHAR(100) NOT NULL UNIQUE
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY, 
    customer_id INT, 
    orderDate DATE, 
    amount DECIMAL(10,2), 
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```

## Sample Data Insertion

```sql
INSERT INTO customers (customer_id, name, email) VALUES 
(1, 'Muthu', 'muthu@gmail.com'),
(2, 'Mano', 'mano@gmail.com'),
(3, 'Kalai', 'kalai@gmail.com'),
(4, 'Gowtham', 'gowtham@gmail.com');

INSERT INTO orders (order_id, customer_id, orderDate, amount) VALUES 
(1, 1, '2025-12-01', 10000),
(2, 1, '2025-12-02', 20000),
(3, 2, '2024-09-03', 10000),
(4, 3, '2023-09-25', 20000),
(5, 4, '2024-09-20', 30000);
```

## Queries and Results

### INNER JOIN

```sql
SELECT t1.name, t2.order_id, t2.orderDate, t2.amount 
FROM customers t1 
INNER JOIN orders t2 
ON t1.customer_id = t2.customer_id;
```

**Result:** Retrieves only the customers with orders.

### LEFT JOIN

```sql
SELECT t1.name, t2.order_id, t2.orderDate, t2.amount 
FROM customers t1 
LEFT JOIN orders t2 
ON t1.customer_id = t2.customer_id;
```

**Result:** Retrieves all customers, showing NULL for those without orders.

### RIGHT JOIN

```sql
SELECT t1.name, t2.order_id, t2.orderDate, t2.amount 
FROM customers t1 
RIGHT JOIN orders t2 
ON t1.customer_id = t2.customer_id;
```

**Result:** Retrieves all orders, showing NULL for those without a corresponding customer.

### CROSS JOIN

```sql
SELECT t1.name, t2.order_id, t2.orderDate, t2.amount 
FROM customers t1 
CROSS JOIN orders t2;
```

**Result:** Produces a Cartesian product of both tables.

### NATURAL JOIN

```sql
SELECT t1.name, t2.order_id, t2.orderDate, t2.amount 
FROM customers t1 
NATURAL JOIN orders t2;
```

**Result:** Automatically joins tables based on common column names.

### SELF JOIN

```sql
SELECT t1.name, t2.email 
FROM customers t1, customers t2 
WHERE t1.customer_id <> t2.customer_id;
```

**Result:** Joins the customers table to itself, listing all combinations except self-pairing.

## Conclusion

This project demonstrates various SQL JOINs and their effects on retrieving related data. Understanding these concepts is crucial for efficient database querying and management.
