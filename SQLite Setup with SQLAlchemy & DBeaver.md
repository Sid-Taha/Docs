# SQLite Setup with SQLAlchemy & DBeaver

This guide provides a step-by-step process for setting up SQLite with SQLAlchemy and managing your database with DBeaver. You'll learn how to initialize a database, create tables, insert data, and perform common SQL operations, all while using the power of SQLAlchemy for database interactions and DBeaver for visual database management.

## Step 1: Install SQLAlchemy

Start by adding SQLAlchemy to your project to handle database interactions. Use the following command:

```sh
uv add sqlalchemy
```

## Step 2: Import SQLAlchemy Modules

Once SQLAlchemy is installed, import the necessary components for creating a database connection and executing SQL queries:

```python
from sqlalchemy import create_engine, text
```

## Step 3: Setup Boilerplate Code

Set up the initial boilerplate code to establish a connection with the SQLite database. Replace `"your_database_name.db"` with your actual database file name:

```python
from sqlalchemy import create_engine, text

# Database file name
DB_FILE = "your_database_name.db"

# Create an SQLite engine connected to the real database
engine = create_engine(f"sqlite:///{DB_FILE}")
```

## Step 4: Create Database and Table

Use the `engine.connect()` method to connect to the database and execute raw SQL queries to create a table if it doesn't already exist:

```python
# Create the database and table using raw SQL
with engine.connect() as connection:
    connection.execute(text(
        """
        CREATE TABLE IF NOT EXISTS students (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name VARCHAR(25) NOT NULL,
            age INTEGER
        );
        """
    ))
```

### Explanation:
- The `CREATE TABLE IF NOT EXISTS` statement ensures that the table is only created if it doesn't already exist.

## Step 5: SQLite3 Setup

Follow steps 1 to 10 from [this SQLite setup guide](https://github.com/Sid-Taha/Docs/blob/main/SQLite-CLI-setup.md) to set up SQLite on your system.

## Step 6: Handle Missing Data

If no data is shown in the table, it means the table is empty. You need to insert data using SQL queries before you can query it.

## Step 7: Insert Data Into Table

Insert data into the `students` table by executing an `INSERT` SQL query:

```python
with engine.connect() as connection:
    connection.execute(text(
        """
        INSERT INTO students
        (name, age)
        VALUES
        ('Taha', 20);
        """
    ))
    connection.commit()
```

### Explanation:
- The `commit()` method saves the changes to the database after executing the `INSERT` statement.

## Step 8: Delete Data From Table

To delete data from the table, execute a `DELETE` statement:

```python
with engine.connect() as connection:
    connection.execute(text(
        """
        DELETE FROM students WHERE id = 2;
        """
    ))
    connection.commit()
```

### Explanation:
- This query deletes the student with `id = 2` from the table.

## Step 9: Search Data by ID

To retrieve data from the `students` table, use a `SELECT` query:

```sh
SELECT * FROM students WHERE id = 1;
```

### Explanation:
- This query retrieves all columns for the student with `id = 1`.

## Step 10: Update Data in Table

To update a record in the `students` table by `id`, execute an `UPDATE` SQL query:

```python
with engine.connect() as connection:
    connection.execute(text(
        """
        UPDATE students
        SET name = 'Alex'
        WHERE id = 3;
        """
    ))
    connection.commit()
```

### Explanation:
- This query updates the `name` of the student with `id = 3` to `'Alex'`.

---

## Step 11: Install DBeaver

DBeaver is a powerful tool for managing databases with a user-friendly interface. To install DBeaver:

1. Go to the [DBeaver download page](https://dbeaver.io/download/).
2. Download the Windows installer version (`dbeaver-ce-25.0.0-x86_64-setup`).
3. Run the installer and follow the setup steps:
   - Select `English` for language.
   - Accept the license agreement.
   - Choose the installation type as `For Me`.
   - Select all installation options.
   - Complete the installation.

## Step 12: Open DBeaver

After installation, open DBeaver from the desktop shortcut.

## Step 13: Connect to SQLite Database

1. In DBeaver, select `SQLite` as the database type.
2. Click `Next` and browse to the location of your SQLite database (`your_database_name.db`).
3. Click `Open` to load the database.

## Step 14: Test Database Connection

- If prompted to download the JDBC library for SQLite, click `Download`.
- Test the connection to ensure it works.

## Step 15: View Database in DBeaver

1. On the left-hand side of DBeaver, you'll see the connected database.
2. Click on the database name, then the `Tables` folder.
3. Right-click on the `students` table and select `View Table`.

## Step 16: View Data

- Click the `Data` tab to view the contents of your table and confirm your data is inserted.

---

## Summary

- **SQLAlchemy**: Used for interacting with SQLite through Python. Key steps include initializing the engine, connecting to the database, and executing raw SQL queries to create tables, insert data, delete, update, and query the database.
- **DBeaver**: A graphical tool for managing SQLite databases. It allows you to connect to, view, and manipulate your database with a user-friendly interface.
- **Key Operations**: 
  - **Insert** data into the table.
  - **Delete** and **update** records based on `id`.
  - **View data** and manipulate tables visually with DBeaver.

This setup streamlines your database management workflow, combining the flexibility of SQLAlchemy with the powerful visualization features of DBeaver.
