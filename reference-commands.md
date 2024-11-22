# Docker Bind Mounts Command Reference Guide

> **Author**: Md Toriqul Islam  
> **Description**: Comprehensive command reference for Docker bind mount operations and NGINX configuration  
> **Learning Focus**: Docker storage management, Linux file systems, and NGINX configuration  
> **Note**: Review and understand each command before execution in production environments.

- [Section 1: Docker Volume Operations](#section-1-docker-volume-operations)
- [Section 2: Container Configuration](#section-2-container-configuration)
- [Section 3: NGINX Service Operations](#section-3-nginx-service-operations)
- [Section 4: Mount Monitoring](#section-4-mount-monitoring)
- [Section 5: File System Maintenance](#section-5-file-system-maintenance)
- [Section 6: Security Operations](#section-6-security-operations)
- [Section 7: Performance Optimization](#section-7-performance-optimization)

## Section 1: Docker Volume Operations

### Basic Bind Mount Operations
```bash
# Create container with bind mount
docker run -d \
    --name nginx-server \
    -v /host/path:/container/path:ro \
    nginx:latest

# List all mounts for a container
docker inspect -f '{{.Mounts}}' container_name

# Remove container with volume
docker rm -v container_name
```

### Advanced Mount Operations
```bash
# Multiple bind mounts with different permissions
docker run -d \
    --name nginx-advanced \
    -v /host/config:/etc/nginx/conf.d:ro \
    -v /host/logs:/var/log/nginx:rw \
    -v /host/html:/usr/share/nginx/html:ro \
    nginx:latest

# Create named volume with bind mount
docker volume create --driver local \
    --opt type=none \
    --opt device=/host/path \
    --opt o=bind \
    volume_name
```

## Section 2: Container Configuration

### NGINX Configuration
```bash
# Validate NGINX configuration
docker exec nginx-server nginx -t

# Reload NGINX configuration
docker exec nginx-server nginx -s reload

# Create custom NGINX configuration
cat > nginx.conf << EOF
server {
    listen 80;
    server_name localhost;
    root /usr/share/nginx/html;
    
    location / {
        index index.html;
    }
}
EOF
```

### Container Environment Setup
```bash
# Set environment variables
export NGINX_HOST=localhost
export NGINX_PORT=80

# Create container with environment variables
docker run -d \
    --name nginx-env \
    -e NGINX_HOST=${NGINX_HOST} \
    -e NGINX_PORT=${NGINX_PORT} \
    nginx:latest
```

## Section 3: Service Operations

### Container Management
```bash
# Start NGINX container
docker start nginx-server

# Stop NGINX container
docker stop nginx-server

# Restart NGINX container
docker restart nginx-server

# Check container logs
docker logs -f nginx-server
```

### Process Control
```bash
# Check NGINX process
docker exec nginx-server ps aux | grep nginx

# Send signals to NGINX
docker exec nginx-server nginx -s [stop|quit|reload|reopen]
```

## Section 4: Mount Monitoring

### Mount Status
```bash
# View mount points
docker inspect -f '{{range .Mounts}}{{.Source}} -> {{.Destination}}{{"\n"}}{{end}}' nginx-server

# Check bind mount permissions
ls -la /host/path

# Monitor mount point usage
df -h /host/path
```

### Log Management
```bash
# View NGINX access logs
tail -f /host/logs/access.log

# View NGINX error logs
tail -f /host/logs/error.log

# Rotate log files
logrotate /etc/logrotate.d/nginx
```

## Section 5: File System Maintenance

### Routine Maintenance
```bash
# Check file system status
df -h
du -sh /host/path

# Clean old logs
find /host/logs -type f -name "*.log" -mtime +30 -delete

# Backup mounted volumes
tar -czf backup.tar.gz /host/path
```

## Section 6: Security Operations

### Permission Management
```bash
# Set correct permissions
chmod 644 /host/config/*.conf
chmod 755 /host/html
chmod 766 /host/logs

# Change ownership
chown -R nginx:nginx /host/logs
```

### Security Checks
```bash
# Audit file permissions
find /host/path -type f -ls

# Check SELinux context
ls -Z /host/path

# Verify mount options
mount | grep /host/path
```

## Section 7: Performance Optimization

### Cache Management
```bash
# Clear NGINX cache
docker exec nginx-server rm -rf /var/cache/nginx/*

# Set up cache directory
docker exec nginx-server mkdir -p /var/cache/nginx
```

### Resource Management
```bash
# Monitor container resources
docker stats nginx-server

# Update container resources
docker update --memory 512m --cpus 2 nginx-server
```

## Learning Notes

1. Bind mounts provide direct host-container file system integration
2. Read-only mounts enhance security
3. Proper permission management is crucial
4. Regular monitoring prevents storage issues
5. Backup strategies are essential for data safety

---

> 💡 **Best Practice**: Always use specific paths and versions in production environments

> ⚠️ **Warning**: Incorrect bind mount permissions can lead to security vulnerabilities

> 📝 **Note**: Test configuration changes in development before applying to production
