# AgentWallet 🤖💰

**A cross-chain USDC wallet skill for AI agents, powered by Circle's CCTP V2.**

Built for the [Circle USDC Hackathon](https://moltbook.com/hackathon) | OpenClaw Skill Track

---

## ✨ What is AgentWallet?

AgentWallet gives AI agents the power to manage USDC across multiple blockchains. With a simple natural language interface, agents can:

- **Create wallets** - Generate secure HD wallets for Solana and EVM chains
- **Check balances** - Monitor USDC holdings across all supported chains
- **Transfer funds** - Send USDC to any address on the same chain
- **Bridge cross-chain** - Move USDC between chains using Circle's CCTP V2

### 🚀 Fast Transfer Technology

AgentWallet leverages **CCTP V2 Fast Transfer** for near-instant cross-chain bridging:

| Transfer Type | Attestation Time | Use Case |
|--------------|------------------|----------|
| **Fast** ⚡ | ~3-8 seconds | Interactive, time-sensitive |
| Standard | 10-30 minutes | Background, cost-optimized |

Fast Transfer enables real-time cross-chain operations that were previously impossible for AI agents.

---

## 🎯 Use Cases for AI Agents

### 1. **Cross-Chain Treasury Management**
AI agents managing organization funds can now optimize holdings across chains:
```
Agent: "Move 10,000 USDC from Ethereum to Base for lower gas fees"
→ Executes CCTP bridge in ~8 seconds
→ Funds available on Base immediately
```

### 2. **Automated Payment Routing**
Agents can pay for services on whatever chain offers the best rates:
```
Agent: "Pay 500 USDC to 0x... on Arbitrum for the API subscription"
→ Checks balances across chains
→ Bridges from highest balance chain if needed
→ Executes payment
```

### 3. **Multi-Chain DeFi Operations**
Agents can chase yield across chains without human intervention:
```
Agent: "APY on Base is 8%, Ethereum is 5%. Moving funds to Base."
→ Bridges USDC to higher-yield chain
→ Deploys to DeFi protocol
→ Reports new position
```

### 4. **Cross-Chain Commerce Agents**
E-commerce agents accepting USDC on any chain:
```
Customer pays on Solana → Agent bridges to Ethereum → Settles with supplier
```

### 5. **DAO Treasury Automation**
AI agents managing DAO treasuries across multiple chains:
```
Agent: "Rebalance treasury: 40% Ethereum, 30% Base, 30% Solana"
→ Calculates required transfers
→ Executes bridges in parallel
→ Reports final allocation
```

### 6. **Payroll & Disbursement Agents**
Automated payroll that pays each recipient on their preferred chain:
```
Agent: "Process payroll: Alice (Base), Bob (Arbitrum), Carol (Solana)"
→ Bridges from treasury to each chain
→ Distributes payments
→ Generates receipts
```

---

## 🛠 Installation

### For OpenClaw Users

```bash
# Install the skill
openclaw skill install agent-wallet

# Configure your wallet seed (or let the skill generate one)
openclaw skill config agent-wallet
```

### For Developers

```bash
git clone https://github.com/voltagemonke/Agent-wallet.git
cd Agent-wallet
npm install
cp .env.example .env
# Edit .env with your seed phrase
```

---

## 📖 Commands

### `create`
Generate a new HD wallet with addresses for all supported chains.

```bash
node scripts/wallet.js create
```

Output:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔐 NEW WALLET CREATED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Seed Phrase: [12 words - SAVE SECURELY!]

Addresses:
├─ Solana:   7xK9f...abc
├─ Base:     0x123...def
├─ Ethereum: 0x123...def
└─ Polygon:  0x123...def
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### `addresses`
Show wallet addresses for all chains.

```bash
node scripts/wallet.js addresses
```

### `balance`
Check USDC balance across all chains.

```bash
node scripts/wallet.js balance
```

Output:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
💰 WALLET BALANCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
├─ Solana:   1,000.00 USDC
├─ Base:     500.00 USDC
├─ Ethereum: 250.00 USDC
└─ Total:    1,750.00 USDC
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### `transfer`
Send USDC on the same chain.

```bash
node scripts/wallet.js transfer <chain> <recipient> <amount>

# Example
node scripts/wallet.js transfer base 0x742d35Cc6634C0532925a3b844Bc9e7595f... 100
```

### `bridge`
Bridge USDC between chains using CCTP V2 Fast Transfer.

```bash
node scripts/wallet.js bridge <from_chain> <to_chain> <amount>

# Example: Bridge 100 USDC from Base to Ethereum
node scripts/wallet.js bridge base ethereum 100
```

### `chains`
List all supported chains and their configuration.

```bash
node scripts/wallet.js chains
```

---

## ⚡ Async Bridge (Production)

For production use, the async bridge provides:
- **State persistence** - Resume from any step after interruption
- **Idempotent operations** - Safe to retry
- **Status tracking** - Monitor bridge progress

```bash
# Start a new bridge
node scripts/bridge-async.js start base_sepolia ethereum_sepolia 100

# Check status
node scripts/bridge-async.js status bridge_123456_abc

# Resume interrupted bridge
node scripts/bridge-async.js resume bridge_123456_abc

# List all bridges
node scripts/bridge-async.js list
```

### Bridge States

```
PENDING → APPROVED → BURNED → ATTESTED → COMPLETE
              │          │         │
              └──────────┴─────────┴── Can resume from any state
```

---

## 🔧 Technical Architecture

### CCTP V2 Integration

AgentWallet uses Circle's Cross-Chain Transfer Protocol V2 for native USDC bridging:

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│ Source Chain│     │   Circle     │     │ Dest Chain  │
│             │     │  Attestation │     │             │
│  1. Burn    │────▶│  2. Sign     │────▶│  3. Mint    │
│    USDC     │     │   (~3 sec)   │     │    USDC     │
└─────────────┘     └──────────────┘     └─────────────┘
```

### Fast Transfer vs Standard

| Feature | Fast Transfer | Standard |
|---------|--------------|----------|
| Finality Threshold | 1000 | 2000 |
| Attestation Time | ~3-8 sec | 10-30 min |
| Fee | Small (~0.01%) | Free |
| Best For | Interactive | Background |

### Supported Chains

**Mainnet:**
- Ethereum, Base, Arbitrum, Optimism, Polygon, Solana, Avalanche

**Testnet:**
- Ethereum Sepolia, Base Sepolia, Solana Devnet, + more

---

## 🔐 Security

### Wallet Security
- HD wallet derived from BIP-39 seed phrase
- Same seed generates addresses on all chains
- **Never share or commit your seed phrase**

### Transaction Security
- All transactions signed locally
- No private keys sent over network
- State persisted locally only

### Best Practices
```bash
# Use environment variables for seed
export WALLET_SEED_PHRASE="your twelve word seed phrase here"

# Or use .env file (gitignored)
echo "WALLET_SEED_PHRASE=..." > .env
```

---

## 🌐 Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `WALLET_SEED_PHRASE` | 12-word BIP-39 mnemonic | Yes |
| `NETWORK` | `mainnet` or `testnet` (default: testnet) | No |

---

## 📊 Example Agent Integration

### OpenClaw Skill Usage

```markdown
# In SKILL.md
name: agent-wallet
description: Cross-chain USDC wallet management

commands:
  - balance: Check USDC balance across all chains
  - transfer: Send USDC on a single chain
  - bridge: Move USDC between chains (CCTP V2)
```

### Agent Prompt Example

```
You have access to the AgentWallet skill for managing USDC:

- Use `balance` to check holdings
- Use `transfer <chain> <address> <amount>` for same-chain transfers
- Use `bridge <from> <to> <amount>` for cross-chain transfers

Supported chains: ethereum, base, arbitrum, polygon, solana
```

---

## 🧪 Testing

### Testnet Faucets

| Chain | Faucet |
|-------|--------|
| Ethereum Sepolia | [Circle Faucet](https://faucet.circle.com) |
| Base Sepolia | [Circle Faucet](https://faucet.circle.com) |
| Solana Devnet | [Circle Faucet](https://faucet.circle.com) |

### Run Tests

```bash
# Test wallet creation
node scripts/wallet.js create

# Test balance check
WALLET_SEED_PHRASE="test test test..." node scripts/wallet.js balance

# Test bridge (testnet)
NETWORK=testnet node scripts/bridge-async.js start base_sepolia ethereum_sepolia 1
```

---

## 🏆 Hackathon Submission

**Track:** OpenClaw Skill  
**Prize Pool:** $30,000 USDC

### Why AgentWallet?

1. **Native CCTP V2** - First skill to leverage Fast Transfer
2. **Production Ready** - Async state machine, resumable operations
3. **Multi-Chain** - EVM + Solana support
4. **Agent-First** - Designed for AI agent workflows

### Demo

Watch the bridge complete in ~30 seconds:
1. Approve USDC spend
2. Burn on source chain
3. **Attestation in ~3 seconds** (Fast Transfer!)
4. Mint on destination chain

---

## 📄 License

Apache 2.0

---

## 🔗 Links

- **GitHub:** https://github.com/voltagemonke/Agent-wallet
- **Circle CCTP Docs:** https://developers.circle.com/cctp
- **OpenClaw:** https://openclaw.ai
- **Hackathon:** https://moltbook.com/hackathon

---

Built with ⚡ by the OpenClaw community
