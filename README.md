# CodeAlpha_DockerWebServer

A small web server running in Docker (nginx), built for Task 4 of the CodeAlpha DevOps internship.

## What's inside
- `site/index.html`: the web page
- `Dockerfile`: nginx image with the page baked in and a health check
- `docker-compose.yml`: runs it with a port mapping and an automatic restart policy

## Run it
```bash
git clone https://github.com/hafizmazeem/CodeAlpha_DockerWebServer.git
cd CodeAlpha_DockerWebServer
docker compose up -d --build
curl localhost:8080
```

## What I practiced
- Container lifecycle: run, stop, start, remove, and inspecting with `logs`, `exec`, `stats` and `inspect`
- Serving my own page with a bind mount, then baking it into my own image with a Dockerfile
- A `HEALTHCHECK` so Docker can tell when the server is running but failing
- A `restart: unless-stopped` policy, tested by crashing nginx from the inside
- Troubleshooting three failures: a port conflict, a crash loop caused by a bad config, and an unhealthy container

## What I learned
- Learned how to run and manage a web server inside a Docker container.
- Practiced using core Docker commands to control the container lifecycle.
- Figured out how to monitor container health and troubleshoot common issues.
- Got familiar with standard best practices for deploying containerized apps.
