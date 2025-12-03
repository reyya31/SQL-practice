# SQL Practice Workspace

## Overview

This repository contains a comprehensive **SQL learning and practice environment** designed to help you master fundamental SQL concepts through interactive Jupyter notebooks. The workspace demonstrates essential SQL operations using SQLite as the database backend.

---

## Workspace Structure

```
SQL/
├── README.md                 # This file - documentation and setup guide
├── requirements.txt          # Python dependencies
├── sqlpractice.ipynb        # Main interactive SQL practice notebook
└── newdb.db                 # SQLite database (generated at runtime)
```

---

## Project Components

### 1. **sqlpractice.ipynb** - Main Practice Notebook

This is the core learning resource containing interactive SQL examples and demonstrations.

#### Architecture Overview:

The notebook is structured as a **step-by-step SQL learning module** that progresses from basic to intermediate SQL concepts:

```
┌─────────────────────────────────────────────────────────┐
│           SQL Practice Notebook Structure              │
├─────────────────────────────────────────────────────────┤
│ 1. Setup & Initialization                              │
│    └─ Load SQL extension & connect to SQLite DB         │
├─────────────────────────────────────────────────────────┤
│ 2. Data Foundation                                      │
│    └─ Create employees table with sample data           │
├─────────────────────────────────────────────────────────┤
│ 3. Basic Queries (SELECT, WHERE, ORDER BY)             │
│    └─ Retrieve and filter data from table              │
├─────────────────────────────────────────────────────────┤
│ 4. Logical Operators (AND, OR, NOT)                    │
│    └─ Combine multiple conditions                       │
├─────────────────────────────────────────────────────────┤
│ 5. Data Manipulation (INSERT, UPDATE, DELETE)          │
│    └─ Modify table records                             │
├─────────────────────────────────────────────────────────┤
│ 6. NULL Handling                                        │
│    └─ Work with missing values                         │
├─────────────────────────────────────────────────────────┤
│ 7. Advanced Queries (LIMIT, LIKE, IN, BETWEEN)         │
│    └─ Filter data with pattern matching & ranges       │
├─────────────────────────────────────────────────────────┤
│ 8. Aggregate Functions (SUM, AVG, COUNT, MIN, MAX)     │
│    └─ Perform calculations on data                     │
├─────────────────────────────────────────────────────────┤
│ 9. Column Aliasing                                      │
│    └─ Rename columns and tables for readability         │
└─────────────────────────────────────────────────────────┘
```

---

## Code Flow & Implementation Details

### Phase 1: Initialization

**Cells 1-2: Environment Setup**
```
1. Load SQL Extension
   └─ %load_ext sql

2. Connect to SQLite Database
   └─ %sql sqlite:///newdb.db
      (Creates/connects to newdb.db file)
```

**Purpose:** Enables Jupyter to execute SQL commands directly using the `%%sql` magic command.

---

### Phase 2: Data Foundation

**Cells 3-4: Table Creation & Population**

**Cell 3 - Create Table:**
```sql
DROP TABLE employees;           -- Remove existing table if present
CREATE TABLE employees (        -- Create fresh table
    id INT PRIMARY KEY,         -- Unique identifier
    name VARCHAR(50),           -- Employee name (up to 50 chars)
    salary DECIMAL(10,2)        -- Salary with 2 decimal places
);
```

**Cell 4 - Insert Sample Data:**
```sql
INSERT INTO employees VALUES 
(1, 'Riya', 55000),
(2, 'John', 60000),
(3, 'Alice', 50000),
(4, 'Bishal', 32000),
(5, 'Kabita', 45000);
```

**Purpose:** Establishes a consistent test dataset for all subsequent queries.

---

### Phase 3: Basic Retrieval Operations

**SELECT Operation (Cells 5-6)**
- **Concept:** Retrieve all data from a table
- **Query:** `SELECT * FROM employees`
- **Output:** Returns all 5 employee records with all columns

**WHERE Operation (Cells 7-8)**
- **Concept:** Filter rows based on conditions
- **Query:** `SELECT * FROM employees WHERE salary > 55000;`
- **Output:** Returns employees with salary exceeding 55000 (John with 60000)

---

### Phase 4: Result Ordering

**ORDER BY (Cells 9-10)**
- **Concept:** Sort results in ascending (ASC) or descending (DESC) order
- **Query:** `SELECT * FROM employees ORDER BY salary DESC;`
- **Output:** Employees sorted from highest to lowest salary (John→Riya→Alice→Kabita→Bishal)

---

### Phase 5: Logical Operators

