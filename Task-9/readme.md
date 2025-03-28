# Stored Procedures and User-Defined Functions in MySQL

## Objective
This guide demonstrates how to encapsulate business logic using stored procedures and user-defined functions in MySQL.

## Requirements
- Create a stored procedure that accepts parameters (e.g., a date range) and returns a result set (such as total sales within that range).
- Write a scalar or table-valued user-defined function that performs a calculation (e.g., calculates a discount or bonus based on input parameters).
- Test the procedure and function by calling them and verifying their outputs.

## Overview
### Stored Procedures
A stored procedure is a set of precompiled SQL statements that perform a task in the database. It can be executed multiple times with different values by calling it.

### User-Defined Functions (UDFs)
A user-defined function (UDF) is a reusable function in SQL that returns a value or a table based on input parameters.

---

## Database and Table Creation
```sql
CREATE DATABASE salesDB;
USE salesDB;

CREATE TABLE sales (
    id INT AUTO_INCREMENT PRIMARY KEY,
    product_name VARCHAR(100),
    sale_date DATE,
    amount DECIMAL(10,2)
);
```

### Sample Data Insertion
```sql
INSERT INTO sales (product_name, sale_date, amount) VALUES
('Laptop', '2024-03-10', 1200.00),
('Phone', '2024-03-12', 800.00),
('Tablet', '2024-03-15', 500.00),
('Laptop', '2024-03-20', 1300.00),
('Phone', '2024-03-22', 750.00);
```

### Viewing Data
```sql
SELECT * FROM sales;
```

---

## Stored Procedure: Calculate Total Sales Between Dates
```sql
DELIMITER //
CREATE PROCEDURE totalsales(IN start_date DATE, IN end_date DATE)
BEGIN
    SELECT SUM(amount) AS total_sales FROM sales WHERE sale_date BETWEEN start_date AND end_date;
END //
DELIMITER ;
```

### Calling the Stored Procedure
```sql
CALL totalsales('2024-03-10', '2024-03-20');
```

**Output:**
| total_sales |
|------------|
| 3800.00    |

---

## User-Defined Function: Calculate Discount
```sql
DELIMITER //
CREATE FUNCTION calculateDiscount(amount DECIMAL(10,2)) RETURNS DECIMAL(10,2) DETERMINISTIC
BEGIN
    DECLARE discount DECIMAL(10,2);
    IF amount > 1000 THEN
        SET discount = amount * 0.10;
    ELSE
        SET discount = 0;
    END IF;
    RETURN discount;
END //
DELIMITER ;
```

### Using the Function
```sql
SELECT calculateDiscount(1200) AS discount;
```

**Output:**
| discount |
|----------|
| 120.00   |

---

## User-Defined Function: Get Total Sales by Product
```sql
DELIMITER //
CREATE FUNCTION getTotalSalesByProduct(product_name VARCHAR(100)) RETURNS DECIMAL(10,2)
READS SQL DATA
BEGIN
    DECLARE total_sales DECIMAL(10,2);
    SELECT SUM(amount) INTO total_sales FROM sales WHERE sales.product_name = product_name;
    RETURN IFNULL(total_sales, 0);
END //
DELIMITER ;
```

### Using the Function
```sql
SELECT getTotalSalesByProduct('Laptop');
```

**Output:**
| getTotalSalesByProduct('Laptop') |
|----------------------------------|
| 2500.00                          |

---

## User-Defined Function: Total Sales from a Start Date to Present
```sql
DELIMITER //
CREATE FUNCTION TotalSales(start_date DATE)
RETURNS DECIMAL(10,2)
READS SQL DATA
BEGIN
    DECLARE total DECIMAL(10,2);
    DECLARE end_date DATE;
    SET end_date = CURRENT_DATE();
    SELECT SUM(amount) INTO total FROM sales WHERE sale_date BETWEEN start_date AND end_date;
    RETURN IFNULL(total, 0);
END //
DELIMITER ;
```

### Using the Function
```sql
SELECT TotalSales('2024-01-01') AS total_sales;
```

**Output:**
| total_sales |
|------------|
| 4550.00    |

---

## Stored Procedure: Get Sales by Date Range
```sql
DELIMITER //
CREATE PROCEDURE GetSalesByDateRange(IN start_date DATE, IN end_date DATE)
BEGIN
    SELECT id, product_name, sale_date, amount FROM sales WHERE sale_date BETWEEN start_date AND end_date;
END //
DELIMITER ;
```

### Calling the Stored Procedure
```sql
CALL GetSalesByDateRange('2024-03-10', '2024-03-22');
```

**Output:**
| id | product_name | sale_date  | amount  |
|----|--------------|------------|---------|
|  1 | Laptop       | 2024-03-10 | 1200.00 |
|  2 | Phone        | 2024-03-12 |  800.00 |
|  3 | Tablet       | 2024-03-15 |  500.00 |
|  4 | Laptop       | 2024-03-20 | 1300.00 |
|  5 | Phone        | 2024-03-22 |  750.00 |

---

## Conclusion
- **Stored Procedures** help encapsulate logic for complex queries and allow reusability.
- **User-Defined Functions** provide efficient calculations and data retrieval.
- **Using these techniques** improves performance, reduces redundant SQL code, and enhances maintainability.

This guide demonstrates practical implementations of stored procedures and UDFs in MySQL. Modify the logic as per your business requirements!

