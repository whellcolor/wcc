# 📁 `docker/`

---

## 1. `backend.Dockerfile`

Cocok untuk Node.js (Express / NestJS / API Web3 backend)

```dockerfile id="backend_docker"
# Base image
FROM node:20-alpine

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy source code
COPY . .

# Build app (if TypeScript)
RUN npm run build

# Expose backend port
EXPOSE 4000

# Start app
CMD ["npm", "run", "start"]
```

---

## 2. `frontend.Dockerfile`

Untuk Next.js / React frontend

```dockerfile id="frontend_docker"
# Base image
FROM node:20-alpine

# Set working directory
WORKDIR /app

# Copy dependencies
COPY package*.json ./

# Install deps
RUN npm install

# Copy all files
COPY . .

# Build Next.js app
RUN npm run build

# Expose frontend port
EXPOSE 3000

# Start production server
CMD ["npm", "run", "start"]
```

---

## 3. `nginx.conf`

Untuk reverse proxy (frontend + backend routing)

```nginx id="nginx_conf"
events {}

http {
  upstream frontend {
    server frontend:3000;
  }

  upstream backend {
    server backend:4000;
  }

  server {
    listen 80;

    # FRONTEND (Next.js)
    location / {
      proxy_pass http://frontend;
      proxy_http_version 1.1;
      proxy_set_header Host $host;
    }

    # BACKEND API
    location /api/ {
      proxy_pass http://backend/;
      proxy_http_version 1.1;
      proxy_set_header Host $host;
    }
  }
}
```

---

# 🚀 (OPTIONAL) `docker-compose.yml` (Recommended)

Kalau kamu mau ini langsung jalan 1 command:

```yml id="docker_compose"
version: "3.9"

services:
  frontend:
    build:
      context: ./frontend
      dockerfile: ../docker/frontend.Dockerfile
    ports:
      - "3000:3000"

  backend:
    build:
      context: ./backend
      dockerfile: ../docker/backend.Dockerfile
    ports:
      - "4000:4000"

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./docker/nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - frontend
      - backend
```

---

# 🧠 Arsitektur sistem

```
                ┌──────────────┐
                │   NGINX      │
                │ Reverse Proxy│
                └──────┬───────┘
                       │
        ┌──────────────┴──────────────┐
        │                             │
 ┌──────────────┐            ┌──────────────┐
 │  Frontend    │            │   Backend    │
 │  Next.js     │            │  API / Web3  │
 └──────────────┘            └──────────────┘
```

---

# 🔥 Upgrade yang bisa kamu tambah

Kalau mau level production:

* 🚀 multi-stage Docker build (lebih ringan 80%)
* 🔐 env secrets injection (Vault / AWS SSM)
* 📦 caching layer (Redis)
* ⚡ auto scaling (Kubernetes)
* 🌐 CDN frontend (Cloudflare)
* 🧠 AI backend worker container

---


