# PostgreSQL Integration with Neon Using `uv` and SQLAlchemy

This guide explains how to set up and interact with a **PostgreSQL database** using **Neon**, manage your Python environment with `uv`, and connect via `SQLAlchemy`. We also use `python-dotenv` to manage environment variables securely.

---

## Prerequisites

Before beginning, ensure the following are installed:

- Python 3.10+
- `uv` (dependency manager)
- A free account on [https://vercel.com](https://vercel.com)

---

## Step-by-Step Setup

### Step 1: Login to Vercel

Login to your [Vercel](https://vercel.com) account to access its services.

> **Purpose:** Required to access the Neon integration for database creation.

---

### Step 2: Open the Storage Tab

Navigate to the **Storage** section in the left sidebar.

> **Purpose:** Neon databases are created and managed under this tab.

---

### Step 3: Create a Neon Database

Click the **Neon → Create** button.

> **Purpose:** Start the database provisioning process.

---

### Step 4: Accept Terms

If prompted, click **Accept and Create**.

> **Purpose:** Approves Neon’s terms and provisions the database.

---

### Step 5: Choose Region

Select your preferred region (e.g., `US-East`, `Europe`).

> **Purpose:** Region selection affects latency and performance.

---

### Step 6: Select Free Plan

Choose the `Free` tier for this setup.

> **Purpose:** Ideal for development and testing purposes.

---

### Step 7: Click Continue

Click on the **Continue** button to proceed with setup.

---

### Step 8: Name Your Database

Enter a name for your database (e.g., `my_app_db`).

---

### Step 9: Finalize Creation

Click **Create** to complete the process.

---

### Step 10: Confirmation

You’ll see a message: **Database Created Successfully**. Click **Done**.

---

### Step 11: Get Connection String

From the `.env.local` file shown on Neon’s Vercel page, **copy the `DATABASE_URL_UNPOOLED`**.

> **Purpose:** This is your database connection string used by SQLAlchemy.

---

## Project Setup Using `uv`

### Step 12: Initialize Project with `uv`

In your project directory, run:

```sh
uv init
```

> **Purpose:** Creates a virtual environment and `pyproject.toml` for managing dependencies.

---

### Step 13: Create `.env` File

Manually create a `.env` file in your project root:

```sh
touch .env
```

> **Purpose:** To securely store sensitive environment variables.

---

### Step 14: Paste Database URL

Inside `.env`, paste the `DATABASE_URL_UNPOOLED`:

```env
DATABASE_URL_UNPOOLED=your_copied_url_here
```

> 💡 _Never commit `.env` files to version control._

---

### Step 15: Install Required Dependencies

Install SQLAlchemy, PostgreSQL driver, and dotenv support:

```sh
uv add sqlalchemy
uv add psycopg2-binary
uv add python-dotenv
```

> **Purpose:** These libraries enable DB communication, PostgreSQL support, and env variable loading.

---

## Step 16: Sample Python Script to Verify DB Connection

Create `main.py` with the following code:

```python
# Import kar rahe hain zaroori libraries
import os
from sqlalchemy import create_engine, text
from dotenv import load_dotenv

# .env file load kar rahe hain
load_dotenv()

# DATABASE_URL ko environment se read kar rahe hain
DATABASE_URL = os.getenv("DATABASE_URL_UNPOOLED")

# SQLAlchemy engine create kar rahe hain
engine = create_engine(DATABASE_URL)

# Core logic function
def main():
    with engine.connect() as connection:
        # Table create kar rahe hain agar exist nahi karti
        connection.execute(text("""
            CREATE TABLE IF NOT EXISTS users (
                id SERIAL PRIMARY KEY,
                name TEXT NOT NULL,
                email TEXT UNIQUE NOT NULL
            );
        """))
        connection.commit()

        # Optional: Sample data insert karne ke liye (commented by default)
        # connection.execute(text("""
        #     INSERT INTO users (name, email)
        #     VALUES ('Taha', 'taha@example.com'),
        #            ('Ahmed', 'ahmed@example.com');
        # """))
        # connection.commit()

        # Data fetch kar rahe hain
        result = connection.execute(text("SELECT * FROM users;")).mappings()

        print("Users in database:")
        for row in result:
            print(dict(row))  # Row ko dictionary mein convert karke print kar rahe hain

# Script ko run karne ke liye
if __name__ == "__main__":
    main()
```

> ✅ **Purpose:** Confirms the database connection and table creation logic.

---

### Step 17: Open Database in Neon

From the Vercel Storage page, click **Open in Neon**.

---

### Step 18: View Data in Table Tab

- Navigate to the **Tables** tab at the bottom left.
- View your schema and data entries directly in the UI.

---

## Summary

| **Aspect**           | **Details**                                       |
|----------------------|---------------------------------------------------|
| **Tooling**          | `uv`, `SQLAlchemy`, `python-dotenv`, `psycopg2`   |
| **UI**               | Managed via **Neon (in Vercel)**                 |
| **Environment Setup**| Initialized with `uv`, using `.env` for secrets   |
| **Best Practices**   | Linting with `ruff` _(add separately)_, comments in code |
| **UI Framework**     | Use **Streamlit** for building interactive UIs (not covered here) |

---

> 📌 _Refer to the Technical Notes Guidelines for consistent formatting, structure, and best practices across all future notes._
