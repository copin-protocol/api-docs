# Open Interest

Query open interest positions across protocols.

---

## Open Interest Position (GraphQL)

**POST** `https://api.copin.io/graphql`

Retrieve open interest positions for multiple protocols via GraphQL.

### Headers

| Header | Type | Description |
|--------|------|-------------|
| Authorization | string | JWT token |
| x-api-key | string | API key |

### GraphQL Query Name

`searchTopOpeningPosition`

### Request Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| operationName | string | Yes | GraphQL operation name (e.g., "Search") |
| variables.index | string | Yes | Index: "copin.positions" |
| variables.body.filter.and | array | Yes | Filter conditions |
| variables.body.sorts | array | Yes | Sorting criteria |
| variables.body.paging.size | number | No | Records to return (default: 12) |
| variables.body.paging.from | number | No | Records to skip (default: 0) |
| variables.protocols | array | Yes | Array of protocol names (e.g., ["JUPITER", "GMX_V2"]) |
| query | string | Yes | GraphQL query string |

### Common Filters

- `status`: "OPEN" (to get only open positions)
- `openBlockTime`: Date range filters with `gte`/`lte`

### Request Example

```bash
curl 'https://api.copin.io/graphql' \
-H 'x-api-key: YOUR_API_KEY' \
-H 'content-type: application/json' \
--data-raw '{"operationName":"Search","variables":{"index":"copin.positions","body":{"filter":{"and":[{"field":"status","match":"OPEN"},{"field":"openBlockTime","gte":"2025-08-21T09:41:12.017Z","lte":"2025-08-28T09:41:12.017Z"}]},"sorts":[{"field":"realisedPnl","direction":"desc"}],"paging":{"size":100,"from":0}},"protocols":["JUPITER","GMX_V2"]},"query":"query Search($index: String!, $protocols: [String!]!, $body: SearchPayload!) { searchTopOpeningPosition(index: $index, protocols: $protocols, body: $body) { data { id account protocol pair size collateral averagePrice fee pnl realisedPnl roi realisedRoi isLong leverage status openBlockTime } meta { total limit offset totalPages } } }"}'
```

### Response Fields (data array)

| Field | Type | Description |
|-------|------|-------------|
| id | string | Position unique ID |
| account | string | Trader address |
| protocol | string | Protocol name |
| pair | string | Trading pair |
| size | number | Position size |
| collateral | number | Collateral amount |
| averagePrice | number | Average entry price |
| fee | number | Fee paid |
| pnl | number | PnL including fee |
| realisedPnl | number | PnL without fee |
| roi | number | ROI including fee |
| realisedRoi | number | ROI without fee |
| isLong | boolean | Long position |
| isWin | boolean | Winning position |
| isLiquidate | boolean | Was liquidated |
| leverage | number | Leverage used |
| orderCount | number | Number of orders |
| status | string | OPEN or CLOSE |
| openBlockTime | string | Open timestamp |
| durationInSecond | number | Duration in seconds |

### Response Meta

| Field | Type | Description |
|-------|------|-------------|
| total | number | Total records |
| limit | number | Records returned |
| offset | number | Records skipped |
| totalPages | number | Total pages |
