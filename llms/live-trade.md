# Live Trade

Monitor live positions and orders in real-time across protocols.

---

## Live Order (GraphQL)

**POST** `https://api.copin.io/graphql`

Retrieve live orders for multiple protocols via GraphQL.

### GraphQL Query Name

`searchOrders`

### Headers

| Header | Type | Description |
|--------|------|-------------|
| Authorization | string | JWT token |
| x-api-key | string | API key |

### Request Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| operationName | string | Yes | "Search" |
| variables.index | string | Yes | "copin.orders" |
| variables.body.filter.and | array | Yes | Filter conditions |
| variables.body.sorts | array | Yes | Sorting criteria |
| variables.body.paging | object | Yes | Pagination (size, from) |
| query | string | Yes | GraphQL query string |

### Common Filters

- `protocol`: Protocol names with `in` operator
- `blockTime`: Date range with `gte`/`lte`

### Request Example

```bash
curl 'https://api.copin.io/graphql' \
-H 'x-api-key: YOUR_API_KEY' \
-H 'content-type: application/json' \
--data-raw '{"operationName":"Search","variables":{"index":"copin.orders","body":{"filter":{"and":[{"field":"protocol","in":["GMX_V2","GNS_APE"]},{"field":"blockTime","lte":"2025-08-28T09:34:15.693Z","gte":"2025-08-27T09:34:15.693Z"}]},"sorts":[{"field":"blockTime","direction":"desc"}],"paging":{"size":20,"from":0}}},"query":"query Search($index: String!, $body: SearchPayload!) { searchOrders(index: $index, body: $body) { data { id account protocol txHash pair sizeDeltaNumber priceNumber feeNumber leverage isLong isOpen isClose type blockTime } meta { total limit offset totalPages } } }"}'
```

### Response Fields (data array)

| Field | Type | Description |
|-------|------|-------------|
| id | string | Order unique ID |
| account | string | Trader address |
| protocol | string | Protocol name |
| txHash | string | Transaction hash |
| pair | string | Trading pair |
| sizeDeltaNumber | number | Size delta |
| sizeNumber | number | Size |
| collateralDeltaNumber | number | Collateral delta |
| priceNumber | number | Execution price |
| feeNumber | number | Fee amount |
| leverage | number | Leverage used |
| isLong | boolean | Long position |
| isOpen | boolean | Opened position |
| isClose | boolean | Closed position |
| type | string | OPEN, INCREASE, DECREASE, CLOSE, MARGIN_TRANSFERRED |
| blockTime | string | Block timestamp |

---

## Live Position (GraphQL)

**POST** `https://api.copin.io/graphql`

Retrieve live positions for multiple protocols via GraphQL.

### GraphQL Query Name

`livePosition`

### Request Body

Same structure as Open Interest, but uses `livePosition` query name and adds `protocols` variable.

### Common Filters

- `openBlockTime` or `closeBlockTime`: Date range with `gte`/`lte` (can use `or` conditions)

### Request Example

```bash
curl 'https://api.copin.io/graphql' \
  -H 'x-api-key: YOUR_API_KEY' \
  -H 'content-type: application/json' \
  --data-raw '{"operationName":"Search","variables":{"index":"copin.positions","body":{"filter":{"and":[{"or":[{"field":"openBlockTime","lte":"2025-08-28T08:41:54.337Z","gte":"2025-08-27T08:41:54.337Z"},{"field":"closeBlockTime","lte":"2025-08-28T08:41:54.337Z","gte":"2025-08-27T08:41:54.337Z"}]}]},"sorts":{"field":"openBlockTime","direction":"desc"},"paging":{"size":20,"from":0}},"protocols":["GMX_V2","GNS_APE"]},"query":"query Search($index: String!, $protocols: [String!]!, $body: SearchPayload!) { livePosition(index: $index, protocols: $protocols, body: $body) { data { id account protocol pair size collateral averagePrice pnl realisedPnl roi realisedRoi isLong leverage status openBlockTime closeBlockTime } meta { total limit offset totalPages } } }"}'
```

### Response

Same position fields as Open Interest endpoint.
