# RustChain Node API Documentation

This directory contains the official OpenAPI 3.0 specification and Swagger UI for the RustChain Node API.

## 📖 Quick Start

### View Online

The Swagger UI is available at: `https://rustchain.org/docs/api/swagger.html`

### Run Locally

1. **Using Python's built-in server:**

```bash
cd docs/api
python3 -m http.server 8080
```

Then open: http://localhost:8080/swagger.html

2. **Using Docker:**

```bash
docker run -p 8080:8080 -v $(pwd)/docs/api:/usr/share/nginx/html:ro nginx:alpine
```

Then open: http://localhost:8080/swagger.html

## 📋 Contents

- **`openapi.yaml`** - OpenAPI 3.0 specification (machine-readable)
- **`swagger.html`** - Interactive Swagger UI documentation
- **`README.md`** - This file

## 🔧 Validation

Validate the OpenAPI specification:

```bash
# Install swagger-cli
npm install -g @apidevtools/swagger-cli

# Validate
swagger-cli validate openapi.yaml
```

## 🧪 Testing Endpoints

The Swagger UI includes a "Try it out" feature. Here's how to test against a live node:

### Example: Get Current Epoch

```bash
curl -s https://rustchain.org/epoch | jq
```

**Response:**
```json
{
  "epoch": 88,
  "slot": 12700,
  "epoch_pot_rtc": 1000,
  "enrolled_miners": 20,
  "blocks_per_epoch": 240,
  "total_supply_rtc": 21000000
}
```

### Example: Get Active Miners

```bash
curl -s https://rustchain.org/api/miners | jq '.[:3]'
```

**Response:**
```json
[
  {
    "miner_id": "dual-g4-125",
    "arch": "G4",
    "enrolled_epoch": 88,
    "last_attest_timestamp": 1740783600,
    "balance_rtc": 125.50,
    "machine_year": 2004,
    "rust_score": 420.5
  }
]
```

### Example: Query Balance

```bash
curl -s "https://rustchain.org/balance?miner_id=dual-g4-125" | jq
```

**Response:**
```json
{
  "miner_id": "dual-g4-125",
  "balance_rtc": 125.50,
  "unconfirmed_rtc": 0,
  "pending_withdrawals_rtc": 0
}
```

## 🔐 Authentication

Most endpoints are public and require no authentication. Admin endpoints require an `X-Admin-Key` header:

```bash
curl -H "X-Admin-Key: your-admin-key" -X POST https://rustchain.org/wallet/transfer \
  -H "Content-Type: application/json" \
  -d '{"from_pubkey": "...", "to_pubkey": "...", "amount_rtc": 100.0}'
```

## 📚 API Categories

### Public Endpoints

| Category | Base Path | Description |
|----------|-----------|-------------|
| Health | `/health`, `/ready` | Node status checks |
| Epoch | `/epoch`, `/lottery/eligibility` | Epoch information |
| Miners | `/api/miners` | Miner data and enrollment |
| Balance | `/balance` | Wallet balance queries |
| Stats | `/api/stats` | Network statistics |
| Hall of Fame | `/api/hall_of_fame` | Leaderboards |
| Fees | `/api/fee_pool` | RIP-301 fee pool |
| Explorer | `/explorer` | Block explorer |

### Authenticated Endpoints

| Category | Base Path | Auth Required |
|----------|-----------|---------------|
| Attestation | `/attest/submit` | API Key |
| Withdrawals | `/withdraw/request` | Signature |
| Wallet Transfer | `/wallet/transfer/signed` | Signature |
| Admin Transfer | `/wallet/transfer` | Admin Key |

## 🚀 Rate Limiting

Public endpoints are rate-limited to **100 requests per minute** per IP address.

**HTTP 429** is returned when rate limit is exceeded.

## 🌐 Base URLs

| Environment | Base URL |
|-------------|----------|
| Production | `https://rustchain.org` |
| Local Development | `http://localhost:5000` |

## 📝 Contributing

Found an issue with the API documentation? Please:

1. Open an issue on [GitHub](https://github.com/Scottcjn/Rustchain/issues)
2. Or submit a PR with your improvements

## 📄 License

MIT License - See [LICENSE](../../LICENSE) for details.

## 🔗 Related Links

- [RustChain GitHub](https://github.com/Scottcjn/Rustchain)
- [RustChain Documentation](../../README.md)
- [RIP-0008: Withdrawals](../../rips/RIP-0008.md)
- [RIP-0009: Finality](../../rips/RIP-0009.md)
- [RIP-301: Fee Pool](../../rips/RIP-0301.md)

---

**Generated:** 2026-03-04
**API Version:** 2.2.1-rip200
