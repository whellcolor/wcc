# 📁 `frontend/app/staking/`

---

## 1. `page.tsx` (Staking Dashboard Main Page)

```tsx id="staking_page"
import StakeCard from "./StakeCard";
import APYChart from "./APYChart";
import RewardsPanel from "./RewardsPanel";
import ClaimRewards from "./ClaimRewards";

export default function StakingPage() {
  return (
    <div style={{ padding: 24 }}>
      <h1>Staking Dashboard</h1>

      <div style={{ display: "grid", gap: 20, marginTop: 20 }}>
        <StakeCard />
        <APYChart />
        <RewardsPanel />
        <ClaimRewards />
      </div>
    </div>
  );
}
```

---

## 2. `StakeCard.tsx`

```tsx id="stake_card"
"use client";

import { useState } from "react";

export default function StakeCard() {
  const [amount, setAmount] = useState("");

  const handleStake = () => {
    alert(`Staked ${amount} tokens`);
  };

  return (
    <div style={cardStyle}>
      <h2>Stake Tokens</h2>

      <input
        placeholder="Enter amount"
        value={amount}
        onChange={(e) => setAmount(e.target.value)}
        style={inputStyle}
      />

      <button onClick={handleStake} style={buttonStyle}>
        Stake
      </button>
    </div>
  );
}

const cardStyle: React.CSSProperties = {
  padding: 16,
  border: "1px solid #ddd",
  borderRadius: 10,
};

const inputStyle: React.CSSProperties = {
  padding: 8,
  width: "100%",
  marginTop: 10,
  marginBottom: 10,
};

const buttonStyle: React.CSSProperties = {
  padding: 10,
  width: "100%",
  background: "black",
  color: "white",
  border: "none",
  cursor: "pointer",
};
```

---

## 3. `APYChart.tsx`

```tsx id="apy_chart"
"use client";

import { useEffect, useState } from "react";

export default function APYChart() {
  const [apy, setApy] = useState<number[]>([]);

  useEffect(() => {
    // mock APY data
    setApy([12, 15, 18, 14, 20, 22, 19]);
  }, []);

  return (
    <div style={cardStyle}>
      <h2>APY Trend</h2>

      <div style={{ display: "flex", gap: 6, marginTop: 10 }}>
        {apy.map((value, i) => (
          <div
            key={i}
            style={{
              height: value * 3,
              width: 20,
              background: "blue",
            }}
            title={`${value}%`}
          />
        ))}
      </div>

      <p style={{ marginTop: 10 }}>
        Current APY: {apy[apy.length - 1]}%
      </p>
    </div>
  );
}

const cardStyle: React.CSSProperties = {
  padding: 16,
  border: "1px solid #ddd",
  borderRadius: 10,
};
```

---

## 4. `RewardsPanel.tsx`

```tsx id="rewards_panel"
"use client";

import { useEffect, useState } from "react";

export default function RewardsPanel() {
  const [rewards, setRewards] = useState(0);

  useEffect(() => {
    setRewards(128.45);
  }, []);

  return (
    <div style={cardStyle}>
      <h2>Your Rewards</h2>

      <h1 style={{ color: "green" }}>
        {rewards.toFixed(2)} TOKEN
      </h1>

      <p>Accrued staking rewards</p>
    </div>
  );
}

const cardStyle: React.CSSProperties = {
  padding: 16,
  border: "1px solid #ddd",
  borderRadius: 10,
};
```

---

## 5. `ClaimRewards.tsx`

```tsx id="claim_rewards"
"use client";

import { useState } from "react";

export default function ClaimRewards() {
  const [loading, setLoading] = useState(false);

  const claim = async () => {
    setLoading(true);

    setTimeout(() => {
      alert("Rewards claimed successfully!");
      setLoading(false);
    }, 1500);
  };

  return (
    <div style={cardStyle}>
      <h2>Claim Rewards</h2>

      <button onClick={claim} style={buttonStyle} disabled={loading}>
        {loading ? "Claiming..." : "Claim Rewards"}
      </button>
    </div>
  );
}

const cardStyle: React.CSSProperties = {
  padding: 16,
  border: "1px solid #ddd",
  borderRadius: 10,
};

const buttonStyle: React.CSSProperties = {
  padding: 10,
  width: "100%",
  background: "green",
  color: "white",
  border: "none",
  cursor: "pointer",
};
```

---

# 🧠 Gambaran Sistem Staking Ini

UI ini biasanya terhubung ke:

* 🪙 Smart contract staking (ERC-20 / BEP-20)
* 📈 APY calculation engine
* 💰 reward distribution contract
* 🔗 wallet integration (MetaMask / WalletConnect)

Flow:

1. User stake token → `StakeCard`
2. APY berubah → `APYChart`
3. Rewards dihitung → `RewardsPanel`
4. User claim → `ClaimRewards`

---

