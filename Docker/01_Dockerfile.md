# Docker Guide - Dockerfile & Docker Hub

## 📝 Dockerfile Structure

A Dockerfile is a script that contains instructions to build a Docker image.

### Basic Dockerfile Example

```dockerfile
# Base Image - Choose the base image for your application
FROM node:18-alpine

# Set Working Directory - All commands will run in this directory
WORKDIR /app

# Copy package files first (for better caching)
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy source code to container
COPY . .

# Expose port that your app will run on
EXPOSE 8000

# Command to start your application
CMD [ "node", "server.js" ]
```

### Common Dockerfile Instructions

- **FROM** - Specifies the base image
- **WORKDIR** - Sets the working directory inside container
- **COPY** - Copies files from host to container
- **RUN** - Executes commands during image build
- **EXPOSE** - Documents which port the container listens on
- **CMD** - Default command to run when container starts

---

## 🚀 Docker Hub Workflow

### Step 1: Create Docker Hub Account
1. Go to [hub.docker.com](https://hub.docker.com)
2. Sign up for a free account
3. Verify your email

### Step 2: Create Repository
1. Login to Docker Hub
2. Click "Create Repository"
3. Enter repository name (e.g., `my-backend-app`)
4. Choose public or private
5. Click "Create"

---

## 💻 Docker Commands

### Login to Docker Hub
```bash
docker login
# Enter your Docker Hub username and password
```

### Build Docker Image
```bash
# Format: docker build -t username/repo-name:tag .
docker build -t yourusername/my-backend-app:latest .
```

**Important:** The image name must match your Docker Hub repository name exactly!

### View Local Images
```bash
docker images
```

### Run Container Locally (Test)
```bash
# Format: docker run -p host-port:container-port image-name
docker run -p 8000:8000 yourusername/my-backend-app:latest
```

### Push Image to Docker Hub
```bash
# Format: docker push username/repo-name:tag
docker push yourusername/my-backend-app:latest
```

### Pull Image from Docker Hub
```bash
docker pull yourusername/my-backend-app:latest
```

### Stop Running Container
```bash
# List running containers
docker ps

# Stop container by ID or name
docker stop container_id
```

### Remove Containers and Images
```bash
# Remove container
docker rm container_id

# Remove image
docker rmi yourusername/my-backend-app:latest
```

---

## 📋 Complete Workflow Example

```bash
# 1. Login to Docker Hub
docker login

# 2. Build your image (make sure you're in project directory)
docker build -t yourusername/my-backend-app:latest .

# 3. Test locally (optional)
docker run -p 8000:8000 yourusername/my-backend-app:latest

# 4. Push to Docker Hub
docker push yourusername/my-backend-app:latest

# 5. Pull on another machine
docker pull yourusername/my-backend-app:latest

# 6. Run the pulled image
docker run -p 8000:8000 yourusername/my-backend-app:latest
```

---

## 🎯 Quick Tips

- **Image Naming:** Always use format `username/repository-name:tag`
- **Tags:** Use `latest` for most recent version or specific versions like `v1.0`
- **Port Mapping:** `-p host-port:container-port`
- **.dockerignore:** Create this file to exclude `node_modules`, `.env` etc. from being copied
- **Rebuild:** After code changes, rebuild the image with same command

### Sample .dockerignore file
```
node_modules
npm-debug.log
.env
.git
.gitignore
README.md
```

---

## ✅ Checklist

- [ ] Create Docker Hub account
- [ ] Create repository on Docker Hub
- [ ] Write Dockerfile in your project
- [ ] Login via `docker login`
- [ ] Build image with correct naming
- [ ] Test locally with `docker run`
- [ ] Push to Docker Hub
- [ ] Verify on Docker Hub website

---

**Remember:** The repository name on Docker Hub must match the name you use in your `docker build` and `docker push` commands!
