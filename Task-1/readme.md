# Database Task: Creating and Populating Tables

## Objective
The goal of this task is to set up a simple database, create tables, insert sample data, and retrieve that data using basic SQL queries.

## Requirements
- Create a database named `Task`.
- Define tables (`EMPLOYEES` and `DEPT`) with appropriate data types and constraints.
- Insert sample data into the tables.
- Perform retrieval operations using `SELECT` queries.
- Implement foreign key constraints and handle updates/deletes properly.

## Steps

### 1. Create the Database
```sql
CREATE DATABASE Task;
```

### 2. Use the Database
```sql
USE Task;
```

### 3. Create the `EMPLOYEES` Table
```sql
CREATE TABLE EMPLOYEES (
    EMPLOYEE_ID INT AUTO_INCREMENT PRIMARY KEY,
    NAME VARCHAR(100) NOT NULL,
    SALARY DECIMAL(10,2) CHECK (SALARY >= 10000),
    STATUS VARCHAR(20) DEFAULT 'Active',
    DEPTID INT,
    EMAIL VARCHAR(100) UNIQUE
);
```

### 4. Modify the `EMPLOYEES` Table (Update Email Constraint)
```sql
ALTER TABLE EMPLOYEES MODIFY COLUMN EMAIL VARCHAR(255) NOT NULL;
```

### 5. Create the `DEPT` Table
```sql
CREATE TABLE DEPT (
    DEPTID INT PRIMARY KEY,
    DEPT_NAME VARCHAR(100) NOT NULL
);
```

### 6. Add Foreign Key Constraint to `EMPLOYEES`
```sql
ALTER TABLE EMPLOYEES
ADD CONSTRAINT fk_dept
FOREIGN KEY (DEPTID)
REFERENCES DEPT(DEPTID)
ON DELETE SET NULL ON UPDATE CASCADE;
```

### 7. Insert Data into `DEPT`
```sql
INSERT INTO DEPT (DEPTID, DEPT_NAME)
VALUES
(1, 'Marketing'),
(2, 'Sales');
```

### 8. Verify Data in `DEPT`
```sql
SELECT * FROM DEPT;
```

### 9. Insert Data into `EMPLOYEES`
```sql
INSERT INTO EMPLOYEES (NAME, SALARY, STATUS, DEPTID, EMAIL)
VALUES
('Muthu', 30000, 'Active', 1, 'muthu@example.com'),
('Gotham', 40000, 'Inactive', 2, 'gotham@example.com');
```

### 10. Retrieve All Employee Data
```sql
SELECT * FROM EMPLOYEES;
```

### 11. Insert Another Employee Record
```sql
INSERT INTO EMPLOYEES (NAME, SALARY, STATUS, DEPTID, EMAIL)
VALUES ('Mano', 40000, 'Active', 1, 'mano@gmail.com');
```

### 12. Retrieve Active Employees
```sql
SELECT * FROM EMPLOYEES WHERE STATUS = 'Active';
```

### 13. Retrieve Active Employees Sorted by Salary (Descending)
```sql
SELECT * FROM EMPLOYEES WHERE STATUS = 'Active' ORDER BY SALARY DESC;
```

### 14. Delete a Department
```sql
DELETE FROM DEPT WHERE DEPTID = 1;
```

### 15. Check Employee Table After Deletion
```sql
SELECT * FROM EMPLOYEES;
```

### 16. Update Employees' Department to 2 Where NULL
```sql
UPDATE EMPLOYEES SET DEPTID = 2 WHERE DEPTID IS NULL;
```

### 17. Verify Final Employee Data
```sql
SELECT * FROM EMPLOYEES;
```

## Summary
This task demonstrated fundamental SQL operations including:
- Creating databases and tables
- Defining constraints (PRIMARY KEY, UNIQUE, CHECK, FOREIGN KEY)
- Inserting and retrieving data
- Modifying table structure
- Handling updates and deletes with foreign key constraints

This guide serves as a foundational exercise for working with relational databases in MySQL.

