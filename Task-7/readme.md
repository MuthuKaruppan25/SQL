# Window Functions and Ranking in SQL

## Objective
The purpose of this task is to leverage window functions in SQL to perform calculations across a set of rows related to the current row, rather than summarizing results at a group level. Window functions allow for ranking, cumulative sums, and accessing adjacent row values efficiently.

## Requirements
- Use window functions like `ROW_NUMBER()`, `RANK()`, and `DENSE_RANK()` to assign ranks within partitions.
- Utilize `PARTITION BY` to group data while ranking employees by salary within each department.
- Experiment with other window functions like `LEAD()` and `LAG()` to access adjacent row values.

## Window Functions Explanation
### 1. **ROW_NUMBER()**
The `ROW_NUMBER()` function assigns a unique sequential integer to rows within a partition, ordered by a specified column.
```sql
SELECT EMPLOYEE_ID, NAME, DEPTID, SALARY,
       ROW_NUMBER() OVER (ORDER BY SALARY) AS rn
FROM employees;
```
#### **Result:**
Assigns a unique row number across all employees based on salary.

To assign row numbers within each department:
```sql
SELECT EMPLOYEE_ID, NAME, DEPTID, SALARY,
       ROW_NUMBER() OVER (PARTITION BY DEPTID ORDER BY SALARY) AS rn
FROM employees;
```
#### **Result:**
Assigns row numbers within each department, restarting from 1 in every department.

### 2. **RANK()**
The `RANK()` function assigns the same rank to rows with identical values but leaves gaps in ranking for duplicates.
```sql
SELECT EMPLOYEE_ID, NAME, DEPTID, SALARY,
       RANK() OVER (PARTITION BY DEPTID ORDER BY SALARY DESC) AS rank_num
FROM employees;
```
#### **Result:**
Assigns ranks within each department based on descending salary, leaving gaps for duplicate values.

### 3. **DENSE_RANK()**
Similar to `RANK()`, but it does not leave gaps in numbering after duplicates.
```sql
SELECT EMPLOYEE_ID, NAME, DEPTID, SALARY,
       DENSE_RANK() OVER (PARTITION BY DEPTID ORDER BY SALARY DESC) AS rank_num
FROM employees;
```
#### **Result:**
Ranks employees within each department without gaps between rank numbers.

### 4. **Cumulative Sum Using SUM()**
The `SUM()` function with `OVER` computes a cumulative sum of salaries.
```sql
SELECT EMPLOYEE_ID, NAME, DEPTID, SALARY,
       SUM(SALARY) OVER (ORDER BY EMPLOYEE_ID) AS cumulative_sal
FROM employees;
```
#### **Result:**
Returns a running total of salaries ordered by employee ID.

### 5. **LEAD() and LAG()**
- `LEAD()` accesses the next row’s value.
- `LAG()` accesses the previous row’s value.

```sql
SELECT EMPLOYEE_ID, NAME, DEPTID, SALARY,
       LAG(SALARY, 1, 0) OVER (PARTITION BY DEPTID ORDER BY SALARY) AS prev_salary,
       LEAD(SALARY, 1, 0) OVER (PARTITION BY DEPTID ORDER BY SALARY) AS next_salary
FROM employees;
```
#### **Result:**
Shows previous and next salaries within each department.

## Summary
Window functions provide advanced analytics without affecting overall aggregation. This guide covered:
- Assigning row numbers with `ROW_NUMBER()`.
- Ranking using `RANK()` and `DENSE_RANK()`.
- Computing cumulative sums with `SUM()`.
- Accessing previous and next values using `LAG()` and `LEAD()`.

Using these techniques improves SQL querying capabilities for analytical computations.

