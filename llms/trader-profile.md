# Trader Profile

Get detailed statistics and position history for specific traders.

---

## Position Statistic of Trader

**GET** `https://api.copin.io/public/{protocol}/position/statistic/trader/{account}`

Retrieve position statistics for a specific trader across time periods.

### Headers

| Header | Type | Description |
|--------|------|-------------|
| Authorization | string | JWT token |
| x-api-key | string | API key |

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| protocol | string | Yes | Protocol identifier (e.g., "KILOEX_BASE", "GMX", "JUPITER") |
| account | string | Yes | Trader's wallet address |

### Request Example

```bash
curl -X GET "https://api.copin.io/public/KILOEX_BASE/position/statistic/trader/0xd649A0876453Fc7626569B28E364262192874E18" \
--header 'x-api-key: YOUR_API_KEY'
```

### Response

Returns statistics grouped by time period keys: `FULL`, `D60`, `D30`, `D15`, `D7`.

Each period contains:

| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique identifier |
| account | string | Trader address |
| totalTrade | number | Total trades |
| totalWin / totalLose | number | Win/lose counts |
| totalGain / totalLoss | number | Gain/loss amounts |
| realisedTotalGain / realisedTotalLoss | number | Realised amounts |
| totalVolume / avgVolume | number | Volume metrics |
| avgRoi / realisedAvgRoi | number | ROI metrics |
| maxRoi / realisedMaxRoi | number | Max ROI |
| pnl / realisedPnl | number | PnL values |
| maxPnl / realisedMaxPnl | number | Max PnL |
| realisedMaxDrawdown / realisedMaxDrawdownPnl | number | Drawdown metrics |
| winRate / profitRate / realisedProfitRate | number | Rate percentages |
| longRate | number | Long positions percentage |
| avgDuration / minDuration / maxDuration | number | Duration (hours) |
| avgLeverage / minLeverage / maxLeverage | number | Leverage metrics |
| totalLiquidation / totalLiquidationAmount | number | Liquidation data |
| runTimeDays | number | Active days |
| totalFee | number | Total fees |
| ranking | object | Percentile rankings for various metrics |
| type | string | Period type (FULL, D60, D30, D15, D7) |
| protocol | string | Protocol identifier |
| indexTokens | array | Token identifiers |
| isOpenPosition | boolean | Has open positions |

---

## Position List By Account

**POST** `https://api.copin.io/{protocol}/position/filter/{account}`

Retrieve positions for a specific trader on a protocol.

### Headers

| Header | Type | Description |
|--------|------|-------------|
| Authorization | string | JWT token |
| x-api-key | string | API key |

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| protocol | string | Yes | Protocol identifier |
| account | string | Yes | Trader's wallet address |

### Request Body

| Field | Type | Description |
|-------|------|-------------|
| pagination.limit | number | Records to return (default: 20) |
| pagination.offset | number | Records to skip (default: 0) |
| sortBy | string | Sort field: `closeBlockTime`, `openBlockTime`, `averagePrice`, `size`, `leverage`, `durationInSecond`, `orderCount`, `roi`, `realisedRoi`, `pnl`, `realisedPnl` |
| sortType | string | Sort direction: `desc`, `asc` |
| queries | array | Filter array with objects: `{fieldName, value}`. fieldName: `account`, `status`, `pair`. status values: `CLOSE`, `OPEN` |

### Request Example

```bash
curl 'https://api.copin.io/JUPITER/position/filter/3XVpxh8hHGhEgnAEmLBy5v7vJuR2gjVk7qMF5yKcfGQy' \
  -H 'x-api-key: YOUR_API_KEY' \
  -H 'content-type: application/json' \
  --data-raw '{"pagination":{"limit":40,"offset":0},"queries":[{"fieldName":"status","value":"CLOSE"}],"sortBy":"closeBlockTime","sortType":"desc"}'
```

### Response Fields (data array)

| Field | Type | Description |
|-------|------|-------------|
| id | string | Position unique ID |
| account | string | Trader address |
| key | string | Position unique key |
| pair | string | Trading pair (e.g., "ETH-USDT") |
| size | number | Position size |
| collateral | number | Collateral amount |
| averagePrice | number | Average entry price |
| fee | number | Fee paid |
| funding | number | Funding amount |
| pnl | number | PnL including fee |
| realisedPnl | number | PnL without fee |
| roi | number | ROI including fee |
| realisedRoi | number | ROI without fee |
| isLong | boolean | Long position |
| isWin | boolean | Winning position |
| isLiquidate | boolean | Was liquidated |
| leverage | number | Leverage used |
| orderCount | number | Number of orders |
| durationInSecond | number | Duration in seconds |
| status | string | OPEN or CLOSE |
| openBlockTime | string | Open timestamp |
| closeBlockTime | string | Close timestamp |
| protocol | string | Protocol name |

### Response Meta

| Field | Type | Description |
|-------|------|-------------|
| limit | number | Records returned |
| offset | number | Records skipped |
| total | number | Total records |
| totalPages | number | Total pages |

---

## Position By ID

**GET** `https://api.copin.io/{protocol}/position/detail/{positionId}`

Retrieve detailed information for a specific position, including its orders.

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| protocol | string | Yes | Protocol identifier |
| positionId | string | Yes | Position unique ID |

### Request Example

```bash
curl -X GET "https://api.copin.io/KWENTA/position/detail/66770ea792f8a3cde4c37a54" \
-H 'x-api-key: YOUR_API_KEY'
```

### Response

Same position fields as above, plus:

| Field | Type | Description |
|-------|------|-------------|
| orders | array | Array of orders for this position |
| orders[].txHash | string | Transaction hash |
| orders[].blockTime | string | Block timestamp |
| orders[].sizeDeltaNumber | number | Size delta |
| orders[].collateralDeltaNumber | number | Collateral delta |
| orders[].priceNumber | number | Execution price |
| orders[].feeNumber | number | Fee amount |
| orders[].leverage | number | Leverage |
| orders[].isLong | boolean | Long position |
| orders[].isOpen | boolean | Opened position |
| orders[].isClose | boolean | Closed position |
| orders[].type | string | OPEN, INCREASE, DECREASE, CLOSE, MARGIN_TRANSFERRED |

---

## Position By Transaction Hash

**GET** `https://api.copin.io/{protocol}/position/{txHash}`

Retrieve positions associated with a specific transaction hash.

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| protocol | string | Yes | Protocol identifier |
| txHash | string | Yes | Open order transaction hash |

### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| logId | string | Open order log ID |
| account | string | Trader's account address |
| isLong | boolean | Whether position is long |

### Request Example

```bash
curl --request GET \
  --url "https://api.copin.io/GMX_V2/position/0xe614b4c0eee87864c62ee58d8caf156ab20c414c15212a851054814806513a27" \
  --header 'x-api-key: YOUR_API_KEY'
```

### Response

Returns an array of position objects (same fields as Position By ID, including orders).