**AND Operation (Cells 11-12)**
- **Concept:** Returns rows where BOTH conditions are true
- **Query:** `SELECT * FROM employees WHERE salary > 50000 AND id = 1;`
- **Output:** Only Riya (id=1 with salary=55000)

**OR Operation (Cells 13-14)**
- **Concept:** Returns rows where AT LEAST ONE condition is true
- **Query:** `SELECT * FROM employees WHERE salary = 55000 OR salary = 60000;`
- **Output:** Riya and John

**NOT Operation (Cells 15-16)**
- **Concept:** Negates a condition
- **Query:** `SELECT * FROM employees WHERE NOT salary = 55000;`
- **Output:** All employees except Riya

---

### Phase 6: Data Modification (CRUD Operations)

**INSERT Operations (Cells 17-20)**

1. **Basic INSERT (Cell 18):**
   - Adds duplicate record (Alice with id=3)
   - May cause conflicts if id is unique

2. **INSERT OR IGNORE (Cell 20):**
   - Ignores duplicate insertion
   - Prevents PRIMARY KEY violations

3. **INSERT OR REPLACE (Cell 22):**
   - Replaces existing record with same primary key
   - Useful for upsert operations

4. **INSERT New Record (Cell 24):**
   - Adds new employee (Aman with id=6)
   - No conflict as id is unique

**UPDATE Operation (Cells 25-26)**
- **Cell 25:** Updates specific record
  ```sql
  UPDATE employees
  SET name = 'Alice', salary = 50000
  WHERE id = 3;
  ```
- **Cell 27:** Modifies salary for employee with id=4 (Bishal: 35000)

**DELETE Operation (Cell 28)**
- Removes employee with id=5 (Kabita)
- Uses WHERE clause to target specific record

---

### Phase 7: NULL Handling

**IS NULL / IS NOT NULL (Cells 29-32)**
- **Concept:** Identify missing values in database
- **Query 1:** `SELECT * FROM employees WHERE salary IS NULL;`
- **Query 2:** `SELECT * FROM employees WHERE salary IS NOT NULL ORDER BY id ASC;`
- **Purpose:** Handle incomplete data records

---

### Phase 8: Advanced Filtering Techniques

**LIMIT / SELECT TOP (Cells 33-34)**
- Retrieves top N records
- SQLite uses LIMIT instead of TOP
- `SELECT * FROM employees LIMIT 3;` → Returns first 3 employees

**LIKE Pattern Matching (Cells 35-40)**
- `LIKE 'A%'` → Names starting with 'A'
- `LIKE '%a'` → Names ending with 'a'
- `LIKE '_____'` → Names with exactly 5 characters

**WILDCARDS (Cell 37-38)**
- `%` → Zero or more characters
- `_` → Exactly one character

**IN Operator (Cells 41-42)**
- Selects rows matching any value in a list
- `SELECT * FROM employees WHERE salary IN (32000, 50000);`
- Returns Bishal and Alice

**BETWEEN Operator (Cells 43-44)**
- Selects rows within a range (inclusive)
- `SELECT * FROM employees WHERE salary BETWEEN 40000 AND 60000;`
- Returns Alice, Riya, John, and Kabita

---

### Phase 9: Aggregate Functions

**Purpose:** Calculate statistics across multiple rows

**COUNT (Cells 45-48)**
```sql
SELECT COUNT(*) FROM employees;              -- Total: 5 employees
SELECT COUNT(*) FROM employees WHERE salary > 45000;  -- Count: 3
```

**SUM (Cells 49-50)**
- `SELECT SUM(salary) FROM employees;` → Total salary sum

**AVG (Cells 51-52)**
- `SELECT AVG(salary) FROM employees;` → Average salary

**MIN & MAX (Cells 45-46)**
```sql
SELECT MIN(salary) AS lowest_salary,
       MAX(salary) AS highest_salary
FROM employees;
```
- Returns minimum and maximum salary values

---

### Phase 10: Column Aliasing

**Column Aliases (Cells 53-54)**
```sql
SELECT name AS employee_name,
       salary AS monthly_salary
FROM employees;
```
- Renames columns for better readability

**Table Aliases (Cells 55-56)**
```sql
SELECT e.name, e.salary
FROM employees AS e;
```
- Shortens table references in queries
- Useful for complex queries with multiple tables

---

## Database Schema

### Employees Table

| Column | Type | Constraints | Purpose |
|--------|------|-------------|---------|
| id | INT | PRIMARY KEY | Unique identifier for each employee |
| name | VARCHAR(50) | NOT NULL | Employee's full name |
| salary | DECIMAL(10,2) | | Annual salary (max 10 digits, 2 decimals) |

