# Trader Board

Access trader leaderboards.

---

## Trader Leaderboard

**GET** `https://api.copin.io/leaderboards-v2/page`

Retrieve trader leaderboard data for a specific protocol and time period.

### Headers

| Header | Type | Description |
|--------|------|-------------|
| Authorization | string | JWT token |
| x-api-key | string | API key |

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| protocol | string | Yes | Protocol identifier (e.g., "KWENTA", "GMX") |
| queryDate | number | Yes | Date in timestamp format (milliseconds) |
| statisticType | string | Yes | Period type: `MONTH` or `WEEK` |
| limit | number | No | Records to return (default: 20) |
| offset | number | No | Records to skip (default: 0) |
| sort_by | string | No | Sort field: `ranking`, `totalPnl`, `totalRealisedPnl`, `totalVolume`, `totalFee`, `totalTrade`, `totalWin`, `totalLose`, `totalLiquidation`, `totalLiquidationAmount` |
| sort_type | string | No | Sort direction: `asc`, `desc` |

### Request Example

```bash
curl -X GET "https://api.copin.io/leaderboards-v2/page?protocol=KWENTA&queryDate=1701835880676&statisticType=MONTH&limit=20&offset=0&sort_by=ranking&sort_type=asc" \
  -H 'x-api-key: YOUR_API_KEY'
```

### Response Fields (data array)

| Field | Type | Description |
|-------|------|-------------|
| id | string | Leaderboard entry ID |
| account | string | Trader address |
| protocol | string | Protocol name |
| statisticType | string | MONTH or WEEK |
| rankingAt | string | Ranking calculation timestamp |
| ranking | number | Current ranking position |
| lastRanking | number | Previous ranking position |
| totalPnl | number | Total PnL including fee |
| totalRealisedPnl | number | Total PnL without fee |
| totalVolume | number | Total trading volume |
| totalFee | number | Total fees paid |
| totalTrade | number | Total trades |
| totalWin | number | Winning trades |
| totalLose | number | Losing trades |
| totalLiquidation | number | Total liquidations |
| totalLiquidationAmount | number | Total liquidation amount |
| createdAt | string | Entry creation timestamp |

### Response Meta

| Field | Type | Description |
|-------|------|-------------|
| limit | number | Records returned |
| offset | number | Records skipped |
| total | number | Total records |
| totalPages | number | Total pages |

### Response Example

```json
{
  "data": [
    {
      "id": "656fb9897fdc207f19e4ef1f",
      "account": "0x92812499fF2c040f93121Aab684680a6e603C4A7",
      "protocol": "KWENTA",
      "statisticType": "MONTH",
      "ranking": 1,
      "lastRanking": 1,
      "totalPnl": 2124229.39,
      "totalRealisedPnl": 2137944.46,
      "totalVolume": 11012185.07,
      "totalFee": 13715.07,
      "totalTrade": 1,
      "totalWin": 1,
      "totalLose": 0,
      "totalLiquidation": 0,
      "totalLiquidationAmount": 0
    }
  ],
  "meta": { "limit": 20, "offset": 0, "total": 728, "totalPages": 37 }
}
```
