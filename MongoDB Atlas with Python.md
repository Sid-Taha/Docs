# ✅ Full Steps to Use MongoDB Atlas with Python

1. **Create a Cluster**
   - Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
   - Sign in and create a new **free cluster** (choose a region close to you)

2. **Create a Database User (Database Access tab)**
   - Go to **Database Access**
   - Create a user (e.g. `tahaahmed`) with a password (e.g. `taha1234`)
   - Set the role to "Read and write to any database" or a specific one

3. **Set Network Access**
   - Go to **Network Access**
   - Click **Add IP Address**
     - You can add your own IP address
     - Or use `0.0.0.0/0` to allow access from **anywhere** (useful during dev)

4. **Wait for Cluster to Be Ready**
   - This can take 1–3 minutes on the free tier

5. **Get Your Connection String**
   - Go to **Clusters > Connect > Connect your application**
   - Copy the connection string (looks like: `mongodb+srv://<user>:<pass>@cluster0...`)
   - Replace `<user>` and `<pass>` with your actual username/password (URL-encoded if needed)

6. **Use That URI in Your Program**
   - Paste it into your `.env` file or directly in code (prefer `.env` for security)
   - Connect using `pymongo.MongoClient(uri)`

7. **Send Data from Python**
   - Write your insert/find/update code

8. **Check Atlas for Results**
   - Go to **Clusters > Collections**
   - Open your database and collection
   - You’ll see the inserted documents

---

### ✅ Bonus Tips

- 🔒 Use `.env` and `python-dotenv` to keep credentials secure
- ⚠️ Encode special characters in passwords (e.g. `@` → `%40`)
- 🧪 You can test connection with `.find_one()` before inserting
- 🧼 You don’t need to manually "create" a database or collection — **MongoDB auto-creates them on first use**

---

Awesome — let's clean this up and organize your code into a clear **MongoDB CRUD boilerplate** using Python + `pymongo` + `rich`.

---

### 🧠 Final Structure: MongoDB CRUD with Boilerplate

```python
# 🔧 Boilerplate Setup
import os
from dotenv import load_dotenv
from pymongo import MongoClient
import rich

load_dotenv()

client = MongoClient(os.getenv("Db_uri"))
db = client["myschooldb"]
collection = db["student"]
```

---

### 📥 Create (Insert)

```python
# CREATE (Insert)
student_info = {
    "name": "Ahmed",
    "age": 21
}

collection.insert_one(student_info)
rich.print("[green]Document inserted successfully.[/green]")
```

---

### 📖 Read (Find)

```python
# READ (Find)
students = collection.find()

for student in students:
    rich.print(student)
```

---

### 🛠️ Update

```python
# UPDATE
collection.update_one(
    {"name": "Ahmed"},             # Filter
    {"$set": {"name": "Mohamed"}}  # Update
)
rich.print("[yellow]Document updated successfully.[/yellow]")
```

---

### ❌ Delete

```python
# DELETE
collection.delete_one({"name": "Mohamed"})
rich.print("[red]Document deleted successfully.[/red]")
```
