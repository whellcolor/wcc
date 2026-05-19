# 📁 `database/`

```txt
database/
├── migrations/
├── schema.sql
└── seed.ts
```

---

# 1. `schema.sql` (Core Database Design)

Ini contoh schema untuk platform:

* user
* deploy token
* staking
* revenue tracking

```sql id="db_schema"
-- USERS TABLE
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(100),
  email VARCHAR(150) UNIQUE NOT NULL,
  role VARCHAR(20) DEFAULT 'user',
  created_at TIMESTAMP DEFAULT NOW()
);

-- DEPLOYS TABLE (smart contract deployments)
CREATE TABLE deploys (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id),
  chain VARCHAR(50),
  contract_address VARCHAR(255),
  status VARCHAR(20) DEFAULT 'pending',
  created_at TIMESTAMP DEFAULT NOW()
);

-- STAKING TABLE
CREATE TABLE stakes (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id),
  amount DECIMAL NOT NULL,
  apy DECIMAL NOT NULL,
  rewards DECIMAL DEFAULT 0,
  created_at TIMESTAMP DEFAULT NOW()
);

-- REVENUE TABLE
CREATE TABLE revenue (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  source VARCHAR(50),
  amount DECIMAL NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);

-- TREASURY TABLE
CREATE TABLE treasury (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  balance DECIMAL DEFAULT 0,
  updated_at TIMESTAMP DEFAULT NOW()
);
```

---

# 2. `seed.ts` (Initial Data Seeder)

Biasanya pakai Node.js + PostgreSQL (pg)

```ts id="seed_ts"
import { Pool } from "pg";

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
});

async function seed() {
  try {
    console.log("Seeding database...");

    // USERS
    await pool.query(`
      INSERT INTO users (name, email, role)
      VALUES
      ('Admin', 'admin@platform.com', 'admin'),
      ('User One', 'user1@platform.com', 'user')
    `);

    // TREASURY
    await pool.query(`
      INSERT INTO treasury (balance)
      VALUES (100000)
    `);

    // REVENUE
    await pool.query(`
      INSERT INTO revenue (source, amount)
      VALUES
      ('deploy_fee', 500),
      ('staking_fee', 300)
    `);

    console.log("Seed completed successfully");
  } catch (err) {
    console.error("Seed error:", err);
  } finally {
    await pool.end();
  }
}

seed();
```

---

# 3. `migrations/` (Folder Structure)

Biasanya isi file seperti ini:

```txt
migrations/
├── 001_create_users.sql
├── 002_create_deploys.sql
├── 003_create_staking.sql
├── 004_create_revenue.sql
└── 005_create_treasury.sql
```

---

## Contoh migration file

### `001_create_users.sql`

```sql id="mig_users"
CREATE TABLE IF NOT EXISTS users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT,
  email TEXT UNIQUE NOT NULL,
  role TEXT DEFAULT 'user',
  created_at TIMESTAMP DEFAULT NOW()
);
```

---

### `002_create_deploys.sql`

```sql id="mig_deploys"
CREATE TABLE IF NOT EXISTS deploys (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id),
  chain TEXT,
  contract_address TEXT,
  status TEXT DEFAULT 'pending',
  created_at TIMESTAMP DEFAULT NOW()
);
```

---

### `003_create_staking.sql`

```sql id="mig_staking"
CREATE TABLE IF NOT EXISTS stakes (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id),
  amount NUMERIC,
  apy NUMERIC,
  rewards NUMERIC DEFAULT 0,
  created_at TIMESTAMP DEFAULT NOW()
);
```

---

### `004_create_revenue.sql`

```sql id="mig_revenue"
CREATE TABLE IF NOT EXISTS revenue (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  source TEXT,
  amount NUMERIC,
  created_at TIMESTAMP DEFAULT NOW()
);
```

---

### `005_create_treasury.sql`

```sql id="mig_treasury"
CREATE TABLE IF NOT EXISTS treasury (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  balance NUMERIC DEFAULT 0,
  updated_at TIMESTAMP DEFAULT NOW()
);
```

---

# 🧠 Arsitektur Database Ini

```txt
Users
  ↓
Deploys (multi-chain contracts)
  ↓
Stakes (DeFi staking system)
  ↓
Revenue (platform income)
  ↓
Treasury (fund management)
```

---

# 🚀 Upgrade Level Production (kalau mau)

Aku bisa bantu kamu naikkan sistem ini jadi:

### 🔥 1. Prisma ORM version

* type-safe schema
* migration auto
* relations cleaner

### 🔥 2. Multi-chain indexing

* listen event dari Ethereum / BSC / Base

### 🔥 3. Real-time analytics

* staking APY live
* revenue dashboard streaming

### 🔥 4. Web3 DB hybrid

* PostgreSQL + Redis + TheGraph

---

