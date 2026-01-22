## 📚 Project Idea: **Library Management System**

---

### 🔹 Phase 1: Basics (DDL, DML, DQL)
- **Create Database & Tables (DDL)**  
  - `Books`, `Members`, `Loans`, `Authors`
- **Insert Data (DML)**  
  - Add sample books, members, and loan records.
- **Query Data (DQL)**  
  - Find all books by a certain author.  
  - List overdue loans.  
  - Count how many books each member borrowed.

---

### 🔹 Phase 2: Intermediate (Constraints, Joins, Views)
- Add **Primary Keys, Foreign Keys, Unique, Check constraints**.  
- Write queries with **INNER JOIN, LEFT JOIN** (e.g., members and their borrowed books).  
- Create **Views** (e.g., `ActiveLoansView`).

---

### 🔹 Phase 3: Advanced (Stored Procedures, Functions, Triggers)
- **Stored Procedure**: Issue a book loan automatically.  
- **Function**: Calculate late fees based on due date.  
- **Trigger**: Prevent issuing a book if stock is 0.

---

### 🔹 Phase 4: Security & Transactions (DCL, TCL)
- Use **GRANT/REVOKE** to control access (e.g., librarians vs. members).  
- Apply **COMMIT/ROLLBACK** when issuing or returning books to ensure consistency.

---

### 🔹 Phase 5: Optimization & Extras
- Add **Indexes** for faster searches.  
- Write **complex queries** (e.g., top 5 most borrowed books).  
- Explore **Normalization** (splitting tables properly).  
- Try **denormalization** for reporting.

---

## 🧩 Why This Project Works
- It’s **relatable** (everyone understands a library).  
- It touches **every SQL concept** progressively.  
- You can keep expanding it as you move into advanced topics.  
- Later, you can even connect it to a front-end app (Python, Java, or web) to see SQL in action.

---

**starter database schema** for the Library Management System.

---

## 📚 Entities and Relationships

### 1. **Members**
- Stores information about library members.
- **Columns**:
  - `MemberID` (PK)
  - `FirstName`
  - `LastName`
  - `Email`
  - `Phone`
  - `JoinDate`

---

### 2. **Authors**
- Stores information about book authors.
- **Columns**:
  - `AuthorID` (PK)
  - `FirstName`
  - `LastName`
  - `Country`

---

### 3. **Books**
- Stores information about books.
- **Columns**:
  - `BookID` (PK)
  - `Title`
  - `ISBN`
  - `PublishedYear`
  - `Genre`
  - `CopiesAvailable`
  - `AuthorID` (FK → Authors.AuthorID)

---

### 4. **Loans**
- Tracks which member borrowed which book.
- **Columns**:
  - `LoanID` (PK)
  - `MemberID` (FK → Members.MemberID)
  - `BookID` (FK → Books.BookID)
  - `LoanDate`
  - `DueDate`
  - `ReturnDate`

---

### 5. **Staff (Optional, for advanced features)**
- Stores librarian/staff details for managing the library.
- **Columns**:
  - `StaffID` (PK)
  - `FirstName`
  - `LastName`
  - `Role`

---

## 🔗 Relationships
- **Members ↔ Loans**: One member can have many loans.  
- **Books ↔ Loans**: One book can be borrowed many times.  
- **Authors ↔ Books**: One author can write many books.  
- **Staff ↔ Loans** (optional): Staff may issue/approve loans.

---

## 🧩 ER Diagram (Textual Sketch)

```
Authors (AuthorID PK) ───< Books (BookID PK, AuthorID FK)
Members (MemberID PK) ───< Loans (LoanID PK, MemberID FK, BookID FK) >─── Books
Staff (StaffID PK) ───< Loans (optional, StaffID FK)
```

---

**Library Management System** idea and expand it into a **medium-to-advanced level schema**. This version introduces more complexity, normalization, and advanced features you can practice with.

---

## 📚 Medium/Advanced Library Database Schema

