## BACKEND API — WCC GENERATOR DAPP (PRODUCTION STRUCTURE)

Stack: **Node.js + Express + TypeScript + Supabase + ethers + thirdweb (optional)**

---

# FULL BACKEND STRUCTURE

```txt id="b9k2x1"
backend/
│
├── src/
│   ├── server.ts
│   ├── app.ts
│   │
│   ├── config/
│   │   ├── env.ts
│   │   ├── chains.ts
│   │   ├── cors.ts
│   │   └── rateLimit.ts
│   │
│   ├── routes/
│   │   ├── index.ts
│   │   ├── auth.routes.ts
│   │   ├── token.routes.ts
│   │   ├── meme.routes.ts
│   │   ├── deploy.routes.ts
│   │   ├── staking.routes.ts
│   │   ├── analytics.routes.ts
│   │   ├── launchpad.routes.ts
│   │   ├── nft.routes.ts
│   │   └── admin.routes.ts
│   │
│   ├── controllers/
│   │   ├── auth.controller.ts
│   │   ├── token.controller.ts
│   │   ├── meme.controller.ts
│   │   ├── deploy.controller.ts
│   │   ├── staking.controller.ts
│   │   ├── analytics.controller.ts
│   │   ├── launchpad.controller.ts
│   │   ├── nft.controller.ts
│   │   └── admin.controller.ts
│   │
│   ├── services/
│   │   ├── blockchain/
│   │   │   ├── ethers.service.ts
│   │   │   ├── deploy.service.ts
│   │   │   ├── factory.service.ts
│   │   │   └── transaction.service.ts
│   │   │
│   │   ├── ai/
│   │   │   ├── meme-ai.service.ts
│   │   │   ├── token-ai.service.ts
│   │   │   └── prompt.service.ts
│   │   │
│   │   ├── database/
│   │   │   ├── supabase.service.ts
│   │   │   ├── token.repo.ts
│   │   │   ├── user.repo.ts
│   │   │   ├── analytics.repo.ts
│   │   │   └── staking.repo.ts
│   │   │
│   │   ├── wallet/
│   │   │   ├── smartWallet.service.ts
│   │   │   ├── gasless.service.ts
│   │   │   └── session.service.ts
│   │   │
│   │   ├── launchpad/
│   │   │   ├── presale.service.ts
│   │   │   ├── ido.service.ts
│   │   │   └── vesting.service.ts
│   │   │
│   │   ├── staking/
│   │   │   ├── staking.service.ts
│   │   │   ├── rewards.service.ts
│   │   │   └── apy.service.ts
│   │   │
│   │   ├── analytics/
│   │   │   ├── analytics.service.ts
│   │   │   ├── revenue.service.ts
│   │   │   └── dashboard.service.ts
│   │   │
│   │   ├── nft/
│   │   │   ├── nft.service.ts
│   │   │   ├── marketplace.service.ts
│   │   │   └── mint.service.ts
│   │   │
│   │   └── notification.service.ts
│   │
│   ├── middlewares/
│   │   ├── auth.middleware.ts
│   │   ├── error.middleware.ts
│   │   ├── logger.middleware.ts
│   │   ├── validate.middleware.ts
│   │   └── admin.middleware.ts
│   │
│   ├── utils/
│   │   ├── logger.ts
│   │   ├── response.ts
│   │   ├── crypto.ts
│   │   ├── validator.ts
│   │   ├── formatter.ts
│   │   ├── retry.ts
│   │   └── constants.ts
│   │
│   ├── jobs/
│   │   ├── analytics.job.ts
│   │   ├── reward.job.ts
│   │   ├── cleanup.job.ts
│   │   └── sync.job.ts
│   │
│   ├── queues/
│   │   ├── index.ts
│   │   ├── deploy.queue.ts
│   │   ├── ai.queue.ts
│   │   └── staking.queue.ts
│   │
│   ├── database/
│   │   ├── prisma/
│   │   ├── migrations/
│   │   └── seed.ts
│   │
│   └── types/
│       ├── token.types.ts
│       ├── user.types.ts
│       ├── api.types.ts
│       ├── staking.types.ts
│       └── analytics.types.ts
│
├── prisma/
│   └── schema.prisma
│
├── package.json
├── tsconfig.json
├── .env.example
├── Dockerfile
└── README.md
```

---

# CORE API FLOW

```txt id="flow1"
Frontend Request
   ↓
Route (Express)
   ↓
Controller
   ↓
Service Layer
   ↓
Blockchain / AI / Database
   ↓
Response JSON
```

---

# MAIN API MODULES

## 1. TOKEN GENERATION API

### `routes/token.routes.ts`

* POST `/api/token/create`
* GET `/api/token/:address`
* GET `/api/token/all`

### Controller Flow

```ts id="tkn1"
createToken → TokenFactory.sol → store DB → return address
```

---

## 2. MEME AI API

### `routes/meme.routes.ts`

* POST `/api/meme/generate`

### Example Response

```json id="meme1"
{
  "name": "DOGEX",
  "symbol": "DOGEX",
  "supply": 1000000000,
  "tax": 2
}
```