**Sample Data:**
```
id | name    | salary
1  | Riya    | 55000
2  | John    | 60000
3  | Alice   | 50000
4  | Bishal  | 32000
5  | Kabita  | 45000
```

---

## Setup & Installation

### Prerequisites

- Python 3.7 or higher
- Jupyter Lab or Jupyter Notebook

### Step 1: Install Dependencies

```bash
pip install -r requirements.txt
```

**Required Packages:**
- `jupyterlab` - Interactive notebook environment
- `ipython-sql` - SQL magic commands for Jupyter
- `sqlalchemy` - SQL toolkit and ORM
- `pandas` - Data manipulation library
- `numpy` - Numerical computing

### Step 2: Launch Jupyter

```bash
jupyter lab
```

Or if using Jupyter Notebook:

```bash
jupyter notebook
```

### Step 3: Open Notebook

Navigate to `sqlpractice.ipynb` and open it.

### Step 4: Run Cells Sequentially

Start from the top and execute cells in order using `Shift + Enter`.

---

## Running in GitHub Codespaces

### Initial Setup

1. **Open in Codespace:**
   - Click "Code" → "Codespaces" → "Create codespace on main"

2. **Install Dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Start Jupyter:**
   ```bash
   jupyter lab --ip=0.0.0.0 --allow-root
   ```

4. **Access Jupyter:**
   - VS Code will prompt you to open Jupyter Lab
   - If not, forward port 8888 and visit the provided URL

---

## Learning Path

### Beginner Level
1. Run Setup cells (1-4)
2. Execute SELECT and WHERE operations (5-8)
3. Try ORDER BY examples (9-10)
4. Experiment with logical operators (11-16)

### Intermediate Level
5. Practice INSERT, UPDATE, DELETE (17-28)
6. Work with NULL handling (29-32)
7. Explore LIMIT, LIKE, BETWEEN operators (33-44)

### Advanced Level
8. Master aggregate functions (45-52)
9. Use aliases for complex queries (53-56)

---

## Key SQL Concepts Covered

| Concept | Purpose | Example |
|---------|---------|---------|
| **SELECT** | Retrieve data | `SELECT * FROM employees;` |
| **WHERE** | Filter rows | `WHERE salary > 50000` |
| **ORDER BY** | Sort results | `ORDER BY salary DESC` |
| **AND/OR/NOT** | Combine conditions | `WHERE salary > 50000 AND id = 1` |
| **INSERT** | Add records | `INSERT INTO employees VALUES (...)` |
| **UPDATE** | Modify records | `UPDATE employees SET salary = 35000 WHERE id = 4` |
| **DELETE** | Remove records | `DELETE FROM employees WHERE id = 5` |
| **LIKE** | Pattern matching | `WHERE name LIKE 'A%'` |
| **IN** | Multiple values | `WHERE salary IN (32000, 50000)` |
| **BETWEEN** | Range selection | `WHERE salary BETWEEN 40000 AND 60000` |
| **Aggregate Functions** | Calculate statistics | `SUM()`, `AVG()`, `COUNT()`, `MIN()`, `MAX()` |
| **Aliases** | Rename columns/tables | `SELECT name AS employee_name FROM employees AS e` |

---

## Database File

- **Location:** `newdb.db`
- **Type:** SQLite database file
- **Created:** Automatically on first connection
- **Contents:** Employees table with sample data

---

## Tips for Learning

1. **Run cells in order** - Each cell builds on previous data
2. **Modify queries** - Change numbers and conditions to see results
3. **Experiment** - Try different operators and combinations
4. **Check outputs** - Review query results to understand behavior
5. **Use markdown cells** - Read descriptions before executing SQL

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| `%sql` magic command not recognized | Run `%load_ext sql` in first cell |
| Database connection error | Ensure `newdb.db` exists or restart notebook |
| Duplicate key error | Use `INSERT OR IGNORE` or `INSERT OR REPLACE` |
| No results returned | Check WHERE condition and table contents first |
| Permission denied | Ensure write permissions on workspace directory |

---

## Resources for Further Learning

- **SQLite Documentation:** https://www.sqlite.org/docs.html
- **SQL Basics:** https://www.w3schools.com/sql/
- **Jupyter Notebook Guide:** https://jupyter-notebook.readthedocs.io/

---

## Notes

- All queries use **SQLite** syntax
- The `newdb.db` file is local to this workspace
- Changes persist between notebook sessions
- To reset data, re-run the table creation cell

---

## Author Notes

This notebook is designed as a **progressive learning tool** where each section builds foundational SQL knowledge for practical database manipulation and analysis.

**Happy Learning! 🎓**
