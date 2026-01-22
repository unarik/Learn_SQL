The **constraints and attributes** make SQL tables powerful. In SQL Server, when you define a table, you can attach various **column-level and table-level attributes** to enforce rules, defaults, and relationships. Let’s break them down clearly:

---

## 🔑 Common Attributes & Constraints in SQL Server

### 1. **NULL / NOT NULL**
- **NULL** → column can store missing values.
- **NOT NULL** → column must always have a value.
```sql
FirstName NVARCHAR(50) NOT NULL
MiddleName NVARCHAR(50) NULL
```

---

### 2. **DEFAULT**
- Provides a default value if none is supplied.
- Can use constants or functions.
```sql
HireDate DATE DEFAULT GETDATE()
Status NVARCHAR(20) DEFAULT 'Active'
```

---

### 3. **PRIMARY KEY**
- Uniquely identifies each row.
- Automatically creates a **unique index**.
```sql
EmployeeID INT PRIMARY KEY
```

---

### 4. **FOREIGN KEY**
- Enforces relationship between tables.
- Ensures values exist in the referenced table.
```sql
DepartmentID INT FOREIGN KEY REFERENCES Departments(DepartmentID)
```

---

### 5. **UNIQUE**
- Ensures all values in a column are distinct.
```sql
Email NVARCHAR(100) UNIQUE
```

---

### 6. **CHECK**
- Enforces a condition on values.
```sql
Salary DECIMAL(10,2) CHECK (Salary > 0)
Age INT CHECK (Age >= 18)
```

---

### 7. **IDENTITY**
- Auto-incrementing numeric column.
```sql
EmployeeID INT IDENTITY(1,1) PRIMARY KEY
```
*(starts at 1, increments by 1)*

---

### 8. **DEFAULT Functions**
- Common built-in functions used with defaults:
  - `GETDATE()` → current date/time
  - `NEWID()` → generates a unique GUID
  - `SYSDATETIME()` → more precise timestamp

---

### 9. **INDEX**
- Not part of `CREATE TABLE` directly, but often added:
```sql
CREATE INDEX idx_lastname ON Employees(LastName);
```

---

### 10. **Constraints at Table Level**
You can define constraints outside the column definition:
```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    Quantity INT,
    Price DECIMAL(10,2),
    CONSTRAINT chk_positive CHECK (Quantity > 0 AND Price > 0)
);
```

---

## 📋 Summary of Attributes
- **NULL / NOT NULL** → control missing values  
- **DEFAULT** → assign automatic values  
- **PRIMARY KEY** → unique row identifier  
- **FOREIGN KEY** → enforce relationships  
- **UNIQUE** → prevent duplicates  
- **CHECK** → enforce conditions  
- **IDENTITY** → auto-increment numbers  
- **INDEX** → improve performance  

---

👉 These are the **core attributes/constraints** you’ll use most often when designing SQL Server tables.