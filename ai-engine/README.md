# 📁 `ai-engine/`

---

## 1. `token-name-generator.ts`

```ts
const adjectives = [
  "Moon", "Turbo", "Ultra", "Hyper", "Mega", "Nano", "Quantum",
  "Alpha", "Beta", "Crypto", "Degen", "Meta", "AI", "Cyber"
];

const nouns = [
  "Doge", "Pepe", "Shiba", "Rocket", "Chain", "Vault",
  "Coin", "Bot", "Zilla", "Kitty", "Dragon", "Node"
];

export function generateTokenName(seed?: string): string {
  const adj = adjectives[Math.floor(Math.random() * adjectives.length)];
  const noun = nouns[Math.floor(Math.random() * nouns.length)];

  const base = `${adj}${noun}`;

  if (seed) {
    return `${base}-${seed.slice(0, 4).toUpperCase()}`;
  }

  return base;
}
```

---

## 2. `meme-description.ts`

```ts
export function generateMemeDescription(tokenName: string, trend?: string): string {
  const templates = [
    `${tokenName} is a community-powered experiment fueled by internet chaos.`,
    `Born from memes and powered by hype, ${tokenName} is here to disrupt crypto culture.`,
    `${tokenName} rides the wave of ${trend || "viral trends"} with unstoppable energy.`,
    `A degen-driven ecosystem where ${tokenName} becomes more than just a token.`
  ];

  return templates[Math.floor(Math.random() * templates.length)];
}
```

---

## 3. `tokenomics-generator.ts`

```ts
export interface Tokenomics {
  totalSupply: number;
  burnRate: number;
  marketingTax: number;
  liquidityPool: number;
  stakingReward: number;
}

export function generateTokenomics(): Tokenomics {
  const totalSupply = 1_000_000_000;

  const burnRate = Math.floor(Math.random() * 5); // 0-4%
  const marketingTax = Math.floor(Math.random() * 6) + 2; // 2-7%
  const stakingReward = Math.floor(Math.random() * 10) + 5; // 5-14%

  const liquidityPool = 100 - (burnRate + marketingTax + stakingReward);

  return {
    totalSupply,
    burnRate,
    marketingTax,
    liquidityPool,
    stakingReward
  };
}
```

---

## 4. `logo-generator.ts`

```ts
export interface LogoPrompt {
  tokenName: string;
  style: string;
  prompt: string;
}

export function generateLogoPrompt(tokenName: string): LogoPrompt {
  const styles = [
    "cyberpunk neon",
    "minimal flat design",
    "3D futuristic metallic",
    "meme cartoon style",
    "glowing sci-fi emblem"
  ];

  const style = styles[Math.floor(Math.random() * styles.length)];

  const prompt = `
Design a logo for crypto token "${tokenName}".
Style: ${style}.
Make it highly recognizable, viral, and suitable for meme coin branding.
Include bold shapes, high contrast, and iconic symbol.
`;

  return {
    tokenName,
    style,
    prompt: prompt.trim()
  };
}
```

---

## 5. `trend-analyzer.ts`

```ts
export interface TrendResult {
  keywords: string[];
  sentiment: "bullish" | "neutral" | "bearish";
  hypeScore: number;
}

const mockTrends = [
  "AI agents",
  "Solana memes",
  "Bitcoin ETF",
  "DeFi revival",
  "Crypto gaming",
  "NFT comeback"
];

export function analyzeTrends(): TrendResult {
  const shuffled = mockTrends.sort(() => 0.5 - Math.random());
  const keywords = shuffled.slice(0, 3);

  const hypeScore = Math.floor(Math.random() * 100);

  let sentiment: "bullish" | "neutral" | "bearish" = "neutral";

  if (hypeScore > 70) sentiment = "bullish";
  else if (hypeScore < 30) sentiment = "bearish";

  return {
    keywords,
    sentiment,
    hypeScore
  };
}
```

---

## 6. `twitter-scraper.ts`

*(mock version — aman & tidak pakai scraping ilegal)*

```ts
export interface Tweet {
  username: string;
  text: string;
  likes: number;
  retweets: number;
}

export async function fetchTrendingTweets(keyword: string): Promise<Tweet[]> {
  // Mock data (replace with Twitter API v2 if needed)
  return [
    {
      username: "crypto_alpha",
      text: `${keyword} is going insane right now 🚀`,
      likes: 1200,
      retweets: 340
    },
    {
      username: "degen_trader",
      text: `I think ${keyword} might be the next big meta`,
      likes: 980,
      retweets: 210
    },
    {
      username: "web3_daily",
      text: `${keyword} narrative is heating up again`,
      likes: 1500,
      retweets: 500
    }
  ];
}
```

---

# 🔥 Bonus: Cara Integrasi Semua Modul

```ts
import { generateTokenName } from "./token-name-generator";
import { generateMemeDescription } from "./meme-description";
import { generateTokenomics } from "./tokenomics-generator";
import { generateLogoPrompt } from "./logo-generator";
import { analyzeTrends } from "./trend-analyzer";

async function run() {
  const trend = analyzeTrends();

  const tokenName = generateTokenName(trend.keywords[0]);

  const description = generateMemeDescription(tokenName, trend.keywords[0]);

  const tokenomics = generateTokenomics();

  const logo = generateLogoPrompt(tokenName);

  console.log({
    trend,
    tokenName,
    description,
    tokenomics,
    logo
  });
}

run();
```

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
