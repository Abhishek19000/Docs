# SQL & Stored Procedures – Interview Book

> **Who is this for?** A developer with ~2 years of experience, interviewing in **3 days**.
> **Style:** simple words, every concept with an **easy** example and a **complex / real-world** example.
> **Dialect:** T-SQL (SQL Server / Azure SQL). 90% applies to any SQL database.

## How to use this book

1. Run **Chapter 00** script once – it creates a sample shop database used everywhere.
2. Read a chapter, then **type** every example yourself (don't copy-paste – typing builds memory).
3. At the end of each chapter, do the **"Try it yourself"** exercises before peeking at answers.
4. Chapters 12–14 are pure interview practice.

Where to practise (free):
- SQL Server Developer Edition + SSMS / Azure Data Studio
- Online: <https://dbfiddle.uk> (choose SQL Server)

## What interviewers expect at 2 YOE

| Area | Expectation | Weight |
|------|-------------|--------|
| Writing queries (joins, group by, window functions) | Fluent, without help | ★★★★★ |
| Stored procedures, error handling, transactions | Write one from scratch | ★★★★★ |
| Indexes & "why is my query slow?" | Explain + basic tuning steps | ★★★★ |
| Theory differences (DELETE vs TRUNCATE etc.) | Crisp 1–2 line answers | ★★★★ |
| Normalization / design | Explain 1NF–3NF with example | ★★★ |
| Real project stories | 2 stories ready | ★★★★ |

## 3-Day Plan

### Day 1 – Query fluency (5–6 hrs)
- Chapters **00 → 06**
- Evening: solve Chapter 12 queries **1–15** without looking

### Day 2 – Stored procedures & transactions (5–6 hrs)
- Chapters **07, 08, 09** (Chapter 07 twice!)
- Write `TransferFunds` and `PlaceOrder` procedures from memory
- Evening: Chapter 12 queries **16–30**

### Day 3 – Performance, design, mock (5 hrs)
- Chapters **10, 11**
- Chapter **13** read aloud (speak the answers)
- Chapter **14** timed mock (45 min) + prepare your 2 project stories
- Night before: only the **Cheat Sheet**. Sleep well.

## Golden rule

> SQL runs in this order: **FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → TOP/OFFSET**
>
> Remember this and half of the "why doesn't this work?" questions answer themselves.


---

# 00. Setup – Sample Database

Run this whole script once. Every chapter uses these tables.

```sql
CREATE DATABASE ShopDB;
GO
USE ShopDB;
GO

CREATE TABLE Departments (
    DeptId   INT PRIMARY KEY,
    DeptName NVARCHAR(50) NOT NULL
);

CREATE TABLE Employees (
    EmpId     INT PRIMARY KEY,
    Name      NVARCHAR(100) NOT NULL,
    Email     NVARCHAR(100) NULL,
    DeptId    INT NULL REFERENCES Departments(DeptId),
    ManagerId INT NULL REFERENCES Employees(EmpId),
    Salary    DECIMAL(10,2) NOT NULL,
    HireDate  DATE NOT NULL
);

CREATE TABLE Customers (
    CustomerId INT IDENTITY(1,1) PRIMARY KEY,
    Name       NVARCHAR(100) NOT NULL,
    City       NVARCHAR(50) NULL,
    Email      NVARCHAR(100) NULL,
    Credit     DECIMAL(10,2) NOT NULL DEFAULT 5000
);

CREATE TABLE Products (
    ProductId INT PRIMARY KEY,
    Name      NVARCHAR(100) NOT NULL,
    Category  NVARCHAR(50) NOT NULL,
    Price     DECIMAL(10,2) NOT NULL CHECK (Price > 0)
);

CREATE TABLE Orders (
    OrderId    INT IDENTITY(1,1) PRIMARY KEY,
    CustomerId INT NOT NULL REFERENCES Customers(CustomerId),
    OrderDate  DATE NOT NULL,
    Amount     DECIMAL(10,2) NOT NULL CHECK (Amount > 0)
);

CREATE TABLE OrderItems (
    OrderItemId INT IDENTITY(1,1) PRIMARY KEY,
    OrderId     INT NOT NULL REFERENCES Orders(OrderId),
    ProductId   INT NOT NULL REFERENCES Products(ProductId),
    Qty         INT NOT NULL CHECK (Qty > 0),
    UnitPrice   DECIMAL(10,2) NOT NULL
);

CREATE TABLE Accounts (
    AccId   INT PRIMARY KEY,
    Holder  NVARCHAR(100) NOT NULL,
    Balance DECIMAL(12,2) NOT NULL CHECK (Balance >= 0)
);

CREATE TABLE AuditLog (
    LogId     INT IDENTITY PRIMARY KEY,
    TableName SYSNAME,
    Action    VARCHAR(10),
    KeyValue  INT,
    LoggedOn  DATETIME2 DEFAULT SYSUTCDATETIME()
);
GO

INSERT INTO Departments VALUES (1,'IT'),(2,'HR'),(3,'Sales'),(4,'Finance');

INSERT INTO Employees VALUES
 (1,'Anil','anil@shop.com',1,NULL,150000,'2018-01-10'),
 (2,'Beena','beena@shop.com',1,1,90000,'2019-03-15'),
 (3,'Chetan','chetan@shop.com',1,1,90000,'2020-07-01'),
 (4,'Divya','divya@shop.com',1,2,70000,'2021-11-20'),
 (5,'Esha','esha@shop.com',2,1,60000,'2019-05-05'),
 (6,'Farhan','farhan@shop.com',2,5,45000,'2022-02-14'),
 (7,'Gita','gita@shop.com',3,1,80000,'2018-09-09'),
 (8,'Hari','hari@shop.com',3,7,85000,'2021-06-30'),
 (9,'Isha','isha@shop.com',3,7,50000,'2023-01-02'),
 (10,'Jay',NULL,NULL,1,40000,'2024-04-01'),
 (11,'Karan','beena@shop.com',4,1,95000,'2020-01-01');  -- duplicate email on purpose

INSERT INTO Customers (Name, City, Email) VALUES
 ('Asha','Pune','asha@x.com'),('Ravi','Delhi','ravi@x.com'),
 ('Meera','Pune',NULL),('John','Mumbai','john@x.com'),
 ('Sara','Delhi','sara@x.com'),('Asha','Pune','asha@x.com'); -- duplicate customer

INSERT INTO Products VALUES
 (1,'Laptop','Electronics',60000),(2,'Mouse','Electronics',800),
 (3,'Phone','Electronics',30000),(4,'Chair','Furniture',5000),
 (5,'Desk','Furniture',12000),(6,'Pen','Stationery',20),
 (7,'Notebook','Stationery',60);

INSERT INTO Orders (CustomerId, OrderDate, Amount) VALUES
 (1,'2026-01-05',60800),(1,'2026-02-10',5000),(2,'2026-01-20',30000),
 (2,'2026-03-01',12080),(3,'2026-02-15',800),(5,'2026-03-18',17000),
 (1,'2026-03-22',120),(5,'2026-01-11',60000);
-- Customer 4 (John) and 6 have NO orders (useful for LEFT JOIN questions)

INSERT INTO OrderItems (OrderId, ProductId, Qty, UnitPrice) VALUES
 (1,1,1,60000),(1,2,1,800),(2,4,1,5000),(3,3,1,30000),
 (4,5,1,12000),(4,6,4,20),(5,2,1,800),(6,5,1,12000),(6,4,1,5000),
 (7,7,2,60),(8,1,1,60000);

INSERT INTO Accounts VALUES (101,'Asha',10000),(102,'Ravi',2000),(103,'Meera',0);
GO

-- Small practice tables for puzzle questions (Ch.06 gaps & islands, Ch.12 #26-27)
CREATE TABLE Logins (UserId INT NOT NULL, LoginDate DATE NOT NULL);
INSERT INTO Logins VALUES
 (1,'2026-01-01'),(1,'2026-01-02'),(1,'2026-01-03'),(1,'2026-01-05'),
 (2,'2026-01-10'),(2,'2026-01-11'),(2,'2026-01-12'),(2,'2026-01-13');

CREATE TABLE Logs (Id INT PRIMARY KEY, Num INT NOT NULL);
INSERT INTO Logs VALUES (1,1),(2,1),(3,1),(4,2),(5,1),(6,2),(7,2);

CREATE TABLE People (PersonId INT PRIMARY KEY, Name NVARCHAR(50), Gender CHAR(1));
INSERT INTO People VALUES (1,'Amit','M'),(2,'Priya','F'),(3,'Rahul','M');
GO
```

### Data at a glance

- **Employees** form a hierarchy: Anil is the CEO (ManagerId NULL).
- **Jay** has no department (DeptId NULL) → good for LEFT JOIN tests.
- **Finance** has one employee; **HR** salary is low → good for HAVING tests.
- Beena and Chetan have the **same salary** (90000) → good for RANK vs DENSE_RANK.
- Duplicate email & duplicate customer → good for "find/delete duplicates".


---

# 01. Database Basics & Creating Tables

## 1.1 The big picture (simple words)

| Term | Simple meaning | Example |
|------|---------------|---------|
| Database | A cupboard that stores organised data | `ShopDB` |
| Table | One shelf / one Excel sheet | `Employees` |
| Row (record) | One item on the shelf | Anil's details |
| Column (field) | One property of every item | `Salary` |
| Primary Key (PK) | A unique ID card for each row | `EmpId` |
| Foreign Key (FK) | A column pointing to another table's PK | `Employees.DeptId → Departments.DeptId` |
| RDBMS | Software that manages relational tables | SQL Server, MySQL, PostgreSQL, Oracle |

**Why relational?** Instead of repeating "IT department, floor 3, head Anil" on every employee row, we store the department **once** and just point to it (`DeptId`). Less duplication, fewer mistakes.

## 1.2 SQL command families

| Family | Full form | Commands | Works on |
|--------|-----------|----------|----------|
| DDL | Data Definition Language | CREATE, ALTER, DROP, TRUNCATE | Structure |
| DML | Data Manipulation Language | INSERT, UPDATE, DELETE, MERGE | Data |
| DQL | Data Query Language | SELECT | Reading |
| DCL | Data Control Language | GRANT, REVOKE, DENY | Permissions |
| TCL | Transaction Control Language | BEGIN TRAN, COMMIT, ROLLBACK, SAVE TRAN | Transactions |

## 1.3 Data types you must know

| Type | Use for | Note |
|------|---------|------|
| `INT`, `BIGINT`, `SMALLINT`, `TINYINT` | Whole numbers | INT ≈ ±2.1 billion |
| `DECIMAL(p,s)` / `NUMERIC` | Money, exact values | `DECIMAL(10,2)` → 99999999.99 |
| `FLOAT`, `REAL` | Scientific approx values | **Never for money** (rounding errors) |
| `CHAR(n)` | Fixed length text | `CHAR(10)` always uses 10 bytes |
| `VARCHAR(n)` | Variable length, non-Unicode | English only |
| `NVARCHAR(n)` | Variable length, **Unicode** | Hindi, Chinese, emoji. 2 bytes/char |
| `VARCHAR(MAX)` | Up to 2 GB text | Avoid for indexed columns |
| `DATE`, `TIME`, `DATETIME2`, `DATETIMEOFFSET` | Dates/times | Prefer `DATETIME2` over old `DATETIME` |
| `BIT` | True/False | 0/1/NULL |
| `UNIQUEIDENTIFIER` | GUID | `NEWID()` |

## 1.4 Constraints – rules the DB enforces

### Easy example

```sql
CREATE TABLE Students (
    StudentId INT IDENTITY(1,1) PRIMARY KEY,  -- auto 1,2,3...
    Name      NVARCHAR(100) NOT NULL,         -- required
    Email     NVARCHAR(100) UNIQUE,           -- no duplicates (one NULL allowed in SQL Server)
    Age       INT CHECK (Age BETWEEN 5 AND 100),
    JoinedOn  DATE DEFAULT GETDATE()          -- auto value if not given
);
```

### Complex / real-world example – named constraints, composite key, cascade

```sql
CREATE TABLE Enrollments (
    StudentId INT NOT NULL,
    CourseId  INT NOT NULL,
    EnrolledOn DATETIME2 NOT NULL CONSTRAINT DF_Enroll_Date DEFAULT SYSUTCDATETIME(),
    Grade     CHAR(1) NULL,
    CONSTRAINT PK_Enrollments PRIMARY KEY (StudentId, CourseId),          -- composite PK
    CONSTRAINT FK_Enroll_Student FOREIGN KEY (StudentId)
        REFERENCES Students(StudentId) ON DELETE CASCADE,                 -- delete student → delete enrollments
    CONSTRAINT CK_Enroll_Grade CHECK (Grade IN ('A','B','C','D','F'))
);
```

> **Tip:** Always **name** constraints in real projects (`PK_`, `FK_`, `CK_`, `DF_`, `UQ_`). Auto-generated names like `PK__Enrollm__3A2F...` are painful to drop later.

### FK actions
- `ON DELETE CASCADE` – delete parent → children deleted too
- `ON DELETE SET NULL` – children's FK becomes NULL
- `NO ACTION` (default) – delete of parent **fails** if children exist

## 1.5 ALTER, DROP, TRUNCATE

```sql
ALTER TABLE Students ADD Phone VARCHAR(15) NULL;
ALTER TABLE Students ALTER COLUMN Phone VARCHAR(20) NULL;
ALTER TABLE Students DROP COLUMN Phone;
ALTER TABLE Students ADD CONSTRAINT UQ_Students_Name UNIQUE (Name);

TRUNCATE TABLE Enrollments;   -- remove all rows fast
DROP TABLE Enrollments;       -- remove table completely
```

## 1.6 DELETE vs TRUNCATE vs DROP (asked in 90% interviews)

| | DELETE | TRUNCATE | DROP |
|---|---|---|---|
| Type | DML | DDL | DDL |
| Removes | Selected rows (WHERE) | All rows | Table + structure |
| WHERE allowed | ✅ | ❌ | ❌ |
| Logging | Every row logged (slow) | Only page deallocation (fast) | – |
| Fires triggers | ✅ | ❌ | ❌ |
| Resets IDENTITY | ❌ | ✅ | – |
| Rollback possible inside transaction | ✅ | ✅ (in SQL Server!) | ✅ (in SQL Server) |
| Works if table is referenced by FK | ✅ | ❌ | ❌ |

> **Trap:** Many people say "TRUNCATE cannot be rolled back". In SQL Server it **can**, if done inside an explicit transaction.

## 1.7 PRIMARY KEY vs UNIQUE

| | PRIMARY KEY | UNIQUE |
|---|---|---|
| Per table | Only 1 | Many |
| NULL allowed | ❌ | ✅ (one NULL in SQL Server) |
| Default index | Clustered | Non-clustered |

## Try it yourself
1. Create a `Books` table: `BookId` (auto), `Title` (required), `ISBN` (unique), `Price` (> 0), `PublishedOn` (default today).
2. Add a column `AuthorId` and a FK to a new `Authors` table with `ON DELETE SET NULL`.
3. What happens to IDENTITY after `DELETE FROM Books` vs `TRUNCATE TABLE Books`?

<details><summary>Answer 3</summary>
After DELETE, the next insert continues numbering (e.g., 6). After TRUNCATE, numbering restarts from the seed (1).
</details>


---

# 02. Inserting, Updating, Deleting & SELECT

## 2.1 INSERT

```sql
-- Easy
INSERT INTO Customers (Name, City, Email) VALUES ('Neha','Chennai','neha@x.com');

-- Multiple rows
INSERT INTO Customers (Name, City) VALUES ('A1','Pune'), ('A2','Goa');

-- Insert from another query
INSERT INTO Customers (Name, City)
SELECT Name, 'Unknown' FROM Employees WHERE DeptId IS NULL;

-- Get the new IDENTITY value
INSERT INTO Customers (Name) VALUES ('Z');
SELECT SCOPE_IDENTITY() AS NewId;

-- Complex: OUTPUT clause returns inserted rows (great for audit / multiple ids)
INSERT INTO Customers (Name, City)
OUTPUT inserted.CustomerId, inserted.Name
VALUES ('P1','Pune'), ('P2','Pune');
```

## 2.2 UPDATE

```sql
-- Easy
UPDATE Employees SET Salary = Salary * 1.10 WHERE DeptId = 1;

-- Complex: UPDATE with JOIN (give 5% raise to everyone in 'Sales')
UPDATE e
SET    e.Salary = e.Salary * 1.05
FROM   Employees e
JOIN   Departments d ON d.DeptId = e.DeptId
WHERE  d.DeptName = 'Sales';

-- Conditional update with CASE
UPDATE Employees
SET Salary = CASE WHEN Salary < 50000 THEN Salary * 1.20
                  WHEN Salary < 90000 THEN Salary * 1.10
                  ELSE Salary * 1.05 END;
```

## 2.3 DELETE

```sql
DELETE FROM Orders WHERE OrderDate < '2026-01-01';

-- DELETE with JOIN: delete order items of cancelled customers
DELETE oi
FROM OrderItems oi
JOIN Orders o ON o.OrderId = oi.OrderId
WHERE o.CustomerId = 3;
```

> ⚠️ **Safety habit:** write the `SELECT` first, check the rows, then change `SELECT *` into `DELETE`/`UPDATE`. In production, wrap in `BEGIN TRAN ... ROLLBACK` first to test.

## 2.4 MERGE (UPSERT) – insert if new, update if exists

```sql
MERGE INTO Products AS tgt
USING (VALUES (2,'Mouse',900.00,'Electronics'),
              (8,'Keyboard',1500.00,'Electronics')) AS src(ProductId,Name,Price,Category)
ON tgt.ProductId = src.ProductId
WHEN MATCHED THEN
    UPDATE SET tgt.Price = src.Price
WHEN NOT MATCHED BY TARGET THEN
    INSERT (ProductId, Name, Category, Price) VALUES (src.ProductId, src.Name, src.Category, src.Price);
```

## 2.5 SELECT – reading data

```sql
SELECT * FROM Employees;                                 -- all columns (avoid in production)
SELECT Name, Salary FROM Employees;
SELECT Name, Salary * 12 AS AnnualSalary FROM Employees; -- alias
SELECT DISTINCT DeptId FROM Employees;                   -- unique values
SELECT TOP 3 Name, Salary FROM Employees ORDER BY Salary DESC;
SELECT TOP 10 PERCENT * FROM Employees;
```

### Filtering with WHERE

| Operator | Example |
|----------|---------|
| `=`, `<>`, `>`, `<`, `>=`, `<=` | `Salary >= 80000` |
| `AND`, `OR`, `NOT` | `DeptId = 1 AND Salary > 80000` |
| `BETWEEN` (inclusive) | `Salary BETWEEN 50000 AND 90000` |
| `IN` | `DeptId IN (1, 3)` |
| `LIKE` | `Name LIKE 'A%'` (starts with A), `'%a'` (ends), `'_e%'` (2nd letter e) |
| `IS NULL` / `IS NOT NULL` | `Email IS NULL` |

```sql
SELECT * FROM Employees
WHERE (DeptId = 1 OR DeptId = 3) AND Salary > 60000;  -- use brackets! AND runs before OR
```

### NULL – the most misunderstood thing

- NULL = **unknown**, not zero, not empty string.
- `NULL = NULL` → **UNKNOWN** (not TRUE). So `WHERE Email = NULL` returns nothing.
- Any arithmetic with NULL → NULL (`5 + NULL = NULL`).
- Aggregates (`SUM`, `AVG`, `COUNT(col)`) **ignore NULLs**; `COUNT(*)` counts all rows.

```sql
SELECT Name, ISNULL(Email, 'no-email') FROM Employees;             -- SQL Server
SELECT Name, COALESCE(Email, 'no-email') FROM Employees;           -- ANSI, takes many args
SELECT NULLIF(10, 10);  -- returns NULL (useful to avoid divide by zero: x / NULLIF(y,0))
```

### Sorting & paging

```sql
SELECT * FROM Employees ORDER BY DeptId ASC, Salary DESC;

-- Paging: page 2 with 5 rows per page
SELECT EmpId, Name FROM Employees
ORDER BY EmpId
OFFSET 5 ROWS FETCH NEXT 5 ROWS ONLY;
```

### CASE – if/else inside SQL

```sql
SELECT Name, Salary,
  CASE
    WHEN Salary >= 100000 THEN 'Senior'
    WHEN Salary >= 60000  THEN 'Mid'
    ELSE 'Junior'
  END AS Band
FROM Employees;
```

### Useful built-in functions

| Category | Functions |
|----------|-----------|
| String | `LEN`, `UPPER`, `LOWER`, `LTRIM/RTRIM/TRIM`, `SUBSTRING(s,start,len)`, `CHARINDEX`, `REPLACE`, `CONCAT`, `STRING_AGG`, `STRING_SPLIT`, `LEFT`, `RIGHT` |
| Date | `GETDATE()`, `SYSUTCDATETIME()`, `DATEADD(day,7,d)`, `DATEDIFF(day,d1,d2)`, `YEAR/MONTH/DAY`, `EOMONTH`, `FORMAT` |
| Convert | `CAST(x AS INT)`, `CONVERT(VARCHAR, d, 103)`, `TRY_CAST`, `TRY_CONVERT` |
| Math | `ROUND`, `CEILING`, `FLOOR`, `ABS` |

```sql
-- Complex: domain of each email, employees hired in last 3 years, tenure in years
SELECT Name,
       SUBSTRING(Email, CHARINDEX('@', Email) + 1, LEN(Email)) AS Domain,
       DATEDIFF(YEAR, HireDate, GETDATE()) AS TenureYears
FROM Employees
WHERE HireDate >= DATEADD(YEAR, -3, CAST(GETDATE() AS DATE));
```

## Try it yourself
1. Employees whose name has 'a' as the 2nd letter.
2. Show salary band (CASE) and sort Seniors first.
3. Page 3 of customers (3 per page) ordered by name.

<details><summary>Answers</summary>

```sql
SELECT * FROM Employees WHERE Name LIKE '_a%';

SELECT Name, Salary, CASE WHEN Salary>=100000 THEN 'Senior' WHEN Salary>=60000 THEN 'Mid' ELSE 'Junior' END Band
FROM Employees ORDER BY Salary DESC;

SELECT * FROM Customers ORDER BY Name OFFSET 6 ROWS FETCH NEXT 3 ROWS ONLY;
```
</details>


---

# 03. Aggregation: GROUP BY & HAVING

## 3.1 Aggregate functions

| Function | Meaning | NULL handling |
|----------|---------|---------------|
| `COUNT(*)` | Count rows | Counts all rows |
| `COUNT(col)` | Count non-NULL values | Ignores NULL |
| `COUNT(DISTINCT col)` | Count unique non-NULL values | Ignores NULL |
| `SUM`, `AVG`, `MIN`, `MAX` | As named | Ignore NULL |

```sql
SELECT COUNT(*)        AS TotalEmp,      -- 11
       COUNT(Email)    AS WithEmail,     -- 10 (Jay has NULL)
       COUNT(DISTINCT DeptId) AS Depts,  -- 4
       AVG(Salary)     AS AvgSal,
       MAX(Salary)     AS MaxSal
FROM Employees;
```

> **Trap:** `AVG(col)` ignores NULLs → average of 10, 20, NULL = **15**, not 10.

## 3.2 GROUP BY – "one result row per group"

Think: put rows into buckets by a column, then summarise each bucket.

```sql
-- Easy: employees and total salary per department
SELECT DeptId, COUNT(*) AS Cnt, SUM(Salary) AS TotalSal
FROM Employees
GROUP BY DeptId;
```

**Rule:** every column in `SELECT` must either be in `GROUP BY` or inside an aggregate.

```sql
SELECT DeptId, Name, COUNT(*) FROM Employees GROUP BY DeptId;  -- ❌ ERROR: Name not grouped
```

## 3.3 WHERE vs HAVING

| WHERE | HAVING |
|-------|--------|
| Filters **rows** before grouping | Filters **groups** after grouping |
| Cannot use aggregates | Uses aggregates |
| Faster (reduces data early) | Runs later |

```sql
-- Departments (only employees hired after 2019) having more than 1 employee
SELECT DeptId, COUNT(*) AS Cnt
FROM Employees
WHERE HireDate >= '2019-01-01'       -- row filter
GROUP BY DeptId
HAVING COUNT(*) > 1;                  -- group filter
```

## 3.4 Complex examples

```sql
-- 1. Department name, headcount, avg salary, highest paid - only depts with avg > 60000
SELECT d.DeptName,
       COUNT(e.EmpId)          AS HeadCount,
       CAST(AVG(e.Salary) AS DECIMAL(10,2)) AS AvgSal,
       MAX(e.Salary)           AS MaxSal
FROM Departments d
LEFT JOIN Employees e ON e.DeptId = d.DeptId
GROUP BY d.DeptName
HAVING AVG(e.Salary) > 60000
ORDER BY AvgSal DESC;

-- 2. Monthly sales report
SELECT YEAR(OrderDate) AS Yr, MONTH(OrderDate) AS Mn,
       COUNT(*) AS Orders, SUM(Amount) AS Revenue
FROM Orders
GROUP BY YEAR(OrderDate), MONTH(OrderDate)
ORDER BY Yr, Mn;

-- 3. Conditional aggregation (pivot-style) - VERY common interview trick
SELECT CustomerId,
       SUM(CASE WHEN MONTH(OrderDate) = 1 THEN Amount ELSE 0 END) AS Jan,
       SUM(CASE WHEN MONTH(OrderDate) = 2 THEN Amount ELSE 0 END) AS Feb,
       SUM(CASE WHEN MONTH(OrderDate) = 3 THEN Amount ELSE 0 END) AS Mar
FROM Orders
GROUP BY CustomerId;

-- 4. Comma-separated list per group
SELECT DeptId, STRING_AGG(Name, ', ') WITHIN GROUP (ORDER BY Name) AS Members
FROM Employees
GROUP BY DeptId;

-- 5. Subtotals and grand total
SELECT DeptId, SUM(Salary) AS Total
FROM Employees
GROUP BY ROLLUP(DeptId);   -- last row (DeptId NULL) = grand total
```

## 3.5 PIVOT operator (alternative to #3)

```sql
SELECT CustomerId, [1] AS Jan, [2] AS Feb, [3] AS Mar
FROM (SELECT CustomerId, MONTH(OrderDate) AS Mn, Amount FROM Orders) src
PIVOT (SUM(Amount) FOR Mn IN ([1],[2],[3])) p;
```

## 3.6 Logical query processing order (memorise!)

```
1. FROM / JOIN
2. WHERE
3. GROUP BY
4. HAVING
5. SELECT        ← aliases are created here
6. DISTINCT
7. ORDER BY      ← can use aliases
8. TOP / OFFSET
```

**Why this matters:**
```sql
SELECT Salary * 12 AS Annual FROM Employees WHERE Annual > 100000;  -- ❌ Annual unknown in WHERE
SELECT Salary * 12 AS Annual FROM Employees ORDER BY Annual;        -- ✅ works
```

## Try it yourself
1. Count of employees per department **name**, including departments with 0 employees.
2. Customers whose total order amount > 50000.
3. Cities with more than one customer.

<details><summary>Answers</summary>

```sql
SELECT d.DeptName, COUNT(e.EmpId) Cnt          -- COUNT(e.EmpId) not COUNT(*) so empty depts show 0
FROM Departments d LEFT JOIN Employees e ON e.DeptId = d.DeptId
GROUP BY d.DeptName;

SELECT CustomerId, SUM(Amount) Total FROM Orders GROUP BY CustomerId HAVING SUM(Amount) > 50000;

SELECT City, COUNT(*) FROM Customers GROUP BY City HAVING COUNT(*) > 1;
```
</details>


---

# 04. JOINs

## 4.1 Why joins?
Data is split across tables (normalization). A JOIN **glues rows together** using a matching column.

## 4.2 Visual idea

```
Table A (Employees)      Table B (Departments)
 INNER JOIN  → only the overlap         (A ∩ B)
 LEFT JOIN   → all of A + matching B    (B side NULL if no match)
 RIGHT JOIN  → all of B + matching A
 FULL JOIN   → everything from both
 CROSS JOIN  → every A row × every B row
```

## 4.3 Each join with our data

```sql
-- INNER JOIN: employees that HAVE a department (Jay is excluded)
SELECT e.Name, d.DeptName
FROM Employees e
INNER JOIN Departments d ON e.DeptId = d.DeptId;

-- LEFT JOIN: all employees, DeptName NULL for Jay
SELECT e.Name, d.DeptName
FROM Employees e
LEFT JOIN Departments d ON e.DeptId = d.DeptId;

-- RIGHT JOIN: all departments, even without employees
SELECT e.Name, d.DeptName
FROM Employees e
RIGHT JOIN Departments d ON e.DeptId = d.DeptId;

-- FULL OUTER JOIN: everyone + every dept
SELECT e.Name, d.DeptName
FROM Employees e
FULL JOIN Departments d ON e.DeptId = d.DeptId;

-- CROSS JOIN: all combinations (4 depts × 7 products = 28 rows)
SELECT d.DeptName, p.Name FROM Departments d CROSS JOIN Products p;
```

## 4.4 SELF JOIN – a table joined to itself

```sql
-- Employee with their manager's name
SELECT e.Name AS Employee, m.Name AS Manager
FROM Employees e
LEFT JOIN Employees m ON e.ManagerId = m.EmpId;

-- Employees earning MORE than their manager (classic!)
SELECT e.Name, e.Salary, m.Name AS Manager, m.Salary AS MgrSalary
FROM Employees e
JOIN Employees m ON e.ManagerId = m.EmpId
WHERE e.Salary > m.Salary;          -- Hari (85000) > Gita (80000)
```

## 4.5 Anti-join – rows with NO match (asked a lot)

```sql
-- Customers who never ordered
SELECT c.CustomerId, c.Name
FROM Customers c
LEFT JOIN Orders o ON o.CustomerId = c.CustomerId
WHERE o.OrderId IS NULL;

-- Same with NOT EXISTS (often best plan, NULL-safe)
SELECT c.CustomerId, c.Name
FROM Customers c
WHERE NOT EXISTS (SELECT 1 FROM Orders o WHERE o.CustomerId = c.CustomerId);
```

## 4.6 Multi-table join (complex)

```sql
-- Order details: customer, product, qty, line total, only Electronics, sorted
SELECT o.OrderId, c.Name AS Customer, p.Name AS Product,
       oi.Qty, oi.UnitPrice, oi.Qty * oi.UnitPrice AS LineTotal
FROM Orders o
JOIN Customers  c  ON c.CustomerId = o.CustomerId
JOIN OrderItems oi ON oi.OrderId   = o.OrderId
JOIN Products   p  ON p.ProductId  = oi.ProductId
WHERE p.Category = 'Electronics'
ORDER BY o.OrderId;

-- Revenue per category per customer city
SELECT c.City, p.Category, SUM(oi.Qty * oi.UnitPrice) AS Revenue
FROM Orders o
JOIN Customers c   ON c.CustomerId = o.CustomerId
JOIN OrderItems oi ON oi.OrderId   = o.OrderId
JOIN Products p    ON p.ProductId  = oi.ProductId
GROUP BY c.City, p.Category
ORDER BY c.City, Revenue DESC;
```

## 4.7 The ON vs WHERE trap with LEFT JOIN ⚠️

```sql
-- A: filter in WHERE → turns LEFT JOIN into INNER JOIN (customers without Jan orders disappear)
SELECT c.Name, o.Amount
FROM Customers c LEFT JOIN Orders o ON o.CustomerId = c.CustomerId
WHERE o.OrderDate >= '2026-02-01';

-- B: filter in ON → keeps ALL customers, Amount NULL where no matching order
SELECT c.Name, o.Amount
FROM Customers c LEFT JOIN Orders o
  ON o.CustomerId = c.CustomerId AND o.OrderDate >= '2026-02-01';
```

> **Interview line:** "For outer joins, conditions on the *right* table belong in the ON clause; putting them in WHERE removes the NULL rows and effectively makes it an inner join."

## 4.8 Row count puzzle (very common)

Table A has values `1,1,2,NULL`; Table B has `1,1,3,NULL`. How many rows?

| Join | Rows | Why |
|------|------|-----|
| INNER | 4 | each 1 in A matches two 1s in B → 2×2 = 4; NULL never matches NULL |
| LEFT | 6 | 4 matches + A's `2` + A's `NULL` |
| RIGHT | 6 | 4 + B's `3` + B's `NULL` |
| FULL | 8 | 4 + 2 (A unmatched) + 2 (B unmatched) |
| CROSS | 16 | 4 × 4 |

## 4.9 APPLY (bonus, impresses interviewers)

```sql
-- Top 2 most expensive items of each order (correlated "join to a function/subquery")
SELECT o.OrderId, x.ProductId, x.UnitPrice
FROM Orders o
CROSS APPLY (SELECT TOP 2 oi.ProductId, oi.UnitPrice
             FROM OrderItems oi WHERE oi.OrderId = o.OrderId
             ORDER BY oi.UnitPrice DESC) x;
-- OUTER APPLY = like LEFT JOIN (keeps orders with no items)
```

## Try it yourself
1. List department names with no employees (if any).
2. Products never sold.
3. For each employee show name, department name, and manager's name (handle NULLs).

<details><summary>Answers</summary>

```sql
SELECT d.DeptName FROM Departments d
WHERE NOT EXISTS (SELECT 1 FROM Employees e WHERE e.DeptId = d.DeptId);

SELECT p.Name FROM Products p
LEFT JOIN OrderItems oi ON oi.ProductId = p.ProductId
WHERE oi.OrderItemId IS NULL;

SELECT e.Name, ISNULL(d.DeptName,'-') Dept, ISNULL(m.Name,'-') Manager
FROM Employees e
LEFT JOIN Departments d ON d.DeptId = e.DeptId
LEFT JOIN Employees  m ON m.EmpId  = e.ManagerId;
```
</details>


---

# 05. Subqueries, CTEs & Set Operators

## 5.1 Subquery = a query inside a query

### Scalar subquery (returns one value)
```sql
SELECT Name, Salary FROM Employees
WHERE Salary > (SELECT AVG(Salary) FROM Employees);
```

### Multi-row subquery (IN)
```sql
SELECT Name FROM Customers
WHERE CustomerId IN (SELECT CustomerId FROM Orders WHERE Amount > 20000);
```

### Correlated subquery (runs per outer row – refers to outer table)
```sql
-- Employees earning more than THEIR department's average
SELECT e.Name, e.DeptId, e.Salary
FROM Employees e
WHERE e.Salary > (SELECT AVG(e2.Salary) FROM Employees e2 WHERE e2.DeptId = e.DeptId);
```

### Derived table (subquery in FROM)
```sql
SELECT t.DeptId, t.AvgSal
FROM (SELECT DeptId, AVG(Salary) AS AvgSal FROM Employees GROUP BY DeptId) AS t
WHERE t.AvgSal > 60000;
```

## 5.2 EXISTS vs IN vs JOIN

| | IN | EXISTS | JOIN |
|---|---|---|---|
| Returns | Compares values to a list | TRUE/FALSE – stops at first match | Combines columns |
| Duplicates | No duplication | No duplication | Can duplicate rows (1-to-many) |
| NULL issue | `NOT IN` breaks with NULL | Safe | – |
| Use when | Small fixed list | Just checking existence | Need columns from both |

### The NOT IN + NULL trap ⚠️
```sql
-- If subquery returns even one NULL, NOT IN returns ZERO rows!
SELECT DeptName FROM Departments
WHERE DeptId NOT IN (SELECT DeptId FROM Employees);   -- Employees has DeptId NULL (Jay) → empty result!

-- Fix
SELECT DeptName FROM Departments d
WHERE NOT EXISTS (SELECT 1 FROM Employees e WHERE e.DeptId = d.DeptId);
```
**Why?** `x NOT IN (1, 2, NULL)` = `x<>1 AND x<>2 AND x<>NULL` → last part is UNKNOWN → whole thing never TRUE.

## 5.3 CTE – Common Table Expression

A **named temporary result** that lives only for the next statement. Makes complex queries readable (like variables for queries).

```sql
-- Easy
WITH HighEarners AS (
    SELECT EmpId, Name, Salary FROM Employees WHERE Salary > 80000
)
SELECT * FROM HighEarners ORDER BY Salary DESC;

-- Multiple CTEs chained
WITH DeptAvg AS (
    SELECT DeptId, AVG(Salary) AS AvgSal FROM Employees GROUP BY DeptId
),
Above AS (
    SELECT e.Name, e.DeptId, e.Salary, d.AvgSal
    FROM Employees e JOIN DeptAvg d ON d.DeptId = e.DeptId
    WHERE e.Salary > d.AvgSal
)
SELECT a.*, dp.DeptName FROM Above a JOIN Departments dp ON dp.DeptId = a.DeptId;
```

### Recursive CTE – hierarchies & sequences

```sql
-- Org chart with level and path
WITH Org AS (
    -- anchor: top boss
    SELECT EmpId, Name, ManagerId, 0 AS Lvl,
           CAST(Name AS NVARCHAR(500)) AS Path
    FROM Employees WHERE ManagerId IS NULL
    UNION ALL
    -- recursive part: people reporting to previous level
    SELECT e.EmpId, e.Name, e.ManagerId, o.Lvl + 1,
           CAST(o.Path + ' > ' + e.Name AS NVARCHAR(500))
    FROM Employees e
    JOIN Org o ON e.ManagerId = o.EmpId
)
SELECT * FROM Org ORDER BY Path
OPTION (MAXRECURSION 100);   -- default 100; 0 = unlimited

-- Generate dates for a calendar (fill missing days in a report)
WITH Dates AS (
    SELECT CAST('2026-01-01' AS DATE) AS D
    UNION ALL
    SELECT DATEADD(DAY, 1, D) FROM Dates WHERE D < '2026-01-31'
)
SELECT d.D, ISNULL(SUM(o.Amount), 0) AS Sales
FROM Dates d LEFT JOIN Orders o ON o.OrderDate = d.D
GROUP BY d.D;
```

### CTE vs Subquery vs Temp table vs View

| | Scope | Reusable | Indexable | Best for |
|---|---|---|---|---|
| Subquery | Inline | No | No | Simple one-off |
| CTE | Next statement only | Within that statement (can reference many times) | No | Readability, recursion |
| Temp table `#t` | Session | Yes | Yes | Large intermediate data, reused many times |
| View | Permanent object | Yes | (Indexed views) | Reused logic, security |

> **Note:** A CTE is **not stored**; if referenced twice, SQL may execute it twice.

## 5.4 Set operators

Rules: same number of columns, compatible types, column names come from the first query.

```sql
SELECT City FROM Customers WHERE City IS NOT NULL
UNION        -- combine, remove duplicates (sorts/hashes → slower)
SELECT 'Pune';

SELECT Email FROM Employees
UNION ALL    -- combine, keep duplicates (faster)
SELECT Email FROM Customers;

SELECT Email FROM Employees
INTERSECT    -- in both
SELECT Email FROM Customers;

SELECT Email FROM Customers
EXCEPT       -- in first but not second
SELECT Email FROM Employees;
```

| | UNION | UNION ALL |
|---|---|---|
| Duplicates | Removed | Kept |
| Speed | Slower | Faster |
| Use | Need distinct | Know no duplicates / want all |

## Try it yourself
1. Second highest salary using a subquery (no window function).
2. Customers who ordered in **both** January and March 2026 (use INTERSECT).
3. Recursive CTE: print numbers 1–20.

<details><summary>Answers</summary>

```sql
SELECT MAX(Salary) FROM Employees WHERE Salary < (SELECT MAX(Salary) FROM Employees);

SELECT CustomerId FROM Orders WHERE OrderDate >= '2026-01-01' AND OrderDate < '2026-02-01'
INTERSECT
SELECT CustomerId FROM Orders WHERE OrderDate >= '2026-03-01' AND OrderDate < '2026-04-01';

WITH n AS (SELECT 1 x UNION ALL SELECT x+1 FROM n WHERE x < 20) SELECT x FROM n;
```
</details>


---

# 06. Window Functions

## 6.1 What is a "window"?

`GROUP BY` **collapses** rows into one per group.
Window functions **keep every row** and add a calculated column looking at a "window" of related rows.

```
function() OVER (
    PARTITION BY ...   -- split into groups (optional)
    ORDER BY ...       -- order inside each group (needed for ranking/running)
    ROWS/RANGE ...     -- frame: which rows around current row (optional)
)
```

## 6.2 Easy example – compare GROUP BY vs window

```sql
-- GROUP BY: 1 row per dept
SELECT DeptId, AVG(Salary) FROM Employees GROUP BY DeptId;

-- Window: every employee + their dept average next to them
SELECT Name, DeptId, Salary,
       AVG(Salary) OVER (PARTITION BY DeptId) AS DeptAvg,
       Salary - AVG(Salary) OVER (PARTITION BY DeptId) AS DiffFromAvg
FROM Employees;
```

## 6.3 Ranking functions

```sql
SELECT Name, Salary,
  ROW_NUMBER() OVER (ORDER BY Salary DESC) AS RowNum,
  RANK()       OVER (ORDER BY Salary DESC) AS Rnk,
  DENSE_RANK() OVER (ORDER BY Salary DESC) AS DRnk,
  NTILE(4)     OVER (ORDER BY Salary DESC) AS Quartile
FROM Employees;
```

| Salary | ROW_NUMBER | RANK | DENSE_RANK |
|--------|-----------|------|------------|
| 150000 | 1 | 1 | 1 |
| 95000 | 2 | 2 | 2 |
| 90000 | 3 | 3 | 3 |
| 90000 | 4 | 3 | 3 |
| 85000 | 5 | **5** | **4** |

- **ROW_NUMBER** – always unique (ties broken arbitrarily)
- **RANK** – ties share rank, then **gap**
- **DENSE_RANK** – ties share rank, **no gap**

> "Nth highest salary" → use **DENSE_RANK** (so ties don't skip a level).

## 6.4 Aggregate windows – running totals & moving averages

```sql
SELECT OrderId, CustomerId, OrderDate, Amount,
  SUM(Amount) OVER (ORDER BY OrderDate)                         AS RunningTotal,
  SUM(Amount) OVER (PARTITION BY CustomerId ORDER BY OrderDate) AS CustRunningTotal,
  AVG(Amount) OVER (ORDER BY OrderDate
                    ROWS BETWEEN 2 PRECEDING AND CURRENT ROW)   AS MovingAvg3,
  SUM(Amount) OVER ()                                           AS GrandTotal,
  CAST(100.0 * Amount / SUM(Amount) OVER () AS DECIMAL(5,2))    AS PctOfTotal
FROM Orders;
```

> **Frame tip:** With `ORDER BY` and no frame, default is `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` – ties on the ORDER BY value get the same running total. Use `ROWS` for strict row-by-row.

## 6.5 Offset functions – LAG / LEAD / FIRST_VALUE / LAST_VALUE

```sql
SELECT CustomerId, OrderDate, Amount,
  LAG(Amount)  OVER (PARTITION BY CustomerId ORDER BY OrderDate) AS PrevAmount,
  LEAD(Amount) OVER (PARTITION BY CustomerId ORDER BY OrderDate) AS NextAmount,
  Amount - LAG(Amount, 1, 0) OVER (PARTITION BY CustomerId ORDER BY OrderDate) AS ChangeFromPrev,
  DATEDIFF(DAY, LAG(OrderDate) OVER (PARTITION BY CustomerId ORDER BY OrderDate), OrderDate) AS DaysSincePrev,
  FIRST_VALUE(Amount) OVER (PARTITION BY CustomerId ORDER BY OrderDate) AS FirstOrderAmt
FROM Orders;
```

> **LAST_VALUE trap:** needs `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING`, otherwise it returns the current row.

## 6.6 Complex real-world patterns

### a) Top N per group
```sql
WITH r AS (
  SELECT e.*, DENSE_RANK() OVER (PARTITION BY DeptId ORDER BY Salary DESC) AS rk
  FROM Employees e
)
SELECT * FROM r WHERE rk <= 2;
```

### b) Latest record per entity (e.g., last order per customer)
```sql
WITH r AS (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY CustomerId ORDER BY OrderDate DESC) AS rn
  FROM Orders
)
SELECT * FROM r WHERE rn = 1;
```

### c) Remove duplicates, keep the first
```sql
WITH d AS (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY Name, City, Email ORDER BY CustomerId) AS rn
  FROM Customers
)
DELETE FROM d WHERE rn > 1;
```

### d) Month-over-month growth %
```sql
WITH m AS (
  SELECT DATEFROMPARTS(YEAR(OrderDate), MONTH(OrderDate), 1) AS Mon, SUM(Amount) AS Total
  FROM Orders GROUP BY DATEFROMPARTS(YEAR(OrderDate), MONTH(OrderDate), 1)
)
SELECT Mon, Total,
       LAG(Total) OVER (ORDER BY Mon) AS PrevTotal,
       CAST(100.0 * (Total - LAG(Total) OVER (ORDER BY Mon))
            / NULLIF(LAG(Total) OVER (ORDER BY Mon), 0) AS DECIMAL(8,2)) AS GrowthPct
FROM m;
```

### e) Gaps & Islands – consecutive login days (senior-level favourite)
```sql
-- Logins(UserId, LoginDate). Find streaks of consecutive days.
WITH g AS (
  SELECT UserId, LoginDate,
         DATEADD(DAY, -ROW_NUMBER() OVER (PARTITION BY UserId ORDER BY LoginDate), LoginDate) AS Grp
  FROM (SELECT DISTINCT UserId, LoginDate FROM Logins) x
)
SELECT UserId, MIN(LoginDate) AS StreakStart, MAX(LoginDate) AS StreakEnd, COUNT(*) AS Days
FROM g
GROUP BY UserId, Grp
HAVING COUNT(*) >= 3;
```
**Idea:** for consecutive dates, `date - row_number` stays constant → same group.

### f) Paging using ROW_NUMBER (older style than OFFSET)
```sql
WITH p AS (SELECT *, ROW_NUMBER() OVER (ORDER BY EmpId) rn FROM Employees)
SELECT * FROM p WHERE rn BETWEEN 6 AND 10;
```

## 6.7 Where can window functions be used?
Only in **SELECT** and **ORDER BY** (because they run after WHERE/GROUP BY/HAVING). To filter on them → wrap in a CTE/subquery (as above).

## Try it yourself
1. 3rd highest salary per department.
2. For each order show % of that customer's total spend.
3. Employees whose salary is above the previous hire's salary (order by HireDate).

<details><summary>Answers</summary>

```sql
WITH r AS (SELECT *, DENSE_RANK() OVER (PARTITION BY DeptId ORDER BY Salary DESC) rk FROM Employees)
SELECT * FROM r WHERE rk = 3;

SELECT OrderId, CustomerId, Amount,
       100.0 * Amount / SUM(Amount) OVER (PARTITION BY CustomerId) AS PctOfCustomer
FROM Orders;

WITH x AS (SELECT Name, HireDate, Salary, LAG(Salary) OVER (ORDER BY HireDate) PrevSal FROM Employees)
SELECT * FROM x WHERE Salary > PrevSal;
```
</details>


---

# 07. Stored Procedures (Deep Dive) ⭐⭐⭐

> This is the **most important chapter** for a 2 YOE interview. Read it twice. Write every procedure from memory at least once.

## 7.1 What is a stored procedure? (simple words)

A stored procedure (SP / sproc) is a **saved, named set of SQL statements** stored inside the database. You call it by name, like a method in C#/Java.

```
Application  --EXEC dbo.PlaceOrder 1, 500-->  [ Database runs 10 statements inside ]  --> result
```

### Why use them?

| Benefit | Explanation |
|---------|-------------|
| **Reusability** | Write once, call from many apps/places |
| **Performance** | Execution plan is compiled once and **cached/reused** |
| **Less network traffic** | Send one `EXEC` instead of many statements |
| **Security** | Grant `EXECUTE` on SP without giving table access; parameters prevent SQL injection |
| **Maintainability** | Change logic in DB without redeploying app |
| **Transactions** | Group multiple steps in one atomic unit |

### Disadvantages (good to mention – shows maturity)
- Business logic split between app and DB; harder to unit test & version
- Vendor-specific (T-SQL ≠ PL/SQL)
- Debugging is harder than app code
- Parameter sniffing issues (see 7.11)

## 7.2 Level 1 – Simplest procedure

```sql
CREATE PROCEDURE dbo.usp_GetAllEmployees
AS
BEGIN
    SET NOCOUNT ON;
    SELECT EmpId, Name, Salary FROM dbo.Employees;
END
GO

EXEC dbo.usp_GetAllEmployees;   -- or EXECUTE, or just the name if it's the first statement
```

- `CREATE OR ALTER PROCEDURE` – creates or replaces (SQL Server 2016 SP1+)
- `SET NOCOUNT ON` – stops "(n rows affected)" messages → less network chatter, avoids some ORM issues
- `dbo.` – always schema-qualify (faster name resolution, avoids ambiguity)
- Don't prefix with `sp_` – SQL Server first looks in `master` for `sp_` procedures (slower, risk of name clash). Use `usp_` or no prefix.

## 7.3 Level 2 – Input parameters

```sql
CREATE OR ALTER PROCEDURE dbo.usp_GetEmployeesByDept
    @DeptId INT
AS
BEGIN
    SET NOCOUNT ON;
    SELECT EmpId, Name, Salary
    FROM dbo.Employees
    WHERE DeptId = @DeptId;
END
GO

EXEC dbo.usp_GetEmployeesByDept 1;                 -- positional
EXEC dbo.usp_GetEmployeesByDept @DeptId = 1;       -- named (preferred, clearer)
```

## 7.4 Level 3 – Default (optional) parameters

```sql
CREATE OR ALTER PROCEDURE dbo.usp_SearchEmployees
    @DeptId    INT = NULL,            -- optional
    @MinSalary DECIMAL(10,2) = 0      -- optional with default
AS
BEGIN
    SET NOCOUNT ON;
    SELECT EmpId, Name, DeptId, Salary
    FROM dbo.Employees
    WHERE (@DeptId IS NULL OR DeptId = @DeptId)
      AND Salary >= @MinSalary
    OPTION (RECOMPILE);   -- "catch-all" queries benefit from recompile (see 7.11)
END
GO

EXEC dbo.usp_SearchEmployees;                              -- all
EXEC dbo.usp_SearchEmployees @DeptId = 3;                  -- dept 3
EXEC dbo.usp_SearchEmployees @MinSalary = 80000;           -- skip first param by naming
```

## 7.5 Level 4 – OUTPUT parameters & RETURN values

Three ways an SP sends data back:

| Way | Type | Typical use |
|-----|------|-------------|
| Result set (`SELECT`) | Table(s) | Rows of data |
| `OUTPUT` parameter | Any type, many allowed | Single values (new ID, totals) |
| `RETURN` | **INT only**, one | Status code (0 = success) |

```sql
CREATE OR ALTER PROCEDURE dbo.usp_GetDeptStats
    @DeptId     INT,
    @HeadCount  INT            OUTPUT,
    @TotalSalary DECIMAL(12,2) OUTPUT
AS
BEGIN
    SET NOCOUNT ON;

    IF NOT EXISTS (SELECT 1 FROM dbo.Departments WHERE DeptId = @DeptId)
        RETURN 1;                         -- 1 = dept not found

    SELECT @HeadCount   = COUNT(*),
           @TotalSalary = ISNULL(SUM(Salary), 0)
    FROM dbo.Employees
    WHERE DeptId = @DeptId;

    RETURN 0;                             -- 0 = success
END
GO

DECLARE @cnt INT, @tot DECIMAL(12,2), @status INT;
EXEC @status = dbo.usp_GetDeptStats @DeptId = 1,
                                    @HeadCount = @cnt OUTPUT,
                                    @TotalSalary = @tot OUTPUT;
SELECT @status AS Status, @cnt AS HeadCount, @tot AS TotalSalary;
```

> **Trap:** forgetting `OUTPUT` in the `EXEC` call → variable stays NULL (no error!).

## 7.6 Level 5 – Variables, IF/ELSE, WHILE

```sql
CREATE OR ALTER PROCEDURE dbo.usp_GiveRaise
    @EmpId INT
AS
BEGIN
    SET NOCOUNT ON;
    DECLARE @Salary DECIMAL(10,2), @Pct DECIMAL(5,2);

    SELECT @Salary = Salary FROM dbo.Employees WHERE EmpId = @EmpId;

    IF @Salary IS NULL
    BEGIN
        PRINT 'Employee not found';
        RETURN 1;
    END

    IF @Salary < 50000       SET @Pct = 0.15;
    ELSE IF @Salary < 100000 SET @Pct = 0.10;
    ELSE                     SET @Pct = 0.05;

    UPDATE dbo.Employees SET Salary = Salary * (1 + @Pct) WHERE EmpId = @EmpId;
    SELECT @EmpId AS EmpId, @Salary AS OldSalary, @Salary * (1 + @Pct) AS NewSalary;
    RETURN 0;
END
GO
```

```sql
-- WHILE loop: process in batches (real-world: delete millions of rows without locking table)
CREATE OR ALTER PROCEDURE dbo.usp_PurgeOldAudit
    @OlderThanDays INT = 90,
    @BatchSize     INT = 5000
AS
BEGIN
    SET NOCOUNT ON;
    DECLARE @rows INT = 1;
    WHILE @rows > 0
    BEGIN
        DELETE TOP (@BatchSize) FROM dbo.AuditLog
        WHERE LoggedOn < DATEADD(DAY, -@OlderThanDays, SYSUTCDATETIME());
        SET @rows = @@ROWCOUNT;
        -- WAITFOR DELAY '00:00:01';  -- optional: give other queries a breather
    END
END
```

> **Interview tip:** "Set-based queries beat loops/cursors. I use loops only for **batching** large deletes/updates to avoid long locks and log growth."

## 7.7 Level 6 – Error handling: TRY / CATCH / THROW

```sql
BEGIN TRY
    SELECT 1 / 0;          -- error
END TRY
BEGIN CATCH
    SELECT ERROR_NUMBER()    AS Num,
           ERROR_MESSAGE()   AS Msg,
           ERROR_SEVERITY()  AS Sev,
           ERROR_STATE()     AS State,
           ERROR_LINE()      AS Line,
           ERROR_PROCEDURE() AS ProcName;   -- "Proc" is a reserved word, so it can't be an alias
END CATCH
```

### THROW vs RAISERROR

| | THROW | RAISERROR |
|---|---|---|
| Introduced | SQL 2012 | Old |
| Re-throw original error | `THROW;` (no args) | Not directly |
| Custom error number | ≥ 50000 | Any (needs sys.messages for some) |
| Severity | Always 16 | You choose |
| Stops batch | Yes | Not necessarily |
| Needs `;` before it | Yes (previous statement must end with `;`) | No |
| Recommendation | ✅ Use in new code | Legacy / printf-style formatting |

```sql
THROW 50001, 'Amount must be positive.', 1;     -- number, message, state
RAISERROR('Invalid id %d', 16, 1, @Id);         -- message with formatting
```

> **Note:** errors with severity ≤ 10 are info messages (not caught); compile errors / missing object at the same level are **not** caught by the same TRY block.

## 7.8 Level 7 – Transactions inside procedures (the real-world pattern)

### Example A: Bank transfer (THE most asked SP question)

```sql
CREATE OR ALTER PROCEDURE dbo.usp_TransferFunds
    @FromAcc INT,
    @ToAcc   INT,
    @Amount  DECIMAL(12,2)
AS
BEGIN
    SET NOCOUNT ON;
    SET XACT_ABORT ON;   -- any runtime error → whole transaction rolled back automatically

    -- 1. Validate input early (before opening a transaction)
    IF @Amount <= 0
        THROW 50001, 'Amount must be greater than zero.', 1;
    IF @FromAcc = @ToAcc
        THROW 50002, 'Source and target accounts must differ.', 1;

    BEGIN TRY
        BEGIN TRANSACTION;

        -- 2. Debit (condition prevents overdraft and race conditions)
        UPDATE dbo.Accounts
        SET    Balance = Balance - @Amount
        WHERE  AccId = @FromAcc AND Balance >= @Amount;

        IF @@ROWCOUNT = 0
            THROW 50003, 'Source account missing or insufficient balance.', 1;

        -- 3. Credit
        UPDATE dbo.Accounts
        SET    Balance = Balance + @Amount
        WHERE  AccId = @ToAcc;

        IF @@ROWCOUNT = 0
            THROW 50004, 'Target account not found.', 1;

        -- 4. Audit
        INSERT INTO dbo.AuditLog (TableName, Action, KeyValue)
        VALUES ('Accounts', 'TRANSFER', @FromAcc);

        COMMIT TRANSACTION;
    END TRY
    BEGIN CATCH
        IF @@TRANCOUNT > 0
            ROLLBACK TRANSACTION;
        THROW;   -- re-raise original error to the caller (never swallow errors silently!)
    END CATCH
END
GO

EXEC dbo.usp_TransferFunds @FromAcc = 101, @ToAcc = 102, @Amount = 3000;   -- ✅
EXEC dbo.usp_TransferFunds @FromAcc = 103, @ToAcc = 102, @Amount = 100;    -- ❌ insufficient
SELECT * FROM dbo.Accounts;
```

**Explain each piece in the interview:**
1. `SET XACT_ABORT ON` → guarantees rollback even for errors like timeouts from the client.
2. Validation outside the transaction → don't hold locks unnecessarily.
3. `WHERE Balance >= @Amount` in the UPDATE → atomic check-and-debit; no race between "check" and "update".
4. `@@ROWCOUNT` → detects "nothing updated".
5. `@@TRANCOUNT > 0` check → only rollback if a transaction is open.
6. `THROW;` → caller knows it failed.

### Example B: Place order with multiple items (header + details)

```sql
CREATE TYPE dbo.OrderItemList AS TABLE (
    ProductId INT NOT NULL,
    Qty       INT NOT NULL CHECK (Qty > 0)
);
GO

CREATE OR ALTER PROCEDURE dbo.usp_PlaceOrder
    @CustomerId INT,
    @Items      dbo.OrderItemList READONLY,   -- Table-Valued Parameter (TVP)
    @OrderId    INT OUTPUT
AS
BEGIN
    SET NOCOUNT ON;
    SET XACT_ABORT ON;

    IF NOT EXISTS (SELECT 1 FROM @Items)
        THROW 50010, 'Order must contain at least one item.', 1;

    IF EXISTS (SELECT 1 FROM @Items i
               LEFT JOIN dbo.Products p ON p.ProductId = i.ProductId
               WHERE p.ProductId IS NULL)
        THROW 50011, 'One or more products do not exist.', 1;

    DECLARE @Total DECIMAL(10,2);
    SELECT @Total = SUM(i.Qty * p.Price)
    FROM @Items i JOIN dbo.Products p ON p.ProductId = i.ProductId;

    BEGIN TRY
        BEGIN TRANSACTION;

        -- lock customer row so concurrent orders can't overspend credit
        DECLARE @Credit DECIMAL(10,2);
        SELECT @Credit = Credit
        FROM dbo.Customers WITH (UPDLOCK, ROWLOCK)
        WHERE CustomerId = @CustomerId;

        IF @Credit IS NULL     THROW 50012, 'Customer not found.', 1;
        IF @Credit < @Total    THROW 50013, 'Insufficient credit.', 1;

        INSERT INTO dbo.Orders (CustomerId, OrderDate, Amount)
        VALUES (@CustomerId, CAST(GETDATE() AS DATE), @Total);

        SET @OrderId = SCOPE_IDENTITY();

        INSERT INTO dbo.OrderItems (OrderId, ProductId, Qty, UnitPrice)
        SELECT @OrderId, i.ProductId, i.Qty, p.Price
        FROM @Items i JOIN dbo.Products p ON p.ProductId = i.ProductId;

        UPDATE dbo.Customers SET Credit = Credit - @Total WHERE CustomerId = @CustomerId;

        COMMIT TRANSACTION;
    END TRY
    BEGIN CATCH
        IF @@TRANCOUNT > 0 ROLLBACK TRANSACTION;
        THROW;
    END CATCH
END
GO

DECLARE @items dbo.OrderItemList, @newId INT;
INSERT INTO @items VALUES (2, 2), (6, 10);
EXEC dbo.usp_PlaceOrder @CustomerId = 4, @Items = @items, @OrderId = @newId OUTPUT;
SELECT @newId AS NewOrderId;
```

**Concepts shown:** TVP (pass a list from C# as `SqlDbType.Structured`), `UPDLOCK` to avoid race conditions, `SCOPE_IDENTITY`, header/detail insert, atomic transaction.

### SCOPE_IDENTITY vs @@IDENTITY vs IDENT_CURRENT

| Function | Returns last identity… |
|----------|-----------------------|
| `SCOPE_IDENTITY()` | in **current session & current scope** ✅ use this |
| `@@IDENTITY` | in current session, **any scope** (a trigger's insert can change it ❌) |
| `IDENT_CURRENT('tbl')` | for a table, **any session** ❌ |
| `OUTPUT inserted.Id` | Best for multi-row inserts |

## 7.9 Nested transactions & SAVEPOINT

- SQL Server has **no real nested transactions**. Each `BEGIN TRAN` increments `@@TRANCOUNT`; inner `COMMIT` just decrements; only the outermost `COMMIT` really commits.
- A `ROLLBACK` (without savepoint name) rolls back **everything** and sets `@@TRANCOUNT = 0`.

```sql
BEGIN TRAN;                         -- @@TRANCOUNT = 1
  INSERT INTO AuditLog(TableName,Action) VALUES ('T','A');
  SAVE TRANSACTION BeforeB;         -- savepoint
  INSERT INTO AuditLog(TableName,Action) VALUES ('T','B');
  ROLLBACK TRANSACTION BeforeB;     -- undo only B; @@TRANCOUNT still 1
COMMIT;                             -- A is saved
```

## 7.10 Dynamic SQL – safely

Use when table/column/sort direction is decided at runtime.

```sql
-- ❌ DANGEROUS (SQL injection): @Name = ''' ; DROP TABLE Customers; --'
DECLARE @sql NVARCHAR(MAX) = 'SELECT * FROM Customers WHERE Name = ''' + @Name + '''';
EXEC(@sql);
```

```sql
-- ✅ SAFE
CREATE OR ALTER PROCEDURE dbo.usp_SearchCustomers
    @Name     NVARCHAR(100) = NULL,
    @City     NVARCHAR(50)  = NULL,
    @SortCol  SYSNAME       = 'Name',
    @SortDir  VARCHAR(4)    = 'ASC'
AS
BEGIN
    SET NOCOUNT ON;

    -- whitelist identifiers (can't be parameterised)
    IF @SortCol NOT IN ('Name', 'City', 'CustomerId') THROW 50020, 'Invalid sort column.', 1;
    IF @SortDir NOT IN ('ASC', 'DESC')                THROW 50021, 'Invalid sort direction.', 1;

    DECLARE @sql NVARCHAR(MAX) = N'SELECT CustomerId, Name, City FROM dbo.Customers WHERE 1 = 1';

    IF @Name IS NOT NULL SET @sql += N' AND Name LIKE @pName';
    IF @City IS NOT NULL SET @sql += N' AND City = @pCity';

    SET @sql += N' ORDER BY ' + QUOTENAME(@SortCol) + N' ' + @SortDir;

    -- EXEC parameters must be variables/constants, not expressions → prepare the LIKE pattern first
    DECLARE @NameLike NVARCHAR(102) = @Name + N'%';

    EXEC sys.sp_executesql
         @sql,
         N'@pName NVARCHAR(102), @pCity NVARCHAR(50)',
         @pName = @NameLike,
         @pCity = @City;
END
GO
EXEC dbo.usp_SearchCustomers @City = 'Pune', @SortCol = 'Name', @SortDir = 'DESC';
```

| `EXEC(@sql)` | `sp_executesql` |
|---|---|
| No parameters → injection risk | Parameterised → safe |
| New plan for each different string | Plan reused |

**Rules:** values → parameters; identifiers → whitelist + `QUOTENAME()`.

## 7.11 Performance topics around SPs

### Plan caching & parameter sniffing
- First execution compiles a plan using the **first parameter values** ("sniffing") and caches it.
- If data is skewed (e.g., `@City='Pune'` = 1M rows, `@City='Goa'` = 5 rows), a plan good for one is terrible for the other → "SP is fast sometimes, slow sometimes".

**Fixes:**
| Fix | Effect |
|-----|--------|
| `OPTION (RECOMPILE)` on the statement | Fresh plan every run (CPU cost) |
| `OPTION (OPTIMIZE FOR (@City = 'Pune'))` | Plan for a typical value |
| `OPTION (OPTIMIZE FOR UNKNOWN)` / local variable copy | Plan based on average stats |
| `WITH RECOMPILE` on the procedure | Recompile whole SP every time |
| Split into separate procedures per case | Each gets its own plan |
| `EXEC sp_recompile 'dbo.usp_X'` | Clear its cached plan once |

### Other best practices checklist
- `SET NOCOUNT ON` at start
- Schema-qualify objects (`dbo.Table`)
- Avoid `SELECT *`
- Avoid cursors/loops – use set-based logic
- Keep transactions short; validate before `BEGIN TRAN`
- Use `SET XACT_ABORT ON` with explicit transactions
- Don't use `sp_` prefix
- Avoid functions on indexed columns in WHERE (non-SARGable)
- Return status codes or THROW; never swallow errors
- Use TVPs instead of comma-separated strings

## 7.12 Cursors (know them, but avoid)

```sql
DECLARE @Id INT, @Name NVARCHAR(100);
DECLARE c CURSOR LOCAL FAST_FORWARD FOR
    SELECT EmpId, Name FROM dbo.Employees WHERE DeptId = 1;
OPEN c;
FETCH NEXT FROM c INTO @Id, @Name;
WHILE @@FETCH_STATUS = 0
BEGIN
    PRINT CONCAT(@Id, ' - ', @Name);
    FETCH NEXT FROM c INTO @Id, @Name;
END
CLOSE c; DEALLOCATE c;
```
**Why avoid?** Row-by-row ("RBAR" – Row By Agonizing Row) is slow; set-based UPDATE does the same in one shot.
**When OK?** Calling another SP per row, admin scripts, ordered processing that can't be set-based.

## 7.13 Managing procedures

```sql
EXEC sp_helptext 'dbo.usp_TransferFunds';           -- view source
SELECT OBJECT_DEFINITION(OBJECT_ID('dbo.usp_TransferFunds'));
GO
CREATE OR ALTER PROCEDURE dbo.usp_Secret WITH ENCRYPTION AS SELECT 1;  -- hide source (CREATE must start its batch)
GO
EXEC sp_rename 'dbo.usp_Secret', 'usp_Secret2';
DROP PROCEDURE IF EXISTS dbo.usp_Secret2;
-- GRANT EXECUTE ON dbo.usp_TransferFunds TO AppUser;  -- user can run SP but not touch tables
--                                                      (needs an existing database user AppUser)
```

## 7.14 Calling an SP from C# (they may ask)

```csharp
using var conn = new SqlConnection(cs);
using var cmd  = new SqlCommand("dbo.usp_TransferFunds", conn) { CommandType = CommandType.StoredProcedure };
cmd.Parameters.Add("@FromAcc", SqlDbType.Int).Value = 101;
cmd.Parameters.Add("@ToAcc",   SqlDbType.Int).Value = 102;
cmd.Parameters.Add("@Amount",  SqlDbType.Decimal).Value = 500m;
var ret = cmd.Parameters.Add("@ret", SqlDbType.Int);
ret.Direction = ParameterDirection.ReturnValue;
await conn.OpenAsync();
await cmd.ExecuteNonQueryAsync();   // ExecuteReader for result sets, ExecuteScalar for single value
```

## Try it yourself (write without looking!)
1. `usp_AddEmployee` – inputs Name, DeptId, Salary; validate dept exists & salary > 0; return new EmpId via OUTPUT (EmpId = MAX+1 inside a transaction with `UPDLOCK, HOLDLOCK`).
2. `usp_GetTopNPerDept @N INT` returning top N earners per department.
3. `usp_TransferFunds` from memory – then explain each line aloud.
4. What's printed?
   ```sql
   BEGIN TRAN; BEGIN TRAN; PRINT @@TRANCOUNT; COMMIT; PRINT @@TRANCOUNT; ROLLBACK; PRINT @@TRANCOUNT;
   ```

<details><summary>Answers</summary>

```sql
-- 1
CREATE OR ALTER PROCEDURE dbo.usp_AddEmployee
    @Name NVARCHAR(100), @DeptId INT, @Salary DECIMAL(10,2), @EmpId INT OUTPUT
AS
BEGIN
    SET NOCOUNT ON; SET XACT_ABORT ON;
    IF @Salary <= 0 THROW 50030, 'Salary must be positive.', 1;
    IF NOT EXISTS (SELECT 1 FROM dbo.Departments WHERE DeptId = @DeptId)
        THROW 50031, 'Department not found.', 1;
    BEGIN TRY
        BEGIN TRAN;
        SELECT @EmpId = ISNULL(MAX(EmpId), 0) + 1 FROM dbo.Employees WITH (UPDLOCK, HOLDLOCK);
        INSERT INTO dbo.Employees (EmpId, Name, DeptId, Salary, HireDate)
        VALUES (@EmpId, @Name, @DeptId, @Salary, CAST(GETDATE() AS DATE));
        COMMIT;
    END TRY
    BEGIN CATCH
        IF @@TRANCOUNT > 0 ROLLBACK;
        THROW;
    END CATCH
END
GO

-- 2
CREATE OR ALTER PROCEDURE dbo.usp_GetTopNPerDept @N INT = 3
AS
BEGIN
    SET NOCOUNT ON;
    WITH r AS (SELECT e.*, DENSE_RANK() OVER (PARTITION BY DeptId ORDER BY Salary DESC) rk
               FROM dbo.Employees e)
    SELECT * FROM r WHERE rk <= @N ORDER BY DeptId, rk;
END
```
4 → prints `2`, `1`, `0`.
</details>


---

# 08. Functions, Views, Triggers, Temp Tables

## 8.1 User-Defined Functions (UDF)

### a) Scalar function – returns one value
```sql
CREATE OR ALTER FUNCTION dbo.fn_AnnualSalary (@Monthly DECIMAL(10,2))
RETURNS DECIMAL(12,2)
AS
BEGIN
    RETURN @Monthly * 12;
END
GO
SELECT Name, dbo.fn_AnnualSalary(Salary) AS Annual FROM dbo.Employees;
```
> ⚠️ Scalar UDFs in SELECT/WHERE run **per row** and historically blocked parallelism → slow on big tables. (SQL 2019+ can "inline" simple ones.)

### b) Inline Table-Valued Function (iTVF) – returns a table, single SELECT ✅ fast
```sql
CREATE OR ALTER FUNCTION dbo.fn_EmployeesByDept (@DeptId INT)
RETURNS TABLE
AS
RETURN (
    SELECT EmpId, Name, Salary FROM dbo.Employees WHERE DeptId = @DeptId
);
GO
SELECT * FROM dbo.fn_EmployeesByDept(1);

-- with APPLY: top earner for each department
SELECT d.DeptName, t.Name, t.Salary
FROM dbo.Departments d
CROSS APPLY (SELECT TOP 1 * FROM dbo.fn_EmployeesByDept(d.DeptId) ORDER BY Salary DESC) t;
```
iTVF behaves like a **parameterised view** – optimizer expands it inline.

### c) Multi-statement TVF – table variable built over many statements (slower)
```sql
CREATE OR ALTER FUNCTION dbo.fn_SalaryBands()
RETURNS @t TABLE (Band VARCHAR(10), Cnt INT)
AS
BEGIN
    INSERT @t SELECT 'High', COUNT(*) FROM dbo.Employees WHERE Salary >= 90000;
    INSERT @t SELECT 'Low',  COUNT(*) FROM dbo.Employees WHERE Salary <  90000;
    RETURN;
END
```

### Function restrictions
- ❌ No INSERT/UPDATE/DELETE on real tables
- ❌ No TRY/CATCH, no transactions
- ❌ No `EXEC` of procedures, no dynamic SQL
- ❌ No non-deterministic side effects (e.g. `NEWID()` directly in older versions)
- ✅ Can be used in SELECT, WHERE, JOIN, CHECK constraints, computed columns

## 8.2 Stored Procedure vs Function (top 5 interview question)

| Point | Stored Procedure | Function |
|-------|------------------|----------|
| Return | 0..n result sets, OUTPUT params, INT return code | Must return a scalar or a table |
| Use in SELECT/WHERE/JOIN | ❌ | ✅ |
| DML on tables | ✅ | ❌ |
| Transactions / TRY-CATCH | ✅ | ❌ |
| Call the other | SP can call function | Function **cannot** call SP |
| Parameters | Input + Output | Input only |
| How to call | `EXEC` | Inside a query |
| Purpose | Business process / actions | Calculations / reusable expressions |

## 8.3 Views

A **view = saved SELECT** that behaves like a virtual table. No data stored (except indexed views).

```sql
CREATE OR ALTER VIEW dbo.vw_EmployeeDetails
AS
SELECT e.EmpId, e.Name, d.DeptName, m.Name AS Manager, e.Salary
FROM dbo.Employees e
LEFT JOIN dbo.Departments d ON d.DeptId = e.DeptId
LEFT JOIN dbo.Employees  m ON m.EmpId  = e.ManagerId;
GO
SELECT * FROM dbo.vw_EmployeeDetails WHERE DeptName = 'IT';
```

**Why use views?**
- Hide complex joins (simplicity)
- **Security** – expose only some columns/rows (e.g., hide Salary)
- Backward compatibility when tables change

**Can you UPDATE through a view?** Yes, if the change affects only **one base table** and the view has no aggregates/DISTINCT/GROUP BY. `WITH CHECK OPTION` prevents updates that would make the row disappear from the view.

### Indexed (materialized) view – data physically stored
```sql
CREATE VIEW dbo.vw_SalesByCustomer WITH SCHEMABINDING AS
SELECT CustomerId, SUM(Amount) AS Total, COUNT_BIG(*) AS Cnt
FROM dbo.Orders GROUP BY CustomerId;
GO
CREATE UNIQUE CLUSTERED INDEX IX_vwSales ON dbo.vw_SalesByCustomer(CustomerId);
```
Fast reads, but every write to Orders also updates the view.

## 8.4 Triggers

A **trigger** is a special SP that **runs automatically** on an event.

| Type | Fires on |
|------|----------|
| DML `AFTER` (FOR) | After INSERT/UPDATE/DELETE succeeds |
| DML `INSTEAD OF` | Replaces the action (common on views) |
| DDL | CREATE/ALTER/DROP (e.g., audit schema changes) |
| LOGON | User login |

Magic tables inside DML triggers:

| Operation | `inserted` | `deleted` |
|-----------|-----------|-----------|
| INSERT | new rows | empty |
| DELETE | empty | old rows |
| UPDATE | new values | old values |

```sql
-- Audit salary changes (handles MULTI-ROW updates - common mistake is assuming 1 row!)
CREATE TABLE dbo.SalaryHistory (EmpId INT, OldSalary DECIMAL(10,2), NewSalary DECIMAL(10,2),
                                ChangedOn DATETIME2 DEFAULT SYSUTCDATETIME());
GO
CREATE OR ALTER TRIGGER dbo.trg_Employees_SalaryAudit
ON dbo.Employees
AFTER UPDATE
AS
BEGIN
    SET NOCOUNT ON;
    IF NOT UPDATE(Salary) RETURN;

    INSERT INTO dbo.SalaryHistory (EmpId, OldSalary, NewSalary)
    SELECT i.EmpId, d.Salary, i.Salary
    FROM inserted i
    JOIN deleted  d ON d.EmpId = i.EmpId
    WHERE i.Salary <> d.Salary;
END
GO

-- Prevent deletes on Departments
CREATE OR ALTER TRIGGER dbo.trg_Departments_NoDelete
ON dbo.Departments INSTEAD OF DELETE
AS
BEGIN
    THROW 50100, 'Deleting departments is not allowed.', 1;
END
```

**Trigger cautions (say these in interview):**
- Hidden logic → surprises & debugging pain
- Runs inside the caller's transaction → slows writes, can cause deadlocks
- Must handle **multiple rows** (never `SELECT @id = id FROM inserted`)
- Prefer constraints / SP logic where possible

## 8.5 Temp table vs Table variable vs CTE

| | `#Temp` | `##GlobalTemp` | `@TableVar` | CTE |
|---|---|---|---|---|
| Stored in | tempdb | tempdb | tempdb (mostly) | Not stored |
| Scope | Session / SP & children | All sessions | Batch / SP only | Single statement |
| Statistics | ✅ yes | ✅ | ❌ (row estimate 1, better in 2019+) | – |
| Indexes | Any | Any | PK/UNIQUE/inline only | – |
| Affected by ROLLBACK | ✅ | ✅ | ❌ (data survives!) | – |
| Causes recompiles | Can | Can | Fewer | – |
| Best for | Large intermediate sets | Rare | Small sets (< ~1000 rows) | Readability / recursion |

```sql
CREATE TABLE #Top (EmpId INT PRIMARY KEY, Salary DECIMAL(10,2));
INSERT #Top SELECT EmpId, Salary FROM dbo.Employees WHERE Salary > 80000;
SELECT * FROM #Top;
DROP TABLE IF EXISTS #Top;

DECLARE @t TABLE (Id INT PRIMARY KEY);
INSERT @t VALUES (1),(2);
SELECT * FROM @t;
```

## 8.6 Synonyms & Sequences (quick)
```sql
CREATE SEQUENCE dbo.seq_Invoice START WITH 1000 INCREMENT BY 1;
SELECT NEXT VALUE FOR dbo.seq_Invoice;        -- shared counter across tables (vs IDENTITY per table)

CREATE SYNONYM dbo.Emp FOR OtherDb.dbo.Employees;   -- alias to an object
```

## Try it yourself
1. Write an iTVF `fn_OrdersBetween(@From DATE, @To DATE)`.
2. Create a view showing customer name + total spend; can you UPDATE it? Why/why not?
3. Write a trigger that logs every DELETE on `Orders` into `AuditLog` (multi-row safe).

<details><summary>Answers</summary>

```sql
CREATE OR ALTER FUNCTION dbo.fn_OrdersBetween(@From DATE, @To DATE)
RETURNS TABLE AS RETURN
  (SELECT * FROM dbo.Orders WHERE OrderDate >= @From AND OrderDate < DATEADD(DAY,1,@To));
GO

-- 2: Not updatable - it has an aggregate (SUM) + GROUP BY.

CREATE OR ALTER TRIGGER dbo.trg_Orders_Delete ON dbo.Orders AFTER DELETE AS
BEGIN
  SET NOCOUNT ON;
  INSERT dbo.AuditLog (TableName, Action, KeyValue)
  SELECT 'Orders', 'DELETE', d.OrderId FROM deleted d;
END
```
</details>


---

# 09. Transactions, Locking & Isolation

## 9.1 Transaction = all-or-nothing unit of work

Example: transfer ₹500 → debit A **and** credit B. If credit fails, debit must be undone.

```sql
BEGIN TRANSACTION;
    UPDATE Accounts SET Balance -= 500 WHERE AccId = 101;
    UPDATE Accounts SET Balance += 500 WHERE AccId = 102;
COMMIT TRANSACTION;      -- or ROLLBACK TRANSACTION to undo
```

### Transaction modes
- **Autocommit** (default) – every statement is its own transaction
- **Explicit** – you write `BEGIN TRAN ... COMMIT/ROLLBACK`
- **Implicit** – `SET IMPLICIT_TRANSACTIONS ON`; a transaction starts automatically, you must COMMIT

## 9.2 ACID (memorise with an example)

| Letter | Meaning | Bank example |
|--------|---------|-------------|
| **A**tomicity | All or nothing | Debit + credit both happen or neither |
| **C**onsistency | DB moves from one valid state to another (constraints hold) | Balance never negative (CHECK) |
| **I**solation | Concurrent transactions don't see each other's half-done work | Others don't see money "in flight" |
| **D**urability | Once committed, survives crash | Written to transaction log first (WAL) |

## 9.3 Concurrency problems

| Problem | Simple meaning | Example |
|---------|----------------|---------|
| **Dirty read** | Read data another transaction hasn't committed (may be rolled back) | T1 updates salary to 1M, T2 reads 1M, T1 rolls back |
| **Non-repeatable read** | Same row read twice gives different values | T1 reads 100, T2 updates to 200 & commits, T1 reads 200 |
| **Phantom read** | Same query returns **new rows** second time | T1 counts 10 rows, T2 inserts 1, T1 counts 11 |
| **Lost update** | Two updates overwrite each other | Both read 100, both add 10 → 110 instead of 120 |

## 9.4 Isolation levels

| Level | Dirty | Non-repeatable | Phantom | How |
|-------|-------|----------------|---------|-----|
| READ UNCOMMITTED (`NOLOCK`) | ✅ possible | ✅ | ✅ | No shared locks |
| **READ COMMITTED** (default) | ❌ | ✅ | ✅ | Shared locks released after read |
| REPEATABLE READ | ❌ | ❌ | ✅ | Shared locks held till end |
| SERIALIZABLE | ❌ | ❌ | ❌ | Range locks |
| SNAPSHOT | ❌ | ❌ | ❌ | Row versions in tempdb, readers don't block writers |
| READ COMMITTED SNAPSHOT (RCSI) | ❌ | ✅ | ✅ | Statement-level versioning (default in Azure SQL DB) |

```sql
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
BEGIN TRAN;
  SELECT Balance FROM Accounts WHERE AccId = 101;
  -- ... same value guaranteed if re-read
COMMIT;
```

### NOLOCK – why it's dangerous
`SELECT * FROM Orders WITH (NOLOCK)` can return: uncommitted data, **duplicate rows**, **missing rows** (during page splits). OK for rough dashboards, **never** for money/business decisions. Better: enable RCSI/SNAPSHOT.

## 9.5 Locks (basics)

| Lock | Taken by | Compatible with |
|------|----------|----------------|
| Shared (S) | Reads | Other S |
| Exclusive (X) | Writes | Nothing |
| Update (U) | "Read with intent to update" (`UPDLOCK`) | S, not other U → prevents conversion deadlocks |
| Intent (IS/IX) | On higher level (page/table) to signal lower locks | – |

**Granularity:** Row → Page → Table. **Lock escalation:** ~5000 locks on one object → SQL escalates to a table lock.

**Blocking** = T2 waits for T1's lock (normal, temporary).
**Deadlock** = T1 waits for T2 and T2 waits for T1 → SQL Server kills one (**victim**, error **1205**).

### Deadlock example
```
T1: UPDATE Accounts WHERE AccId=101   (locks 101)
T2: UPDATE Accounts WHERE AccId=102   (locks 102)
T1: UPDATE Accounts WHERE AccId=102   (waits for T2)
T2: UPDATE Accounts WHERE AccId=101   (waits for T1)  → DEADLOCK
```

### How to prevent / handle deadlocks
1. Access objects in the **same order** everywhere (e.g., always lower AccId first)
2. Keep transactions **short**; no user interaction inside a transaction
3. Proper **indexes** (fewer rows scanned = fewer locks)
4. Use `UPDLOCK` when you read-then-update
5. Use RCSI/SNAPSHOT for read-heavy workloads
6. **Retry** on error 1205 in the app (or in SP with a loop)
7. Investigate with `system_health` Extended Event / deadlock graph

```sql
-- Retry pattern
DECLARE @retry INT = 3;
WHILE @retry > 0
BEGIN
    BEGIN TRY
        EXEC dbo.usp_TransferFunds 101, 102, 100;
        BREAK;
    END TRY
    BEGIN CATCH
        IF ERROR_NUMBER() = 1205 AND @retry > 1
        BEGIN SET @retry -= 1; WAITFOR DELAY '00:00:00.200'; END
        ELSE THROW;
    END CATCH
END
```

## 9.6 Optimistic vs Pessimistic concurrency

| Pessimistic | Optimistic |
|-------------|-----------|
| Lock first, then update (`UPDLOCK`) | Don't lock; check nothing changed at update time |
| Good for high contention | Good for low contention, web apps |

```sql
-- Optimistic with rowversion
ALTER TABLE Products ADD RowVer ROWVERSION;   -- auto-changes on every update of the row
GO
-- 1. App reads the row AND its version
DECLARE @ver BINARY(8);
SELECT @ver = RowVer FROM Products WHERE ProductId = 2;

-- 2. ...user edits on screen; meanwhile someone else may update the row (RowVer changes)...

-- 3. Save only if the version is still the same
UPDATE Products SET Price = 900
WHERE ProductId = 2 AND RowVer = @ver;
IF @@ROWCOUNT = 0 THROW 50200, 'Row was changed by someone else. Reload and retry.', 1;
```

## Try it yourself
1. Two sessions both run "read stock=10, then set stock = stock-1" → what problem? Fix it two ways.
2. Which isolation level would you choose for a financial report that must be consistent but shouldn't block writers?

<details><summary>Answers</summary>

1. Lost update. Fix: (a) single atomic `UPDATE Stock SET Qty = Qty - 1 WHERE Id=@id AND Qty > 0`; (b) `SELECT ... WITH (UPDLOCK)` in a transaction, or optimistic rowversion check.
2. SNAPSHOT isolation – consistent point-in-time view, readers don't block writers.
</details>


---

# 10. Indexes & Performance Tuning

## 10.1 What is an index? (simple words)

Like the **index at the back of a book**: instead of reading all 500 pages to find "Trigger", you look up the index → page 214.
Without an index SQL reads **every row** (a *scan*). With an index it jumps straight to the rows (a *seek*).

Internally an index is a **B-Tree** (balanced tree): root → intermediate → leaf pages. Finding one row among millions takes only ~3–4 page reads.

## 10.2 Clustered vs Non-clustered

| | Clustered | Non-clustered |
|---|---|---|
| What it is | The **table itself**, physically sorted by the key | Separate structure with key + pointer to the row |
| How many | **1** per table | Up to 999 |
| Leaf level contains | Actual data rows | Key + row locator (clustered key or RID) |
| Default for | PRIMARY KEY | UNIQUE constraint |
| Analogy | Phone book sorted by surname | Book index at the back |
| Table without clustered index | called a **Heap** | – |

```sql
-- Orders already has a clustered PK (only 1 allowed), so demo on a heap copy
SELECT * INTO dbo.OrdersCopy FROM dbo.Orders;                            -- SELECT INTO creates a heap
CREATE CLUSTERED INDEX CIX_OrdersCopy_OrderDate ON dbo.OrdersCopy(OrderDate);
CREATE NONCLUSTERED INDEX IX_Orders_CustomerId ON dbo.Orders(CustomerId);
CREATE UNIQUE NONCLUSTERED INDEX UX_Customers_Email ON dbo.Customers(Email) WHERE Email IS NOT NULL; -- filtered; fails if duplicate emails exist (remove them first - Ch.06)
```

### Good clustered key = **NUSE**
**N**arrow, **U**nique, **S**tatic (never changes), **E**ver-increasing (INT IDENTITY). GUID (`NEWID()`) as clustered key → random inserts → page splits & fragmentation.

## 10.3 Covering index, INCLUDE, Key Lookup

```sql
SELECT OrderDate, Amount FROM dbo.Orders WHERE CustomerId = 1;
```
- With `IX_Orders_CustomerId (CustomerId)` → **Index Seek** finds rows, then **Key Lookup** to the clustered index to get OrderDate & Amount (one lookup per row – expensive for many rows).
- Fix with a **covering index**:

```sql
CREATE NONCLUSTERED INDEX IX_Orders_CustomerId_Cover
ON dbo.Orders(CustomerId)           -- key columns: used for searching/sorting
INCLUDE (OrderDate, Amount);        -- included columns: only stored at leaf, just for returning
```

**Key vs INCLUDE:** columns in WHERE/JOIN/ORDER BY → key. Columns only in SELECT → INCLUDE.

### Composite index column order matters
Index on `(LastName, FirstName)` works for:
- `WHERE LastName = 'X'` ✅
- `WHERE LastName = 'X' AND FirstName = 'Y'` ✅
- `WHERE FirstName = 'Y'` ❌ (can't seek – like searching a phone book by first name)

**Rule:** equality columns first, then range columns, most selective first among equals.

## 10.4 Other index types (know names)
- **Filtered index** – index only some rows (`WHERE IsActive = 1`)
- **Columnstore** – column-wise storage, great for analytics/aggregations on huge tables
- **Unique index** – enforces uniqueness
- **Full-text index** – word searches in large text

## 10.5 Cost of indexes
- Every INSERT/UPDATE/DELETE must update **every** index → slower writes
- Uses disk/memory
- Too many overlapping indexes = waste
> "Indexes speed up reads, slow down writes. I index based on actual query patterns, check missing-index DMVs and remove unused ones."

## 10.6 SARGability – write index-friendly queries

SARG = **S**earch **ARG**ument-able: can SQL use an index seek?

| ❌ Non-SARGable | ✅ SARGable rewrite |
|----------------|--------------------|
| `WHERE YEAR(OrderDate) = 2026` | `WHERE OrderDate >= '2026-01-01' AND OrderDate < '2027-01-01'` |
| `WHERE Salary * 12 > 1200000` | `WHERE Salary > 100000` |
| `WHERE LEFT(Name, 1) = 'A'` | `WHERE Name LIKE 'A%'` |
| `WHERE Name LIKE '%an'` | (can't seek; consider full-text / reverse column) |
| `WHERE ISNULL(DeptId, 0) = 1` | `WHERE DeptId = 1` |
| `WHERE CAST(OrderDate AS VARCHAR) = '...'` | compare as DATE |
| `WHERE VarcharCol = N'abc'` (implicit conversion) | match data types: `'abc'` |
| `WHERE A = @x OR B = @y` | `UNION` of two seeks (sometimes) |

**Golden rule:** never wrap the **column** in a function; transform the **value** instead.

## 10.7 Reading an execution plan

Get it: **Ctrl+M** (actual plan) in SSMS, or `SET STATISTICS IO, TIME ON;`

| Operator | Meaning | Good/Bad |
|----------|---------|----------|
| Index **Seek** | Jump to rows | ✅ usually good |
| Index/Table **Scan** | Read everything | ⚠️ bad on large tables (OK on tiny ones) |
| **Key Lookup** / RID Lookup | Fetch extra columns per row | ⚠️ many rows → add INCLUDE |
| **Nested Loops** join | For each outer row, look in inner | Good for small outer input |
| **Hash Match** join | Builds hash table | Good for large unsorted inputs; memory heavy |
| **Merge** join | Both inputs sorted | Very efficient when sorted |
| **Sort** | Expensive, may spill to tempdb | ⚠️ consider index for ORDER BY |
| Yellow ⚠️ warning | Implicit conversion, missing stats, spill | Investigate |
| **Estimated vs Actual rows** huge difference | Bad statistics / sniffing | Update stats |

```sql
SET STATISTICS IO, TIME ON;
SELECT * FROM dbo.Orders WHERE CustomerId = 1;
-- Messages tab: "logical reads 2" ← lower is better; compare before/after tuning
```

## 10.8 Statistics
Statistics = histogram of data distribution; the optimizer uses them to **estimate rows** and choose a plan. Outdated stats → bad estimates → bad plans.
```sql
UPDATE STATISTICS dbo.Orders;
EXEC sp_updatestats;
```

## 10.9 Fragmentation & maintenance
- Inserts in random order → **page splits** → fragmented index.
- `sys.dm_db_index_physical_stats` shows fragmentation.
- Common rule: 5–30% → `ALTER INDEX ... REORGANIZE`; > 30% → `ALTER INDEX ... REBUILD`.
- **Fill factor** leaves free space in pages to reduce splits.

## 10.10 "This query/SP is slow – what do you do?" (guaranteed question)

Answer in steps:
1. **Reproduce** & measure: `SET STATISTICS IO, TIME ON`, note logical reads & duration.
2. **Actual execution plan**: look for scans on big tables, key lookups, sorts, estimated vs actual rows mismatch, warnings.
3. **Query rewrite**: make predicates SARGable, avoid `SELECT *`, remove unnecessary DISTINCT/ORDER BY, replace cursors with set-based, `EXISTS` instead of `COUNT(*) > 0`, avoid scalar UDFs in large SELECTs.
4. **Indexing**: add/adjust covering indexes; check missing-index DMVs (validate them, don't blindly create).
5. **Statistics**: update if stale.
6. **Parameter sniffing**: fast in SSMS but slow from app? Try `OPTION (RECOMPILE)` / `OPTIMIZE FOR`.
7. **Blocking/locking**: check `sys.dm_exec_requests` (blocking_session_id), `sp_who2`, wait stats.
8. **Data volume**: archive old data, paging, partitioning.
9. **Verify** improvement with the same metrics.

### Useful DMVs
```sql
-- Top CPU queries
SELECT TOP 10 qs.total_worker_time/qs.execution_count AS AvgCPU, qs.execution_count,
       SUBSTRING(st.text, qs.statement_start_offset/2+1, 200) AS QueryText
FROM sys.dm_exec_query_stats qs CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) st
ORDER BY AvgCPU DESC;

-- Who is blocking whom
SELECT session_id, blocking_session_id, wait_type, wait_time, command
FROM sys.dm_exec_requests WHERE blocking_session_id <> 0;

-- Missing index suggestions
SELECT * FROM sys.dm_db_missing_index_details;

-- Unused indexes
SELECT OBJECT_NAME(i.object_id) Tbl, i.name, s.user_seeks, s.user_scans, s.user_updates
FROM sys.indexes i LEFT JOIN sys.dm_db_index_usage_stats s
  ON s.object_id = i.object_id AND s.index_id = i.index_id AND s.database_id = DB_ID()
WHERE OBJECTPROPERTY(i.object_id,'IsUserTable') = 1;
```

Also: **Query Store** (SQL 2016+) – history of plans and regressions; can **force** a good plan.

## 10.11 Quick performance tips list
- `EXISTS` > `COUNT(*)` for existence checks
- `UNION ALL` > `UNION` when duplicates impossible
- Avoid `SELECT *`
- Avoid functions on columns in WHERE/JOIN
- Avoid implicit conversions (match types, `VARCHAR` vs `NVARCHAR`)
- Batch large updates/deletes
- Use appropriate data types (smaller = more rows per page)
- Keep transactions short
- Index foreign key columns (joins + avoids locking on parent delete)

## Try it yourself
1. Query `SELECT Name FROM Employees WHERE DeptId = 1 ORDER BY Salary DESC` – design the best index.
2. Rewrite SARGably: `WHERE DATEDIFF(DAY, OrderDate, GETDATE()) <= 30`
3. Plan shows Index Seek + Key Lookup with 50,000 executions. Fix?

<details><summary>Answers</summary>

1. `CREATE INDEX IX_Emp_Dept_Salary ON Employees(DeptId, Salary DESC) INCLUDE (Name);`
2. `WHERE OrderDate >= DATEADD(DAY, -30, CAST(GETDATE() AS DATE))`
3. Add the looked-up columns to the index's `INCLUDE` list (covering index).
</details>


---

# 11. Normalization & Design

## 11.1 Why normalize?
To remove **redundancy** and avoid **anomalies**:
- **Insert anomaly** – can't add a department until it has an employee
- **Update anomaly** – department name stored in 100 rows; update 99, miss 1 → inconsistent
- **Delete anomaly** – delete last employee → department info lost

## 11.2 Walk-through example

### Unnormalized
| OrderId | Customer | CustomerCity | Products |
|---|---|---|---|
| 1 | Asha | Pune | Laptop, Mouse |

### 1NF – atomic values, no repeating groups
| OrderId | Customer | CustomerCity | Product |
|---|---|---|---|
| 1 | Asha | Pune | Laptop |
| 1 | Asha | Pune | Mouse |
Key = (OrderId, Product).

### 2NF – 1NF + no **partial dependency** (non-key column depends on whole composite key)
`Customer` depends only on `OrderId` (part of the key) → move out:
- Orders(OrderId, Customer, CustomerCity)
- OrderItems(OrderId, Product)

### 3NF – 2NF + no **transitive dependency** (non-key → non-key)
`CustomerCity` depends on `Customer`, not on `OrderId` → move out:
- Customers(CustomerId, Name, City)
- Orders(OrderId, CustomerId)
- OrderItems(OrderId, ProductId, Qty)
- Products(ProductId, Name, Price)

> **One-liner:** "Every non-key column must depend on **the key, the whole key, and nothing but the key**."

### BCNF (bonus)
Every determinant must be a candidate key. A stricter 3NF; rarely asked beyond the definition.

## 11.3 Denormalization
Deliberately adding redundancy for **read performance** (reports, dashboards): e.g., storing `OrderTotal` in Orders instead of summing OrderItems each time.
Trade-off: faster reads, more storage, must keep copies in sync (triggers/jobs/app logic).

**OLTP** (transactions, many small writes) → normalized.
**OLAP / data warehouse** (analytics) → denormalized **star schema** (fact table + dimension tables).

## 11.4 Keys glossary

| Key | Meaning |
|-----|---------|
| Super key | Any set of columns that uniquely identifies a row |
| Candidate key | Minimal super key (no extra columns) |
| Primary key | The chosen candidate key |
| Alternate key | Candidate keys not chosen (enforce with UNIQUE) |
| Composite key | Key with 2+ columns |
| Foreign key | References another table's key |
| Surrogate key | Artificial ID (IDENTITY, GUID) |
| Natural key | Real-world value (PAN, Email) |

**Surrogate vs natural:** surrogate is narrow, stable, never changes → better clustered key. Still put a UNIQUE constraint on the natural key.

## 11.5 Relationships

| Type | Example | How |
|------|---------|-----|
| One-to-One | Employee ↔ EmployeePassport | FK + UNIQUE on FK column |
| One-to-Many | Department → Employees | FK on the "many" side |
| Many-to-Many | Students ↔ Courses | **Junction table** Enrollments(StudentId, CourseId) |

## 11.6 Design question practice (common for 2 YOE)

**"Design tables for a library system."**
```
Members(MemberId PK, Name, Email UNIQUE, JoinedOn)
Authors(AuthorId PK, Name)
Books(BookId PK, Title, ISBN UNIQUE, PublishedYear)
BookAuthors(BookId FK, AuthorId FK, PK(BookId, AuthorId))        -- many-to-many
BookCopies(CopyId PK, BookId FK, Status CHECK IN ('Available','Issued','Lost'))
Loans(LoanId PK, CopyId FK, MemberId FK, IssuedOn, DueOn, ReturnedOn NULL, Fine DECIMAL)
Index: Loans(MemberId), Loans(CopyId) WHERE ReturnedOn IS NULL (filtered - active loans)
```
Talk about: PKs, FKs, constraints, junction table, indexes on FKs, soft delete vs hard delete, audit columns (CreatedOn, ModifiedBy).

## Try it yourself
Normalize to 3NF: `StudentCourse(StudentId, StudentName, CourseId, CourseName, InstructorName, InstructorPhone, Grade)`

<details><summary>Answer</summary>

- Students(StudentId, StudentName)
- Instructors(InstructorId, Name, Phone)
- Courses(CourseId, CourseName, InstructorId FK)
- Enrollments(StudentId FK, CourseId FK, Grade, PK(StudentId, CourseId))
</details>


---

# 12. 30 Must-Practise Interview Queries

> Use the Chapter 00 database. Try each one **yourself first** (cover the answer), then compare.
> ⭐ = very frequently asked.

---

### Level 1 – Warm-up

**1. ⭐ Second highest salary**
```sql
-- Way 1: subquery
SELECT MAX(Salary) FROM Employees WHERE Salary < (SELECT MAX(Salary) FROM Employees);
-- Way 2: OFFSET
SELECT DISTINCT Salary FROM Employees ORDER BY Salary DESC OFFSET 1 ROW FETCH NEXT 1 ROW ONLY;
-- Way 3: DENSE_RANK (best: generalises to Nth & handles ties)
SELECT DISTINCT Salary FROM (SELECT Salary, DENSE_RANK() OVER (ORDER BY Salary DESC) r FROM Employees) x WHERE r = 2;
```

**2. ⭐ Nth highest salary (as a reusable function)**
```sql
CREATE OR ALTER FUNCTION dbo.fn_NthSalary(@N INT) RETURNS TABLE AS RETURN
  (SELECT DISTINCT Salary FROM (SELECT Salary, DENSE_RANK() OVER (ORDER BY Salary DESC) r FROM dbo.Employees) x WHERE r = @N);
GO
SELECT * FROM dbo.fn_NthSalary(3);
```

**3. Employees with no department**
```sql
SELECT * FROM Employees WHERE DeptId IS NULL;
```

**4. Count employees per department name (include empty departments)**
```sql
SELECT d.DeptName, COUNT(e.EmpId) AS Cnt
FROM Departments d LEFT JOIN Employees e ON e.DeptId = d.DeptId
GROUP BY d.DeptName;
```

**5. ⭐ Find duplicate emails**
```sql
SELECT Email, COUNT(*) AS Cnt FROM Employees
WHERE Email IS NOT NULL
GROUP BY Email HAVING COUNT(*) > 1;
```

**6. Employees hired in 2021**
```sql
SELECT * FROM Employees WHERE HireDate >= '2021-01-01' AND HireDate < '2022-01-01';  -- SARGable
```

---

### Level 2 – Joins & grouping

**7. ⭐ Highest paid employee in each department**
```sql
WITH r AS (SELECT *, RANK() OVER (PARTITION BY DeptId ORDER BY Salary DESC) rk FROM Employees WHERE DeptId IS NOT NULL)
SELECT DeptId, Name, Salary FROM r WHERE rk = 1;

-- without window functions
SELECT e.DeptId, e.Name, e.Salary
FROM Employees e
JOIN (SELECT DeptId, MAX(Salary) MaxSal FROM Employees GROUP BY DeptId) m
  ON m.DeptId = e.DeptId AND m.MaxSal = e.Salary;
```

**8. ⭐ Employees earning more than their manager**
```sql
SELECT e.Name, e.Salary, m.Name AS Manager, m.Salary AS MgrSal
FROM Employees e JOIN Employees m ON m.EmpId = e.ManagerId
WHERE e.Salary > m.Salary;
```

**9. ⭐ Customers who never placed an order**
```sql
SELECT c.* FROM Customers c
WHERE NOT EXISTS (SELECT 1 FROM Orders o WHERE o.CustomerId = c.CustomerId);
```

**10. Departments where average salary > company average**
```sql
SELECT DeptId, AVG(Salary) AvgSal FROM Employees
GROUP BY DeptId
HAVING AVG(Salary) > (SELECT AVG(Salary) FROM Employees);
```

**11. Managers with number of direct reports (only those with ≥ 2)**
```sql
SELECT m.Name, COUNT(*) AS Reports
FROM Employees e JOIN Employees m ON m.EmpId = e.ManagerId
GROUP BY m.EmpId, m.Name
HAVING COUNT(*) >= 2;
```

**12. Products never sold**
```sql
SELECT p.* FROM Products p
WHERE NOT EXISTS (SELECT 1 FROM OrderItems oi WHERE oi.ProductId = p.ProductId);
```

**13. Total revenue per product category, highest first**
```sql
SELECT p.Category, SUM(oi.Qty * oi.UnitPrice) Revenue
FROM OrderItems oi JOIN Products p ON p.ProductId = oi.ProductId
GROUP BY p.Category ORDER BY Revenue DESC;
```

**14. Customers who bought from **both** Electronics and Furniture**
```sql
SELECT o.CustomerId
FROM Orders o
JOIN OrderItems oi ON oi.OrderId = o.OrderId
JOIN Products p    ON p.ProductId = oi.ProductId
WHERE p.Category IN ('Electronics', 'Furniture')
GROUP BY o.CustomerId
HAVING COUNT(DISTINCT p.Category) = 2;
```

**15. Customers who bought **every** product in Furniture (relational division)**
```sql
SELECT o.CustomerId
FROM Orders o
JOIN OrderItems oi ON oi.OrderId = o.OrderId
JOIN Products p    ON p.ProductId = oi.ProductId AND p.Category = 'Furniture'
GROUP BY o.CustomerId
HAVING COUNT(DISTINCT p.ProductId) = (SELECT COUNT(*) FROM Products WHERE Category = 'Furniture');
```

---

### Level 3 – Window functions & CTEs

**16. ⭐ Top 2 salaries per department**
```sql
WITH r AS (SELECT *, DENSE_RANK() OVER (PARTITION BY DeptId ORDER BY Salary DESC) rk FROM Employees)
SELECT * FROM r WHERE rk <= 2;
```

**17. ⭐ Delete duplicate customers, keep the lowest Id**
```sql
WITH d AS (SELECT *, ROW_NUMBER() OVER (PARTITION BY Name, City, Email ORDER BY CustomerId) rn FROM Customers)
DELETE FROM d WHERE rn > 1;
```
(If a duplicate has orders, FK blocks delete → first re-point Orders to the kept Id.)

**18. ⭐ Running total of sales by date**
```sql
SELECT OrderDate, Amount,
       SUM(Amount) OVER (ORDER BY OrderDate, OrderId ROWS UNBOUNDED PRECEDING) AS RunningTotal
FROM Orders;
```

**19. Each customer's latest order**
```sql
WITH r AS (SELECT *, ROW_NUMBER() OVER (PARTITION BY CustomerId ORDER BY OrderDate DESC) rn FROM Orders)
SELECT * FROM r WHERE rn = 1;
```

**20. ⭐ Month-over-month revenue change**
```sql
WITH m AS (SELECT DATEFROMPARTS(YEAR(OrderDate), MONTH(OrderDate), 1) Mon, SUM(Amount) Rev
           FROM Orders GROUP BY DATEFROMPARTS(YEAR(OrderDate), MONTH(OrderDate), 1))
SELECT Mon, Rev, Rev - LAG(Rev) OVER (ORDER BY Mon) AS Change,
       CAST(100.0 * (Rev - LAG(Rev) OVER (ORDER BY Mon)) / NULLIF(LAG(Rev) OVER (ORDER BY Mon), 0) AS DECIMAL(8,2)) AS Pct
FROM m;
```

**21. Each employee's salary as % of department total**
```sql
SELECT Name, DeptId, Salary,
       CAST(100.0 * Salary / SUM(Salary) OVER (PARTITION BY DeptId) AS DECIMAL(5,2)) AS PctOfDept
FROM Employees;
```

**22. ⭐ Full org hierarchy with levels (recursive CTE)**
```sql
WITH Org AS (
  SELECT EmpId, Name, ManagerId, 0 Lvl FROM Employees WHERE ManagerId IS NULL
  UNION ALL
  SELECT e.EmpId, e.Name, e.ManagerId, o.Lvl + 1 FROM Employees e JOIN Org o ON e.ManagerId = o.EmpId)
SELECT REPLICATE('   ', Lvl) + Name AS Tree, Lvl FROM Org;
```

**23. All subordinates (direct + indirect) of Anil's report Gita (EmpId 7)**
```sql
WITH Sub AS (
  SELECT EmpId, Name FROM Employees WHERE ManagerId = 7
  UNION ALL
  SELECT e.EmpId, e.Name FROM Employees e JOIN Sub s ON e.ManagerId = s.EmpId)
SELECT * FROM Sub;
```

**24. Days between consecutive orders per customer**
```sql
SELECT CustomerId, OrderDate,
       DATEDIFF(DAY, LAG(OrderDate) OVER (PARTITION BY CustomerId ORDER BY OrderDate), OrderDate) AS DaysGap
FROM Orders;
```

---

### Level 4 – Tricky / senior-flavoured

**25. ⭐ Pivot: revenue per customer per month as columns**
```sql
SELECT CustomerId,
  SUM(CASE WHEN MONTH(OrderDate)=1 THEN Amount ELSE 0 END) Jan,
  SUM(CASE WHEN MONTH(OrderDate)=2 THEN Amount ELSE 0 END) Feb,
  SUM(CASE WHEN MONTH(OrderDate)=3 THEN Amount ELSE 0 END) Mar
FROM Orders GROUP BY CustomerId;
```

**26. Swap gender values M↔F in one UPDATE**
```sql
UPDATE People SET Gender = CASE Gender WHEN 'M' THEN 'F' WHEN 'F' THEN 'M' ELSE Gender END;
```

**27. ⭐ Consecutive numbers appearing 3+ times (LeetCode classic)**
```sql
-- Logs(Id, Num)
SELECT DISTINCT Num FROM (
  SELECT Num, LAG(Num,1) OVER (ORDER BY Id) p1, LAG(Num,2) OVER (ORDER BY Id) p2 FROM Logs) x
WHERE Num = p1 AND Num = p2;
```

**28. Gaps in an ID sequence (missing OrderIds)**
```sql
WITH x AS (SELECT OrderId, LEAD(OrderId) OVER (ORDER BY OrderId) NextId FROM Orders)
SELECT OrderId + 1 AS GapStart, NextId - 1 AS GapEnd FROM x WHERE NextId - OrderId > 1;
```

**29. Comma-separated string → rows, and rows → comma string**
```sql
SELECT value FROM STRING_SPLIT('Pune,Delhi,Mumbai', ',');
SELECT DeptId, STRING_AGG(Name, ', ') AS Members FROM Employees GROUP BY DeptId;
```

**30. ⭐ Cumulative distinct customers per month (new vs returning)**
```sql
WITH firstOrder AS (SELECT CustomerId, MIN(OrderDate) FirstDate FROM Orders GROUP BY CustomerId),
m AS (SELECT DISTINCT o.CustomerId, DATEFROMPARTS(YEAR(o.OrderDate), MONTH(o.OrderDate), 1) Mon,
             DATEFROMPARTS(YEAR(f.FirstDate), MONTH(f.FirstDate), 1) FirstMon
      FROM Orders o JOIN firstOrder f ON f.CustomerId = o.CustomerId)
SELECT Mon,
       SUM(CASE WHEN Mon = FirstMon THEN 1 ELSE 0 END) AS NewCustomers,
       SUM(CASE WHEN Mon > FirstMon THEN 1 ELSE 0 END) AS Returning
FROM m GROUP BY Mon ORDER BY Mon;
```

---

### Output-prediction puzzles (they love these)

| Query | Result | Why |
|-------|--------|-----|
| `SELECT NULL + 5` | NULL | Arithmetic with NULL |
| `SELECT 'A' + NULL` | NULL | (unless CONCAT_NULL_YIELDS_NULL OFF) |
| `SELECT CONCAT('A', NULL)` | `A` | CONCAT treats NULL as '' |
| `SELECT COUNT(*) FROM t` vs `COUNT(col)` on 5 rows with 2 NULLs | 5 vs 3 | COUNT(col) ignores NULL |
| `SELECT 5/2` | 2 | Integer division → use `5/2.0` |
| `SELECT AVG(x)` for 2, 4, NULL | 3 | NULL ignored |
| `WHERE col <> 'A'` on rows A, B, NULL | 1 row (B) | NULL compare → unknown |
| `SELECT LEN('abc  ')` | 3 | LEN ignores trailing spaces (DATALENGTH = 5) |
| `SELECT TOP 0 * INTO NewT FROM Employees` | Empty copy of structure | Common trick |


---

# 13. Theory Q&A – Rapid Fire

> Read these **aloud**. Aim for 1–3 sentence answers. Add an example whenever you can – it shows real understanding.

## A. Basics

**Q1. What is SQL vs MySQL vs SQL Server?**
SQL is the language. MySQL, SQL Server, PostgreSQL, Oracle are database products (RDBMS) that understand SQL. T-SQL is SQL Server's extension (variables, TRY/CATCH, procedures).

**Q2. DDL vs DML vs DCL vs TCL?** Structure / data / permissions / transactions (see Ch.01).

**Q3. DELETE vs TRUNCATE vs DROP?** DELETE removes chosen rows, logged per row, fires triggers. TRUNCATE removes all rows fast, resets identity, no WHERE, fails if FK references the table. DROP removes table structure. TRUNCATE can be rolled back in SQL Server inside a transaction.

**Q4. Primary key vs Unique key?** One PK per table, no NULLs, clustered by default. Many unique keys, one NULL allowed, non-clustered by default.

**Q5. What is a foreign key? Can it be NULL?** Column referencing another table's PK/unique key to enforce referential integrity. Yes, it can be NULL (optional relationship).

**Q6. CHAR vs VARCHAR vs NVARCHAR?** CHAR fixed length (padded), VARCHAR variable non-Unicode, NVARCHAR variable Unicode (2 bytes/char). Use NVARCHAR for multilingual data.

**Q7. What is NULL? How to check?** Unknown/missing value. Use `IS NULL` / `IS NOT NULL`, not `= NULL`. Handle with `ISNULL`/`COALESCE`.

**Q8. ISNULL vs COALESCE?** ISNULL is SQL-Server-only, 2 args, result type of first arg. COALESCE is ANSI, many args, returns first non-NULL, type by precedence.

## B. Querying

**Q9. WHERE vs HAVING?** WHERE filters rows before grouping (no aggregates). HAVING filters groups after GROUP BY (with aggregates).

**Q10. Logical order of SELECT?** FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → TOP. That's why aliases work in ORDER BY but not WHERE.

**Q11. Types of joins?** INNER, LEFT, RIGHT, FULL OUTER, CROSS, SELF. (Explain with a quick example.)

**Q12. UNION vs UNION ALL?** UNION removes duplicates (extra sort/hash, slower); UNION ALL keeps them (faster).

**Q13. IN vs EXISTS?** IN compares to a value list; EXISTS returns true at the first matching row. `NOT EXISTS` is NULL-safe; `NOT IN` returns nothing if the subquery has a NULL.

**Q14. Correlated vs non-correlated subquery?** Non-correlated runs independently once. Correlated references the outer query and logically runs per outer row.

**Q15. CTE vs temp table vs table variable?** CTE: named query for one statement, not stored, supports recursion. #Temp: in tempdb, has statistics and indexes, good for large sets. @TableVar: lightweight, minimal stats, good for small sets, not affected by rollback.

**Q16. RANK vs DENSE_RANK vs ROW_NUMBER?** ROW_NUMBER unique sequence; RANK gives ties same rank and leaves gaps (1,1,3); DENSE_RANK no gaps (1,1,2).

**Q17. What are window functions?** Functions using `OVER()` that calculate across related rows without collapsing them – ranking, running totals, LAG/LEAD.

**Q18. GROUP BY vs PARTITION BY?** GROUP BY returns one row per group; PARTITION BY (in OVER) keeps all rows and computes per group.

## C. Stored procedures & programmability

**Q19. What is a stored procedure and its advantages?** Precompiled, saved set of SQL statements. Benefits: reuse, plan caching, less network traffic, security via EXECUTE permission, SQL-injection protection with parameters, transaction control.

**Q20. Stored procedure vs function?** SP: can modify data, use transactions/TRY-CATCH, output params, called with EXEC, can't be used in SELECT. Function: must return value/table, no DML on tables, usable inside SELECT/WHERE/JOIN, can't call SPs.

**Q21. Types of functions?** Scalar, inline table-valued (fast, like parameterised view), multi-statement table-valued (slower), plus built-in aggregate/system functions.

**Q22. How do you handle errors in an SP?** TRY/CATCH; in CATCH check `@@TRANCOUNT`, rollback, log, and `THROW;` to re-raise. Use `SET XACT_ABORT ON` with transactions.

**Q23. THROW vs RAISERROR?** THROW is newer, simpler, re-throws original error, always severity 16, aborts batch. RAISERROR supports printf-style formatting and custom severity.

**Q24. What is SET NOCOUNT ON?** Stops "n rows affected" messages → less network traffic; good practice in every SP.

**Q25. What are OUTPUT parameters vs RETURN?** OUTPUT returns any data type values (many allowed). RETURN returns only an integer status code.

**Q26. SCOPE_IDENTITY vs @@IDENTITY vs IDENT_CURRENT?** SCOPE_IDENTITY: last identity in current scope & session (safest). @@IDENTITY: any scope in session – triggers can affect it. IDENT_CURRENT: last for a table across all sessions.

**Q27. What is parameter sniffing? How do you fix it?** SQL builds the cached plan for the first parameter values; it may be bad for other values. Fixes: OPTION (RECOMPILE), OPTIMIZE FOR, OPTIMIZE FOR UNKNOWN, local variables, separate procedures, Query Store plan forcing.

**Q28. How do you prevent SQL injection?** Parameterised queries / SPs; `sp_executesql` with parameters for dynamic SQL; whitelist + QUOTENAME for identifiers; least-privilege accounts.

**Q29. EXEC vs sp_executesql?** EXEC runs a string with no parameters (injection risk, poor plan reuse). sp_executesql supports parameters → safe and plan reuse.

**Q30. What is a trigger? Types? inserted/deleted?** Code that runs automatically on DML/DDL/logon events. AFTER and INSTEAD OF. `inserted` has new rows, `deleted` has old rows; UPDATE populates both. Must handle multiple rows.

**Q31. Disadvantages of triggers?** Hidden logic, harder debugging, adds to transaction time, can cause cascading/recursive effects and deadlocks.

**Q32. What is a view? Can you update a view? Indexed view?** Saved SELECT as virtual table for simplicity/security. Updatable if it touches one base table with no aggregates/DISTINCT. Indexed view = materialised with a unique clustered index (needs SCHEMABINDING).

**Q33. What is a cursor? Why avoid?** Row-by-row processing; slow vs set-based logic. Use only when unavoidable (e.g., calling an SP per row).

**Q34. Can we call an SP inside a SELECT?** No. You can call a function. (Workaround: `INSERT INTO #t EXEC sp`.)

**Q35. Can an SP return multiple result sets?** Yes – multiple SELECTs; client reads with `NextResult()`.

**Q36. What is a table-valued parameter?** A user-defined table type passed (READONLY) to an SP – efficient way to send a list of rows from the app instead of comma-separated strings.

## D. Transactions & concurrency

**Q37. What is a transaction / ACID?** Unit of work that is Atomic, Consistent, Isolated, Durable (give bank transfer example).

**Q38. Isolation levels?** Read Uncommitted, Read Committed (default), Repeatable Read, Serializable, Snapshot (+ RCSI). Higher = fewer anomalies, more blocking.

**Q39. Dirty / non-repeatable / phantom read?** Reading uncommitted data / same row changes between reads / new rows appear between reads.

**Q40. What is NOLOCK and its risks?** = READ UNCOMMITTED. Dirty reads, missing or duplicate rows. Avoid for critical data.

**Q41. Blocking vs deadlock?** Blocking: one waits for another's lock (resolves when lock released). Deadlock: circular wait; SQL Server kills a victim (error 1205).

**Q42. How to avoid deadlocks?** Same access order, short transactions, good indexes, UPDLOCK for read-then-write, snapshot isolation, retry logic.

**Q43. Are nested transactions real in SQL Server?** No. Inner COMMIT just decrements @@TRANCOUNT; ROLLBACK rolls back everything. Use SAVE TRANSACTION for partial rollback.

**Q44. What does SET XACT_ABORT ON do?** Any runtime error aborts and rolls back the whole transaction automatically – prevents open/half-done transactions.

## E. Indexes & performance

**Q45. What is an index?** B-tree structure to find rows quickly without scanning the table – like a book index.

**Q46. Clustered vs non-clustered?** Clustered = table data sorted by key, one per table. Non-clustered = separate structure with key + pointer, many per table.

**Q47. What is a covering index / INCLUDE?** An index containing all columns a query needs, so no key lookup is needed. INCLUDE adds non-key columns at leaf level.

**Q48. Disadvantages of indexes?** Slower INSERT/UPDATE/DELETE, extra storage, maintenance (fragmentation).

**Q49. Index seek vs scan?** Seek navigates the B-tree to specific rows; scan reads all rows/pages.

**Q50. What is SARGable?** A predicate that can use an index seek – no functions/calculations on the column.

**Q51. What is a heap?** A table without a clustered index.

**Q52. Composite index column order?** Leftmost prefix rule – the index is usable only if the query filters on the leading column(s).

**Q53. How do you optimise a slow query?** (Chapter 10.10 – 9 steps: measure, plan, rewrite, index, stats, sniffing, blocking, data volume, verify.)

**Q54. What are statistics?** Data distribution histograms the optimizer uses to estimate row counts and choose plans.

**Q55. What is an execution plan?** The steps SQL Server chooses to run a query (operators, joins, costs). Estimated vs actual.

**Q56. Fragmentation – reorganize vs rebuild?** Reorganize: online, light, for 5–30%. Rebuild: recreates index, for > 30%, updates stats.

## F. Design

**Q57. Normalization 1NF/2NF/3NF?** Atomic values / no partial dependency / no transitive dependency. "The key, the whole key and nothing but the key."

**Q58. Denormalization – when?** For read-heavy reporting where joins are too costly; trade-off is redundancy and sync effort.

**Q59. Surrogate vs natural key?** Surrogate: system-generated ID (stable, narrow). Natural: real-world value (may change). Prefer surrogate PK + unique on natural.

**Q60. OLTP vs OLAP?** OLTP: many small transactions, normalized. OLAP: analytics, large reads, denormalized star schema.

## G. Behavioural / experience (prepare your own answers!)

**Q61. Tell me about a complex SP you wrote.** → Use **STAR**: Situation, Task, Action (validation, transaction, TVP, error handling), Result (numbers!).

**Q62. Tell me about a performance issue you fixed.** → "Report took 40 s. Plan showed a scan + key lookup due to `YEAR(OrderDate)=…`. Rewrote as range predicate and added covering index → 0.3 s, logical reads 120k → 300."

**Q63. How do you deploy DB changes safely?** Scripts in source control, idempotent (`IF NOT EXISTS`, `CREATE OR ALTER`), backward-compatible changes, test on lower environment, backups/rollback plan.

**Q64. How do you debug a failing SP in production?** Reproduce with same params, check error logs/ERROR_* captured in log table, execution plan, Query Store, blocking DMVs.


---

# 14. Mock Interview & Cheat Sheet

## 14.1 Timed mock (45 minutes) – do this on Day 3

Set a timer. No looking at notes. Write on paper or a blank editor.

| # | Question | Time |
|---|----------|------|
| 1 | Third highest salary (handle ties) | 3 min |
| 2 | Department name + headcount + avg salary, only depts with > 2 employees | 4 min |
| 3 | Customers with no orders – write 2 different ways | 4 min |
| 4 | Employees earning more than their manager | 3 min |
| 5 | Delete duplicate rows keeping one | 4 min |
| 6 | Running total + previous order amount per customer | 5 min |
| 7 | Write `usp_TransferFunds` with validation, transaction, TRY/CATCH | 8 min |
| 8 | Explain: SP vs function (5 points) | 2 min |
| 9 | Explain: clustered vs non-clustered, covering index | 3 min |
| 10 | "A procedure is slow only when called from the app" – what's happening & how to fix? | 3 min |
| 11 | Explain isolation levels & deadlock prevention | 3 min |
| 12 | Normalize a given table to 3NF (use Ch.11 exercise) | 3 min |

**Scoring:** correct & explained = 2, mostly right = 1, wrong = 0. **18+/24 → you're ready.** Below that, re-read the chapters for the ones you missed.

<details><summary>Hint for Q10</summary>
Parameter sniffing (app sends different values than when the plan was cached) and/or different SET options (SSMS has ARITHABORT ON, ADO.NET OFF → different cached plans). Fix: RECOMPILE / OPTIMIZE FOR / update stats / Query Store plan forcing.
</details>

## 14.2 Prepare your 2 stories (write them now)

```
Story 1 – Complex SP / feature
  Situation:
  Task:
  Action (validation, transaction, error handling, TVP, indexes...):
  Result (with numbers):

Story 2 – Performance / production issue
  Situation:
  Problem found (plan, blocking, sniffing...):
  Fix:
  Result (before → after):
```

## 14.3 Interview-day technique

1. **Clarify** before writing: "Should ties be included? What if salary is NULL? Duplicates possible?"
2. **Think aloud** – explain logic in the order SQL runs it.
3. **Start simple**, then improve ("This works; for performance I'd add an index on …").
4. **Mention edge cases**: NULLs, ties, empty groups, divide-by-zero (`NULLIF`).
5. Don't know? "I haven't used X directly, but I'd approach it by … and verify with the execution plan."

---

## 14.4 ONE-PAGE CHEAT SHEET (read the night before)

### Query order
`FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → TOP`

### Patterns
```sql
-- Nth highest
SELECT DISTINCT Salary FROM (SELECT Salary, DENSE_RANK() OVER (ORDER BY Salary DESC) r FROM T) x WHERE r = @N;
-- Top N per group
WITH r AS (SELECT *, DENSE_RANK() OVER (PARTITION BY G ORDER BY V DESC) rk FROM T) SELECT * FROM r WHERE rk <= N;
-- Delete duplicates
WITH d AS (SELECT *, ROW_NUMBER() OVER (PARTITION BY Cols ORDER BY Id) rn FROM T) DELETE FROM d WHERE rn > 1;
-- No match (anti-join)
SELECT * FROM A WHERE NOT EXISTS (SELECT 1 FROM B WHERE B.AId = A.Id);
-- Running total / previous
SUM(x) OVER (PARTITION BY g ORDER BY d ROWS UNBOUNDED PRECEDING), LAG(x) OVER (PARTITION BY g ORDER BY d)
-- Hierarchy
WITH h AS (anchor UNION ALL SELECT ... FROM T JOIN h ON T.ParentId = h.Id) SELECT * FROM h;
-- Pivot
SUM(CASE WHEN col = 'X' THEN val ELSE 0 END) AS X
```

### Stored procedure template
```sql
CREATE OR ALTER PROCEDURE dbo.usp_Name
    @In INT, @Out INT OUTPUT
AS
BEGIN
    SET NOCOUNT ON; SET XACT_ABORT ON;
    IF @In <= 0 THROW 50001, 'Invalid input.', 1;     -- validate first
    BEGIN TRY
        BEGIN TRAN;
            -- work; check @@ROWCOUNT; SET @Out = SCOPE_IDENTITY();
        COMMIT;
    END TRY
    BEGIN CATCH
        IF @@TRANCOUNT > 0 ROLLBACK;
        THROW;
    END CATCH
END
```

### Key differences
| A | vs | B |
|---|---|---|
| WHERE (rows) | | HAVING (groups) |
| DELETE (rows, logged, triggers) | | TRUNCATE (all, fast, resets identity) |
| UNION (distinct) | | UNION ALL (faster) |
| RANK (1,1,3) | | DENSE_RANK (1,1,2) |
| SP (DML, tran, EXEC) | | Function (in SELECT, no DML) |
| Clustered (1, data sorted) | | Non-clustered (many, pointer) |
| #temp (stats, indexes) | | @table (small, survives rollback) |
| SCOPE_IDENTITY (safe) | | @@IDENTITY (trigger risk) |
| NOT EXISTS (NULL-safe) | | NOT IN (breaks on NULL) |
| THROW (modern, re-throw) | | RAISERROR (legacy, formatting) |
| sp_executesql (params) | | EXEC(@sql) (injection risk) |

### NULL rules
`= NULL` never true • aggregates ignore NULL • `COUNT(*)` counts all • `NULL + x = NULL` • `CONCAT` treats NULL as ''

### Performance checklist
Plan → scans/lookups → SARGable → covering index → stats → sniffing → blocking → verify with `STATISTICS IO`

### Isolation
RU (dirty) < RC default < RR < Serializable; Snapshot = versioning, no reader/writer blocking.

### Deadlock fixes
Same order • short tran • indexes • UPDLOCK • snapshot • retry on 1205

### Normal forms
1NF atomic • 2NF no partial dep • 3NF no transitive dep — "the key, the whole key, nothing but the key"

---

**All the best for your interview! 🚀**

