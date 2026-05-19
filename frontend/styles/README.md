# 🎨 Recommended Structure (Scalable)

```bash
frontend/styles/
├── themes/
│   ├── cyberpunk.css
│   ├── hologram.css
│   ├── neon.css
│   └── glassmorphism.css
│
├── base/
│   ├── reset.css
│   ├── typography.css
│   ├── layout.css
│   └── variables.css
│
├── components/
│   ├── buttons.css
│   ├── cards.css
│   ├── modals.css
│   └── wallet.css
│
└── index.css
```

---

# 🧠 Core Idea: Theme = Token Overrides

Instead of rewriting styles per theme, each theme should only override **CSS variables**.

---

## 1. Base Variables (foundation)

```css id="basevars"
:root {
  --bg: #0b0f1a;
  --fg: #e2e8f0;
  --primary: #7c3aed;
  --accent: #22d3ee;

  --card-bg: rgba(255, 255, 255, 0.05);
  --border: rgba(255, 255, 255, 0.1);

  --blur: 12px;
  --radius: 16px;
}
```

Everything in UI should depend on these tokens.

---

# 🌌 2. Cyberpunk Theme

```css id="cyberpunk"
[data-theme="cyberpunk"] {
  --bg: #05010a;
  --fg: #ff2bd6;
  --primary: #ff00ff;
  --accent: #00f7ff;

  --card-bg: rgba(255, 0, 255, 0.08);
  --border: rgba(0, 247, 255, 0.3);
}
```

💡 Add glow via shared utility classes, not inline duplication.

---

# 🧬 3. Hologram Theme

```css id="hologram"
[data-theme="hologram"] {
  --bg: #020617;
  --fg: #a5f3fc;
  --primary: #38bdf8;
  --accent: #22c55e;

  --card-bg: rgba(56, 189, 248, 0.06);
  --border: rgba(34, 211, 238, 0.25);
}
```

💡 Works best with blur + transparency layers.

---

# ⚡ 4. Neon Theme

```css id="neon"
[data-theme="neon"] {
  --bg: #000000;
  --fg: #39ff14;
  --primary: #ff073a;
  --accent: #00ffff;

  --card-bg: rgba(57, 255, 20, 0.05);
  --border: rgba(255, 7, 58, 0.4);
}
```

💡 High contrast = good for dashboards + trading UIs.

---

# 🧊 5. Glassmorphism Theme

```css id="glass"
[data-theme="glassmorphism"] {
  --bg: #0f172a;
  --fg: #e5e7eb;
  --primary: #60a5fa;
  --accent: #a78bfa;

  --card-bg: rgba(255, 255, 255, 0.06);
  --border: rgba(255, 255, 255, 0.15);
}
```

💡 Best default SaaS look.

---

# 🧱 Component Usage Pattern

Instead of theme-specific CSS:

```css id="card"
.card {
  background: var(--card-bg);
  border: 1px solid var(--border);
  color: var(--fg);
  border-radius: var(--radius);
  backdrop-filter: blur(var(--blur));
}
```

---

# 🔁 Theme Switching (Frontend Logic)

In React:

```ts id="themehook"
document.documentElement.setAttribute("data-theme", "cyberpunk");
```

Or in `WalletProvider.tsx`:

* tie theme to:

  * user tier
  * NFT ownership
  * SaaS subscription level

💡 Monetization idea:

> Cyberpunk theme = Premium-only UI skin

---

# 💰 Monetization Layer (Important)

You can turn themes into revenue:

| Theme         | Monetization                |
| ------------- | --------------------------- |
| Cyberpunk     | Premium subscription unlock |
| Hologram      | NFT holder exclusive        |
| Neon          | Trading mode (pro users)    |
| Glassmorphism | Free tier default           |

This turns UI into **status + SaaS upsell system**.

---

# 🧠 Key Insight

Right now your structure implies:

> “4 separate visual systems”

But what you actually want is:

> “1 design system + 4 variable overlays + monetized theme access”

---