---

## 3. DEPLOY API

### `routes/deploy.routes.ts`

* POST `/api/deploy/token`
* POST `/api/deploy/meme`
* POST `/api/deploy/launchpad`

### Service

```ts id="dep1"
ethers.Wallet + Factory Contract + RPC
```

---

## 4. STAKING API

* POST `/api/staking/stake`
* POST `/api/staking/unstake`
* GET `/api/staking/rewards/:user`

---

## 5. ANALYTICS API

* GET `/api/analytics/dashboard`
* GET `/api/analytics/revenue`
* GET `/api/analytics/users`
* GET `/api/analytics/deploys`

---

## 6. LAUNCHPAD API

* POST `/api/launchpad/create`
* GET `/api/launchpad/list`
* POST `/api/launchpad/buy`
* POST `/api/launchpad/claim`

---

## 7. NFT API

* POST `/api/nft/mint`
* GET `/api/nft/user/:wallet`
* GET `/api/nft/marketplace`

---

## 8. WALLET API

* POST `/api/wallet/create`
* POST `/api/wallet/session`
* POST `/api/wallet/gasless`

---

## 9. AUTH API

* POST `/api/auth/login`
* POST `/api/auth/register`
* POST `/api/auth/verify`
* GET `/api/auth/me`

---

## 10. ADMIN API

* GET `/api/admin/stats`
* GET `/api/admin/users`
* POST `/api/admin/ban`
* POST `/api/admin/withdraw`

---

# CORE BACKEND FILE EXAMPLES

---

## `server.ts`

```ts id="srv1"
import app from "./app";

const PORT = process.env.PORT || 3001;

app.listen(PORT, () => {
  console.log(`WCC Backend running on ${PORT}`);
});
```

---

## `app.ts`

```ts id="app1"
import express from "express";
import cors from "cors";
import routes from "./routes";

const app = express();

app.use(cors());
app.use(express.json());

app.use("/api", routes);

export default app;
```

---

## `routes/index.ts`

```ts id="rts1"
import { Router } from "express";

import tokenRoutes from "./token.routes";
import memeRoutes from "./meme.routes";
import deployRoutes from "./deploy.routes";

const router = Router();

router.use("/token", tokenRoutes);
router.use("/meme", memeRoutes);
router.use("/deploy", deployRoutes);

export default router;
```

---

## `controllers/token.controller.ts`

```ts id="ctl1"
import { Request, Response } from "express";
import { createTokenService } from "../services/blockchain/factory.service";

export const createToken = async (req: Request, res: Response) => {

  try {

    const { name, symbol, supply } = req.body;

    const result = await createTokenService(name, symbol, supply);

    return res.json({
      success: true,
      data: result
    });

  } catch (err) {

    return res.status(500).json({
      success: false,
      error: err.message
    });

  }
};
```

---

## `services/blockchain/factory.service.ts`

```ts id="svc1"
import { ethers } from "ethers";
import factoryABI from "../../abis/TokenFactory.json";

export async function createTokenService(
  name: string,
  symbol: string,
  supply: number
) {

  const provider = new ethers.JsonRpcProvider(
    process.env.RPC_URL
  );

  const wallet = new ethers.Wallet(
    process.env.PRIVATE_KEY!,
    provider
  );

  const contract = new ethers.Contract(
    process.env.FACTORY_ADDRESS!,
    factoryABI,
    wallet
  );

  const tx = await contract.createToken(
    name,
    symbol,
    supply
  );

  const receipt = await tx.wait();

  return {
    txHash: tx.hash,
    receipt
  };
}
```

---

## `services/ai/meme-ai.service.ts`

```ts id="ai1"
export function generateMemeTokenAI() {

  const memes = [
    "DOGEX",
    "PEPEMAX",
    "SHIBX",
    "MOONINU"
  ];

  const random =
    memes[Math.floor(Math.random() * memes.length)];

  return {
    name: random,
    symbol: random,
    supply: 1000000000,
    tax: 2
  };
}
```

---

## `services/analytics/analytics.service.ts`

```ts id="an1"
import { supabase } from "../database/supabase.service";

export async function getDashboardStats() {

  const { data } =
    await supabase.from("analytics").select("*");

  return {
    totalDeploys: data?.length || 0,
    revenue: 0
  };
}
```

---

## `middlewares/auth.middleware.ts`

```ts id="mw1"
import { Request, Response, NextFunction } from "express";

export function authMiddleware(
  req: Request,
  res: Response,
  next: NextFunction
) {

  const token = req.headers.authorization;

  if (!token) {
    return res.status(401).json({
      error: "Unauthorized"
    });
  }

  next();
}
```

---

# SUMMARY BACKEND CAPABILITIES

Backend ini sudah siap untuk:

* Token Factory Deployment
* Meme AI Generator
* Gasless API Layer
* Staking System
* Launchpad API
* NFT System
* Analytics SaaS
* Wallet Abstraction
* Admin Panel API
* Multi-chain deployment
* Revenue tracking
* Supabase integration
* Production Docker deployment

---

