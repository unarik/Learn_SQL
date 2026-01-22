#### Tech Words
- syntax, indentation, parser, attributes

## 📘 Notes & Definitions

### 1. Introduction to SQL
- **SQL (Structured Query Language)** is the standard language used to interact with relational databases.  
- It allows you to **store, retrieve, manipulate, and manage data** efficiently.  
- SQL is supported by many systems: Microsoft SQL Server (SSMS), MySQL, PostgreSQL, Oracle, etc.  

---

### 2. SQL Architecture
- **Client Layer**: Where users or applications send queries (e.g., SSMS).  
- **SQL Engine**: The core component that interprets queries, optimizes them, and executes them.  
  - **Parser**: Checks syntax.  
  - **Optimizer**: Finds the best way to execute queries.  
  - **Executor**: Runs the query.  
- **Storage Layer**: Where actual data resides in tables, indexes, and files.  

---

### 3. Main Operations (CRUD)
CRUD represents the four fundamental operations in SQL:
- **Create** → `INSERT INTO` adds new records.  
- **Read** → `SELECT` retrieves data.  
- **Update** → `UPDATE` modifies existing records.  
- **Delete** → `DELETE` removes records.  

Example:
```sql
-- Create
INSERT INTO Customers (Name, Age) VALUES ('Alice', 30);

-- Read
SELECT * FROM Customers;

-- Update
UPDATE Customers SET Age = 31 WHERE Name = 'Alice';

-- Delete
DELETE FROM Customers WHERE Name = 'Alice';
```

---

### 4. Database and Tables Creation
- **Create Database**:
```sql
CREATE DATABASE SalesDB;
```
- **Create Table**:
```sql
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    Name NVARCHAR(100),
    Age INT,
    Email NVARCHAR(100)
);
```
- Tables are the **basic storage units** in SQL, holding rows (records) and columns (fields).

---

## 📂 Showing Databases and Tables
- **List all databases**:
  ```sql
  SELECT name FROM sys.databases;
  ```
  Or in SSMS Object Explorer, expand the **Databases** node.
- **List all tables in a database**:
  ```sql
  SELECT * FROM sys.tables;
  ```
  Or expand **Databases → YourDB → Tables** in SSMS.

---

## 🏗️ CREATE TABLE Syntax
Basic syntax in SQL Server:
```sql
CREATE TABLE schema_name.table_name (
    column1 datatype [NULL | NOT NULL] [PRIMARY KEY],
    column2 datatype [NULL | NOT NULL],
    column3 datatype [NULL | NOT NULL] DEFAULT default_value,
    ...
);
```

### Example:
```sql
CREATE TABLE dbo.Employees (
    EmployeeID INT PRIMARY KEY,
    FirstName NVARCHAR(50) NOT NULL,
    LastName NVARCHAR(50) NOT NULL,
    HireDate DATE DEFAULT GETDATE(),
    Salary DECIMAL(10,2) CHECK (Salary > 0)
);
```

---

## 🔢 SQL Server Data Types & Attributes
SQL Server supports multiple categories:

| Category        | Examples | Notes |
|-----------------|----------|-------|
| **Numeric**     | `INT`, `BIGINT`, `DECIMAL(p,s)`, `FLOAT` | For integers, decimals, floating-point |
| **Character**   | `CHAR(n)`, `NCHAR(n)`, `VARCHAR(n)`, `NVARCHAR(n)` | `N` prefix = Unicode |
| **Date/Time**   | `DATE`, `DATETIME`, `DATETIME2`, `TIME` | Precise date/time storage |
| **Binary**      | `VARBINARY`, `IMAGE` (deprecated) | For files, blobs |
| **Other**       | `BIT`, `UNIQUEIDENTIFIER`, `XML`, `JSON` (via NVARCHAR) | Special use cases |

### INT vs BIGINT
 - INT: 4 bytes, range: –2,147,483,648 to 2,147,483,647.
 - BIGINT: 8 bytes, range: –9,223,372,036,854,775,808 to 9,223,372,036,854,775,807.
 - Use INT for most IDs and counts; BIGINT when values can exceed 2 billion.

*DECIMAL(p, s)* : Fixed precision and scale, p = total digits, s = digits after decimal.

### CHAR(n), VARCHAR(n), NVARCHAR(n)
 - CHAR(n): Fixed length. Always stores n characters (pads with spaces).
 - VARCHAR(n): Variable length. Stores only actual characters.
 - NVARCHAR(n): Like VARCHAR, but supports Unicode (international characters).

### DATE, DATETIME, DATETIME2, TIME
 - DATE: Only date (YYYY-MM-DD). -- '2026-01-20'
 - DATETIME: Date + time, accuracy ~3.33 ms, range 1753–9999. -- '2026-01-20 14:27:00.123'
 - DATETIME2: Improved DATETIME, accuracy 100 ns, larger range. -- '2026-01-20 14:27:00.1234567'
 - TIME: Only time of day. -- '14:27:00'

