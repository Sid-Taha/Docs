
# MySQL Setup with SQLAlchemy & DBeaver (Local)

This guide provides a comprehensive step-by-step walkthrough for setting up **MySQL** on Windows, connecting it with **SQLAlchemy** in Python, and managing databases visually using **DBeaver**.

> **Refer to the Technical Notes Guidelines** for formatting and setup standards.

---

## 🛠️ Project Initialization & Tooling

### Step 1: Initialize the Project with `uv`

```sh
uv init
```

Initializes a virtual environment and prepares the project for dependency management.

### Step 2: Add Code Linter `ruff`

```sh
uv add ruff
```

Ensures code quality and stylistic consistency using a lightweight Python linter.

---

## 🧩 MySQL Installation (Windows)

### Step 3: Download MySQL Installer

- Visit the official site:  
  [`https://dev.mysql.com/downloads/installer/`](https://dev.mysql.com/downloads/installer/)
- Select:
  - **Windows (x86, 32-bit), MSI Installer**
  - Click **“No thanks, just start my download.”**

### Step 4: Run Installer and Perform Full Setup

Open the downloaded setup file:

```sh
mysql-installer-web-community-8.0.41.0
```

Follow the installation wizard:

- `Next` → `Execute` → `Full` → `Execute`
- Proceed with:
  - Repeated `Next` prompts
  - **Set root password** when prompted
  - Uncheck:
    - **MySQL Workbench**
    - **MySQL Shell**
  - Final steps:
    - `Execute` → `Finish`

---

## 🧭 Environment Variable Setup

### Step 5: Add MySQL to System PATH

Add the following path to the Windows environment variable:

```sh
C:\Program Files\MySQL\MySQL Server 8.0\bin
```

> ✅ This enables direct use of `mysql` in CMD.

---

## 🖥️ MySQL Command-Line Setup

### Step 6: Open CMD in Project Directory

```sh
cd path\to\your\project
```

### Step 7: Login to MySQL

```sh
mysql -u root -p
```

Enter your root password when prompted.

### Step 8: Create and Select a Database

```sql
CREATE DATABASE school;
USE school;
```

> 🎯 You’ve now initialized a new database for your project.

---

## 🧱 Python Dependencies Setup

### Step 9: Add `pymysql` Library

```sh
uv add pymysql
```

> Required for Python to interact with MySQL via SQLAlchemy.

### Step 10: Add `sqlalchemy`

```sh
uv add sqlalchemy
```

> Core ORM used to manage database objects and queries in Python.

---

## 🧪 Example Code: Creating a Table & Inserting Data

The following script connects to MySQL, creates a table, and inserts data:

```python
from sqlalchemy import create_engine, text

# MySQL Connection Details
USER = 'root'               # Username
PASSWORD = 'rootpass'       # Password
HOST = 'localhost'          # Local Server
DB_NAME = 'school'          # Target Database

# Establish SQLAlchemy Engine
engine = create_engine(f"mysql+pymysql://{USER}:{PASSWORD}@{HOST}/{DB_NAME}")

# Table Creation
with engine.connect() as connection:
    connection.execute(text("""
        CREATE TABLE IF NOT EXISTS students (
            id INT AUTO_INCREMENT PRIMARY KEY,
            name VARCHAR(25) NOT NULL,
            age INT
        );
    """))

# Insert a Sample Record
with engine.connect() as connection:
    connection.execute(text("""
        INSERT INTO students (name, age)
        VALUES ('Zeeshan', 30);
    """))
    connection.commit()  # 🧠 Don't forget to commit the transaction
```

> 💡 *Inline Comments:*
> - `# English`: Clarifies function
> - `# Hinglish`: Aids bilingual understanding

---

## 📊 Optional: Query the Table in Python

Uncomment and use the following code to fetch data from MySQL:

```python
# Fetch and Display Student Records
with engine.connect() as connection:
    result = connection.execute(text("SELECT * FROM students"))
    for row in result:
        print(row)  # 👁️‍🗨️ Print each row (student record)
```

---

## 🧪 Check Your Data

### Step 11: View via MySQL CLI

Repeat:

```sh
mysql -u root -p
USE school;
SELECT * FROM students;
```

### Step 12: View via DBeaver GUI

1. Open **DBeaver**.
2. Create a **New Connection**.
3. Choose **MySQL**, enter:
   - Host: `localhost`
   - User: `root`
   - Password: your password
4. Click **Finish**.
5. Browse tables and view your data visually.

---

## 📌 Note: MySQL vs SQLite

| Database | Description |
|----------|-------------|
| **SQLite** | Stores `.db` file directly in project directory |
| **MySQL**  | Stores database in system path (`C:\Program Files\MySQL\...`) |

---

## ✅ Summary

- ✅ **Tools Used**: `uv`, `ruff`, `sqlalchemy`, `pymysql`, `DBeaver`
- ✅ **Steps Followed**:
  - Installed MySQL and configured environment
  - Created and connected to a MySQL database
  - Used SQLAlchemy to create tables and insert data
  - Visualized data in both CLI and GUI (DBeaver)
- ✅ **Next Steps**: 
  - Continue building ORM models in SQLAlchemy
  - Expand database schema
  - Use Streamlit for UI (Refer to UI Development Guidelines)

---

📎 *Reference*: Follow the [Technical Notes Guidelines] for consistent formatting, project tooling, and best practices.
```
