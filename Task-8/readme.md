# Common Table Expressions (CTEs) and Recursive Queries

## Objective
This guide demonstrates the use of Common Table Expressions (CTEs) in MySQL to simplify complex queries and process hierarchical data.

## Requirements
- Write a non-recursive CTE to structure a multi-step query for readability (e.g., breaking down a complex aggregation).
- Create a recursive CTE to display hierarchical data (e.g., an organizational chart or a category tree).
- Ensure proper termination of the recursive CTE to avoid infinite loops.

## Understanding CTEs
CTEs allow the creation of temporary result sets within a single query, making complex queries more readable and maintainable. Recursive CTEs are useful for handling hierarchical data structures.

---

## Non-Recursive CTE Example

### Table Structure
```sql
describe employees;
```
| Field       | Type          | Null | Key | Default | Extra          |
|------------|--------------|------|-----|---------|----------------|
| EMPLOYEE_ID | int         | NO   | PRI | NULL    | auto_increment |
| NAME        | varchar(100)| NO   |     | NULL    |                |
| SALARY      | decimal(10,2) | YES |     | NULL    |                |
| STATUS      | varchar(20) | YES  |     | Active  |                |
| DEPTID      | int         | YES  | MUL | NULL    |                |
| EMAIL       | varchar(255)| NO   | UNI | NULL    |                |

### Query
```sql
WITH emp_with_good_sal AS (
    SELECT deptid, AVG(salary) AS avg_sal
    FROM employees
    GROUP BY deptid
)
SELECT e.name, e.salary, h.avg_sal
FROM employees e
JOIN emp_with_good_sal h
ON e.deptid = h.deptid
WHERE e.salary > h.avg_sal;
```
### Output
| Name   | Salary   | Avg_Sal      |
|--------|---------|-------------|
| Gotham | 40000.00 | 37500.00    |
| Mano   | 40000.00 | 37500.00    |
| Priya  | 42000.00 | 37500.00    |
| Suresh | 41000.00 | 37500.00    |
| Neha   | 52000.00 | 51666.67    |
| Anjali | 53000.00 | 51666.67    |
| Divya  | 62000.00 | 60000.00    |
| Ajay   | 47000.00 | 46000.00    |
| Sanjay | 53000.00 | 52000.00    |
| Pooja  | 50000.00 | 49000.00    |

---

## Recursive CTE Example

### Table Creation
```sql
CREATE TABLE employeedetails(
    emp_id INT PRIMARY KEY,
    name VARCHAR(200),
    email VARCHAR(200),
    manager_id INT,
    FOREIGN KEY (manager_id) REFERENCES employeedetails(emp_id)
);
```

### Insert Data
```sql
INSERT INTO employeedetails (emp_id, name, email, manager_id) VALUES
(1, 'Rajesh Kumar', 'rajesh.kumar@example.com', NULL),
(2, 'Priya Sharma', 'priya.sharma@example.com', 1),
(3, 'Vikram Singh', 'vikram.singh@example.com', 1),
(4, 'Aditi Verma', 'aditi.verma@example.com', 2),
(5, 'Karthik Rao', 'karthik.rao@example.com', 2),
(6, 'Sneha Das', 'sneha.das@example.com', 3),
(7, 'Manoj Iyer', 'manoj.iyer@example.com', 3),
(8, 'Deepa Nair', 'deepa.nair@example.com', 4),
(9, 'Ravi Patel', 'ravi.patel@example.com', 5),
(10, 'Ananya Sen', 'ananya.sen@example.com', 6);
```

### Query
```sql
WITH RECURSIVE emp_hierarchy AS (
    SELECT emp_id, name, manager_id, 1 AS level
    FROM employeedetails
    WHERE manager_id IS NULL
    UNION ALL
    SELECT e.emp_id, e.name, e.manager_id, h.level + 1
    FROM employeedetails e
    JOIN emp_hierarchy h
    ON e.manager_id = h.emp_id
)
SELECT * FROM emp_hierarchy;
```

### Output
| Emp_ID | Name         | Manager_ID | Level |
|--------|------------|------------|-------|
| 1      | Rajesh Kumar | NULL      | 1     |
| 2      | Priya Sharma | 1        | 2     |
| 3      | Vikram Singh | 1        | 2     |
| 4      | Aditi Verma  | 2        | 3     |
| 5      | Karthik Rao  | 2        | 3     |
| 6      | Sneha Das    | 3        | 3     |
| 7      | Manoj Iyer   | 3        | 3     |
| 8      | Deepa Nair   | 4        | 4     |
| 9      | Ravi Patel   | 5        | 4     |
| 10     | Ananya Sen   | 6        | 4     |

---

## Limiting the Recursive Depth
To prevent infinite loops, we can limit the recursion depth by adding a condition:

```sql
WITH RECURSIVE emp_hierarchy AS (
    SELECT emp_id, name, manager_id, 1 AS level
    FROM employeedetails
    WHERE manager_id IS NULL
    UNION ALL
    SELECT e.emp_id, e.name, e.manager_id, h.level + 1
    FROM employeedetails e
    JOIN emp_hierarchy h
    ON e.manager_id = h.emp_id
    WHERE h.level < 3
)
SELECT * FROM emp_hierarchy;
```

### Output (Limited to Level 3)
| Emp_ID | Name         | Manager_ID | Level |
|--------|------------|------------|-------|
| 1      | Rajesh Kumar | NULL      | 1     |
| 2      | Priya Sharma | 1        | 2     |
| 3      | Vikram Singh | 1        | 2     |
| 4      | Aditi Verma  | 2        | 3     |
| 5      | Karthik Rao  | 2        | 3     |
| 6      | Sneha Das    | 3        | 3     |
| 7      | Manoj Iyer   | 3        | 3     |

---

## Conclusion
- **Non-recursive CTEs** help simplify complex queries by structuring multi-step operations.
- **Recursive CTEs** are useful for hierarchical data representation, like organizational charts.
- **Limiting recursion depth** ensures performance efficiency and avoids infinite loops.