### VARBINARY vs IMAGE
 - VARBINARY(n): Variable-length binary data (up to 2 GB with VARBINARY(MAX)).
 - IMAGE: Deprecated, older type for large binary objects. Use VARBINARY(MAX) instead.

### ⚙️ Other Special Types
 - BIT: Boolean-like: 0, 1, or NULL.
 - UNIQUEIDENTIFIER: Stores a GUID (Globally Unique Identifier).
 - XML: Stores XML data, can be queried with XQuery.
 - JSON: SQL Server doesn’t have a native JSON type; JSON is stored in NVARCHAR(MAX).

 ```sql
   UserID UNIQUEIDENTIFIER DEFAULT NEWID()
 ```

**SQL Server provides functions (OPENJSON, JSON_VALUE) to query JSON text.**

```sql
   --OPENJSON--
   DECLARE @json NVARCHAR(MAX) = N'{
      "id": 1,
      "name": "Alice",
      "skills": ["SQL", "Python"]
   }';

   SELECT * 
   FROM OPENJSON(@json);
   
   --OR--

   SELECT id, name
   FROM OPENJSON(@json)
   WITH (
      id INT,
      name NVARCHAR(50)
   );

   --JSON_VALUE--
   DECLARE @json NVARCHAR(MAX) = N'{
      "id": 1,
      "name": "Alice",
      "skills": ["SQL", "Python"]
   }';

   SELECT JSON_VALUE(@json, '$.name') AS Name,
          JSON_VALUE(@json, '$.skills[0]') AS FirstSkill;

```

_Result_

| key    | value            | type       |
|--------|------------------|------------|
| id     | 1                | 2 (int)    |
| name   | Alice            | 1 (string) |
| skills | ["SQL","Python"] | 5 (array)  |

**Attributes you can apply:**
- `NOT NULL` → column must have a value  
- `DEFAULT` → assign default value if none provided  
- `PRIMARY KEY` → uniquely identifies rows  
- `FOREIGN KEY` → enforces relationship with another table  
- `CHECK` → enforces condition (e.g., `Salary > 0`)  

You can check detailed explanation of Attributes and Constraints [here.](./attributes-and-constraints.md)

---

## 📝 Naming Rules & Conventions
SQL Server naming rules:
- **Databases, tables, columns:**
  - Must start with a letter or underscore.
  - Can include letters, numbers, and underscores.
  - Cannot use reserved keywords (e.g., `SELECT`, `TABLE`).
  - Max length: **128 characters**.
- **Best Practices:**
  - Use **PascalCase** or **snake_case** consistently (`EmployeeID`, `employee_id`).
  - Avoid spaces; if needed, wrap in brackets (`[Employee Name]`).
  - Prefix tables with schema (`dbo.Employees`).
  - Use meaningful names (avoid `temp1`, `new_table`).
  - Plural vs singular: choose one style and stick to it (e.g., `Employees` vs `Employee`).

---

## 📑 Full Topic List for SQL Learning (Recommended Path)

Here’s a structured roadmap you can compare with your own list:

1. [**Introduction to SQL**](#1-introduction-to-sql)
   - What is SQL, history, importance
   - Relational database concepts

2. [**SQL Architecture**](#2-sql-architecture)
   - Client-server model
   - SQL engine components
   - Storage and indexing basics

3. [**Basic Operations**](#3-main-operations-crud)
   - CRUD (Create, Read, Update, Delete)
   - Filtering with `WHERE`
   - Sorting with `ORDER BY`
   - Limiting results (`TOP`, `LIMIT`)

4. [**Database & Table Management**](#4-database-and-tables-creation)
   - Creating databases and tables
   - Data types
   - Constraints (PRIMARY KEY, FOREIGN KEY, UNIQUE, CHECK, DEFAULT)

5. **Joins & Relationships**
   - INNER, LEFT, RIGHT, FULL joins
   - Normalization and relationships

6. **Advanced Queries**
   - GROUP BY, HAVING
   - Aggregate functions (SUM, COUNT, AVG, MIN, MAX)
   - Subqueries

7. **Indexes & Performance**
   - Clustered vs non-clustered indexes
   - Query optimization basics

8. **Views & Stored Procedures**
   - Creating views
   - Writing stored procedures and functions

9. **Transactions & Security**
   - COMMIT, ROLLBACK
   - ACID properties
   - User roles and permissions

10. **Backup & Restore**
    - BACKUP DATABASE
    - RESTORE DATABASE

11. **Advanced Topics (Optional)**
    - Triggers
    - Window functions
    - CTEs (Common Table Expressions)

---

✅ This roadmap ensures you cover **fundamentals first** (intro, CRUD, tables) and then progress to **intermediate and advanced topics** like joins, transactions, and optimization.  

Would you like me to turn this into a **step-by-step study plan with exercises** (like practice queries for each topic) so you can learn by doing in SSMS?
