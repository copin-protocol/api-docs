# Copy Trades

Create, manage, and monitor copy trading configurations. All endpoints require JWT authentication.

---

## Copy Wallet List

**GET** `https://api.copin.io/copy-wallets/list`

Retrieve all copy trade wallets for the authenticated user.

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| Authorization | Yes | JWT token |

### Response (array)

| Field | Type | Description |
|-------|------|-------------|
| id | string | Wallet unique ID |
| userId | string | Owner user ID |
| name | string | Wallet display name |
| exchange | string | Connected exchange (e.g., "HYPERLIQUID") |
| hyperliquid.apiKey | string | HyperLiquid API key |
| hyperliquid.embeddedWallet | string | Embedded wallet address |
| hyperliquid.isEmbedded | boolean | Is embedded wallet |
| hyperliquid.verified | boolean | Is verified |
| balance | number | Total balance |
| availableBalance | number | Available balance |
| createdAt | string | Creation timestamp |

---

## Copy Trade List

**POST** `https://api.copin.io/copy-trades/list`

Retrieve all copy trade configurations.

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| Authorization | Yes | JWT token |

### Request Body

| Field | Type | Description |
|-------|------|-------------|
| ids | string[] | Optional array of copy trade IDs to filter |

### Response (array)

| Field | Type | Description |
|-------|------|-------------|
| id | string | Copy trade ID |
| title | string | Display name |
| userId | string | Owner user ID |
| account | string | Account being copied |
| accounts | array | Additional accounts for multiple copying |
| copyWalletId | string | Wallet used for copying |
| tokenAddresses | array | Specific tokens to copy |
| excludingTokenAddresses | array | Tokens to exclude |
| stopLossType | string | "PERCENT" or "USD" |
| enableStopLoss | boolean | Stop loss enabled |
| stopLossAmount | number | Stop loss value |
| takeProfitType | string | "PERCENT" or "USD" |
| enableTakeProfit | boolean | Take profit enabled |
| takeProfitAmount | number | Take profit value |
| maxVolMultiplier | number | Max volume multiplier |
| leverage | number | Leverage setting |
| volume | number | Volume amount |
| volumeProtection | boolean | Volume protection enabled |
| lookBackOrders | number | Orders to look back |
| reverseCopy | boolean | Copy in reverse direction |
| skipLowLeverage | boolean | Skip low leverage trades |
| lowLeverage | number | Low leverage threshold |
| skipLowCollateral | boolean | Skip low collateral trades |
| skipLowSize | boolean | Skip low size trades |
| copyAll | boolean | Copy all trades |
| status | string | RUNNING or STOPPED |
| multipleCopy | boolean | Multiple copying enabled |
| exchange | string | Exchange platform |
| side | string | BOTH, LONG, or SHORT |
| type | string | Copy trading type (e.g., "COPY_TRADER") |
| protocol | string | Protocol name |
| pnl7D | number | 7-day PnL |
| pnl30D | number | 30-day PnL |
| pnl | number | Total PnL |
| createdAt | string | Creation timestamp |

---

## Create Copy Trade

**POST** `https://api.copin.io/copy-trades`

Create a new copy trade configuration.

### Required Fields

| Field | Type | Description |
|-------|------|-------------|
| title | string | Display name |
| volume | number | Volume for copied trades |
| account | string | Account to copy from |
| leverage | number | Leverage setting |
| protocol | string | Protocol (e.g., "KILOEX_OPBNB") |
| exchange | string | Exchange (e.g., "HYPERLIQUID") |
| copyWalletId | string | Wallet ID for copying |
| type | string | Copy type (e.g., "COPY_TRADER") |
| excludingTokenAddresses | array | Tokens to exclude |
| tokenAddresses | array | Specific tokens to copy |

### Optional Fields

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| reverseCopy | boolean | false | Copy in reverse |
| enableStopLoss | boolean | false | Enable stop loss |
| stopLossType | string | "USD" | "PERCENT" or "USD" |
| stopLossAmount | number | - | Stop loss value |
| volumeProtection | boolean | true | Volume protection |
| lookBackOrders | number | - | Orders to look back |
| enableTakeProfit | boolean | false | Enable take profit |
| takeProfitType | string | "USD" | "PERCENT" or "USD" |
| takeProfitAmount | number | - | Take profit value |
| maxVolMultiplier | number | - | Max volume multiplier |
| skipLowLeverage | boolean | false | Skip low leverage |
| lowLeverage | number | - | Min leverage threshold |
| skipLowCollateral | boolean | false | Skip low collateral |
| lowCollateral | number | - | Min collateral threshold |
| skipLowSize | boolean | false | Skip low size |
| lowSize | number | - | Min size threshold |
| copyAll | boolean | true | Copy all trades |
| hasExclude | boolean | false | Apply exclusion rules |
| multipleCopy | boolean | false | Enable multiple copying |
| side | string | "BOTH" | "BOTH", "LONG", "SHORT" |

### Response

Returns the created copy trade object with all fields.

---

## Update Copy Trade

**PUT** `https://api.copin.io/copy-trades/{id}`

