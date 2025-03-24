# AWS RDS Free Tier: Step-by-Step Guide for MySQL

Below are comprehensive notes on creating and configuring an AWS RDS (MySQL) database under the Free Tier, following the **Refined Technical Notes Guidelines**. Refer to the guidelines for more details on formatting, code quality, and best practices.

---

## Preliminary Project Setup & Tooling

Before diving into AWS RDS, ensure your local project environment meets the requirements for clean code and maintainability:

### Step 1: Initialize your project with `uv`

Initialize the project environment using `uv` to manage dependencies efficiently.

```sh
uv init
```

### Step 2: Add `ruff` (Linting Tool)

Add **ruff** to maintain high-quality Python code.

```sh
uv add ruff
```

### Step 3: Plan for a UI (Optional)

If you plan to build an interactive interface, use **Streamlit**:

```sh
uv add streamlit
```

*(You can now develop a UI to interact with your AWS RDS database.)*

---

## Creating an AWS RDS Free Tier (MySQL) Instance

Below are the sequential steps to launch and configure a MySQL database in AWS RDS using the Free Tier. 

### Step 1: Make an AWS Account
- **What & Why:** Create an AWS account if you don’t have one. This grants you access to AWS services, including RDS.
- **Tip:** Ensure you choose the **Free Tier** eligible options to avoid unexpected billing.

