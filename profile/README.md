# HexMarket SDKs

Official SDKs for the [HexMarket](https://www.hexmarket.xyz) prediction market protocol on Solana.

## Overview

HexMarket is a decentralized prediction market where anyone can trade on real-world event outcomes with trustless, on-chain settlement. These SDKs provide programmatic access to the full HexMarket API — enabling quantitative trading, market making, analytics, and custom integrations.

### What You Can Build

- **Trading bots** — Automated order placement, cancellation, and portfolio management
- **Market makers** — Symmetric quoting, inventory management, cross-outcome arbitrage
- **Analytics tools** — Orderbook monitoring, price history, volume tracking
- **Custom UIs** — Build your own trading interface on top of HexMarket

### API Capabilities

All SDKs cover the complete HexMarket API surface:

| Category | Endpoints | Auth |
|----------|-----------|------|
| **Markets & Events** | Browse events, outcomes, categories | Public |
| **Orderbook** | Direct and merged (cross-outcome) orderbook | Public |
| **Trades** | Trade history and price snapshots | Public |
| **Orders** | Place, cancel, list open/closed orders | L2 (HMAC) |
| **Balances** | USDC balance, locked collateral | L2 (HMAC) |
| **Positions** | Outcome token holdings | L2 (HMAC) |
| **Vault** | Create vault, deposit, withdraw, submit transactions | L2 (HMAC) |

### Authentication

HexMarket uses a two-layer authentication model:

- **L1 (Wallet):** Ed25519 signature for one-time API key creation
- **L2 (HMAC):** API-key HMAC-SHA256 for all trading requests — handled automatically by the SDKs

Each order also includes an Ed25519 wallet signature, required for trustless on-chain settlement via Solana's Ed25519Verify.

## SDKs

### Rust SDK

Low-level, high-performance client for latency-sensitive trading systems.

| | |
|---|---|
| **Repository** | [github.com/hexmarketxyz/hexmarket_rust_sdk](https://github.com/hexmarketxyz/hexmarket_rust_sdk) |
| **Install** | `hexmarket-sdk = { git = "https://github.com/hexmarketxyz/hexmarket_rust_sdk.git" }` |
| **Async Runtime** | Tokio |
| **HTTP Client** | reqwest |
| **Key Dependencies** | `ed25519-dalek`, `hmac`, `rust_decimal` |

```rust
use hexmarket_sdk::*;

let client = HexClient::new(HexClientConfig {
    api_url: "https://api.hexmarket.xyz".into(),
});
let events = client.list_events(&Default::default()).await?;
```

### Python SDK

Async client with Pydantic models — ideal for research, backtesting, and rapid prototyping.

| | |
|---|---|
| **Repository** | [github.com/hexmarketxyz/hexmarket_python_sdk](https://github.com/hexmarketxyz/hexmarket_python_sdk) |
| **Install** | `pip install git+https://github.com/hexmarketxyz/hexmarket_python_sdk.git` |
| **Requires** | Python 3.10+ |
| **HTTP Client** | httpx (async) |
| **Key Dependencies** | `pydantic`, `pynacl`, `base58` |

```python
from hexmarket_sdk import HexClient

async with HexClient("https://api.hexmarket.xyz") as client:
    events = await client.list_events(status="active")
```

### TypeScript SDK

First-party SDK used by the HexMarket web app — also available as a standalone package.

| | |
|---|---|
| **Repository** | [github.com/hexmarketxyz/hexmarket_typescript_sdk](https://github.com/hexmarketxyz/hexmarket_typescript_sdk) |
| **Install** | `npm install git+https://github.com/hexmarketxyz/hexmarket_typescript_sdk.git` |
| **Runtime** | Browser & Node.js 18+ |
| **Key Dependencies** | Web Crypto API for HMAC signing, full TypeScript type definitions |

```typescript
import { HexMarketClient } from '@hexmarket/sdk';

const client = new HexMarketClient({ apiUrl: 'https://api.hexmarket.xyz' });
const markets = await client.markets.list();
```

## Getting Started

1. **Get API credentials** — Create an account on [www.hexmarket.xyz](https://www.hexmarket.xyz), connect your Solana wallet, and generate API keys from the settings page.

2. **Install an SDK** — Pick the language that fits your stack.

3. **Set credentials** — Pass your `api_key`, `secret`, and `passphrase` to the client. The SDK signs every request automatically.

4. **Start trading** — Browse markets, read orderbooks, place orders. See the `examples/` directory in each SDK for working code.

## License

MIT
