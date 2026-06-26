# Mamlaka Hub and Spoke Payment API

**Version:** `3.01`  
**Base URL:** `https://payments.mam-laka.com`  
**Author:** Collins  
**Email:** `collins@mam-laka.com`

---

## Overview

The Mamlaka Payment API lets merchants initiate and track:

- Mobile money collections: customer pays merchant
- Mobile money payouts: merchant sends funds to customer
- Till and paybill payouts
- Airtime disbursement
- Card, bank, and provider-based payments
- Wallet balance checks
- Transaction status lookup

All merchant-facing transaction statuses are standardized:

```text
PENDING
COMPLETE
FAILED
```

For reporting, `transactionReport` describes the transaction type:

```text
collection
withdraw
airtime
```

---

## Authentication

Most endpoints require a merchant JWT.

### Generate Token

```http
GET /api/v1
Authorization: Basic <base64 username:password>
```

Example:

```bash
curl --location 'https://payments.mam-laka.com/api/v1' \
  --header 'Authorization: Basic <base64 username:password>'
```

Use the returned token as:

```http
Authorization: Bearer <token>
```

---

## Standard Response Fields

Most initiation endpoints return:

```json
{
  "message": "Payment initiation successful",
  "externalId": "merchant-reference",
  "secureId": "system-secure-id"
}
```

Some legacy endpoints may return `transactionId`; this is usually the merchant `externalId`.

---

## Mobile Money Collection

Use this endpoint to collect money from a customer.

### Endpoint

```http
POST /api/v1/mobile/initiate
Authorization: Bearer <token>
Content-Type: application/json
```

### Kenya M-Pesa Request

```json
{
  "impalaMerchantId": "your-merchant-id",
  "displayName": "AVIATOR",
  "currency": "KES",
  "amount": 10,
  "payerPhone": "254768899729",
  "mobileMoneySP": "M-Pesa",
  "externalId": "PAYIN-001",
  "callbackUrl": "https://merchant.example.com/callback"
}
```

### Success Response

```json
{
  "message": "Payment initiation successful",
  "externalId": "PAYIN-001",
  "secureId": "qdml8553ZeInavKorBHzLA=="
}
```

### Merchant Callback: Complete

```json
{
  "amount": 10,
  "currency": "KES",
  "externalId": "PAYIN-001",
  "netAmount": 10,
  "secureId": "qdml8553ZeInavKorBHzLA==",
  "transactionReport": "COMPLETE",
  "transactionStatus": "COMPLETE"
}
```

### Merchant Callback: Failed

```json
{
  "amount": 10,
  "currency": "KES",
  "externalId": "PAYIN-001",
  "netAmount": 10,
  "secureId": "qdml8553ZeInavKorBHzLA==",
  "transactionReport": "FAILED",
  "transactionStatus": "FAILED"
}
```

---

## Mobile Money Payout

Use this endpoint to send money from the merchant payout wallet to a customer.

### Endpoint

```http
POST /api/v1/mobile/transfer
Authorization: Bearer <token>
Content-Type: application/json
```

### Kenya M-Pesa Request

```json
{
  "impalaMerchantId": "your-merchant-id",
  "currency": "KES",
  "amount": 10,
  "recipientPhone": "254768899729",
  "mobileMoneySP": "M-Pesa",
  "externalId": "PAYOUT-001",
  "callbackUrl": "https://merchant.example.com/callback"
}
```

### Success Response

```json
{
  "message": "Payment initiation successful",
  "externalId": "PAYOUT-001",
  "secureId": "qdml8553ZeInavKorBHzLA=="
}
```

### Merchant Callback: Complete

```json
{
  "amount": 10,
  "currency": "KES",
  "externalId": "PAYOUT-001",
  "secureId": "qdml8553ZeInavKorBHzLA==",
  "transactionReceipt": "TBB8KE0GQW",
  "transactionReport": "COMPLETE",
  "transactionStatus": "COMPLETE"
}
```

### Merchant Callback: Failed

```json
{
  "amount": 10,
  "currency": "KES",
  "externalId": "PAYOUT-001",
  "secureId": "qdml8553ZeInavKorBHzLA==",
  "transactionReport": "FAILED",
  "transactionStatus": "FAILED"
}
```

---

## Airtime Disbursement

Use this endpoint to send airtime. Airtime uses the merchant payout wallet field `artmBalance`.

### Important Rules

- Supported provider: `AIRTEL`
- Internal currency: `ARTM`
- Transaction report: `airtime`
- Transaction status: `PENDING`, `COMPLETE`, or `FAILED`
- The system checks `artmBalance` before sending airtime.
- On provider failure, the reserved `ARTM` balance is refunded.

### Endpoint

```http
POST /api/v1/mobile/airtime
Authorization: Bearer <token>
Content-Type: application/json
```

### Request

```json
{
  "impalaMerchantId": "your-merchant-id",
  "amount": 10,
  "phone": "254768899729",
  "mobileMoneySP": "AIRTEL",
  "externalId": "AIRTIME-001"
}
```

Optional field:

```json
{
  "email": "customer@example.com"
}
```

Phone numbers can be sent as:

```text
254768899729
0768899729
768899729
```

### Success Response

```json
{
  "status": "success",
  "message": "Airtime disbursed successful",
  "secureId": "qdml8553ZeInavKorBHzLA==",
  "transactionId": "AIRTIME-001"
}
```

### Failed Response

```json
{
  "status": "failed",
  "message": "Airtime disbursion failed",
  "secureId": "qdml8553ZeInavKorBHzLA==",
  "transactionId": "AIRTIME-001"
}
```

### Insufficient ARTM Balance

```json
{
  "error": "INSUFFICIENT_ARTM_BALANCE",
  "message": "Insufficient ARTM balance. Available: 0.00 ARTM, Required: 10.00 ARTM"
}
```

