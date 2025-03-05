# Doc-sanity-backup

## Sanity Backup and Restore Guide

### 🟢 Step 1: Install Sanity CLI
Open your terminal and run the following command to install the Sanity CLI globally:
```bash
npm install -g @sanity/cli
```

### 🟢 Step 2: Export Your Sanity Dataset
After installation, navigate to your project folder and run this command in the VS Code terminal:
```bash
sanity dataset export production sanity-backup.tar.gz
```
- This command will create a backup of your current dataset as a `.tar.gz` file.
- Make sure to **save your schema files** as well for a complete backup.

### 🟢 Step 3: Import Data to a New Sanity Project
If you’ve created a new project and want to import your old project’s data:

1. **Update your new project settings:**
   - Replace the new project’s **Project ID** and **API Key** with the old project’s details (if needed).

2. **Run the import command:**
```bash
sanity dataset import myData.gz production
```
- This will import your old project’s data into the new project’s dataset.

Would you like me to add steps for automating backups or handling schema conflicts? Let me know! 🚀


**Verify Your Project ID**
```bash
sanity projects list
```



1. Log Out of Sanity
First, log out from the current Sanity session:

bash
Copy
Edit
sanity logout
2. Log In Again
Log back in, and this time, choose the correct Google account:

bash
Copy
Edit
sanity login
It will open a browser window — make sure you select the right Google account (the one linked to your current Sanity project).

3. Verify the Project
After logging in, check your projects again:

bash
Copy
Edit
sanity projects list
You should now see the correct project. Double-check that the project ID matches what you have in your app's config.






