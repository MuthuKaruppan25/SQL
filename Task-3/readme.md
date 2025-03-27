# Simple Aggregation and Grouping in SQL

## Objective
The goal of this task is to summarize data using aggregate functions and grouping. This includes calculating totals, averages, and other summary statistics while grouping data by specific columns.

## Requirements
- Use aggregate functions such as `COUNT()`, `SUM()`, `AVG()` to derive insights from the data.
- Use the `GROUP BY` clause to categorize data based on a specific column (e.g., counting employees per department).
- Optionally, filter grouped results using the `HAVING` clause.

---

## Queries Used and Their Results

### 1. Display all employees
```sql
SELECT * FROM employees;
```
#### Result:
| EMPLOYEE_ID | NAME    | SALARY   | STATUS   | DEPTID | EMAIL               |
|------------|---------|----------|----------|--------|---------------------|
| 1          | Muthu   | 30000.00 | Active   | 2      | muthu@example.com   |
| 2          | Gotham  | 40000.00 | Inactive | 2      | gotham@example.com  |
| 3          | Mano    | 40000.00 | Active   | 2      | mano@gmail.com      |
| 4          | Priya   | 42000.00 | Inactive | 2      | priya@example.com   |
| 5          | Ravi    | 50000.00 | Active   | 3      | ravi@example.com    |
| 6          | Sneha   | 55000.00 | Active   | 4      | sneha@example.com   |
| 7          | Vikram  | 60000.00 | Inactive | 5      | vikram@example.com  |
| 8          | Deepa   | 45000.00 | Active   | 6      | deepa@example.com   |
| 9          | Karthik | 52000.00 | Inactive | 7      | karthik@example.com |
| 10         | Meera   | 48000.00 | Active   | 8      | meera@example.com   |

---

### 2. Count the total number of employees
```sql
SELECT COUNT(*) AS Total_employees FROM employees;
```
#### Result:
| Total_employees |
|---------------|
| 10            |

---

### 3. Calculate the average salary of employees
```sql
SELECT AVG(salary) AS AVG_SALARY FROM employees;
```
#### Result:
| AVG_SALARY   |
|-------------|
| 46200.000000 |

---

### 4. Calculate the total salary of all employees
```sql
SELECT SUM(salary) AS total_salary FROM employees;
```
#### Result:
| total_salary |
|-------------|
| 462000.00   |

---

### 5. Find the minimum salary
```sql
SELECT MIN(salary) AS Minimum_salary FROM employees;
```
#### Result:
| Minimum_salary |
|--------------|
| 30000.00     |

---

### 6. Find the maximum salary
```sql
SELECT MAX(salary) AS Maximum_salary FROM employees;
```
#### Result:
| Maximum_salary |
|--------------|
| 60000.00     |

---

### 7. Count employees per department
```sql
SELECT COUNT(*) AS employee_count, deptid FROM employees GROUP BY deptid;
```
#### Result:
| employee_count | deptid |
|--------------|------|
| 4            | 2    |
| 1            | 3    |
| 1            | 4    |
| 1            | 5    |
| 1            | 6    |
| 1            | 7    |
| 1            | 8    |

---

### 8. Find the maximum salary per department where the max salary is greater than 40,000
```sql
SELECT MAX(salary) FROM employees GROUP BY deptid HAVING MAX(salary) > 40000;
```
#### Result:
| max(salary) |
|-------------|
| 42000.00    |
| 50000.00    |
| 55000.00    |
| 60000.00    |
| 45000.00    |
| 52000.00    |
| 48000.00    |

---

### 9. Find the average salary per department where the avg salary is greater than 40,000
```sql
SELECT AVG(salary), deptid FROM employees GROUP BY deptid HAVING AVG(salary) > 40000;
```
#### Result:
| avg(salary)  | deptid |
|-------------|------|
| 50000.000000 | 3    |
| 55000.000000 | 4    |
| 60000.000000 | 5    |
| 45000.000000 | 6    |
| 52000.000000 | 7    |
| 48000.000000 | 8    |

---

## Summary
This task covered basic aggregation and grouping operations in SQL, demonstrating how to:
- Count, sum, and average values in a table.
- Find minimum and maximum values.
- Group data by department and apply filtering criteria using `HAVING`.

These queries help analyze employee data effectively and gain meaningful insights from the dataset.

