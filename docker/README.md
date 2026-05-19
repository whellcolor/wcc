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


# 🚀 Dukung Proyek Ini

## 🪙 Wallet Ethereum
Scan QR berikut untuk mengirim donasi:

![QR Ethereum](https://api.qrserver.com/v1/create-qr-code/?size=220x220&data=ethereum:0xd8519A8b8825Aa0DcC73aAD572f447FAE102fe88)

Atau kirim langsung ke wallet:  
`0xd8519A8b8825Aa0DcC73aAD572f447FAE102fe88`  

🔗 [Buka di Etherscan](https://etherscan.io/address/0xd8519A8b8825Aa0DcC73aAD572f447FAE102fe88)

---

## 💳 Langganan Layanan (PayPal)

Pilih paket langganan sesuai kebutuhan:  

[![Subscribe](https://www.paypalobjects.com/en_US/i/btn/btn_subscribe_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=JZ8YZT9LM257A)

📌 Paket tersedia:  

| Paket      | Harga | Durasi |
|-----------|-------|--------|
| Subcribe  | $5    | Mingguan |
| Premium   | $25   | Bulanan |
| Golden    | $100  | Tahunan |

---

## 📌 Informasi Tambahan

<details>
<summary>FAQ / Cara Donasi</summary>

1. **Bagaimana cara scan QR?**  
   Gunakan wallet Ethereum seperti MetaMask, TrustWallet, atau Rainbow untuk scan kode QR di atas.  

2. **Bagaimana berlangganan via PayPal?**  
   Klik tombol PayPal, pilih paket yang kamu inginkan, lalu konfirmasi pembayaran.  

3. **Apakah aman?**  
   Semua transaksi dilakukan langsung melalui wallet atau PayPal resmi, tidak ada penyimpanan dana di server proyek.
</details>

---

## 🔗 Tautan Lain
- [Thirdweb Project](https://thirdweb.com/)  
- [Ethereum Explorer](https://etherscan.io/)  
- [Supabase](https://supabase.com/)  

---

> ⚠️ **Catatan:** Jangan pernah menaruh private key atau secret key di README.md atau repository publik.

