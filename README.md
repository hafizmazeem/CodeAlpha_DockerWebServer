# CodeAlpha_DockerWebServer

![Docker](https://img.shields.io/badge/Docker-nginx%3Aalpine-2496ED?logo=docker&logoColor=white)
![Docker Compose](https://img.shields.io/badge/Docker_Compose-restart_policy-2496ED?logo=docker&logoColor=white)
![Health Check](https://img.shields.io/badge/Health_Check-Enabled-brightgreen)

A web server running inside Docker containers, built for **Task 4: Web Server using Docker** of the CodeAlpha DevOps Internship.

This project demonstrates Docker containerization, custom Nginx image creation, Docker Compose deployment, health checks, restart policies, and hands-on troubleshooting.

## Project Links

- **GitHub Repository:** [CodeAlpha_DockerWebServer](https://github.com/hafizmazeem/CodeAlpha_DockerWebServer)
- **GitHub Profile:** [@hafizmazeem](https://github.com/hafizmazeem)
- **LinkedIn Profile:** [Hafiz Muhammad Azeem](https://linkedin.com/in/hafiz-muhammad-azeem-205190287)

## What This Project Covers

| Task | Implementation |
|---|---|
| Docker Containerization | Built a custom image using a Dockerfile |
| Web Server Deployment | Served a custom webpage using Nginx |
| Docker Compose | Managed the web server with Docker Compose |
| Port Mapping | Mapped host port 8080 to container port 80 |
| Health Monitoring | Configured a Docker health check |
| Restart Policy | Used `unless-stopped` |
| Troubleshooting | Investigated port conflicts, crash loops, and unhealthy containers |

## Project Structure

```text
CodeAlpha_DockerWebServer/
├── Dockerfile
├── docker-compose.yml
├── site/
│   └── index.html
├── troubleshooting/
│   └── broken.conf
├── screenshots/
│   ├── 01-healthy-and-page.png
│   ├── 02-dockerfile-healthcheck.png
│   ├── 03-restart-policy.png
│   ├── 04-crash-loop-logs.png
│   └── 05-unhealthy-state.png
└── README.md
```

## Prerequisites

- Docker
- Docker Compose V2
- Git

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/hafizmazeem/CodeAlpha_DockerWebServer.git
cd CodeAlpha_DockerWebServer
```

### 2. Build and Start the Server

```bash
docker compose up -d --build
```

### 3. Verify the Deployment

```bash
docker ps
curl http://localhost:8080
```

Open the website in your browser:

[http://localhost:8080](http://localhost:8080)

### 4. Stop the Server

```bash
docker compose down
```

## How It Works

### Dockerfile

```dockerfile
FROM nginx:alpine

COPY site/ /usr/share/nginx/html/

EXPOSE 80

HEALTHCHECK --interval=10s --timeout=3s --retries=3 \
  CMD wget -qO- http://localhost/ || exit 1
```

- `FROM nginx:alpine` uses a lightweight Nginx image.
- `COPY` adds the website files to the image.
- `EXPOSE 80` documents the container's web server port.
- `HEALTHCHECK` periodically checks whether the website responds successfully.

### Docker Compose

```yaml
services:
  web:
    build: .
    container_name: codealpha-web
    ports:
      - "8080:80"
    restart: unless-stopped
```

- `build: .` builds the image from the Dockerfile.
- `ports` maps host port 8080 to container port 80.
- `restart: unless-stopped` allows Docker to restart the container after unexpected exits.

## Demo and Screenshots

### 1. Running and Healthy Container

The Nginx container runs successfully, and the custom webpage is accessible.

![Healthy Container and Webpage](screenshots/01-healthy-and-page.png)

### 2. Dockerfile with Health Check

![Dockerfile Health Check](screenshots/02-dockerfile-healthcheck.png)

### 3. Automatic Restart Policy

The Nginx process was terminated from inside the container to demonstrate restart behavior.

```bash
docker exec codealpha-web kill 1
docker ps
```

![Restart Policy](screenshots/03-restart-policy.png)

### 4. Crash Loop and Logs

An intentionally invalid Nginx configuration caused repeated startup failures. Docker logs helped identify the error.

![Crash Loop and Logs](screenshots/04-crash-loop-logs.png)

### 5. Unhealthy Container

The website file was removed from the running container. The container remained running, but its health check failed.

![Unhealthy Container](screenshots/05-unhealthy-state.png)

## Troubleshooting Walkthrough

| Problem | Cause | Solution |
|---|---|---|
| Port Conflict | Host port 8080 is already in use | Stop the conflicting service or use another host port |
| Crash Loop | Invalid Nginx configuration | Check logs, fix the configuration, and recreate the container |
| Unhealthy Container | Website file is missing or health check fails | Diagnose the health check and recreate the container from the correct image |

### Reproduce a Port Conflict

Run a test container on another port:

```bash
docker run -d --name test-web -p 8081:80 nginx:alpine
```

Open [http://localhost:8081](http://localhost:8081).

Remove the test container:

```bash
docker rm -f test-web
```

### Reproduce a Crash Loop

The `troubleshooting/broken.conf` file intentionally contains an invalid Nginx directive.

```bash
docker run -d \
  --name broken \
  -p 8082:80 \
  --restart unless-stopped \
  -v "$(pwd)/troubleshooting/broken.conf:/etc/nginx/conf.d/default.conf:ro" \
  nginx:alpine

docker ps
docker logs --tail 20 broken
docker rm -f broken
```

### Reproduce an Unhealthy Container

```bash
docker exec codealpha-web rm /usr/share/nginx/html/index.html

sleep 45

docker ps
```

Restore the working container:

```bash
docker compose up -d --force-recreate
```

## Useful Docker Commands

```bash
# List running containers
docker ps

# List all containers
docker ps -a

# View logs
docker logs codealpha-web

# Open a shell inside the container
docker exec -it codealpha-web sh

# Monitor resource usage
docker stats --no-stream

# Inspect container details
docker inspect codealpha-web

# Check health status
docker inspect --format '{{.State.Health.Status}}' codealpha-web

# Stop and start the container
docker stop codealpha-web
docker start codealpha-web

# Remove the Compose deployment
docker compose down
```

## What I Learned

- Building custom Docker images using Dockerfiles.
- Deploying and managing Nginx using Docker Compose.
- Configuring port mappings, health checks, and restart policies.
- Troubleshooting container failures through logs and inspection.
- Understanding the difference between a running container and a healthy application.
- Recreating containers from a known-good image instead of relying on manual changes inside a running container.

## Author

**Hafiz Muhammad Azeem**

- **GitHub Repository:** [CodeAlpha_DockerWebServer](https://github.com/hafizmazeem/CodeAlpha_DockerWebServer)
- **GitHub:** [github.com/hafizmazeem](https://github.com/hafizmazeem)
- **LinkedIn:** [Hafiz Muhammad Azeem](https://linkedin.com/in/hafiz-muhammad-azeem-205190287)

---

*Developed as part of the CodeAlpha DevOps Internship — Task 4: Web Server using Docker.*
