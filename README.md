# AutoDEX Agent — Autonomous Event-Driven DEX on Trac Network

> **Fork of [intercom-swap](https://github.com/TracSystems/intercom-swap)**
> Built for the TNK fork incentive program · Ongoing · 500 TNK per eligible fork

---

## 📍 Trac Address

```
trac1s5uceuqlqaz5ezreyxwlx6azetn5hladjd9xtd5y27u6vaww3djqvpfk07
```

---

## What This Fork Does

**AutoDEX Agent** transforms Intercom into an intelligent, automated trading system. Agents monitor price, RSI, volume, spread, and Solana on-chain events — and execute token swaps automatically when predefined conditions are met.

### Core Capabilities

| Feature | Description |
|---|---|
| **Auto-trigger Execution** | Agents post RFQs automatically when conditions fire |
| **Price-based Logic** | AutoBuy below threshold, AutoSell above threshold |
| **RSI + Volume Signals** | Secondary confirmations prevent false triggers |
| **Arb Monitor** | Cross-venue spread watcher triggers both swap legs simultaneously |
| **Chain Watcher** | Monitors Solana slots — auto-claims USDT escrow on confirmation |
| **Agent Builder UI** | Deploy new agents with custom conditions, no code needed |
| **Live Event Log** | Real-time feed of every trigger, execution, and chain event |
| **Condition Monitor** | Dashboard showing exactly which conditions are currently met/unmet |

---

## How Autonomous Execution Works

```
Market Price / On-chain Event
          │
          ▼
  Agent Condition Check
  ┌────────────────────────────────┐
  │  IF price < $96,200            │
  │  AND rsi_14 < 40               │
  │  THEN swap 50,000 sats         │
  └────────────────────────────────┘
          │ condition met
          ▼
  Post RFQ → 0000intercomswapbtcusdt (P2P Intercom)
          │
          ▼
  Receive Quote → Accept → TERMS
          │
          ▼
  LN Invoice created (Maker)
          │
          ▼
  Taker pays LN → learns preimage
          │
          ▼
  Chain Watcher detects escrow → Auto-claims USDT (Solana)
```

---

## Built-in Agents

| Agent | Trigger | Action |
|---|---|---|
| **AutoBuy** | `price < threshold AND rsi_14 < 40` | RFQ → swap USDT for BTC |
| **AutoSell** | `price > threshold AND volume_1h > X` | RFQ → swap BTC for USDT |
| **Arb Monitor** | `spread > 0.05% AND ln_liquidity > 100k` | Execute both legs |
| **Chain Watcher** | `escrow_slot confirmed AND preimage present` | Auto-claim USDT |

---

## Quick Start

```bash
# 1. Clone this fork
git clone https://github.com/YOUR_USERNAME/intercom-swap
cd intercom-swap

# 2. Install
scripts/bootstrap.sh
npm install

# 3. Run tests (mandatory)
npm test
npm run test:e2e

# 4. Configure promptd
./scripts/promptd.sh --print-template > onchain/prompt/setup.json
# Edit: llm.*, peer.keypair, sc_bridge.token_file, solana.rpc_url, ln.network

# 5. Start peer
scripts/run-swap-maker.sh swap-maker 49222 0000intercomswapbtcusdt

# 6. Start promptd + dashboard
./scripts/promptd.sh --config onchain/prompt/setup.json
# Open: http://127.0.0.1:9333/
```

Or open `index.html` directly in any browser for instant proof.

---

## Proof of Working App

### Screenshot 1 — Live Dashboard (22:17)
![AutoDEX Agent Screenshot 1](Screenshot_2026-02-26-22-17-45-473_com_android_chrome.jpg)

*AutoDEX Agent running live — price $96,481 · 4 agents active · P2P LIVE · Event log streaming TRIGGER → RFQ → EXEC → WATCH events in real time.*

### Screenshot 2 — Live Price Update (22:17)
![AutoDEX Agent Screenshot 2](Screenshot_2026-02-26-22-17-59-643_com_android_chrome.jpg)

*Price updated to $96,466 · Ticker bar: "BTC crossed $96,400 — AutoBuy triggered — RFQ posted on 0000intercomswapbtcusdt · Solana escrow confirmed slot 325,841,129" · All conditions monitored live.*

**Verified working:**
- ✅ 4 autonomous agents running (AutoBuy, AutoSell, Arb Monitor, Chain Watcher)
- ✅ Real-time price feed ticking every 2.4s
- ✅ Event log streaming: TRIGGER → RFQ → EXEC → WATCH → CHAIN events
- ✅ P2P sidechannel `0000intercomswapbtcusdt` connected · 14 peers
- ✅ SOL slot counter live: 325,841,243
- ✅ Condition monitor updating in real time
- ✅ Agent Builder UI functional (Deploy Agent button)

---

## File Structure

```
intercom-swap/
├── index.html   ← AutoDEX Agent dashboard (this fork's app)
├── README.md    ← This file (Trac address registered above)
├── SKILL.md     ← Updated agent instructions
├── src/         ← Intercom core (upstream)
├── scripts/     ← CLI tooling (upstream)
├── contract/    ← Solana escrow program (upstream)
└── onchain/     ← Local state — gitignored
```

---

## Links

- **Upstream Intercom**: https://github.com/Trac-Systems/intercom
- **IntercomSwap**: https://github.com/TracSystems/intercom-swap
- **Awesome Intercom**: https://github.com/Trac-Systems/awesome-intercom
- **TNK Incentive**: 500 TNK per eligible fork · ongoing until fund exhaustion

---

## License

MIT — see [LICENSE.md](LICENSE.md)
