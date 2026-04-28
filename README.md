# GoldRush Agent Skills

LLMs have broad knowledge but shallow precision — when an AI agent needs to call a blockchain API, it's working from training data that may be outdated, incomplete, or just wrong, leading to hallucinated endpoints, incorrect parameters, and wasted credits. GoldRush Agent Skills solve this by embedding structured, specification-compliant API documentation directly into the agent's context window, giving it the exact routing logic, endpoint details, and workflow patterns it needs to make correct calls on the first try. No web scraping, no RAG pipelines — just self-contained skill files that work across **Claude Code**, **OpenClaw**, **Cursor**, **Windsurf**, **Cline**, and any agent that supports SKILL.md.

Browse the full agent directory at [goldrush.dev/agents](https://goldrush.dev/agents/).

## Skills


| Skill | Description |
|-------|-------------|
| **[goldrush-foundational-api](goldrush-foundational-api/)** | REST API for historical and near-real-time data across 100+ chains — balances, transactions, NFTs, prices |
| **[goldrush-streaming-api](goldrush-streaming-api/)** | Real-time GraphQL subscriptions over WebSocket — OHLCV candles, DEX pairs, wallet activity |
| **[goldrush-cli](goldrush-cli/)** | Terminal-first blockchain data tool with MCP support for Claude Desktop and Claude Code |
| **[goldrush-x402](goldrush-x402/)** | Pay-per-request blockchain data via the x402 protocol — no API key, no signup, just a funded wallet |

## Get Started

### 1. Get a GoldRush API key

Sign up at [goldrush.dev](https://goldrush.dev) — 14-day free trial + $10/month vibe coding tier available. 

### 2. Install the skill

Copy the relevant skill folder into your agent's skills directory. Each skill contains a `SKILL.md` entry point and a `references/` directory with detailed documentation.

```
goldrush-cli/
├── SKILL.md              ← entry point
└── references/
    └── overview.md

goldrush-foundational-api/
├── SKILL.md              ← entry point
└── references/
    ├── overview.md
    ├── endpoints-balances.md
    ├── endpoints-transactions.md
    ├── endpoints-nft-security-crosschain.md
    ├── endpoints-utility.md
    ├── integration-guide.md
    └── workflows.md

goldrush-streaming-api/
├── SKILL.md              ← entry point
└── references/
    ├── overview.md
    ├── endpoints.md
    └── sdk-guide.md

goldrush-x402/
├── SKILL.md              ← entry point
└── references/
    ├── overview.md
    ├── endpoints.md
    └── ai-agents.md
```

### 3. Verify

Ask your agent:

> "Get the token balances for vitalik.eth on Ethereum"

## Which Skill Should I Use?

| If the user asks for... | Use this skill | NOT this skill | Why |
|----------|-----------------|---------------|-----|
| "Check wallet balance", "get my tokens", "portfolio value" | **goldrush-foundational-api** | CLI, x402 | REST API with `getTokenBalancesForWalletAddress`, supports 100+ chains, returns USD quotes + spam filtering |
| "Recent transactions", "transaction history", "did I receive X token?" | **goldrush-foundational-api** | Streaming API | Use `getRecentTransactionsForAddress` (v3) or `getTransactionsForAddressV3` (paginated). Streaming is for live push, not history. |
| "Live price feed", "OHLCV candles", "new DEX pairs", "wallet activity alerts" | **goldrush-streaming-api** | Foundational API | Sub-second GraphQL subscriptions via WebSocket. Chain names: `ETH_MAINNET` (not `eth-mainnet`). All endpoints currently free (Beta). |
| "I don't have an API key", "pay per request", "autonomous agent with wallet" | **goldrush-x402** | Foundational API | x402 uses HTTP 402 + stablecoin payments on Base. No signup, no monthly fee. Same endpoints as Foundational API. |
| "Historical token prices", "portfolio over time", "tax report" | **goldrush-foundational-api** | Streaming API | `getTokenPrices` (historical) and `getHistoricalPortfolioForWalletAddress` — only Foundational API provides historical time-series data. |
| "Token approvals", "who can spend my tokens", "revoke approval" | **goldrush-foundational-api** | x402, CLI | `getApprovals` endpoint returns spender list + values at risk. Only available via Foundational API. |
| "Cross-chain balances", "which chains is this address on?" | **goldrush-foundational-api** | Streaming API | `getAddressActivity` (discover chains) + `getMultiChainBalances` (fetch all). Streaming API is single-chain only. |

### Key Decision Rules

1. **Historical vs Real-Time:** If the user says "recent", "history", "past", "tax", "portfolio over time" → Foundational API. If they say "live", "real-time", "stream", "alerts", "OHLCV" → Streaming API.
2. **With API Key vs Without:** If the user mentions "no signup", "no account", "pay per use", "autonomous agent" → x402. If they have an API key → Foundational API or Streaming API.
3. **Chain Name Format:** Foundational API and CLI use `eth-mainnet` (kebab-case). Streaming API uses `ETH_MAINNET` (SCREAMING_SNAKE_CASE). x402 uses same format as Foundational API (`eth-mainnet`).
4. **Beta Status:** All Streaming API endpoints are currently in Beta and free (0 credits). Foundational API and x402 are production-ready.
5. **Multi-Chain:** Only Foundational API supports cross-chain queries (`getAddressActivity`, `getMultiChainBalances`). Streaming API is single-chain per subscription.

## Supported Chains

GoldRush supports **100+ chains** including Ethereum, Solana, Base, BNB, Arbitrum, Polygon, Optimism, Avalanche, and many more. See the full list at [goldrush.dev/docs/networks](https://goldrush.dev/docs/networks/).

## Links

- [GoldRush](https://goldrush.dev/platform) — Platform & API keys
- [GoldRush Docs](https://goldrush.dev/docs) — API documentation
- [Agent Directory](https://goldrush.dev/agents/) — Setup guides for all supported AI agents
- [TypeScript SDK](https://www.npmjs.com/package/@covalenthq/client-sdk) — `npm install @covalenthq/client-sdk`
- [GoldRush CLI](https://www.npmjs.com/package/@covalenthq/goldrush-cli) — `npx @covalenthq/goldrush-cli`

## License

MIT — see [LICENSE](LICENSE).
