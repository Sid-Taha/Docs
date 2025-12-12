# Docker Compose Guide

## What is Docker Compose?
Docker Compose is a tool for defining and running multi-container Docker applications using a YAML file. Instead of running multiple `docker run` commands, you define everything in one file.

---

## Basic Structure

```yaml
version: '3.8'  # Specifies the Compose file format version (optional in newer versions)

services:  # Defines all the containers you want to run
  
  web:  # Name of your first service/container
    image: nginx:latest  # Docker image to use from Docker Hub
    container_name: my_web_container  # Custom name for the container (optional)
    ports:  # Maps ports from container to host
      - "8080:80"  # HOST_PORT:CONTAINER_PORT
    volumes:  # Mounts files/directories from host to container
      - ./html:/usr/share/nginx/html  # HOST_PATH:CONTAINER_PATH
    environment:  # Sets environment variables
      - ENV_VAR=production
      - DEBUG=false
    networks:  # Connects service to custom networks
      - my_network
    depends_on:  # Ensures this service starts after others
      - database
    restart: always  # Restart policy (no, always, on-failure, unless-stopped)

  database:  # Name of your second service/container
    image: postgres:15  # PostgreSQL image
    container_name: my_postgres_db
    environment:  # Environment variables for postgres
      POSTGRES_USER: myuser  # Database username
      POSTGRES_PASSWORD: mypassword  # Database password
      POSTGRES_DB: mydb  # Database name
    volumes:  # Persist database data
      - db_data:/var/lib/postgresql/data  # Named volume
    networks:
      - my_network
    ports:
      - "5432:5432"

networks:  # Define custom networks
  my_network:  # Network name
    driver: bridge  # Network driver type

volumes:  # Define named volumes for data persistence
  db_data:  # Volume name
    driver: local  # Volume driver
```

---

## Complete Example with Comments

```yaml
version: '3.8'

services:
  # Frontend service
  frontend:
    build:  # Build from Dockerfile instead of using pre-built image
      context: ./frontend  # Directory containing Dockerfile
      dockerfile: Dockerfile  # Name of Dockerfile (default is "Dockerfile")
      args:  # Build arguments
        - NODE_ENV=production
    image: my-frontend:1.0  # Tag the built image
    container_name: frontend_app
    ports:
      - "3000:3000"
    environment:
      - API_URL=http://backend:5000
    volumes:
      - ./frontend/src:/app/src  # Mount source code for development
    networks:
      - app_network
    depends_on:
      - backend
    restart: unless-stopped
    command: npm start  # Override default command
    
  # Backend service
  backend:
    build: ./backend
    container_name: backend_api
    ports:
      - "5000:5000"
    environment:
      DATABASE_URL: postgresql://user:pass@db:5432/mydb
      SECRET_KEY: mysecretkey
    env_file:  # Load environment variables from file
      - .env
    volumes:
      - ./backend:/app
      - /app/node_modules  # Anonymous volume to prevent overwriting
    networks:
      - app_network
    depends_on:
      db:
        condition: service_healthy  # Wait for health check
    healthcheck:  # Define health check
      test: ["CMD", "curl", "-f", "http://localhost:5000/health"]
      interval: 30s  # Check every 30 seconds
      timeout: 10s  # Timeout after 10 seconds
      retries: 3  # Retry 3 times before marking unhealthy
      start_period: 40s  # Grace period before starting checks
    
  # Database service
  db:
    image: postgres:15-alpine
    container_name: postgres_db
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql  # Initialization script
    networks:
      - app_network
    ports:
      - "5432:5432"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user"]
      interval: 10s
      timeout: 5s
      retries: 5

  # Redis cache
  cache:
    image: redis:7-alpine
    container_name: redis_cache
    ports:
      - "6379:6379"
    networks:
      - app_network
    command: redis-server --appendonly yes  # Enable persistence
    volumes:
      - redis_data:/data

networks:
  app_network:
    driver: bridge
    ipam:  # IP Address Management
      config:
        - subnet: 172.20.0.0/16  # Custom subnet

volumes:
  postgres_data:
  redis_data:
```

---

## Major Configuration Options

### 1. **Service Level Options** (50+ options available)
- `image` - Docker image to use
- `build` - Build configuration
- `container_name` - Custom container name
- `ports` - Port mapping
- `volumes` - Volume mounting
- `environment` - Environment variables
- `env_file` - Load env vars from file
- `command` - Override default command
- `entrypoint` - Override default entrypoint
- `depends_on` - Service dependencies
- `networks` - Network connections
- `restart` - Restart policy
- `healthcheck` - Health check configuration
- `deploy` - Deployment configuration (Swarm mode)
- `labels` - Metadata labels
- `logging` - Logging configuration
- `dns` - Custom DNS servers
- `extra_hosts` - Add host mappings
- `privileged` - Run with extended privileges
- `user` - User to run container as
- `working_dir` - Working directory
- `stdin_open` - Keep STDIN open
- `tty` - Allocate pseudo-TTY
- `sysctls` - Kernel parameters
- `ulimits` - Resource limits
- `security_opt` - Security options
- `cap_add` / `cap_drop` - Linux capabilities

### 2. **Build Options**
- `context` - Build context path
- `dockerfile` - Dockerfile name
- `args` - Build arguments
- `target` - Build target stage
- `cache_from` - Images for cache
- `labels` - Image labels
- `network` - Network mode during build

### 3. **Volume Options**
- Short syntax: `HOST:CONTAINER` or `VOLUME_NAME:CONTAINER`
- Long syntax with `type`, `source`, `target`, `read_only`
- Named volumes vs bind mounts vs tmpfs

### 4. **Network Options**
- `driver` - Network driver (bridge, host, overlay, none)
- `driver_opts` - Driver-specific options
- `ipam` - IP address management
- `external` - Use existing network
- `attachable` - Allow manual container attachment

### 5. **Environment Variables**
- Array format: `- KEY=value`
- Dictionary format: `KEY: value`
- From file: `env_file: .env`

---

## Common Commands

```bash
# Start all services
docker-compose up

# Start in background (detached mode)
docker-compose up -d

# Stop all services
docker-compose down

# Stop and remove volumes
docker-compose down -v

# View logs
docker-compose logs

# View logs for specific service
docker-compose logs frontend

# Restart a service
docker-compose restart backend

# Build or rebuild services
docker-compose build

# List running containers
docker-compose ps

# Execute command in running container
docker-compose exec backend bash

# Scale a service
docker-compose up -d --scale backend=3
```

---

## Tips

1. **Use `.env` files** for sensitive data
2. **Named volumes** persist data between container restarts
3. **depends_on** only waits for container start, not readiness (use healthcheck)
4. **Version** field is now optional in newer Docker Compose versions
5. **Use `docker-compose.override.yml`** for local development overrides

---

## Total Configuration Options

Docker Compose has **100+ configuration options** across different sections:
- **Service options**: ~60+
- **Build options**: ~10
- **Network options**: ~15
- **Volume options**: ~10
- **Deploy options** (Swarm): ~20
- **Top-level directives**: version, services, networks, volumes, configs, secrets

The exact number varies by Compose file version, but the core options remain consistent.
