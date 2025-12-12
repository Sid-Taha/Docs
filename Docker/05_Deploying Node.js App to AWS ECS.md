# 🚀 Deploying Node.js App to AWS ECS (Fargate) - The Complete Guide

This guide covers everything from creating a Node.js app to deploying it on AWS ECS using Docker. It includes fixes for common mistakes encountered during our sessions.

-----

## ✅ Prerequisites

1.  **Node.js** installed.
2.  **Docker Desktop** installed and **Running** (Green Icon).
3.  **AWS CLI** installed and configured (`aws configure`).

-----

## Step 1: Node.js Project Setup

Create a folder and initialize the project:

```bash
npm init -y
npm install express
```

### Create `index.js`

**⚠️ Important:** Always include `(req, res)` in route handlers to avoid server crashes.

```javascript
const express = require("express");
const app = express();
const PORT = process.env.PORT || 3000;

// 1. Health Check Route (Root)
app.get("/", (req, res) => {
    res.status(200).send("Health Check Passed: Server is Active!");
});

// 2. Main API Route
app.get("/api", (req, res) => {
    return res.json({
        message: "Hello from AWS ECS!",
        developer: "Taha",
        cloud: "AWS",
        timestamp: new Date()
    });
});

app.listen(PORT, () => {
    console.log(`Server is up and running on port ${PORT}`);
});
```

-----

## Step 2: Docker Setup

Create a file named **`Dockerfile`** (no extension) in the root folder.

### `Dockerfile`

**💡 Pro Tip:** We install `curl` because the lightweight Alpine image doesn't have it, and we define `HEALTHCHECK` here so AWS picks it up automatically.

```dockerfile
# 1. Base Image
FROM node:18-alpine

# 2. Set Working Directory
WORKDIR /app

# 3. Install curl for Healthcheck (Crucial for Alpine)
RUN apk --no-cache add curl

# 4. Install Dependencies
COPY package*.json ./
RUN npm install

# 5. Copy Code
COPY . .

# 6. Expose Port
EXPOSE 3000

# 7. Healthcheck Command
# Checks every 30s if the server is responsive
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost:3000/ || exit 1

# 8. Start Application
CMD ["node", "index.js"]
```

-----

## Step 3: Push Image to AWS ECR

1.  Go to **AWS Console \> ECR \> Repositories**.
2.  Click **Create repository** (e.g., `my-api-repo`).
3.  Click on the repo name and hit **"View push commands"**.

Run these commands in your **CMD** (Command Prompt):

1.  **Login:**

    ```cmd
    aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin YOUR_ACCOUNT_ID.dkr.ecr.ap-south-1.amazonaws.com
    ```

    *(Note: Use the pipe `|` command for CMD, not the PowerShell syntax).*

2.  **Build:** `docker build -t my-api-repo .`

3.  **Tag:** `docker tag my-api-repo:latest ...` (Paste the full URI from AWS).

4.  **Push:** `docker push ...`

-----

## Step 4: Deploy to AWS ECS (Fargate)

### 1\. Create Cluster (The Computer)

  * **ECS \> Clusters \> Create cluster**.
  * **Name:** `my-production-cluster`.
  * **Infrastructure:** Select **AWS Fargate (serverless)**.
  * Click **Create**.

### 2\. Create Task Definition (The Recipe)

  * **ECS \> Task definitions \> Create new task definition**.
  * **Family name:** `api-task`.
  * **Launch type:** AWS Fargate.
  * **CPU/Memory:** .5 vCPU / 1 GB (Cost effective).
  * **Container Details:**
      * **Name:** `api-container`
      * **Image URI:** Copy URI from ECR (e.g., `...amazonaws.com/my-api-repo:latest`).
      * **Container Port:** `3000`.
  * **HealthCheck Section (CRITICAL STEP):**
      * Leave the **Command** field **COMPLETELY EMPTY**.
      * Leave Interval, Timeout, etc., **EMPTY**.
      * *Reason:* AWS will auto-read the `HEALTHCHECK` instruction from your Dockerfile. If you type here, it might conflict or give syntax errors.

### 3\. Create Service (The Switch)

  * Go inside your **Cluster \> Services Tab \> Create**.
  * **Launch type:** FARGATE.
  * **Task Definition:** `api-task` (Revision: Latest).
  * **Service name:** `api-service`.
  * **Desired tasks:** `1`.
  * **Networking (CRITICAL STEP):**
      * **Security Group:** Create new security group.
      * **Inbound Rules:**
          * **Type:** Custom TCP
          * **Port range:** `3000`
          * **Source:** Anywhere (`0.0.0.0/0`)
  * Click **Create**.

-----

## Step 5: Verify & Test

1.  Wait for the Service Status to become **Active** and Task Status to become **Running** (Green).
2.  Click on the **Task ID** \> **Configuration/Networking**.
3.  Copy the **Public IP**.
4.  Open in Browser: `http://PUBLIC_IP:3000`

-----

## Step 6: 🧹 Cleanup (Save Money\!)

**Order is important to avoid errors:**

1.  **Delete Service:**
      * Go to Cluster \> Services.
      * If you can't delete immediately, **Update** service -\> Set **Desired tasks** to `0`.
      * Wait for tasks to stop.
      * Then **Delete** the service.
2.  **Delete Cluster:**
      * ECS \> Clusters \> Delete cluster.
3.  **Delete ECR Repo:**
      * ECR \> Repositories \> Delete.
4.  *(Optional)* **Delete Log Group:**
      * CloudWatch \> Log groups \> Delete `/ecs/api-task`.

-----

## 🚫 Common Mistakes & Solutions (Troubleshooting)

| Problem | Symptom | Solution |
| :--- | :--- | :--- |
| **Code Crash** | Task starts and immediately stops. | Check your code. Did you forget `(req, res)` parameters? |
| **Docker Build Fail** | "Pipe error" / "System cannot find file". | **Docker Desktop** is not running. Start it first. |
| **Console Error** | "Command is required" in Task Def. | You entered text in the HealthCheck command box. **Clear it completely.** |
| **Site Not Loading** | Task runs but browser spins. | Check **Security Group**. Did you open Port **3000** for **Anywhere**? |
| **Can't Delete Service** | "Service is active" error. | Update Service first, set **Desired Tasks = 0**, wait for drain, then delete. |
| **Missing Resources** | Everything disappeared from Console. | Check your **Region** (top-right). Are you in Mumbai (`ap-south-1`) or Sydney? |
| **Image Update** | Pushed new code but ECS still shows old? | When updating Service, check **"Force new deployment"**. |
