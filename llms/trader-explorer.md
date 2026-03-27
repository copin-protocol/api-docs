# Trader Explorer

Search and filter traders across multiple protocols using GraphQL.

---

## Position Statistic List (GraphQL)

**POST** `https://api.copin.io/graphql`

Retrieve position statistics for multiple protocols via GraphQL.

### Headers

| Header | Type | Description |
|--------|------|-------------|
| Authorization | string | JWT token: `Authorization {token}` |
| x-api-key | string | API key: `x-api-key {api-key}` |

### GraphQL Query Name

`searchPositionStatistic`

### Request Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| operationName | string | Yes | GraphQL operation name (e.g., "Search") |
| variables | object | Yes | GraphQL variables |
| variables.index | string | Yes | Index to search (e.g., "copin.position_statistics_v2") |
| variables.body | object | Yes | Query parameters |
| variables.body.filter | object | Yes | Filter criteria |
| variables.body.filter.and | array | Yes | Array of filter conditions (all must be satisfied) |
| variables.body.sorts | array | Yes | Sorting criteria |
| variables.body.paging | object | Yes | Pagination |
| variables.body.paging.size | number | No | Records to return (default: 12) |
| variables.body.paging.from | number | No | Records to skip (default: 0) |
| query | string | Yes | GraphQL query string |

### Filter Conditions

Each filter condition in the `and` array can use these operators:
- `field`: Field name to filter on
- `gte`: Greater than or equal
- `lte`: Less than or equal
- `in`: Array of values to match
- `match`: Exact match

Common filterable fields: `totalTrade`, `profitRate`, `pnl`, `winRate`, `runTimeDays`, `protocol`, `type`

### Request Example

```bash
curl 'https://api.copin.io/graphql' \
  -H 'content-type: application/json' \
  -H 'x-api-key: YOUR_API_KEY' \
  --data-raw '{"operationName":"Search","variables":{"index":"copin.position_statistics_v2","body":{"filter":{"and":[{"field":"totalTrade","gte":"2"},{"field":"profitRate","gte":"60"},{"field":"pnl","gte":"100"},{"field":"winRate","gte":"51"},{"field":"runTimeDays","gte":"7"},{"field":"protocol","in":["GMX","JUPITER"]},{"field":"type","match":"D30"}]},"sorts":[{"field":"realisedPnl","direction":"desc"}],"paging":{"size":12,"from":0}}},"query":"query Search($index: String!, $body: SearchPayload!) { searchPositionStatistic(index: $index, body: $body) { data { id account protocol type totalTrade totalWin totalLose winRate pnl realisedPnl avgRoi realisedAvgRoi maxRoi realisedMaxRoi totalVolume avgVolume avgLeverage maxLeverage runTimeDays lastTradeAt lastTradeAtTs pairs } meta { total limit offset totalPages } } }"}'
```

### Response Fields (data array)

| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique identifier |
| account | string | Trader's address |
| protocol | string | Protocol identifier |
| type | string | Time period (e.g., "D15", "D30") |
| totalTrade | number | Total trades |
| totalWin | number | Winning trades |
| totalLose | number | Losing trades |
| totalGain | number | Total gain from all trades |
| realisedTotalGain | number | Realised total gain (closed positions) |
| totalLoss | number | Total loss from all trades |
| realisedTotalLoss | number | Realised total loss (closed positions) |
| totalVolume | number | Total trading volume |
| avgVolume | number | Average volume per trade |
| avgRoi | number | Average ROI across all trades |
| realisedAvgRoi | number | Realised average ROI |
| maxRoi | number | Maximum ROI in a single trade |
| realisedMaxRoi | number | Realised maximum ROI |
| pnl | number | Total profit and loss |
| realisedPnl | number | Realised PnL (closed positions) |
| maxPnl | number | Maximum PnL achieved |
| realisedMaxPnl | number | Realised maximum PnL |
| realisedMaxDrawdown | number | Max drawdown from realised positions |
| realisedMaxDrawdownPnl | number | Max drawdown amount |
| winRate | number | Win rate percentage |
| profitRate | number | Profitable trades percentage |
| realisedProfitRate | number | Profitable closed positions percentage |
| orderPositionRatio | number | Orders to positions ratio |
| profitLossRatio | number | Profit to loss ratio |
| realisedProfitLossRatio | number | Realised profit to loss ratio |
| longRate | number | Long positions percentage |
| gainLossRatio | number | Gains to losses ratio |
| realisedGainLossRatio | number | Realised gains to losses ratio |
| avgDuration | number | Average position duration (hours) |
| minDuration | number | Minimum position duration (hours) |
| maxDuration | number | Maximum position duration (hours) |
| avgLeverage | number | Average leverage |
| minLeverage | number | Minimum leverage |
| maxLeverage | number | Maximum leverage |
| totalLiquidation | number | Total liquidated positions |
| totalLiquidationAmount | number | Total liquidation amount |
| runTimeDays | number | Days trader has been active |
| lastTradeAtTs | number | Last trade timestamp (ms) |
| totalFee | number | Total fees paid |
| statisticAt | string | Statistics calculation timestamp |
| lastTradeAt | string | Last trade timestamp (ISO) |
| indexTokens | array | Token identifiers traded |
| createdAt | string | Record creation timestamp |
| isOpenPosition | boolean | Has open positions |

### Response Meta

| Field | Type | Description |
|-------|------|-------------|
| limit | number | Records returned |
| offset | number | Records skipped |
| total | number | Total matching records |
| totalPages | number | Total pages |

---

## PnL Statistic

**POST** `https://api.copin.io/public/position/statistic/pnl-statistics-v2`

Retrieve PnL time series data for specific accounts.

### Headers

| Header | Type | Description |
|--------|------|-------------|
| Authorization | string | JWT token |
| x-api-key | string | API key |

### Request Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| accounts | array | Yes | List of account/protocol pairs |
| accounts[].account | string | Yes | Account address |
| accounts[].protocol | string | Yes | Protocol (e.g., "KILOEX_BASE") |
| statisticType | string | No | Time period: FULL, D60, D30, D15, D7 |

### Request Example

```bash
curl --request POST \
--url "https://api.copin.io/public/position/statistic/pnl-statistics-v2" \
--header 'Content-Type: application/json' \
--header 'x-api-key: YOUR_API_KEY' \
--data '{
  "accounts": [
    {
      "account": "0xd649A0876453Fc7626569B28E364262192874E18",
      "protocol": "KILOEX_BASE"
    }
  ],
  "statisticType": "D15"
}'
```

### Response

Object keyed by account address, each containing:

| Field | Type | Description |
|-------|------|-------------|
| date | array | Array of dates (ISO format) |
| realisedPnl | array | Realised PnL values per date |
| unrealisedPnl | array | Unrealised PnL values per date |
| fee | array | Fee values per date |

### Response Example

```json
{
  "0xd649A0876453Fc7626569B28E364262192874E18": {
    "date": ["2025-04-12T00:00:00.000Z", "2025-04-13T00:00:00.000Z", "..."],
    "realisedPnl": [0, 0, 3323232.953125, "..."],
    "unrealisedPnl": [0, 0, 0, "..."],
    "fee": [0, 0, 47.004, "..."]
  }
}
```
