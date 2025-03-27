# Basic Filtering and Sorting in MySQL

## Objective

The objective of this task is to write queries that filter records and sort the result set using MySQL.

## Requirements

- Use the `WHERE` clause to filter records based on a condition (e.g., `WHERE Department = 'Sales'`).
- Apply the `ORDER BY` clause to sort the results (e.g., by `LastName` or `Salary`).
- Experiment with multiple conditions using `AND`/`OR`.

## Setup Instructions

1. Ensure you have MySQL installed and running.
2. Create the necessary tables and insert the sample data using the queries below.

## Queries Used

### Creating the `DEPT` Table and Inserting Data
```sql
INSERT INTO DEPT (DEPTID, DEPT_NAME)
VALUES
    (3, 'Human Resources'),
    (4, 'Finance'),
    (5, 'IT'),
    (6, 'Operations'),
    (7, 'Customer Support'),
    (8, 'Research & Development');
```

### Creating the `employees` Table and Inserting Data
```sql
INSERT INTO employees (NAME, SALARY, STATUS, DEPTID, email)
VALUES  ('Priya', 42000, 'Inactive', 2, 'priya@example.com'),
        ('Ravi', 50000, 'Active', 3, 'ravi@example.com'),
        ('Sneha', 55000, 'Active', 4, 'sneha@example.com'),
        ('Vikram', 60000, 'Inactive', 5, 'vikram@example.com'),
        ('Deepa', 45000, 'Active', 6, 'deepa@example.com'),
        ('Karthik', 52000, 'Inactive', 7, 'karthik@example.com'),
        ('Meera', 48000, 'Active', 8, 'meera@example.com');
```

### Selecting All Employees
```sql
SELECT * FROM employees;
```

### Filtering Employees by Department ID
```sql
SELECT * FROM employees WHERE deptid = 2;
```

### Filtering and Sorting Employees by Salary (Descending)
```sql
SELECT * FROM employees WHERE deptid = 2 ORDER BY salary DESC;
```

### Sorting Employees by Department and Salary (Descending)
```sql
SELECT * FROM employees ORDER BY deptid, salary DESC;
```

### Filtering Employees with `AND` Condition
```sql
SELECT * FROM employees WHERE deptid = 2 AND salary >= 40000;
```

### Filtering Employees with `OR` Condition
```sql
SELECT * FROM employees WHERE deptid = 2 OR salary >= 40000;
```

### Filtering Employees Using `IN` Operator
```sql
SELECT * FROM employees WHERE deptid IN (2, 4, 5);
```

### Filtering Employees by Name Pattern (Starting with 'M')
```sql
SELECT * FROM employees WHERE name LIKE 'M%';
```

### Filtering Employees by Salary Range
```sql
SELECT * FROM employees WHERE salary BETWEEN 30000 AND 40000;
```

### Filtering Employees by Name Pattern (Containing Specific Characters)
```sql
SELECT * FROM employees WHERE name LIKE '_u%u';
```

### Selecting Top 2 Employees with Highest Salary
```sql
SELECT * FROM employees ORDER BY salary DESC LIMIT 2;
```

## Notes
- The above queries demonstrate different filtering and sorting techniques in MySQL.
- Modify the `WHERE` conditions as needed to test different filtering scenarios.
- Ensure the `employees` table exists before executing queries.

### Happy Querying! 🎯

