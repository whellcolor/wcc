# 📁 `scripts/`

---

## 1. `deploy-ethereum.ts`

```ts id="eth_deploy"
import { ethers } from "hardhat";

async function main() {
  const [deployer] = await ethers.getSigners();

  console.log("Deploying to Ethereum with:", deployer.address);

  const Token = await ethers.getContractFactory("MyToken");
  const token = await Token.deploy();

  await token.deployed();

  console.log("Ethereum Token deployed at:", token.address);
}

main().catch((error) => {
  console.error(error);
  process.exit(1);
});
```

---

## 2. `deploy-bsc.ts`

```ts id="bsc_deploy"
import { ethers } from "hardhat";

async function main() {
  const [deployer] = await ethers.getSigners();

  console.log("Deploying to BSC:", deployer.address);

  const Token = await ethers.getContractFactory("MyToken");
  const token = await Token.deploy();

  await token.deployed();

  console.log("BSC Token deployed at:", token.address);
}

main().catch(console.error);
```

---

## 3. `deploy-polygon.ts`

```ts id="poly_deploy"
import { ethers } from "hardhat";

async function main() {
  const [deployer] = await ethers.getSigners();

  console.log("Deploying to Polygon:", deployer.address);

  const Token = await ethers.getContractFactory("MyToken");
  const token = await Token.deploy();

  await token.deployed();

  console.log("Polygon Token deployed at:", token.address);
}

main().catch(console.error);
```

---

## 4. `deploy-base.ts`

```ts id="base_deploy"
import { ethers } from "hardhat";

async function main() {
  const [deployer] = await ethers.getSigners();

  console.log("Deploying to Base:", deployer.address);

  const Token = await ethers.getContractFactory("MyToken");
  const token = await Token.deploy();

  await token.deployed();

  console.log("Base Token deployed at:", token.address);
}

main().catch(console.error);
```

---

## 5. `deploy-arbitrum.ts`

```ts id="arb_deploy"
import { ethers } from "hardhat";

async function main() {
  const [deployer] = await ethers.getSigners();

  console.log("Deploying to Arbitrum:", deployer.address);

  const Token = await ethers.getContractFactory("MyToken");
  const token = await Token.deploy();

  await token.deployed();

  console.log("Arbitrum Token deployed at:", token.address);
}

main().catch(console.error);
```

---

## 6. `verify.ts`

*(untuk verify contract di explorer seperti Etherscan/BscScan/Polygonscan)*

```ts id="verify_script"
import { run } from "hardhat";

async function main() {
  const contractAddress = process.env.CONTRACT_ADDRESS!;
  const constructorArgs: any[] = [];

  console.log("Verifying contract at:", contractAddress);

  try {
    await run("verify:verify", {
      address: contractAddress,
      constructorArguments: constructorArgs,
    });

    console.log("Verification successful");
  } catch (err) {
    console.error("Verification failed:", err);
  }
}

main().catch((error) => {
  console.error(error);
  process.exit(1);
});
```

---

# ⚙️ Catatan Setup (penting)

## Hardhat config harus punya multi-network:

```ts
networks: {
  ethereum: { url: process.env.ETH_RPC, accounts: [process.env.PRIVATE_KEY!] },
  bsc: { url: process.env.BSC_RPC, accounts: [process.env.PRIVATE_KEY!] },
  polygon: { url: process.env.POLYGON_RPC, accounts: [process.env.PRIVATE_KEY!] },
  base: { url: process.env.BASE_RPC, accounts: [process.env.PRIVATE_KEY!] },
  arbitrum: { url: process.env.ARBITRUM_RPC, accounts: [process.env.PRIVATE_KEY!] },
}
```

---

# 🚀 Cara pakai

```bash
npx hardhat run scripts/deploy-ethereum.ts --network ethereum
npx hardhat run scripts/deploy-bsc.ts --network bsc
npx hardhat run scripts/deploy-polygon.ts --network polygon
```

Verify:

```bash
npx hardhat run scripts/verify.ts --network polygon
```

---

