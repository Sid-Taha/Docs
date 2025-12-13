
# 🚀 Automated Deployment Guide: Node.js to AWS ECS Fargate

**Tech Stack:** Node.js | Docker | AWS ECR & ECS | GitHub Actions
**Goal:** A complete guide to deploying a containerized Node.js application to AWS Serverless (Fargate) with a CI/CD pipeline.

-----

## 🏁 Getting Started

### 1\. Prerequisites

  * Node.js installed locally.
  * An AWS Account.
  * A GitHub Account.

### 2\. Initial Project Setup

Create a new folder and initialize the project.

```bash
mkdir my-aws-project
cd my-aws-project
npm init -y
npm install express mongoose dotenv zod cors
```

### 3\. Folder Structure

Ensure your project looks like this:

```text
my-aws-project/
├── .github/
│   └── workflows/
│       └── deploy.yml      # CI/CD Script
├── src/
│   ├── config/
│   ├── controllers/
│   └── routes/
├── Dockerfile              # Docker Configuration
├── .dockerignore
├── .gitignore
└── server.js               # Main Entry Point
```

### 4\. Application Code (`server.js`)

Use this robust server setup that includes database connection handling and a health check route.

```javascript
require('dotenv').config();
const express = require('express');
const mongoose = require('mongoose');
const app = express();

const PORT = process.env.PORT || 3000;

// Middleware
app.use(express.json());

// Database Connection
mongoose.connect(process.env.MONGODB_URL)
    .then(() => console.log("✅ MongoDB Connected"))
    .catch(err => {
        console.error("❌ DB Connection Error:", err);
        process.exit(1); // Exit process if DB fails (Container will restart)
    });

// Health Check Route (Required for AWS Load Balancers/Health Checks)
app.get('/', (req, res) => {
    res.status(200).send("Health Check Passed: Server is Active!");
});

app.listen(PORT, () => console.log(`🚀 Server running on port ${PORT}`));
```

-----

## 🐳 Docker Configuration

### 1\. `Dockerfile`

This file tells Docker how to build your application image.

```dockerfile
# 1. Base Image
FROM node:18-alpine

# 2. Install curl for health checking
RUN apk --no-cache add curl

# 3. Set working directory
WORKDIR /app

# 4. Copy package files first (optimizes cache)
COPY package*.json ./

# 5. Install only production dependencies
RUN npm install --production

# 6. Copy the rest of the application code
COPY . .

# 7. Expose the application port
EXPOSE 3000

# 8. Health Check (Runs every 10s to ensure app is alive)
HEALTHCHECK --interval=10s --timeout=5s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:3000/ || exit 1

# 9. Start Command (Make sure this matches your file name!)
CMD ["node", "server.js"]
```

### 2\. `.dockerignore`

Files to exclude from the Docker image to keep it small and secure.

```text
node_modules
.git
.gitignore
.dockerignore
Dockerfile
.env
README.md
```

### 3\. `.gitignore` (Critical Configuration)

**Important:** Do NOT ignore the `Dockerfile`. GitHub Actions needs to see it.

```text
node_modules
.env                # Never push .env to GitHub!
```

-----

## ☁️ AWS Manual Setup (One-Time Setup)

Before automation can work, you must manually create the empty resources on AWS.

### 1\. IAM User (Permissions)

1.  Go to **IAM** \> **Users** \> **Create user**.
2.  Attach policies: `AmazonEC2ContainerRegistryFullAccess` and `AmazonECS_FullAccess` (Or `AdministratorAccess` for testing).
3.  **Security Credentials:** Create an **Access Key** (CLI) and save the `Access Key ID` and `Secret Access Key`.

### 2\. ECR Repository (Image Storage)

1.  Go to **Elastic Container Registry (ECR)**.
2.  Click **Create repository**.
3.  Name: `my-node-app`.
4.  Keep settings as **Private**.

### 3\. ECS Cluster & Task Definition

1.  **Create Cluster:**
      * Go to **Elastic Container Service (ECS)** \> **Clusters**.
      * Name: `my-cluster`.
      * Infrastructure: **AWS Fargate** (Serverless).
2.  **Create Task Definition:**
      * Go to **Task Definitions** \> **Create new**.
      * Family Name: `my-node-task`.
      * Launch Type: **AWS Fargate**.
      * Container Name: `node-container`.
      * Image URI: Type `node` temporarily (the pipeline will update this).
      * Container Port: `3000`.
      * **Environment Variables:** Add `MONGODB_URL` here with your Atlas connection string.

### 4\. ECS Service (Running the App)

