# **Linux Commands**

**`pwd`** - **P**rint **W**orking **D**irectory
- Shows your current location in the file system
- Example output: `/home/user/documents`

**`ls`** - **L**i**s**t
- Lists files and folders in the current directory

**`mkdir`** - **M**a**k**e **Dir**ectory
- Creates a new folder
- Example: `mkdir myfolder`

**`touch`** - Create empty file or update timestamp
- Creates a new empty file
- Example: `touch myfile.txt`

**`cat`** - Con**cat**enate
- Displays the contents of a file
- Example: `cat myfile.txt`

**`rm`** - **R**e**m**ove
- Deletes files or directories
- Example: `rm myfile.txt` or `rm -r myfolder/` (recursive for folders)

**`clear`** - Clear screen
- Clears the terminal screen

**`ping`** - Test network connectivity
- Sends packets to test if a server/website is reachable
- Example: `ping google.com`

---

# **Docker Commands**

**`docker run -it ubuntu`**
- **run** - Creates and starts a new container
- **-it** - Interactive terminal mode
- **ubuntu** - Image to use
- Launches an Ubuntu container with an interactive shell

**`exit`**
- Exits the container and stops it

**`docker`**
- Shows all available Docker commands and options

**`docker ps`** - **P**rocess **S**tatus
- Lists all **currently running** containers

**`docker ps --help`**
- Shows help documentation for the `docker ps` command

**`docker ps -a`** - **a**ll
- Lists **all** containers (running and stopped)

**`docker images`**
- Lists all Docker images stored locally on your system

**`docker images --help`**
- Shows help documentation for the `docker images` command

**`docker run -it busybox`**
- Runs a lightweight Linux container (BusyBox)
- Creates and runs a new container from the busybox image

**`d`**
- This seems incomplete - might be a typo or shorthand you're noting

**`docker version`**
- Shows Docker version information (client and server)

**`docker info`**
- Displays detailed system-wide information about Docker

**Shutdown Docker and try to run container**
- This tests what happens when Docker daemon isn't running
- You'll get an error saying Docker isn't available

**`docker pull busybox`**
- Downloads the busybox image from Docker Hub without running it

**`docker image inspect busybox`**
- Shows detailed information about the busybox image (JSON format)
- Includes layers, configuration, size, etc.

**`docker image rm busybox`** - **r**e**m**ove
- Deletes the busybox image from your system

**`docker container rm <containerName>`**
- Removes a specific stopped container
- Replace `<containerName>` with actual container name or ID
- Example: `docker container rm mycontainer`

**`docker image prune`**
- Removes all unused/dangling images to free up space
- Cleans up images that aren't tagged or referenced by any container

---

## **Quick Workflow Example:**
```bash
docker pull ubuntu          # Download Ubuntu image
docker images              # See it in your images list
docker run -it ubuntu      # Run it interactively
pwd                        # Check where you are inside
ls                         # See what's inside
exit                       # Leave container
docker ps -a               # See stopped container
docker container rm <ID>   # Clean up the container
```