### Stored Transaction

```text
transactionStatus = COMPLETE | PENDING | FAILED
transactionReport = airtime
currency = ARTM
sourceOfFunds = AIRTIME
```

---

## Till Payment

Use this endpoint to pay a till from the merchant payout wallet.

### Endpoint

```http
POST /api/v1/till/payment
Authorization: Bearer <token>
Content-Type: application/json
```

### Request

```json
{
  "amount": "10",
  "currency": "KES",
  "creditAccount": "123456",
  "externalId": "TILL-001",
  "callbackUrl": "https://merchant.example.com/callback",
  "narration": "Till payment withdrawal"
}
```

### Response

```json
{
  "message": "Till payment initiated successfully. Await callback for final status.",
  "secureId": "ABC123XYZ789",
  "externalId": "TILL-001",
  "status": "PENDING",
  "transactionReport": "withdraw",
  "channel": "TILL"
}
```

### Merchant Callback

```json
{
  "transactionStatus": "COMPLETE",
  "transactionReport": "withdraw",
  "channel": "TILL",
  "currency": "KES",
  "amount": 10,
  "secureId": "ABC123XYZ789",
  "externalId": "TILL-001"
}
```

---

## Paybill Payment

Use this endpoint to pay a paybill from the merchant payout wallet.

### Endpoint

```http
POST /api/v1/paybill/payment
Authorization: Bearer <token>
Content-Type: application/json
```

### Request

```json
{
  "amount": "10",
  "currency": "KES",
  "creditAccount": "123456",
  "externalId": "PAYBILL-001",
  "callbackUrl": "https://merchant.example.com/callback",
  "narration": "Paybill payment withdrawal"
}
```

### Response

```json
{
  "message": "Paybill payment initiated successfully. Await callback for final status.",
  "secureId": "ABC123XYZ789",
  "externalId": "PAYBILL-001",
  "status": "PENDING",
  "transactionReport": "withdraw",
  "channel": "PAYBILL"
}
```

---

## Test Transactions

The API supports test transactions for simulation.

### Mobile Number Simulators

Use these phone numbers on mobile collection or mobile payout requests.

| Test Number | Result |
|---|---|
| `0710000000` | Success |
| `0720000000` | Failed |

Accepted formats:

```text
0710000000
254710000000
+254710000000
```

### Till and Paybill Simulators

Use these values as `creditAccount` for till/paybill requests.

| Test Identifier | Result |
|---|---|
| `888888` | Success |
| `999999` | Failed |

### Test Response Shape

The initiation response keeps the normal shape:

```json
{
  "externalId": "TEST-001",
  "message": "Payment initiation successful",
  "secureId": "TEST-qdml8553ZeInavKorBHzLA=="
}
```

Test callbacks include:

```json
{
  "transactionStatus": "COMPLETE",
  "transactionReport": "test transaction",
  "reason": "test transaction"
}
```

---

## Balance APIs

### Payout Wallet Balance

```http
GET /api/v1/read/payouts/balance
Authorization: Bearer <token>
```

The payout wallet includes:

```text
kesBalance
usdBalance
ugxBalance
ngnBalance
gmdBalance
rwfBalance
artmBalance
```

`artmBalance` is used for airtime disbursement.

### Payin Wallet Balance

```http
GET /api/v1/read/payins/balance
Authorization: Bearer <token>
```

---

## Transaction Search

```http
GET /api/v1/transaction
Authorization: Bearer <token>
```

Common query parameters:

```text
merchantId
phone
amount
currency
transactionStatus
transactionReport
sourceOfFunds
reference
page
limit
```

Example:

```bash
curl --location 'https://payments.mam-laka.com/api/v1/transaction?transactionStatus=COMPLETE&transactionReport=airtime' \
  --header 'Authorization: Bearer <token>'
```

---

## Stale Pending Transactions

This endpoint marks pending transactions older than 3 hours as failed and sends failed callbacks where applicable.

```http
POST /api/v1/transactions/fail-stale-pending?limit=20
```

Maximum `limit` is `100`.

---

## Limits

Limits vary by merchant profile, wallet setup, country, provider, and channel. If a transaction exceeds an enabled limit, the API returns `AMOUNT_LIMIT_EXCEEDED`.

Provider-side limits may also apply.

---

## Status and Reporting Standard

Use `transactionStatus` for lifecycle:

```text
PENDING
COMPLETE
FAILED
```

Use `transactionReport` for transaction category:

```text
collection
withdraw
airtime
test transaction
```

Examples:

| Use Case | transactionStatus | transactionReport |
|---|---|---|
| Pending STK collection | `PENDING` | `collection` |
| Complete M-Pesa payout | `COMPLETE` | `withdraw` |
| Failed till payout | `FAILED` | `withdraw` |
| Complete airtime | `COMPLETE` | `airtime` |

---

## Common Errors

### Duplicate External ID

```json
{
  "error": "DUPLICATE_EXTERNAL_ID",
  "message": "A transaction with this externalId already exists for this merchant"
}
```

### Amount Limit Exceeded

```json
{
  "error": "AMOUNT_LIMIT_EXCEEDED",
  "message": "KES withdrawal amount cannot exceed KES 10",
  "amount": 20,
  "limit": 10
}
```

### Unsupported Airtime Provider

```json
{
  "error": "UNSUPPORTED_AIRTIME_PROVIDER",
  "message": "Only AIRTEL airtime is supported"
}
```

---

## JSON Reminder

Do not add trailing commas in JSON request bodies.

Invalid:

```json
{
  "externalId": "AIRTIME-001",
}
```

Valid:

```json
{
  "externalId": "AIRTIME-001"
}
```
