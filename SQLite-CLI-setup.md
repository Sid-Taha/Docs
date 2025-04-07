# **SQLite Setup and Usage Guide**

---

### **Step 1: Download SQLite**
1. Visit the official SQLite download page: [SQLite Downloads](https://www.sqlite.org/download.html)

### **Step 2: Find the Precompiled Binaries**
2. Scroll down and look for the section titled **"Precompiled Binaries for Windows"**.

### **Step 3: Download the Tools**
3. Click on the link to download the SQLite tools:
   - **`sqlite-tools-win-x64-3490100.zip`** (6.12 MiB)

### **Step 4: Extract the Files**
4. After downloading, you'll receive a **.zip** file. Extract it to access the contents, which include the following file:
   - **`sqlite3.exe`**

### **Step 5: Place SQLite in Your Project Folder**
5. Copy the **`sqlite3.exe`** file and paste it into your project folder.

### **Step 6: Open the Command Prompt**
6. Open **Command Prompt** (CMD) and navigate to your project directory.

### **Step 7: Start SQLite Shell**
7. Run the following command to start the SQLite shell with your database:
   ```sh
   .\sqlite3.exe your_database_name.db
   ```

### **Step 8: Configure the Output Format**
8. To view data in a readable table format, run these commands inside the SQLite shell:
   ```sh
   .headers on
   ```

    ```sh
   .mode box
   ```

### **Step 9: List Tables**
9. To see a list of all tables in the database, use this command:
   ```sh
   .tables
   ```

### **Step 10: Query Data**
10. To retrieve all data from the **books** table, run the SQL query:
    ```sql
    SELECT * FROM books;
    ```

### **Step 11: Exit SQLite**
11. To exit the SQLite shell, type:
    ```sh
    .exit
    ```

### **Step 12: SQLite ...>**
12. To exit the SQLite ..>, type:
    ```sh
    ;
    ```

### **Step 13: .shell cls**
13. To clear screen, type:
    ```sh
    .shell cls
    ```

---

With these steps, you'll have SQLite set up and ready to use in your project. Let me know if you’d like me to add anything or refine the notes further! 🚀