### 1. **Members**
- `MemberID` (PK)  
- `FirstName`, `LastName`  
- `Email`, `Phone`  
- `AddressID` (FK → Addresses.AddressID)  
- `MembershipTypeID` (FK → MembershipTypes.MembershipTypeID)  
- `JoinDate`, `Status`

---

### 2. **Addresses** (Normalization)
- `AddressID` (PK)  
- `Street`, `City`, `State`, `PostalCode`, `Country`

---

### 3. **MembershipTypes**
- `MembershipTypeID` (PK)  
- `TypeName` (e.g., Regular, Premium, Student)  
- `MaxBooksAllowed`  
- `MembershipFee`

---

### 4. **Authors**
- `AuthorID` (PK)  
- `FirstName`, `LastName`  
- `Country`

---

### 5. **Publishers**
- `PublisherID` (PK)  
- `Name`, `Country`, `ContactInfo`

---

### 6. **Books**
- `BookID` (PK)  
- `Title`, `ISBN`, `PublishedYear`, `Genre`  
- `PublisherID` (FK → Publishers.PublisherID)  
- `CopiesTotal`, `CopiesAvailable`

---

### 7. **BookAuthors** (Many-to-Many between Books & Authors)
- `BookID` (FK → Books.BookID)  
- `AuthorID` (FK → Authors.AuthorID)  
- **Composite PK**: (`BookID`, `AuthorID`)

---

### 8. **Loans**
- `LoanID` (PK)  
- `MemberID` (FK → Members.MemberID)  
- `BookID` (FK → Books.BookID)  
- `LoanDate`, `DueDate`, `ReturnDate`  
- `StaffID` (FK → Staff.StaffID)

---

### 9. **Reservations** (Advanced Feature)
- `ReservationID` (PK)  
- `MemberID` (FK → Members.MemberID)  
- `BookID` (FK → Books.BookID)  
- `ReservationDate`  
- `Status` (Pending, Fulfilled, Cancelled)

---

### 10. **Staff**
- `StaffID` (PK)  
- `FirstName`, `LastName`  
- `Role` (Librarian, Admin, Assistant)  
- `HireDate`

---

### 11. **Fines** (Advanced Feature)
- `FineID` (PK)  
- `LoanID` (FK → Loans.LoanID)  
- `Amount`  
- `PaidStatus`  
- `PaymentDate`

---

## 🔗 Relationships
- **Members ↔ Loans**: One-to-many.  
- **Books ↔ Loans**: One-to-many.  
- **Books ↔ Authors**: Many-to-many via `BookAuthors`.  
- **Books ↔ Publishers**: Many-to-one.  
- **Members ↔ Reservations**: One-to-many.  
- **Loans ↔ Fines**: One-to-one (each loan may generate a fine).  
- **Staff ↔ Loans**: One-to-many (staff issue loans).  
- **Members ↔ MembershipTypes**: Many-to-one.  
- **Members ↔ Addresses**: Many-to-one.

---

## 🧩 ER Diagram (Textual Sketch)

```
Members (MemberID PK, MembershipTypeID FK, AddressID FK)
   ├──< Loans (LoanID PK, MemberID FK, BookID FK, StaffID FK) >── Books (BookID PK, PublisherID FK)
   │       └── Fines (FineID PK, LoanID FK)
   └──< Reservations (ReservationID PK, MemberID FK, BookID FK)

Books ───< BookAuthors (BookID FK, AuthorID FK) >── Authors
Books ───< PublisherID FK >── Publishers
Members ───< MembershipTypeID FK >── MembershipTypes
Members ───< AddressID FK >── Addresses
Loans ───< StaffID FK >── Staff
```

---

## 🎯 Why This Schema is Advanced
- Introduces **many-to-many relationships** (`BookAuthors`).  
- Adds **normalization** (Addresses, MembershipTypes).  
- Includes **business rules** (Reservations, Fines).  
- Allows you to practice **transactions, triggers, stored procedures, and security**.  
- Provides real-world complexity for advanced SQL queries (e.g., “Find members with unpaid fines who reserved books by a certain author”).

---