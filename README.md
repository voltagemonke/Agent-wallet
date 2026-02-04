# AgentWallet 👛

Multi-chain wallet skill for AI agents. One seed phrase, all chains.

**Built for Circle USDC Hackathon 2026** | OpenClaw Skill Track

## Features

- 🔐 **BIP-39 seed generation** - Standard 12-word mnemonic
- 🌐 **Multi-chain support** - Solana, Base, Ethereum from single seed
- 💵 **USDC native** - First-class support for Circle USDC
- 🌉 **Cross-chain bridging** - Bridge USDC via Circle CCTP V2
- 🔒 **Security-first** - Seeds shown once, never logged, memory-only derivation
- 🤖 **Agent-ready** - Designed for AI agent integration

## Quick Start

```bash
# Install
npm install

# Create new wallet
node scripts/wallet.js create

# Show addresses (after adding seed to .env)
node scripts/wallet.js addresses

# Check balances
node scripts/wallet.js balance

# Transfer USDC (same chain)
node scripts/wallet.js transfer solana USDC 10 <recipient>

# Bridge USDC (cross-chain)
node scripts/wallet.js bridge base solana 10
```

## Commands

| Command | Description |
|---------|-------------|
| `create` | Generate new wallet (shows seed phrase once) |
| `addresses` | Show derived addresses for all chains |
| `balance [chain]` | Check native + USDC balances |
| `transfer <chain> <token> <amount> <recipient>` | Transfer tokens (same chain) |
| `bridge <from> <to> <amount>` | Bridge USDC cross-chain (CCTP V2) |
| `chains` | List supported chains and USDC contracts |

## Environment Variables

```bash
# Required
WALLET_SEED_PHRASE="your twelve word seed phrase here"

# Optional
NETWORK=testnet          # testnet (default) or mainnet
SOLANA_RPC=              # Custom Solana RPC
BASE_RPC=                # Custom Base RPC  
ETH_RPC=                 # Custom Ethereum RPC
```

## Supported Chains

| Chain | Native Token | USDC Support | Testnet |
|-------|--------------|--------------|---------|
| Solana | SOL | ✅ | Devnet |
| Base | ETH | ✅ | Sepolia |
| Ethereum | ETH | ✅ | Sepolia |

## Security Model

- **One seed per agent** - Each agent instance isolated
- **Seed shown once** - Only at creation, never in logs
- **Memory only** - Private keys derived on-demand
- **No chat import** - Seeds added via `.env` only (not via chat)

## OpenClaw Integration

This is an [OpenClaw](https://openclaw.ai) skill. Install it in your agent:

```bash
# Copy to skills directory
cp -r agent-wallet ~/.openclaw/skills/

# Or use the .skill package
openclaw skill install agent-wallet.skill
```

Then your agent can respond to:
- "Create a new wallet"
- "Show my addresses"
- "Check my balance"
- "Send 10 USDC to 0x..."

## Derivation Paths

| Chain | Path | Standard |
|-------|------|----------|
| Solana | `m/44'/501'/0'/0'` | Solana/Phantom |
| EVM | `m/44'/60'/0'/0/0` | BIP-44 Ethereum |

EVM chains (Base, Ethereum) share the same derived address.

## License

MIT

---

**Hackathon Submission** | Circle USDC Hackathon 2026 | OpenClaw Skill Track ($10k)