1.  Go to your Cluster (`my-cluster`) \> **Services** \> **Create**.
2.  Compute options: **Launch type** (Fargate).
3.  Family: `my-node-task`.
4.  Service Name: `my-node-service`.
5.  Desired tasks: `1`.
6.  **Networking:** Create a new Security Group that allows **Custom TCP** on Port **3000** from **Anywhere**.
7.  **Public IP:** Turned **ON**.

-----

## 🤖 CI/CD Pipeline (GitHub Actions)

Create `.github/workflows/deploy.yml`.

**Note:** This script includes a critical fix (`jq`) for the `enableFaultInjection` error.

```yaml
name: Deploy to Amazon ECS

on:
  push:
    branches: [ "main" ]

env:
  AWS_REGION: ap-south-1                  # Your AWS Region
  ECR_REPOSITORY: my-node-app             # Your ECR Repo Name
  ECS_SERVICE: my-node-service            # Your Service Name
  ECS_CLUSTER: my-cluster                 # Your Cluster Name
  ECS_TASK_DEFINITION_FAMILY: my-node-task 
  CONTAINER_NAME: node-container          

jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest
    environment: production

    steps:
    - name: Checkout
      uses: actions/checkout@v4

    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ env.AWS_REGION }}

    - name: Login to Amazon ECR
      id: login-ecr
      uses: aws-actions/amazon-ecr-login@v1

    - name: Build, tag, and push image to Amazon ECR
      id: build-image
      env:
        ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
        IMAGE_TAG: ${{ github.sha }}
      run: |
        docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
        docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG
        echo "image=$ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG" >> $GITHUB_OUTPUT

    # FIX: Removes 'enableFaultInjection' key which causes AWS deployment errors
    - name: Download task definition
      run: |
        aws ecs describe-task-definition --task-definition ${{ env.ECS_TASK_DEFINITION_FAMILY }} --query taskDefinition | jq 'del(.enableFaultInjection)' > task-definition.json

    - name: Fill in the new image ID in the Amazon ECS task definition
      id: task-def
      uses: aws-actions/amazon-ecs-render-task-definition@v1
      with:
        task-definition: task-definition.json
        container-name: ${{ env.CONTAINER_NAME }}
        image: ${{ steps.build-image.outputs.image }}

    - name: Deploy Amazon ECS task definition
      uses: aws-actions/amazon-ecs-deploy-task-definition@v1
      with:
        task-definition: ${{ steps.task-def.outputs.task-definition }}
        service: ${{ env.ECS_SERVICE }}
        cluster: ${{ env.ECS_CLUSTER }}
        wait-for-service-stability: true
```

### GitHub Secrets Setup

In your repository, go to **Settings** \> **Secrets and variables** \> **Actions** and add:

1.  `AWS_ACCESS_KEY_ID`
2.  `AWS_SECRET_ACCESS_KEY`

-----

## ⚠️ Troubleshooting & Lessons Learned (Important)

These are specific errors we encountered and how to fix them:

### 1\. "Cannot find module '/app/index.js'"

  * **Cause:** The `Dockerfile` CMD was set to `index.js`, but the actual file was `server.js`.
  * **Solution:** Ensure `CMD ["node", "server.js"]` matches your filename exactly.

### 2\. GitHub Action cannot find Dockerfile

  * **Cause:** The `Dockerfile` was listed inside `.gitignore`.
  * **Solution:** Remove `Dockerfile` from `.gitignore` so it gets pushed to GitHub.

### 3\. "Unexpected key 'enableFaultInjection'"

  * **Cause:** A recent AWS update added a setting that the GitHub Action didn't understand.
  * **Solution:** Use the `jq` command in the YAML file to delete this key before deployment.

### 4\. "Unexpected end of JSON input"

  * **Cause:** The GitHub Action tried to update a Service/Task that didn't exist (because we deleted them during cleanup).
  * **Solution:** You must manually create the ECS Service and Task Definition in the AWS Console for the **first time**. Subsequent updates are automated.

### 5\. Application Crash (Circuit Breaker)

  * **Cause:** The application couldn't connect to the database.
  * **Solution:** Ensure `MONGODB_URL` is added to the **Task Definition \> Environment Variables** and that MongoDB Atlas Network Access allows `0.0.0.0/0`.

-----

## 🧹 Cleanup Checklist

To avoid unexpected AWS charges, delete resources in this order:

1.  **ECS Service** (`my-node-service`)
2.  **ECS Cluster** (`my-cluster`)
3.  **ECR Repository** (`my-node-app`)
4.  **IAM Access Keys** (Optional, for security)