### Step 2: Visit the AWS Console
- **URL:** [AWS Console (EU North 1 Region)](https://eu-north-1.console.aws.amazon.com/console/home?region=eu-north-1#)
- **What & Why:** This link takes you directly to the AWS Management Console in the **eu-north-1** region.

### Step 3: Click on **Create Database**
- **What & Why:** Navigate to **RDS** in your AWS Console, then click **Create database** to begin the setup process.

### Step 4: Choose a Database Creation Method
- **Option Selected:** **Standard create** 
- **What & Why:** Gives you full control over configuration details.

### Step 5: Engine Options
- **Option Selected:** **MySQL**
- **What & Why:** We are setting up a MySQL database under RDS.

### Step 6: Edition
- **Option Selected:** **MySQL Community**
- **What & Why:** The Community edition is free and commonly used.

### Step 7: Templates
- **Option Selected:** **Free tier**
- **What & Why:** Ensures you stay within AWS’s free usage limits.

### Step 8: Availability and Durability
- **Option Selected:** **Single-AZ DB instance deployment (1 instance)**
- **What & Why:** Single-AZ is sufficient for dev or test environments within Free Tier constraints.

### Step 9: Settings / DB Instance Identifier
- **Instruction:** Enter a descriptive name, for example:  
  ```
  database-any_name
  ```
- **What & Why:** This identifier helps you recognize the database in the AWS console.

### Step 10: Master Username
- **Option Selected:** 
  ```
  admin
  ```
- **What & Why:** The master username is used to connect to your DB instance.

### Step 11: Credentials Management
- **Option Selected:** **Self managed**
- **What & Why:** You’ll manually supply the master password.

### Step 12: Master Password
- **Instruction:** 
  ```
  type_any_password_for_database
  ```
- **What & Why:** Keep this password secure; it’s required for DB connections.

### Step 13: Storage Type
- **Option Selected:** **General Purpose SSD (gp2)**
- **What & Why:** Standard SSD option that’s usually free within certain limits.

### Step 14: Additional Storage Configuration
- **Instruction:** **Uncheck** “Enable storage autoscaling”
- **What & Why:** Prevents accidental scaling beyond free tier limits.

### Step 15: Public Access
- **Option Selected:** **Yes**
- **What & Why:** Allows external connections (e.g., from your local machine or other services).

### Step 16: Create the Database
- **Instruction:** Click on the **Create database** button.
- **What & Why:** This initiates the creation of your MySQL DB instance.

### Step 17: Close Any Add-On Windows
- **What & Why:** AWS may show suggestions or add-ons; you can close these for now.

### Step 18: Select Your Database
- **Instruction:** Click on your **database name** under **DB Identifier**.
- **What & Why:** To access the database’s configuration and connectivity info.

### Step 19: Access VPC Security Groups
- **Instruction:** Under **Connectivity & security**, click on the **VPC security groups** link.
- **What & Why:** You need to configure inbound rules to allow connections to MySQL.

### Step 20: Click on the Security Group ID
- **Instruction:** Choose the **link** under **Security group ID**.
- **What & Why:** This opens the detailed view of your security group settings.

### Step 21: Edit Inbound Rules
- **Instruction:** In **Inbound rules**, click on **Edit inbound rules**.
- **What & Why:** You must explicitly allow traffic to connect to your database.

### Step 22: Add Rule (First Inbound Rule)
- **Instruction:** Click **Add rule** and select:
  - **Type:** Custom TCP
  - **Protocol:** TCP
  - **Port Range:** (Use the RDS instance port from **Connectivity & security**)
  - **Source:** `0.0.0.0/0`
- **What & Why:** This rule enables IPv4 traffic from any IP to the database on the specified port.

### Step 23: Add Rule (Second Inbound Rule)
- **Instruction:** Click **Add rule** again and select:
  - **Type:** Custom TCP
  - **Protocol:** TCP
  - **Port Range:** (Use the same RDS instance port)
  - **Source:** `::/0`
- **What & Why:** This rule enables IPv6 traffic from any IP.

### Step 24: Test Your Connection
- **Instruction:** Optionally test from a local database client (e.g., **DBeaver**, **MySQL Workbench**, or **Streamlit** app).
- **What & Why:** Verify that you can connect using your **host**, **username**, and **password**.

---

## Sample Python Code: Connecting and Inserting Records

Below is minimal Python code to connect to your new RDS MySQL instance and perform a test query.  
*(Inline comments are provided in both English and Hinglish for clarity.)*

```python
from sqlalchemy import create_engine, text

# MySQL connection details
USER = 'admin'  # Your MySQL username
PASSWORD = 'rootmysqldatabase'  # Aapka MySQL password
HOST = 'database-mysql.c9oqw0gkgofn.eu-north-1.rds.amazonaws.com'  # AWS RDS host
DB_NAME = 'my_taha'  # Target database name

# Create the SQLAlchemy engine
# SQLAlchemy engine banate hain taaki hum database se connect ho sakein
engine = create_engine(f"mysql+pymysql://{USER}:{PASSWORD}@{HOST}/{DB_NAME}")

# Create a table (if not already exists) and insert some data
with engine.connect() as connection:
    # Create table if it doesn't exist
    connection.execute(text(
        """
        CREATE TABLE IF NOT EXISTS collage (
            id INT AUTO_INCREMENT PRIMARY KEY,
            name VARCHAR(25) NOT NULL,
            age INT
        );
        """
    ))
    
    # Insert a sample record
    connection.execute(text(
        """
        INSERT INTO collage (name, age)
        VALUES ('Zeeshan', 30);
        """
    ))
    
    # Commit the transaction
    connection.commit()
```

> **Note:** Replace the placeholder **`PASSWORD`**, **`HOST`**, and **`DB_NAME`** with your actual values from the AWS RDS Console.

---

## Summary

1. **Project Setup & Tooling**: We started by initializing our project environment using `uv init`, added **ruff** for linting, and noted **Streamlit** as an option for building a UI to interact with the database.  
2. **AWS RDS Configuration**: Created an AWS account, chose **MySQL** under the **Free tier**, configured the DB instance identifier, credentials, and networking rules (Security Groups).  
3. **Testing & Validation**: Used inbound rules to allow access, then tested connectivity with a simple Python script using **SQLAlchemy**.

Following this guide ensures a quick and efficient way to launch a MySQL database under the AWS Free Tier, apply best practices for code quality, and maintain a modular project structure. 

For more in-depth details on formatting and best practices, **refer to the Technical Notes Guidelines**.
