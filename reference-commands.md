# Docker Bind Mounts - Command Reference Guide

- [Basic Operations](#basic-operations)
- [Configuration Management](#configuration-management)
- [Container Operations](#container-operations)
- [File Management](#file-management)
- [Troubleshooting](#troubleshooting)

> **Author**: Md Toriqul Islam  
> **Description**: Reference commands for Docker bind mount operations  
> **Note**: These are reference commands. Always verify paths and parameters before execution.

## Basic Operations

### Creating Host Files
```bash
# Create log file
touch ~/example.log

# Create NGINX configuration
cat > ~/example.conf <<EOF
server {
    listen 80;
    server_name localhost;
    access_log /var/log/nginx/custom.host.access.log main;
    location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
    }
}
EOF
```

### Setting Environment Variables
```bash
# Define source and destination paths
CONF_SRC=~/example.conf
CONF_DST=/etc/nginx/conf.d/default.conf
LOG_SRC=~/example.log
LOG_DST=/var/log/nginx/custom.host.access.log
```

## Configuration Management

### Basic Container with Bind Mounts
```bash
# Run container with bind mounts
docker run -d --name diaweb \
    --mount type=bind,src=${CONF_SRC},dst=${CONF_DST} \
    --mount type=bind,src=${LOG_SRC},dst=${LOG_DST} \
    -p 8000:80 \
    nginx:latest
```

### Read-Only Configuration
```bash
# Run container with read-only configuration
docker run -d --name diaweb \
    --mount type=bind,src=${CONF_SRC},dst=${CONF_DST},readonly=true \
    --mount type=bind,src=${LOG_SRC},dst=${LOG_DST} \
    -p 8000:80 \
    nginx:latest
```

## Container Operations

### Container Management
```bash
# Stop container
docker stop diaweb

# Remove container
docker rm -f diaweb

# List running containers
docker ps

# List all containers
docker ps -a
```

### Container Interaction
```bash
# Execute command in container
docker exec diaweb command

# Access container shell
docker exec -it diaweb /bin/bash

# View container logs
docker logs diaweb
```

## File Management

### Checking Mount Points
```bash
# View mount details
docker inspect diaweb | grep -A 10 Mounts

# Check bind mount source
docker inspect -f '{{ .HostConfig.Binds }}' diaweb
```

### File Operations
```bash
# Check NGINX configuration
docker exec diaweb nginx -t

# View log file contents
tail -f ~/example.log

# Check file permissions
ls -l ~/example.conf
ls -l ~/example.log
```

## Troubleshooting

### Common Diagnostics
```bash
# Check NGINX status
docker exec diaweb service nginx status

# Test NGINX configuration
docker exec diaweb nginx -t

# View real-time logs
docker logs -f diaweb

# Check mount points
docker inspect -f '{{range .Mounts}}{{.Source}} -> {{.Destination}}{{"\n"}}{{end}}' diaweb
```

### Network Verification
```bash
# Test NGINX accessibility
curl http://localhost:8000

# Check port mapping
docker port diaweb

# View network settings
docker inspect diaweb | grep -A 20 NetworkSettings
```

### Permission Issues
```bash
# Check file ownership
ls -l ${CONF_SRC}
ls -l ${LOG_SRC}

# Modify file permissions if needed
chmod 644 ${CONF_SRC}
chmod 666 ${LOG_SRC}
```