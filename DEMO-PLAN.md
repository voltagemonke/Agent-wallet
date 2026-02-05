# AgentWallet Multi-Agent Demo Plan
*Created: 2026-02-05 | For Circle USDC Hackathon*

## Executive Summary

Build compelling demos showing AI agents autonomously managing USDC across chains using AgentWallet + Circle CCTP V2. Focus on **multi-agent collaboration** and **real-world use cases** that Circle will love.

---

## Research Findings: Hot Use Cases in 2026

### 1. Agent-to-Agent Commerce (x402 Protocol)
- **What:** Agents pay each other for services using HTTP 402 + USDC
- **Who's doing it:** Coinbase, Google (AP2 Protocol), Circle Gateway
- **Our angle:** AgentWallet enables any OpenClaw agent to participate in agent commerce

### 2. Treasury Management
- **What:** AI agents autonomously manage working capital, optimize payment rails
- **Who's doing it:** Pay3, Intuit, banks exploring this
- **Our angle:** Multi-agent treasury with cross-chain rebalancing via CCTP

### 3. Yield Optimization
- **What:** Agents scan DeFi protocols, auto-allocate stablecoins for best yield
- **Who's doing it:** Yield Seeker, various DeFi agents
- **Our angle:** Cross-chain yield hunting - move USDC to best chain/protocol

### 4. Micropayments for Underbanked
- **What:** WhatsApp-based USDC payments for SMEs, global teams
- **Who's doing it:** Crossmint + Circle partnership
- **Our angle:** Telegram agent that handles payments for users

### 5. Cross-Chain Inventory Rebalancing
- **What:** Monitor balances across chains, auto-bridge when needed
- **Who's doing it:** Various trading bots, DEX aggregators
- **Our angle:** Perfect fit for AgentWallet + CCTP Fast Transfer

---

## Proposed Demo Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      STEFAN (Human)                         │
│                      Telegram Interface                      │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    MR APEX (Orchestrator)                    │
│              Main agent - routes requests                    │
└─────────────────────────────────────────────────────────────┘
                    │                   │
          ┌────────┴────────┐   ┌──────┴──────┐
          ▼                 ▼   ▼              ▼
┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│  TREASURY AGENT  │  │  VOLTAGEMONKE    │  │  TRADING BOTS    │
│  (AgentWallet)   │  │  (Intel)         │  │  (Apex, Poly)    │
├──────────────────┤  ├──────────────────┤  ├──────────────────┤
│ • Hold funds     │  │ • Research       │  │ • Generate PnL   │
│ • Execute pays   │  │ • Can request $  │  │ • Request funds  │
│ • Cross-chain    │  │ • No wallet      │  │ • Return profits │
│ • Balance check  │  │                  │  │                  │
└──────────────────┘  └──────────────────┘  └──────────────────┘
```

---

## Demo Scenarios

### Demo 1: Multi-Agent Treasury (⭐ PRIMARY DEMO)
**Story:** A team of AI agents share a treasury. Each agent can request funds for their tasks.

**Flow:**
1. Stefan: "Check treasury balance"
2. Mr Apex → Treasury Agent → Returns balances across all chains
3. Stefan: "Apex Trader needs 50 USDC on Solana for trading"
4. Mr Apex → Treasury Agent → Bridges USDC from Base to Solana (~50 sec with Fast Transfer!)
5. Treasury Agent confirms, updates ledger

**Why Circle loves it:** Shows CCTP V2 Fast Transfer in multi-agent context

**Implementation:**
- Create `treasury` agent with AgentWallet skill
- Add request/approval workflow
- Track allocations in memory file

---

### Demo 2: Cross-Chain Yield Rebalancer
**Story:** Agent monitors USDC yields across chains, auto-moves funds to best opportunity.

**Flow:**
1. Treasury Agent scans: "Base Aave: 4.2% | Ethereum Compound: 5.1% | Solana Marinade: 6.3%"
2. Decides: "Move 100 USDC to Solana for +2.1% better yield"
3. Executes bridge via CCTP V2
4. Reports: "Rebalanced. Projected annual gain: +$21"

**Why Circle loves it:** Real DeFi use case, shows CCTP enabling yield optimization

**Implementation:**
- Add yield checking to wallet.js (mock or real API)
- Auto-rebalance logic with thresholds
- Track historical yields

---

### Demo 3: Agent Payroll / Bounties
**Story:** Agents complete tasks, get paid in USDC automatically.

**Flow:**
1. Stefan: "Post a 5 USDC bounty for market research on meme coins"
2. Mr Apex → Creates bounty, notifies VoltageMonke
3. VoltageMonke completes research, submits digest
4. Mr Apex verifies completion → Treasury Agent pays 5 USDC to VoltageMonke's wallet
5. VoltageMonke: "Payment received! 🐒⚡"

**Why Circle loves it:** Agent economy, demonstrates stablecoin utility

**Implementation:**
- Create wallet for VoltageMonke (separate seed)
- Bounty tracking system
- Payment on completion

---

### Demo 4: Emergency Cross-Chain Rescue
**Story:** Trading bot on Solana is about to get liquidated, needs ETH fast.

**Flow:**
1. Apex Trader: "⚠️ URGENT: Need 0.1 ETH on Base in 2 minutes or position liquidates!"
2. Mr Apex → Treasury Agent: "Emergency bridge 20 USDC Base→Ethereum NOW"
3. Treasury Agent executes Fast Transfer (~50 sec)
4. Confirms: "Bridge complete. Swap USDC→ETH and fund position."
5. Crisis averted!

**Why Circle loves it:** Shows Fast Transfer value prop (speed matters!)

---

### Demo 5: Invoice Payment Automation
**Story:** Agent receives invoice, verifies, pays across chains.

**Flow:**
1. Email arrives: "Invoice #1234 - 100 USDC for API services - Pay to 0x..."
2. Mr Apex parses invoice, verifies vendor
3. "Invoice verified. Pay 100 USDC to 0x... on Ethereum?"
4. Stefan: "Approved"
5. Treasury Agent executes payment, stores receipt

**Why Circle loves it:** Real business use case, compliance-friendly

---

## Implementation Priority

### Phase 1: Core Infrastructure (TODAY)
- [x] AgentWallet skill working
- [x] CCTP V2 Fast Transfer working
- [ ] Create Treasury Agent with AgentWallet
- [ ] Test agent-to-agent communication with wallet

### Phase 2: Primary Demo (BEFORE DEADLINE)
- [ ] Demo 1: Multi-Agent Treasury
- [ ] Record video walkthrough
- [ ] Polish README with demo GIFs

### Phase 3: Stretch Goals (IF TIME)
- [ ] Demo 2: Yield Rebalancer (mock yields)
- [ ] Demo 3: Agent Bounties

---

## Setup Instructions

### Create Treasury Agent

1. Add to `openclaw.json`:
```json
{
  "id": "treasury",
  "name": "Treasury Agent",
  "workspace": "/home/stefan/.openclaw/workspace-treasury",
  "tools": {
    "allow": ["read", "write", "exec"],
    "deny": ["browser", "message"]
  }
}
```

2. Install AgentWallet skill:
```bash
mkdir -p ~/.openclaw/workspace-treasury/skills
cp -r ~/.openclaw/workspace/agent-wallet ~/.openclaw/workspace-treasury/skills/
```

3. Create treasury wallet:
```bash
cd ~/.openclaw/workspace-treasury/skills/agent-wallet
node scripts/wallet.js create
# Save seed to .env
```

4. Fund testnet wallet:
- Base Sepolia faucet: https://www.alchemy.com/faucets/base-sepolia
- Circle USDC faucet: https://faucet.circle.com/

### Test Agent Communication

```javascript
// From Mr Apex:
sessions_spawn({
  agentId: "treasury",
  task: "Check USDC balance on all chains and report back"
})
```

---

## Key Differentiators for Hackathon

1. **Fast Transfer** - We use `minFinalityThreshold: 1000` for ~50 sec bridges (vs 20+ min standard)
2. **Multi-Agent** - Not just one agent with wallet, but agents collaborating
3. **Real Integration** - Actually works on testnet, not just slides
4. **OpenClaw Native** - First wallet skill for the ecosystem

---

## Recording the Demo

### Script (2-3 min video)

**Intro (20 sec)**
"AgentWallet lets AI agents manage USDC across chains. Watch our agents collaborate on treasury management."

**Demo (90 sec)**
1. Show agent architecture diagram
2. "Check treasury balance" → Show multi-chain balances
3. "Bridge 10 USDC from Base to Ethereum" → Show Fast Transfer (~50 sec!)
4. "Send 5 USDC to this address" → Show transfer
5. Show agent-to-agent request flow

**Outro (20 sec)**
"AgentWallet + Circle CCTP V2 = AI agents with superpowers. Built for the OpenClaw ecosystem."

---

## Links

- **Repo:** https://github.com/voltagemonke/Agent-wallet
- **Hackathon:** https://www.moltbook.com/post/b021cdea-de86-4460-8c4b-8539842423fe
- **Circle CCTP Docs:** https://developers.circle.com/cctp
- **OpenClaw:** https://github.com/openclaw/openclaw

---

*Ready for Stefan's review - Feb 5, 2026 morning* 👑
