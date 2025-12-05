# Docker & Linux Commands Reference

## Table of Contents
- [Linux Basics](#linux-basics)
- [Docker Images](#docker-images)
- [Docker Containers](#docker-containers)
- [Container Lifecycle](#container-lifecycle)
- [Inspection & Debugging](#inspection--debugging)
- [Cleanup Commands](#cleanup-commands)
- [Quick Workflows](#quick-workflows)

---

## Linux Basics

### Navigation & File Operations

**`pwd`** - Print Working Directory
- Shows your current location in the file system
- Example output: `/home/user/documents`

**`ls`** - List
- Lists files and folders in the current directory
- Common options: `ls -la` (detailed list with hidden files)

**`mkdir <directory>`** - Make Directory
- Creates a new folder
- Example: `mkdir myfolder`

**`touch <filename>`** - Create File
- Creates a new empty file or updates timestamp
- Example: `touch myfile.txt`

**`cat <filename>`** - Concatenate
- Displays the contents of a file
- Example: `cat myfile.txt`

**`rm <filename>`** - Remove
- Deletes files or directories
- `rm myfile.txt` - Delete file
- `rm -r myfolder/` - Delete folder recursively
- `rm -rf myfolder/` - Force delete without confirmation

**`clear`** - Clear Screen
- Clears the terminal screen
- Shortcut: `Ctrl + L`

### Network

**`ping <host>`** - Test Connectivity
- Sends packets to test if a server/website is reachable
- Example: `ping google.com`
- Stop with `Ctrl + C`

---

## Docker Images

### Viewing Images

**`docker images`**
- Lists all Docker images stored locally
- Shows: REPOSITORY, TAG, IMAGE ID, CREATED, SIZE

**`docker images --help`**
- Shows help documentation for the `docker images` command

### Managing Images

**`docker pull <image>`**
- Downloads an image from Docker Hub without running it
- Example: `docker pull ubuntu`
- Example: `docker pull nginx:latest`

**`docker image inspect <image>`**
- Shows detailed JSON information about an image
- Includes layers, configuration, size, environment variables
- Example: `docker image inspect busybox`

**`docker image rm <image>`**
- Removes/deletes an image from your system
- Must stop all containers using it first
- Example: `docker image rm busybox`

**`docker image prune`**
- Removes all unused/dangling images
- Frees up disk space
- Add `-a` to remove all unused images (not just dangling)

---

## Docker Containers

### Running Containers

**`docker run -it <image>`**
- **run** - Creates and starts a new container
- **-i** - Interactive (keeps STDIN open)
- **-t** - Allocates a pseudo-TTY (terminal)
- Example: `docker run -it ubuntu`

**`docker run -itd --name <name> <image>`**
- **-d** - Detached mode (runs in background)
- **--name** - Assigns a custom name to the container
- Example: `docker run -itd --name taha_cont ubuntu`

**`docker run -it --name <name> <image>`**
- Creates and runs container in foreground with interactive terminal
- Example: `docker run -it --name taha_cont ubuntu`

### Viewing Containers

**`docker ps`** - Process Status
- Lists all **currently running** containers
- Shows: CONTAINER ID, IMAGE, COMMAND, STATUS, PORTS, NAMES

**`docker ps -a`** - All
- Lists **all** containers (running and stopped)
- Use this to see containers you've exited

**`docker ps --help`**
- Shows help documentation for the `docker ps` command

---

## Container Lifecycle

### Starting & Stopping

**`docker stop <container>`**
- Stops a running container gracefully
- Example: `docker stop taha_cont`
- Can use container name or ID

**`docker start <container>`**
- Starts a stopped container
- Example: `docker start taha_cont`
- Container must already exist

**`docker restart <container>`**
- Stops and then starts a container
- Example: `docker restart taha_cont`

### Entering Containers

**`exit`** (inside container)
- Exits the container shell
- **Stops** the container if it's the main process

**`docker exec -it <container> <command>`**
- Executes a command in a **running** container
- Does NOT stop the container when you exit
- Example: `docker exec -it taha_cont bash`
- Example: `docker exec -it taha_cont sh` (if bash not available)

**`docker attach <container>`**
- Attaches to the main process of a running container
- **Warning**: Exiting will stop the container

### Key Difference: `docker run` vs `docker exec`

| Command | Use Case | Creates New Container? |
|---------|----------|----------------------|
| `docker run` | Start a NEW container from an image | ✅ Yes |
| `docker exec` | Enter an EXISTING running container | ❌ No |
| `docker start` | Start a STOPPED container | ❌ No |

**Example Workflow:**
```bash
# First time - create and run
docker run -itd --name taha_cont ubuntu

# Later - enter the running container
docker exec -it taha_cont bash

# If stopped - restart it first
docker start taha_cont
docker exec -it taha_cont bash
```

---

## Inspection & Debugging

**`docker version`**
- Shows Docker version information (client and server)

**`docker info`**
- Displays detailed system-wide information about Docker
- Shows: containers, images, storage driver, etc.

**`docker logs <container>`**
- Shows logs/output from a container
- Example: `docker logs taha_cont`
- Add `-f` to follow logs in real-time

**`docker inspect <container>`**
- Shows detailed JSON information about a container
- Includes network settings, mounts, environment variables

---

## Cleanup Commands

### Removing Containers

**`docker container rm <container>`**
- Removes a **stopped** container
- Example: `docker container rm taha_cont`
- Shorthand: `docker rm <container>`

**`docker container rm -f <container>`**
- Force removes a container (even if running)
- Example: `docker container rm -f taha_cont`

**`docker container prune`**
- Removes all stopped containers
- Cleans up space automatically

### Removing Images

**`docker image rm <image>`**
- Removes an image
- Example: `docker image rm ubuntu`
- Shorthand: `docker rmi <image>`

**`docker image prune`**
- Removes dangling/unused images

**`docker image prune -a`**
- Removes ALL unused images (not referenced by any container)

### System Cleanup

**`docker system prune`**
- Removes:
  - All stopped containers
  - All dangling images
  - All unused networks
  - All build cache

**`docker system prune -a`**
- More aggressive cleanup
- Removes all unused images (not just dangling)

---

## Quick Workflows

### Workflow 1: Basic Container Usage
```bash
# Pull an image
docker pull ubuntu

# See it in your images
docker images

# Run it interactively
docker run -it ubuntu

# Inside container:
pwd                        # Check location
ls                         # List files
exit                       # Leave container

# Back on host:
docker ps -a               # See stopped container
docker container rm <ID>   # Clean up
```

### Workflow 2: Named Container (Recommended)
```bash
# Create named container in detached mode
docker run -itd --name taha_cont ubuntu

# Check it's running
docker ps

# Enter the container
docker exec -it taha_cont bash

# Do your work inside...
# Exit (container keeps running)
exit

# Stop when done
docker stop taha_cont

# Restart later
docker start taha_cont
docker exec -it taha_cont bash

# Remove when finished
docker rm taha_cont
```

### Workflow 3: Testing & Cleanup
```bash
# Quick test with auto-removal
docker run -it --rm ubuntu

# The --rm flag automatically removes container on exit
# Great for testing!
```

### Workflow 4: Working with Different Images
```bash
# Lightweight Linux
docker run -it busybox

# Web server
docker run -d -p 8080:80 nginx

# Check running containers
docker ps

# View logs
docker logs <container_name>

# Stop and remove
docker stop <container_name>
docker rm <container_name>
```

---

## Common Mistakes & Fixes

### ❌ Container already running
**Error**: Trying to `docker run` when container exists
**Fix**: Use `docker exec` to enter, or `docker start` if stopped

### ❌ Cannot remove running container
**Error**: Container is in use
**Fix**: Stop first with `docker stop`, then `docker rm`
Or force: `docker rm -f <container>`

### ❌ Image in use
**Error**: Cannot delete image
**Fix**: Remove all containers using it first
```bash
docker ps -a | grep <image>
docker rm <container_ids>
docker rmi <image>
```

### ❌ Port already allocated
**Error**: Port conflict when running container
**Fix**: Use different port or stop conflicting container
```bash
docker run -d -p 8081:80 nginx  # Use 8081 instead of 8080
```

---

## Command Quick Reference

| Task | Command |
|------|---------|
| Download image | `docker pull <image>` |
| List images | `docker images` |
| Run new container | `docker run -it <image>` |
| Run in background | `docker run -itd --name <name> <image>` |
| List running | `docker ps` |
| List all | `docker ps -a` |
| Enter running container | `docker exec -it <container> bash` |
| Stop container | `docker stop <container>` |
| Start container | `docker start <container>` |
| Remove container | `docker rm <container>` |
| Remove image | `docker rmi <image>` |
| View logs | `docker logs <container>` |
| Cleanup stopped | `docker container prune` |
| Cleanup images | `docker image prune` |
| System cleanup | `docker system prune` |

---

## Tips & Best Practices

1. **Always name your containers** with `--name` for easier management
2. **Use `-d` flag** to run containers in background (detached mode)
3. **Use `docker exec`** to enter running containers (not `docker run` again!)
4. **Regular cleanup** with `prune` commands to save disk space
5. **Check `docker ps -a`** to see all containers before creating new ones
6. **Use `--rm` flag** for temporary test containers that auto-delete
7. **Read logs** with `docker logs` when debugging issues

---

## Additional Resources

- Official Docker Docs: https://docs.docker.com/
- Docker Hub (find images): https://hub.docker.com/
- Docker Cheat Sheet: https://docs.docker.com/get-started/docker_cheatsheet.pdf
