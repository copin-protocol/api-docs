# Copin API Documentation

**Comprehensive API for accessing perpetual trading data, trader statistics, positions, and copy trading across 50+ DeFi protocols.**

**Ready to start?** Contact us for API access or jump to the API Reference below.

## Introduction

The Copin API provides programmatic access to on-chain perpetual trading data across multiple DeFi protocols. It enables you to explore trader statistics, monitor live positions, query historical trades, manage copy trading, and more.

This documentation is designed to be LLM-friendly — structured for easy parsing by AI agents and language models.

## Base URL

All API requests should be made to:

```
https://api.copin.io
```

## Authentication

All API requests require authentication. Two methods are supported:

**API Key (recommended for data access):**
```
x-api-key: YOUR_API_KEY
```

**JWT Token (required for user-specific endpoints like copy trading):**
```
Authorization: YOUR_JWT_TOKEN
```

To obtain a JWT token, use the Web3 login flow (see Authentication section).

## Rate Limiting

Default rate limit: 30 requests per minute.

## What This Documentation Covers

- **Trader Explorer** — Search and filter traders across protocols using GraphQL
- **Hyperliquid Explorer** — Search and filter Hyperliquid traders and PnL statistics
- **Trader Profile** — Get detailed statistics and position history for specific traders
- **Open Interest** — Query open interest positions across protocols
- **Live Trade** — Monitor live positions and orders in real-time
- **Trader Board** — Access trader leaderboards
- **Authentication** — Web3 login/logout and user management
- **Copy Trades** — Create, manage, and monitor copy trading configurations
- **Wallets** — Manage embedded wallets, deposits, and withdrawals
- **Tokens** — Get token information across all protocols

## Supported Protocols

The API supports 50+ perpetual trading protocols including:

GMX, GMX_V2, GMX_V2_AVAX, GMX_AVAX, GMX_SOL, KWENTA, POLYNOMIAL, POLYNOMIAL_L2, GNS, GNS_POLY, GNS_BASE, GNS_APE, LEVEL_BNB, LEVEL_ARB, MUX_ARB, APOLLOX_BNB, APOLLOX_BASE, AVANTIS_BASE, EQUATION_ARB, LOGX_BLAST, LOGX_MODE, MYX_ARB, DEXTORO, VELA_ARB, HMX_ARB, SYNTHETIX, SYNTHETIX_V3, SYNTHETIX_V3_ARB, KTX_MANTLE, CYBERDEX, YFX_ARB, KILOEX_OPBNB, KILOEX_BNB, KILOEX_MANTA, KILOEX_BASE, ROLLIE_SCROLL, MUMMY_FANTOM, HYPERLIQUID, SYNFUTURE_BASE, MORPHEX_FANTOM, PERENNIAL_ARB, BSX_BASE, DYDX, UNIDEX_ARB, VERTEX_ARB, HORIZON_BNB, HOLDSTATION_ZKSYNC, HOLDSTATION_BERA, ZENO_METIS, LINEHUB_LINEA, BMX_BASE, FOXIFY_ARB, DEPERP_BASE, ELFI_ARB, PERPETUAL_OP, JUPITER, OSTIUM_ARB

## Quick Links

- App: https://app.copin.io
- Docs: https://docs.copin.io
- GitHub: https://github.com/copin-protocol
- Twitter: https://x.com/copin_io

## Example Integration

```javascript
async function getLeaderboardData() {
  const url = "https://api.copin.io/leaderboards-v2/page";
  const apiKey = "YOUR_COPIN_API_KEY";
  const headers = {
    "X-API-KEY": apiKey,
    Accept: "application/json",
  };
  const params = new URLSearchParams({
    protocol: "GMX",
    queryDate: "1741408905620",
    statisticType: "MONTH",
    offset: "0",
    limit: "10",
    sort_by: "ranking",
    sort_type: "asc",
  });
  const response = await fetch(`${url}?${params}`, { method: "GET", headers });
  const data = await response.json();
  return data;
}
```

## API Sections

- [Authentication](./authentication.md)
- [Trader Explorer](./trader-explorer.md)
- [Hyperliquid Explorer](./hyperliquid-explorer.md)
- [Trader Profile](./trader-profile.md)
- [Open Interest](./open-interest.md)
- [Live Trade](./live-trade.md)
- [Trader Board](./trader-board.md)
- [Copy Trades](./copy-trades.md)
- [Wallets](./wallets.md)
- [Tokens](./tokens.md)
- [Mixins](./mixins.md)
