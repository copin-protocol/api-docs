# Authentication

Web3 authentication endpoints for the Copin API.

---

## Login

**POST** `https://api.copin.io/auth/web3/login`

Obtain a verification code for Web3 authentication.

### Request Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| address | string | Yes | Ethereum wallet address of the user |

### Request Example

```bash
curl --request POST \
--url "https://api.copin.io/auth/web3/login" \
--header 'Content-Type: application/json' \
--data '{"address":"0x1535484c1eec1d203fe1a53ea11a621a884ae067"}'
```

### Response

| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique identifier for the user |
| username | string | User's wallet address used as username |
| role | string | User's role (e.g., "guest", "user", "admin") |
| isBlocked | boolean | Whether user account is blocked |
| isActivated | boolean | Whether user account is activated |
| blockNote | string | Block reason note |
| referralCode | string | User's referral code |
| isSkippedReferral | boolean | Whether user skipped entering referral code |
| copyTradeQuota | number | Number of copy trades the user can create |
| plan | number | User's subscription plan level |
| createdAt | string | Account creation timestamp |
| access_token | string | JWT token for authenticated API calls |
| isAddedReferral | boolean | Whether user has added a referral code |

### Response Example

```json
{
    "id": "654849d04857e1f744646cdd",
    "username": "0x1535484c1eec1d203fe1a53ea11a621a884ae067",
    "role": "guest",
    "isBlocked": false,
    "isActivated": true,
    "blockNote": "",
    "referralCode": "IEZ8LC",
    "isSkippedReferral": true,
    "copyTradeQuota": 3,
    "plan": 0,
    "createdAt": "2023-11-06T02:05:04.079Z",
    "access_token": "eyJhbGciOiJIUzI1NiIs...",
    "isAddedReferral": false
}
```

---

## Verify Login

**POST** `https://api.copin.io/auth/web3/verify-login`

Verify Web3 authentication and complete the login process.

### Request Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| address | string | Yes | Ethereum wallet address |
| sign | string | Yes | Signature created by signing the verification code. Message format: `I want to login on Copin.io at ${time}. Login code: ${verifyCode}` |
| time | string | Yes | Date-time of the verification request |
| referralCode | string | No | Optional referral code |
| brokerId | string | No | Optional broker ID |

### Request Example

```bash
curl --request POST \
--url "https://api.copin.io/auth/web3/verify-login" \
--header 'Content-Type: application/json' \
--data '{
 "address": "0x1535484c1eec1d203fe1a53ea11a621a884ae067",
 "sign": "0x5d99b6f7f6d1f73d1a26497f2b1c89b24c...",
 "time": "2025-04-14T09:55:27.767Z"
}'
```

### Response

Same structure as Login response — returns user object with `access_token` JWT.

---

## Logout

**POST** `https://api.copin.io/auth/logout`

Logout and invalidate the JWT token.

### Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| Authorization | string | Yes | JWT token |

### Request Example

```bash
curl --request POST \
--url "https://api.copin.io/auth/logout" \
--header 'Authorization: YOUR_JWT_TOKEN'
```

### Response

Status: 201 Created

---

## Get Current User

**GET** `https://api.copin.io/me`

Get information about the authenticated user.

### Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| Authorization | string | Yes | JWT token |

### Request Example

```bash
curl --request GET \
--url "https://api.copin.io/me" \
--header 'Authorization: YOUR_JWT_TOKEN'
```

### Response

| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique identifier for the user |
| username | string | User's wallet address |
| role | string | User's role |
| isBlocked | boolean | Whether account is blocked |
| isActivated | boolean | Whether account is activated |
| referralCode | string | User's referral code |
| copyTradeQuota | number | Copy trade quota |
| plan | number | Subscription plan level |
| referralTier | string | User's referral tier |
| totalFeeLast30Days | number | Total fees in last 30 days |
| createdAt | string | Account creation timestamp |

### Response Example

```json
{
    "id": "654849d04857e1f744646cdd",
    "username": "0x1535484c1eec1d203fe1a53ea11a621a884ae067",
    "role": "guest",
    "isBlocked": false,
    "isActivated": true,
    "blockNote": "",
    "referralCode": "IEZ8LC",
    "isSkippedReferral": true,
    "copyTradeQuota": 3,
    "plan": 0,
    "referralTier": "TIER_1",
    "totalFeeLast30Days": 0,
    "createdAt": "2023-11-06T02:05:04.079Z"
}
```
