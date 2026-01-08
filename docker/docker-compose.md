# Docker Compose

To create the "most advanced" Docker Compose file, we must move beyond simple container orchestration and leverage the Compose Specification features introduced in 2024 and 2025.

The following example is designed for a production-grade, microservices architecture. It demonstrates Modularization (`include`), Advanced Networking (custom drivers), Health Checks, Profiles, Resource Limits, and Development Watchers.

#### The "Ultimate" Docker Compose File

This setup assumes a multi-service app (API, Worker, Database, Cache, and Reverse Proxy).

YAML

```
# Note: Version tags are now optional in the Compose Specification
name: enterprise-stack

# 1. MODULARITY: Include other compose files (New in 2024)
# This allows you to split databases, monitoring, or legacy services into separate files.
include:
  - path: ./infrastructure/logging-stack.yaml
  - path: ./infrastructure/db-cluster.yaml

# 2. REUSABLE FRAGMENTS (YAML Anchors)
# Avoid repetition for common configurations like logging and environment.
x-common-configs: &default-logging
  logging:
    driver: "json-file"
    options:
      max-size: "100m"
      max-file: "3"

x-resource-limits: &standard-resources
  deploy:
    resources:
      limits:
        cpus: '0.5'
        memory: 512M
      reservations:
        cpus: '0.25'
        memory: 256M

services:
  # --- API GATEWAY / REVERSE PROXY ---
  proxy:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/conf.d:/etc/nginx/conf.d:ro
    networks:
      - public_net
      - private_net
    depends_on:
      api:
        condition: service_healthy
    <<: *default-logging

  # --- MAIN BACKEND SERVICE ---
  api:
    build:
      context: ./api
      dockerfile: Dockerfile
      target: runner # Multi-stage build target
    environment:
      - NODE_ENV=production
      - DB_URL=postgres://user:${DB_PASSWORD}@db:5432/myapp
    secrets:
      - db_password
    configs:
      - source: app_config
        target: /app/config.json
    networks:
      - private_net
    deploy:
      replicas: 3 # Deploy three replicas of the container
      update_config:
        parallelism: 1
        delay: 10s
        order: start-first # Zero-downtime deployments
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
    <<: [*standard-resources, *default-logging]
    
    # NEW: Compose Watch (Development only)
    develop:
      watch:
        - action: sync
          path: ./api
          target: /app
          ignore:
            - node_modules/
        - action: rebuild
          path: ./api/package.json

  # --- BACKGROUND WORKER ---
  worker:
    image: my-app-worker:latest
    profiles: ["background-tasks"] # Only starts if --profile background-tasks is called
    depends_on:
      redis:
        condition: service_started
    networks:
      - private_net
    <<: *default-logging

  # --- REDIS CACHE ---
  redis:
    image: redis:7-alpine
    command: ["redis-server", "--appendonly", "yes"]
    volumes:
      - redis_data:/data
    networks:
      - private_net

# 3. ADVANCED NETWORKING
networks:
  public_net:
    driver: bridge
  private_net:
    internal: true # Isolates backend from the outside world

# 4. PERSISTENT STORAGE
volumes:
  redis_data:
    driver: local

# 5. SECRETS MANAGEMENT
# Secrets are mounted as files in /run/secrets/ within the container
secrets:
  db_password:
    file: ./secrets/db_password.txt

# 6. EXTERNAL CONFIGURATIONS
configs:
  app_config:
    file: ./configs/prod_config.json
```

***

#### Why this is "Advanced":

1. `include` Directive: Instead of one 500-line file, you can now import other YAML files. This is the industry standard for managing complexity in 2025.
2. `develop: watch`: This replaces `nodemon` or manual restarts. It automatically syncs files from your host to the container or triggers a rebuild when specific files (like `package.json`) change.
3. `profiles`: Allows you to define services (like a Heavy Worker or a Debugger) that don't start by default unless specifically requested via `docker compose --profile background-tasks up`.
4. `deploy: update_config`: Enables Zero-Downtime Deployment patterns by starting the new container version _before_ killing the old one (`start-first`).
5. Secrets & Configs: Instead of insecure environment variables, it uses Docker Secrets and Configs, which are mounted as read-only files in memory, preventing credential leakage in logs.
6. Health Check Dependencies: The `proxy` won't start until the `api` is not just "running," but actually "healthy" (returning a 200 OK).
7. YAML Anchors (`&` and `<<: *`): Prevents "DRY" (Don't Repeat Yourself) violations by sharing logging and resource limit templates across multiple services.

### Advanced but useful concepts

{% embed url="https://docs.docker.com/compose/compose-file/10-fragments/" %}

{% embed url="https://docs.docker.com/reference/compose-file/develop/" %}