Update an existing copy trade configuration. Only send fields you want to update.

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| id | string | Yes | Copy trade ID |

### Request Example

```bash
curl --request PUT \
--url "https://api.copin.io/copy-trades/67fe13a16c6729db9c4b8f3c" \
--header 'Authorization: YOUR_JWT_TOKEN' \
--header 'Content-Type: application/json' \
--data '{"volume": 20}'
```

### Response

Returns the updated copy trade object.

---

## Pre-Delete Copy Trade

**GET** `https://api.copin.io/copy-trades/pre-delete/{id}`

Check if a copy trade can be deleted (no open positions).

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| id | string | Yes | Copy trade ID |

### Response

| Field | Type | Description |
|-------|------|-------------|
| totalOpeningPositions | number | Must be 0 to allow deletion |

---

## Delete Copy Trade

**DELETE** `https://api.copin.io/copy-trades/{id}`

Delete a copy trade configuration. Must have no open positions (check with pre-delete first).

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| id | string | Yes | Copy trade ID |

### Response

200 OK

---

## Copy Positions

**POST** `https://api.copin.io/copy-positions/page`

Retrieve copy positions with pagination.

### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| limit | number | Records per page (default: 20) |
| offset | number | Records to skip (default: 0) |
| status | string | "OPEN" or "CLOSE" |
| isLong | boolean | Position side |

### Response Fields (data array)

| Field | Type | Description |
|-------|------|-------------|
| id | string | Copy position ID |
| copyTradeId | string | Copy trade config ID |
| copyAccount | string | Address copied from |
| pair | string | Trading pair |
| isLong | boolean | Long position |
| entryPrice | number | Entry price |
| leverage | number | Leverage |
| pnl | number | Total PnL |
| realisedPnl | number | Realised PnL |
| fee | number | Fees paid |
| status | string | OPEN or CLOSE |
| closeType | string | How closed: COPY_TRADE, MANUAL, FORCE_CLOSE, TAKE_PROFIT, STOP_LOSS, LIQUIDATE, OVERWRITE |
| protocol | string | Protocol |
| exchange | string | Exchange |
| isReverse | boolean | Reverse copy |
| copyTradeTitle | string | Copy trade title |
| createdAt | string | Creation timestamp |

---

## Copy Orders

**GET** `https://api.copin.io/copy-positions/{id}/orders`

Retrieve orders for a specific copy position.

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| id | string | Yes | Copy position ID |

### Response (array)

| Field | Type | Description |
|-------|------|-------------|
| price | number | Execution price |
| size | number | Order size |
| pnl | number | Order PnL |
| realisedPnl | number | Realized PnL |
| fee | number | Fee paid |
| isLong | boolean | Long position |
| isIncrease | boolean | Increases position |
| protocol | string | Protocol |
| txHash | string | Transaction hash |
| createdAt | string | Creation timestamp |

---

## Activity Logs

**POST** `https://api.copin.io/activity-logs/page`

Retrieve copy trading activity logs.

### Request Body

| Field | Type | Description |
|-------|------|-------------|
| pagination.limit | number | Records per page (default: 20) |
| pagination.offset | number | Records to skip (default: 0) |
| sortBy | string | Sort field (default: "createdAt") |
| sortType | string | "asc" or "desc" (default: "desc") |

### Response Fields (data array)

| Field | Type | Description |
|-------|------|-------------|
| id | string | Log ID |
| protocol | string | Protocol |
| sourceAccount | string | Source account copied from |
| copyTradeTitle | string | Copy trade name |
| exchange | string | Exchange |
| pair | string | Trading pair |
| price | number | Execution price |
| sourcePrice | number | Source price |
| volume | number | Volume |
| leverage | number | Leverage |
| isLong | boolean | Long position |
| type | string | OPEN or CLOSE |
| isSuccess | boolean | Was successful |
| sourceTxHash | string | Source transaction hash |
| copyWalletName | string | Copy wallet name |
| isReverse | boolean | Reverse copy |
| createdAt | string | Activity timestamp |

---

## Hyperliquid Wallet List

**GET** `https://api.copin.io/copy-wallets/hyperliquid-embedded/list`

Retrieve Hyperliquid embedded wallets with balance and PnL info.

### Response (array)

Same as Copy Wallet List, plus:

| Field | Type | Description |
|-------|------|-------------|
| pnl24h | number | 24-hour PnL |
| totalPnl | number | Total PnL |

---

## Get All Tokens

**GET** `https://api.copin.io/pairs/tokens`

Retrieve tokens for all protocols. No authentication required.

### Response

Object keyed by protocol name, each containing array of:

| Field | Type | Description |
|-------|------|-------------|
| symbol | string | Token symbol (e.g., "ETH", "BTC") |
| indexTokens | array | Contract addresses for this token |
| isForex | boolean | Whether it's a forex pair |

### Response Example

```json
{
  "data": {
    "APOLLOX_BASE": [
      { "symbol": "ETH", "indexTokens": ["0x4200..."], "isForex": false },
      { "symbol": "BTC", "indexTokens": ["0xBc76..."], "isForex": false }
    ]
  }
}
```
