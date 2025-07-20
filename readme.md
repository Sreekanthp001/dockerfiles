# 🐳 Docker Image Building Best Practices

This guide outlines best practices for building efficient, secure, and production-ready Docker images.

---

## 📦 1. Use a Small Base Image

- ✅ Prefer minimal images like `alpine`, `debian:slim`, or `busybox`.
- ❌ Avoid large base images like `ubuntu` unless necessary.

```Dockerfile
FROM node:18-alpine


2. Use Multi-Stage Builds
Separate build dependencies from the final image for a smaller, cleaner result.

Dockerfile
Copy

Edit
# Stage 1: Builder
FROM golang:1.20-alpine AS builder
WORKDIR /app
COPY . .
RUN go build -o myapp

# Stage 2: Runtime
FROM alpine
COPY --from=builder /app/myapp /usr/local/bin/myapp
ENTRYPOINT ["myapp"]


🧹 3. Minimize Layers
Use && to combine related RUN commands.

Dockerfile
Copy

Edit
RUN apt-get update && \
    apt-get install -y curl git && \
    rm -rf /var/lib/apt/lists/*
🚫 4. Use .dockerignore
Avoid copying unnecessary files into the image.

dockerignore
Copy

Edit
.git
node_modules
*.log
Dockerfile
README.md


🔐 5. Avoid Hardcoding Secrets
Never include sensitive data in your Dockerfile.

✅ Use:

Environment variables via --env-file

Docker secrets (Swarm)

External secrets managers (e.g., AWS Secrets Manager, Vault)


📌 6. Use Specific Image Tags
Pin image versions for reproducible builds.

Dockerfile
Copy
Edit
FROM nginx:1.25.1-alpine


📁 7. Use COPY Instead of ADD
Use ADD only for .tar.gz or remote URLs.

Dockerfile
Copy

Edit
COPY ./app /app


❤️ 8. Add Health Checks
Helps detect when the container is not working as expected.

Dockerfile
Copy
Edit
HEALTHCHECK CMD curl --fail http://localhost:8080/health || exit 1


🔒 9. Run as Non-Root User
Reduce security risks by creating a non-root user.

Dockerfile
Copy

Edit
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser


🏷️ 10. Label Your Image
Add useful metadata to your image.

Dockerfile
Copy

Edit
LABEL maintainer="yourname@example.com"
LABEL version="1.0"
LABEL description="My App Docker Image"