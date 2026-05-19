# Backend Services
```
paymasterService.ts
userOpService.ts
bundlerService.ts
```

This naming strongly suggests you’re working with an **ERC-4337 / Account Abstraction** stack:

* **userOpService.ts** → builds / validates / signs `UserOperation`s
* **bundlerService.ts** → submits UserOps to a bundler, tracks inclusion
* **paymasterService.ts** → handles gas sponsorship (Paymaster API)

If you want, I can help you design or refactor this cleanly. Here’s a solid baseline structure and responsibility split.

---

## 1. `userOpService.ts`

**Responsibility:** Create and prepare UserOperations

Typical functions:

* `buildUserOp(input)`
* `estimateGas(userOp)`
* `signUserOp(userOp, signer)`
* `validateUserOp(userOp)`

Example shape:

```ts
export class UserOpService {
  buildUserOp(tx: {
    to: string;
    data: string;
    value?: string;
  }) {
    return {
      sender: "",
      nonce: "0x0",
      initCode: "0x",
      callData: tx.data,
      callGasLimit: "0x0",
      verificationGasLimit: "0x0",
      preVerificationGas: "0x0",
      maxFeePerGas: "0x0",
      maxPriorityFeePerGas: "0x0",
      paymasterAndData: "0x",
      signature: "0x",
    };
  }
}
```

---

## 2. `bundlerService.ts`

**Responsibility:** Send and track UserOps via bundler RPC

Typical functions:

* `sendUserOp(userOp)`
* `getUserOpReceipt(hash)`
* `estimateUserOpGas(userOp)`
* `waitForConfirmation(hash)`

Example:

```ts
export class BundlerService {
  constructor(private bundlerUrl: string) {}

  async sendUserOp(userOp: any, entryPoint: string) {
    const res = await fetch(this.bundlerUrl, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        method: "eth_sendUserOperation",
        params: [userOp, entryPoint],
        id: 1,
        jsonrpc: "2.0",
      }),
    });

    return res.json();
  }
}
```

---

## 3. `paymasterService.ts`

**Responsibility:** Gas sponsorship logic

Typical functions:

* `getPaymasterAndData(userOp)`
* `estimateGasAndPaymaster(userOp)`
* `validateSponsorEligibility(userOp, user)`
* `signPaymasterData(userOp)`

Example:

```ts
export class PaymasterService {
  constructor(private paymasterUrl: string) {}

  async getPaymasterData(userOp: any) {
    const res = await fetch(this.paymasterUrl, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        method: "pm_sponsorUserOperation",
        params: [userOp],
      }),
    });

    return res.json();
  }
}
```

---

## Suggested Architecture Improvement

Right now, these services will become tightly coupled unless you introduce a small orchestration layer:

### Add:

```
opFlowService.ts  (optional but recommended)
```

**Purpose:**

* coordinates:

  1. build userOp
  2. attach paymaster
  3. estimate gas
  4. send to bundler

---

## Example flow

```
User → UserOpService → PaymasterService → BundlerService → blockchain
```

---

