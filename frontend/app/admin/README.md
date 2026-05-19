# 📁 `frontend/app/admin/`

---

## 1. `page.tsx` (Admin Dashboard Home)

```tsx id="admin_home"
import Link from "next/link";

export default function AdminPage() {
  return (
    <div style={{ padding: 24 }}>
      <h1>Admin Dashboard</h1>

      <div style={{ display: "grid", gap: 12, marginTop: 20 }}>
        <Link href="/admin/users">Users</Link>
        <Link href="/admin/deploys">Deployments</Link>
        <Link href="/admin/revenue">Revenue</Link>
        <Link href="/admin/chains">Chains</Link>
        <Link href="/admin/treasury">Treasury</Link>
      </div>
    </div>
  );
}
```

---

## 2. `users.tsx`

```tsx id="admin_users"
"use client";

import { useEffect, useState } from "react";

type User = {
  id: string;
  name: string;
  email: string;
  role: "admin" | "user";
};

export default function UsersPage() {
  const [users, setUsers] = useState<User[]>([]);

  useEffect(() => {
    // mock fetch
    setUsers([
      { id: "1", name: "Alex", email: "alex@mail.com", role: "admin" },
      { id: "2", name: "Budi", email: "budi@mail.com", role: "user" },
    ]);
  }, []);

  return (
    <div style={{ padding: 24 }}>
      <h1>Users</h1>

      <table border={1} cellPadding={8}>
        <thead>
          <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Email</th>
            <th>Role</th>
          </tr>
        </thead>

        <tbody>
          {users.map((u) => (
            <tr key={u.id}>
              <td>{u.id}</td>
              <td>{u.name}</td>
              <td>{u.email}</td>
              <td>{u.role}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}
```

---

## 3. `deploys.tsx`

```tsx id="admin_deploys"
"use client";

import { useEffect, useState } from "react";

type Deploy = {
  id: string;
  chain: string;
  contract: string;
  status: "success" | "pending" | "failed";
};

export default function DeploysPage() {
  const [deploys, setDeploys] = useState<Deploy[]>([]);

  useEffect(() => {
    setDeploys([
      {
        id: "d1",
        chain: "Ethereum",
        contract: "0x123...",
        status: "success",
      },
      {
        id: "d2",
        chain: "BSC",
        contract: "0x456...",
        status: "pending",
      },
    ]);
  }, []);

  return (
    <div style={{ padding: 24 }}>
      <h1>Deployments</h1>

      {deploys.map((d) => (
        <div
          key={d.id}
          style={{
            padding: 12,
            border: "1px solid #ccc",
            marginBottom: 10,
          }}
        >
          <p>Chain: {d.chain}</p>
          <p>Contract: {d.contract}</p>
          <p>Status: {d.status}</p>
        </div>
      ))}
    </div>
  );
}
```

---

## 4. `revenue.tsx`

```tsx id="admin_revenue"
"use client";

import { useEffect, useState } from "react";

export default function RevenuePage() {
  const [revenue, setRevenue] = useState(0);

  useEffect(() => {
    setRevenue(12500); // mock data
  }, []);

  return (
    <div style={{ padding: 24 }}>
      <h1>Revenue</h1>

      <div
        style={{
          padding: 20,
          background: "#111",
          color: "white",
          borderRadius: 8,
        }}
      >
        <h2>${revenue.toLocaleString()}</h2>
        <p>Total platform revenue</p>
      </div>
    </div>
  );
}
```

---

## 5. `chains.tsx`

```tsx id="admin_chains"
"use client";

import { useState } from "react";

type Chain = {
  name: string;
  rpc: string;
  active: boolean;
};

export default function ChainsPage() {
  const [chains] = useState<Chain[]>([
    { name: "Ethereum", rpc: "https://eth.rpc", active: true },
    { name: "BSC", rpc: "https://bsc.rpc", active: true },
    { name: "Polygon", rpc: "https://polygon.rpc", active: true },
    { name: "Arbitrum", rpc: "https://arb.rpc", active: false },
  ]);

  return (
    <div style={{ padding: 24 }}>
      <h1>Chains</h1>

      {chains.map((c) => (
        <div key={c.name} style={{ marginBottom: 10 }}>
          <strong>{c.name}</strong>
          <p>{c.rpc}</p>
          <p>Status: {c.active ? "🟢 Active" : "🔴 Inactive"}</p>
        </div>
      ))}
    </div>
  );
}
```

---

## 6. `treasury.tsx`

```tsx id="admin_treasury"
"use client";

import { useEffect, useState } from "react";

export default function TreasuryPage() {
  const [balance, setBalance] = useState(0);

  useEffect(() => {
    setBalance(420000);
  }, []);

  return (
    <div style={{ padding: 24 }}>
      <h1>Treasury</h1>

      <div
        style={{
          padding: 20,
          border: "1px solid #ddd",
          borderRadius: 10,
        }}
      >
        <h2>{balance.toLocaleString()} USDT</h2>
        <p>Platform treasury balance</p>
      </div>
    </div>
  );
}
```

---

# 🧠 Arsitektur Dashboard Ini

Frontend ini biasanya dipakai untuk:

* 📊 monitor AI token generator system
* 🚀 track multi-chain deploy (Ethereum, BSC, Polygon, Base, Arbitrum)
* 👤 user management SaaS
* 💰 revenue tracking (fee deploy / subscription)
* 🌐 chain status monitoring
* 🏦 treasury management

---


