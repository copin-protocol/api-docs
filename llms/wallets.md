# Wallets

Manage embedded wallets, deposits, and withdrawals. All endpoints require JWT authentication.

---

## Withdraw

**POST** `https://api.copin.io/embedded-wallets/hyperliquid/withdraw`

Withdraw funds from an embedded Hyperliquid wallet.

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| Authorization | Yes | JWT token |

### Request Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| sourceAddress | string | Yes | Embedded wallet address (source) |
| destinationAddress | string | Yes | Destination wallet address |
| amount | number | Yes | Amount to withdraw |

### Request Example

```bash
curl --request POST \
--url "https://api.copin.io/embedded-wallets/hyperliquid/withdraw" \
--header 'Content-Type: application/json' \
--header 'Authorization: YOUR_JWT_TOKEN' \
--data '{
  "sourceAddress":"0x06791c29FaEDb81A87A2Ef85ce2A986fE6dDF96e",
  "destinationAddress": "0x1535484c1Eec1D203fE1A53EA11a621a884AE067",
  "amount": 10
}'
```

### Response

| Field | Type | Description |
|-------|------|-------------|
| userId | string | User who initiated withdrawal |
| embeddedWallet | string | Embedded wallet address |
| data.destinationAddress | string | Destination address |
| data.chargeFeeTime | string | Fee charge timestamp |
| data.chargeFeeTxHash | string | Fee transaction hash |
| data.withdrawTime | string | Withdrawal timestamp |
| data.txHash | string | Withdrawal transaction hash |
| data.amount | number | Total amount requested |
| data.feeAmount | number | Total fee |
| data.hyperliquidWithdrawFeeAmount | number | HyperLiquid withdraw fee |
| data.hyperliquidTransferFeeAmount | number | HyperLiquid transfer fee |
| data.withdrawAmount | number | Amount being withdrawn |
| data.receiveAmount | number | Amount received after fees |
| type | string | "WITHDRAW" |
| status | string | "SUCCESSFUL", "PENDING", "FAILED" |
| createdAt | string | Request creation timestamp |

---

## Deposit and Withdraw History

**GET** `https://api.copin.io/embedded-wallets/hyperliquid/transactions/page`

Retrieve deposit and withdrawal transaction history.

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| Authorization | Yes | JWT token |

### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| limit | number | Records per page |
| offset | number | Records to skip |

### Request Example

```bash
curl 'https://api.copin.io/embedded-wallets/hyperliquid/transactions/page?limit=20&offset=0' \
  -H 'Authorization: YOUR_JWT_TOKEN'
```

### Response Fields (data array)

| Field | Type | Description |
|-------|------|-------------|
| embeddedWallet | string | Embedded wallet address |
| estimatedFinishTime | string | Estimated completion time |
| type | string | "DEPOSIT" or "WITHDRAW" |
| status | string | "PENDING", "SUCCESSFUL", "FAILED" |
| createdAt | string | Transaction creation timestamp |
| data | object | Transaction details (varies by type) |

**Withdraw data fields:**

| Field | Type | Description |
|-------|------|-------------|
| data.destinationAddress | string | Destination address |
| data.amount | number | Withdrawal amount |
| data.feeAmount | number | Total fee |
| data.withdrawAmount | number | Amount withdrawn |
| data.receiveAmount | number | Amount received |
| data.txHash | string | Transaction hash |

**Deposit data fields:**

| Field | Type | Description |
|-------|------|-------------|
| data.requestTxHash | string | Deposit request transaction hash |
| data.requestTime | string | Request timestamp |
| data.amount | number | Deposit amount |
| data.relayerAddress | string | Relayer address |
| data.permit.signature | string | Permit signature |
| data.permit.deadline | number | Permit deadline timestamp |
