# full contoh GitHub Actions CI/CD workflows

```
.github/workflows/
├── frontend.yml
├── contracts.yml
└── backend.yml
```

Ini dibuat untuk project **full-stack Web3 (frontend + backend + smart contracts)**.

---

# 📁 `.github/workflows/`

---

## 1. `frontend.yml` (Next.js CI + Build)

```yml id="frontend_ci"
name: Frontend CI

on:
  push:
    branches: [ "main" ]
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: "npm"

      - name: Install dependencies
        run: npm install
        working-directory: ./frontend

      - name: Lint
        run: npm run lint
        working-directory: ./frontend

      - name: Build
        run: npm run build
        working-directory: ./frontend
```

---

## 2. `contracts.yml` (Hardhat Test + Compile + Deploy Check)

```yml id="contracts_ci"
name: Smart Contracts CI

on:
  push:
    branches: [ "main" ]
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: "npm"

      - name: Install dependencies
        run: npm install
        working-directory: ./contracts

      - name: Compile contracts
        run: npx hardhat compile
        working-directory: ./contracts

      - name: Run tests
        run: npx hardhat test
        working-directory: ./contracts
```

---

## 3. `backend.yml` (API CI - Express / NestJS / Node)

```yml id="backend_ci"
name: Backend CI

on:
  push:
    branches: [ "main" ]
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: "npm"

      - name: Install dependencies
        run: npm install
        working-directory: ./backend

      - name: Run lint
        run: npm run lint
        working-directory: ./backend

      - name: Run tests
        run: npm test
        working-directory: ./backend
```

---

# 🚀 OPTIONAL (Upgrade Level Production)

Kalau kamu mau sistem ini jadi **real DevOps pipeline Web3 SaaS**, kamu bisa tambah:

---

## 🔥 Auto Deploy Frontend (Vercel)

Tambahkan ke `frontend.yml`:

```yml
      - name: Deploy to Vercel
        run: npx vercel --prod --token=${{ secrets.VERCEL_TOKEN }}
        working-directory: ./frontend
```

---

## 🔥 Auto Deploy Contracts (Sepolia / BSC / Polygon)

```yml
      - name: Deploy Contracts
        run: npx hardhat run scripts/deploy.ts --network sepolia
        working-directory: ./contracts
        env:
          PRIVATE_KEY: ${{ secrets.PRIVATE_KEY }}
```

---

## 🔐 Secrets yang harus ditambahkan di GitHub

* `PRIVATE_KEY`
* `RPC_URL_ETH`
* `RPC_URL_BSC`
* `RPC_URL_POLYGON`
* `VERCEL_TOKEN`
* `INFURA_API_KEY` / `ALCHEMY_KEY`

---

# 🧠 Arsitektur CI/CD ini

Pipeline ini otomatis:

### Frontend

* lint → build → deploy

### Backend

* test → lint → validate API

### Contracts

* compile → test → deploy

---

