# Mixins

Additional API endpoints for position statistics, positions, and orders on specific protocols.

---

## Position Statistic List (Search After / API Key)

**POST** `https://api.copin.io/trial/position/statistic/search-after`

Retrieve position statistics using cursor-based pagination (search-after pattern). Authenticated via API key only.

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| x-api-key | Yes | API key |

### Request Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| size | number | No | Records per page (max: 1000) |
| pitId | string | No | Point-in-time ID for consistent results (5 min TTL). Omit on first call. |
| searchAfter | array | No | Cursor for next page. Omit on first call. |
| sort | array | No | Sort criteria, e.g., `[{"id": "asc"}]` |
| queries | array | No | Filter: `[{"fieldName": "type", "value": "D15"}]` |

### Request Example

```bash
curl --request POST \
--url "https://api.copin.io/trial/position/statistic/search-after" \
--header 'Content-Type: application/json' \
--header 'x-api-key: YOUR_API_KEY' \
--data '{
    "size": 100,
    "sort": [{"id": "asc"}],
    "queries": [{"fieldName": "type", "value": "D15"}]
}'
```

### Pagination

Use `meta.pitId` and `meta.searchAfter` from the response in subsequent requests to paginate through results.

### Response Meta

| Field | Type | Description |
|-------|------|-------------|
| pitId | string | Point-in-time ID for next request |
| searchAfter | array | Cursor for next page |
| total | number | Total matching records |
| totalPages | number | Total pages |

---

## Position Statistic List (Per Protocol)

**POST** `https://api.copin.io/{protocol}/position/statistic/filter`

Retrieve position statistics for a single protocol.

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| protocol | string | Yes | Protocol identifier |

### Request Body

| Field | Type | Description |
|-------|------|-------------|
| pagination.limit | number | Records per page (default: 20) |
| pagination.offset | number | Records to skip (default: 0) |
| sortBy | string | Sort field (e.g., `pnl`, `realisedPnl`, `avgVolume`) |
| sortType | string | `desc` or `asc` |
| queries | array | Filter: `[{"fieldName": "type", "value": "D15"}]` |

---

## Trader Statistics (Public)

**POST** `https://api.copin.io/public/{protocol}/position/statistic/filter`

Retrieve detailed trader statistics with range-based filtering.

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| protocol | string | Yes | Protocol identifier |

### Request Body

| Field | Type | Description |
|-------|------|-------------|
| pagination | object | `{limit, offset}` |
| sortBy | string | Sort field |
| sortType | string | `desc` or `asc` |
| queries | array | `[{fieldName, value}]` — fieldName: `type`, `account` |
| ranges | array | `[{fieldName, gte, lte}]` — range filters |

### Available Range Fields

- `lastTradeAtTs` — Last trade timestamp
- `runTimeDays` — Days since first trade
- `pnl` / `realisedPnl` — PnL values
- `avgRoi` / `realisedAvgRoi` — Average ROI
- `totalGain` / `realisedTotalGain` — Gain values
- `totalLoss` / `realisedTotalLoss` — Loss values
- `totalFee` / `totalVolume` / `avgVolume` — Volume/fee metrics
- `maxRoi` / `realisedMaxRoi` — Maximum ROI
- `realisedMaxDrawdown` — Max drawdown
- `totalTrade` / `totalWin` / `totalLose` / `totalLiquidation` — Trade counts
- `winRate` / `profitRate` / `realisedProfitRate` — Rate percentages
- `longRate` — Long position percentage
- `orderPositionRatio` — Orders to positions ratio
- `profitLossRatio` / `realisedProfitLossRatio` — Profit/loss ratio
- `gainLossRatio` / `realisedGainLossRatio` — Gain/loss ratio
- `avgLeverage` / `maxLeverage` / `minLeverage` — Leverage metrics
- `avgDuration` / `minDuration` / `maxDuration` — Duration metrics

### Request Example

```json
{
  "pagination": { "limit": 20, "offset": 0 },
  "queries": [{ "fieldName": "type", "value": "D30" }],
  "ranges": [
    { "fieldName": "realisedPnl", "gte": 1000 },
    { "fieldName": "totalWin", "gte": 5 },
    { "fieldName": "winRate", "gte": 80 },
    { "fieldName": "realisedMaxDrawdown", "gte": -30 }
  ],
  "sortBy": "realisedPnl",
  "sortType": "desc"
}
```

---

## Position List (Per Protocol)

**POST** `https://api.copin.io/{protocol}/position/filter`

Retrieve positions for a specific protocol (not filtered by account).

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| protocol | string | Yes | Protocol identifier |

### Request Body

| Field | Type | Description |
|-------|------|-------------|
| pagination | object | `{limit, offset}` |
| sortBy | string | `closeBlockTime`, `openBlockTime`, `size`, `leverage`, `roi`, `pnl`, etc. |
| sortType | string | `desc` or `asc` |
| queries | array | `[{fieldName, value}]` — fieldName: `account`, `status`, `pair` |

---

## Order List (Per Protocol)

**POST** `https://api.copin.io/{protocol}/order/filter`

Retrieve orders for a specific protocol.

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| protocol | string | Yes | Protocol identifier |

### Request Body

| Field | Type | Description |
|-------|------|-------------|
| pagination | object | `{limit, offset}` |
| sortBy | string | `blockTime`, `blockNumber` |
| sortType | string | `desc` or `asc` |
| queries | array | `[{fieldName, value}]` — fieldName: `account`, `status`, `pair`. type values: `OPEN`, `INCREASE`, `DECREASE`, `CLOSE` |
