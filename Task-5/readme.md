


# Subqueries and Nested Queries

## Objective
- Understand and use subqueries to filter or compute values within a main query.
- Learn the difference between correlated and non-correlated subqueries.

## Table Schema

```sql
DESCRIBE employees;
```

| Field       | Type          | Null | Key | Default | Extra          |
|------------|--------------|------|-----|---------|----------------|
| EMPLOYEE_ID | int           | NO   | PRI | NULL    | auto_increment |
| NAME        | varchar(100)  | NO   |     | NULL    |                |
| SALARY      | decimal(10,2) | YES  |     | NULL    |                |
| STATUS      | varchar(20)   | YES  |     | Active  |                |
| DEPTID      | int           | YES  | MUL | NULL    |                |
| email       | varchar(255)  | NO   | UNI | NULL    |                |

## Example Queries

### 1. Subquery in `WHERE` Clause

Find employees whose salary is below the average salary of all employees:

```sql
SELECT * FROM employees 
WHERE salary < (SELECT AVG(salary) FROM employees);
```

### 2. Finding Departments with Average Salary > 40,000

```sql
SELECT DISTINCT DEPTID
FROM employees
WHERE DEPTID IN (
    SELECT DEPTID FROM employees 
    GROUP BY DEPTID
    HAVING AVG(SALARY) > 40000
);
```

**Explanation:** This is an example of an uncorrelated subquery since the subquery executes only once, making it faster.

### 3. Correlated Subquery Example

```sql
SELECT name, salary,
       (SELECT AVG(salary) FROM employees WHERE deptid = e.deptid) AS deptavgsalary
FROM employees e;
```

**Explanation:** Here, the subquery runs once per row in the outer query, making it slower for large tables.

### 4. Finding Employees with the Highest Salary in Each Department

```sql
SELECT * FROM employees 
WHERE (deptid, salary) IN (
    SELECT deptid, MAX(salary) 
    FROM employees 
    GROUP BY deptid
);
```

**Explanation:** This is an example of an uncorrelated subquery.

### 5. Join Alternative to Correlated Subqueries

```sql
SELECT name, salary, d.deptavgsalary
FROM employees e1
JOIN (SELECT deptid, AVG(salary) AS deptavgsalary FROM employees GROUP BY deptid) d
ON e1.deptid = d.deptid;
```

**Explanation:** Instead of a correlated subquery, we use a join for better performance.

### 6. Finding Second Highest Salary

```sql
SELECT MAX(salary) 
FROM employees 
WHERE salary < (SELECT MAX(salary) FROM employees);
```

**Explanation:** The subquery finds the maximum salary, and the outer query retrieves the second-highest salary.

## Performance Considerations
- **Uncorrelated subqueries** are generally faster as they execute only once.
- **Correlated subqueries** run once per row in the outer query, making them slow for large datasets.
- Using **JOINs** instead of subqueries can improve performance in many cases.

## Conclusion
Subqueries are powerful but should be used wisely. When performance is critical, consider alternatives like JOINs.

---

Hope this guide helps you understand and implement subqueries efficiently! 🚀

