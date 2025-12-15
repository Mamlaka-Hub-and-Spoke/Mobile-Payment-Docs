# Mamlaka Hub and Spoke Payment API (v3.0.0)

**Author:** Collins  
**Email:** collins@mam-laka.com  
**Base URL:** `https://payments.mam-laka.com`

---

## Table of Contents

1. [Authentication](#authentication)
2. [Mobile APIs](#mobile-apis)
   - [STK Push (C2B)](#stk-push-c2b)
   - [Money Transfer (B2C)](#money-transfer-b2c)
3. [Card Transaction](#card-transaction)
4. [Balance APIs](#balance-apis)
   - [Payins Balance](#payins-balance)
   - [Payouts Balance](#payouts-balance)
5. [Transaction Status](#transaction-status)

---

## Authentication

### Generate Token

To perform API operations, you need to first generate a token using the username and password issued during the setup process. The token will be used in the Authorization header for subsequent operations.

#### Endpoint
- **URL:** `{{baseUrl}}/api/v1`
- **Method:** `GET`
- **Headers:** 
  - `Authorization: Basic Y29sbHM6Y29tZXRhcHBtYWlu`

#### Example cURL Request
```bash
curl --location 'https://payments.mam-laka.com/api/v1' \
--header 'Authorization: Basic Y29sbHM6Y29tZXRhcHBtYWlu'
```

![Authentication Flow](https://github.com/user-attachments/assets/5c9f6e7a-2e12-4f04-af3a-5feaf971ac94)

---

## Mobile APIs

### STK Push (C2B)

Initiate an STK push to request payment from a customer's mobile money account.

#### Endpoint
- **URL:** `{{BASE_URL}}/api/v1/mobile/initiate`
- **Method:** `POST`
- **Headers:** `Authorization: Bearer <token>`

#### Request Body
```json
{
  "impalaMerchantId": "{{username}}",
  "displayName": "AVIATOR",
  "currency": "KES",
  "amount": 10,
  "payerPhone": "254768899729",
  "mobileMoneySP": "M-Pesa",
  "externalId": "ImpadlTdest2",
  "callbackUrl": "https://97e6-217-21-116-242.ngrok-free.app/"
}
```

#### Sample Response
```json
{
  "message": "Payment initiation successful",
  "secureId": "qdml8553ZeInavKorBHzLA==",
  "transactionId": "ImpadlTdest2"
}
```

#### Callback Success Response
```json
{
  "amount": 10,
  "currency": "KES",
  "externalId": "ImpadlTdest25",
  "netAmount": 10,
  "secureId": "i1FHTQgFpHP0g-MRacZXUQ==",
  "transactionReport": "COMPLETE",
  "transactionStatus": "COMPLETE"
}
```

#### Callback Failed Response
```json
{
  "amount": 10,
  "currency": "KES",
  "externalId": "ImpadlTdest25",
  "netAmount": 10,
  "secureId": "6S4mi15UgO2xYkjjyqH4gA==",
  "transactionReport": "FAILED",
  "transactionStatus": "FAILED"
}
```

![STK Push Flow](https://github.com/user-attachments/assets/fef0fb1e-bdec-4ded-9cbc-3e7d83f83a80)

---

### Money Transfer (B2C)

Transfer money from your merchant account to a customer's mobile money account.

#### Endpoint
- **URL:** `{{BASE_URL}}/api/v1/mobile/transfer`
- **Method:** `POST`
- **Headers:** `Authorization: Bearer <token>`

#### Request Body
```json
{
  "impalaMerchantId": "{{username}}",
  "currency": "KES",
  "amount": 10,
  "recipientPhone": "254768899729",
  "mobileMoneySP": "M-Pesa",
  "externalId": "joeltest404",
  "callbackUrl": "https://97e6-217-21-116-242.ngrok-free.app/"
}
```

#### Sample Response
```json
{
  "message": "Payment initiation successful",
  "secureId": "qdml8553ZeInavKorBHzLA==",
  "transactionId": "a392-45d1-93c1-58f9447915e717138558"
}
```

#### Callback Success Response
```json
{
  "amount": 10,
  "currency": "KES",
  "externalId": "joeltest404",
  "netAmount": 10,
  "receiverPartyPublicName": "07688XXXX - Collins Williams",
  "secureId": "4p-LIGOa5iO-1xjDR_bZZg==",
  "transactionCompletedDateTime": "11.02.2025 01:44:17",
  "transactionReceipt": "TBB8KE0GQW",
  "transactionReport": "COMPLETE",
  "transactionStatus": "COMPLETE"
}
```

#### Callback Failed Response
```json
{
  "amount": 10,
  "currency": "KES",
  "externalId": "ImpadlTdest2",
  "netAmount": 10,
  "secureId": "ldFuXRQn_JmCVoili4ItEw==",
  "transactionReport": "FAILED",
  "transactionStatus": "FAILED"
}
```

---

## Card Transaction

Process card payments through a secure payment gateway.

#### Endpoint
- **URL:** `https://payments.mam-laka.com/api/v1/card/initiate`
- **Method:** `POST`
- **Headers:** `Authorization: Bearer <token>`

#### Request Body
```json
{
  "impalaMerchantId": "{{username}}",
  "currency": "USD",
  "amount": 1,
  "mobileMoneySP": "card",
  "externalId": "ImpadlTdest2",
  "redirectUrl": "backtoyoursite.com",
  "callbackUrl": "https://8bd0-2c0f-fe38-2413-2c98-7114-6990-db59-11c.ngrok-free.app/mc/log.php"
}
```

#### Sample Response
```json
{
  "cardLink": "http://commetagri.mam-laka.com/uba.php?data=YW1vdW50PTEuMDAmbWVyY2hhbnQ9YXBwJmNhbGxiYWNrPWh0dHBzOi8vOGJkMC0yYzBmLWZlMzgtMjQxMy0yYzk4LTcxMTQtNjk5MC1kYjU5LTExYy5uZ3Jvay1mcmVlLmFwcC9tYy9sb2cucGhwJnJlZGlyZWN0PU9XWEpKRFVNZ1B2aDlYUWhTQnk5eGc9PSZleHRlcm5hbGlkPUltcGFkbFRkZXN0Mg==",
  "message": "card Payment initiation successful",
  "secureId": "OWXJJDUMgPvh9XQhSBy9xg=="
}
```

#### Callback Success Response
```json
{
  "transactionStatus": "COMPLETED",
  "transactionReport": "SUCCESS",
  "currency": "KES",
  "amount": "5.00",
  "netAmount": "5.00",
  "secureId": "8e6bfa77-4f2a-4ff6-af48-ef774f2178ca",
  "externalId": "BoomW",
  "redirectUrl": "bRbHRh6Tmi1mBNUzKKTtzA=="
}
```

#### Callback Failed Response
```json
{
  "transactionStatus": "FAILED",
  "transactionReport": "N/A",
  "currency": "KES",
  "amount": "5.00",
  "netAmount": "5.00",
  "secureId": "8e6bfa77-4f2a-4ff6-af48-ef774f2178ca",
  "externalId": "BoomW",
  "redirectUrl": "bRbHRh6Tmi1mBNUzKKTtzA=="
}
```

---

## Balance APIs

### Payins Balance

Retrieve your current payins balance across all supported currencies.

#### Endpoint
- **URL:** `https://payments.mam-laka.com/api/v1/read/payins/balance`
- **Method:** `GET`
- **Headers:** `Authorization: Bearer <token>`

#### Example cURL Request
```bash
curl --location 'https://payments.mam-laka.com/api/v1/read/payins/balance' \
--header 'Authorization: Bearer <your_token_here>'
```

#### Sample Response
```json
{
  "Balances": {
    "baseCurrency": "KES",
    "eurBalance": 0,
    "gbpBalance": 0,
    "impaBalance": 0,
    "kesBalance": 6020.95,
    "lumenBalance": 0,
    "merchantId": "app",
    "totalBalance": 27476.126842105266,
    "tzsBalance": 0,
    "ugxBalance": 0,
    "usdBalance": 0,
    "usdcBalance": 0,
    "usdtBalance": 0,
    "xafBalance": 99064
  },
  "baseCurrency": "KES",
  "merchantId": "app"
}
```

---

### Payouts Balance

Retrieve your current payouts balance across all supported currencies.

#### Endpoint
- **URL:** `https://payments.mam-laka.com/api/v1/read/payouts/balance`
- **Method:** `GET`
- **Headers:** `Authorization: Bearer <token>`

#### Example cURL Request
```bash
curl --location 'https://payments.mam-laka.com/api/v1/read/payouts/balance' \
--header 'Authorization: Bearer <your_token_here>'
```

#### Sample Response
```json
{
  "Balances": {
    "baseCurrency": "KES",
    "eurBalance": 1.24,
    "gbpBalance": 1.28,
    "impaBalance": 299999740,
    "kesBalance": 6180,
    "lumenBalance": 0,
    "merchantId": "app",
    "totalBalance": 740700119774.2283,
    "tzsBalance": 0,
    "ugxBalance": 200,
    "usdBalance": 6043.45,
    "usdcBalance": 68.993935,
    "usdtBalance": 2.43,
    "xafBalance": 400
  },
  "baseCurrency": "KES",
  "merchantId": "app"
}
```

---

## Transaction Status

Check the status of a transaction using the secure ID returned during transaction initialization.

#### Endpoint
- **URL:** `https://payments.mam-laka.com/api/v1/transaction?merchant=<your_merchant_id>&secureId=<secure_id>`
- **Method:** `GET`
- **Headers:** `Authorization: Bearer <token>`

**Note:**
- Replace `<your_merchant_id>` with your actual merchant ID
- Replace `<secure_id>` with the `secure_id` returned during transaction initialization

#### Example Request
```
https://payments.mam-laka.com/api/v1/transaction?merchant=impala&secureId=d4XeBKNruNJnvhOMOd8RLg==
```

#### Sample Response
```json
{
  "transaction": {
    "amount": 1,
    "callback_url": "https://f0d6-197-232-22-252.ngrok-free.app",
    "currency": "USD",
    "date_added": 1741675246,
    "external_id": "ImpadlTdest2",
    "impalaMerchantId": "impala",
    "secure_id": "d4XeBKNruNJnvhOMOd8RLg==",
    "source_of_funds": "card",
    "transaction_report": "collection",
    "transaction_status": "PENDING"
  }
}
```

**Transaction Status Values:**
- `PENDING` - Transaction is being processed
- `COMPLETED` - Transaction was successful
- `FAILED` - Transaction failed

---

## Error Codes

| Code | Description |
|------|-------------|
| 400  | Bad Request - Invalid parameters |
| 401  | Unauthorized - Invalid or missing token |
| 403  | Forbidden - Insufficient permissions |
| 404  | Not Found - Resource not found |
| 500  | Internal Server Error |

---

## Rate Limits

- **Mobile APIs:** 
- **Card Transactions:** 
- **Balance APIs:** 
- **Transaction Status:** 

---

## Support

For technical support or questions about this API, please contact:
- **Email:** collins@mam-laka.com
- **Documentation Version:** 3.0.0
- **Last Updated:** February 2025


# Transaction Search API Documentation

## Overview

The Transaction Search API allows you to search and filter transactions based on multiple criteria including merchant ID, phone number, amount, currency, status, and more. This endpoint supports pagination and returns detailed transaction information.

## Endpoint

```
GET /api/v1/transactions/search
```

## Authentication

This endpoint requires authentication. Include a Bearer token in the Authorization header:

```
Authorization: Bearer <your_token>
```

## Query Parameters

All parameters are optional. You can combine multiple filters to narrow down your search results.

| Parameter | Type | Description | Example |
|-----------|------|-------------|---------|
| `merchantId` | string | Filter by merchant ID. If authenticated, this is automatically set from your token. | `merchant123` |
| `phone` | string | Search by phone number (partial match supported). Searches in the `msisdn` field. | `254712345678` |
| `amount` | integer | Filter by exact transaction amount. | `1000` |
| `currency` | string | Filter by currency code (KES, USD, XOF, UGX, etc.). | `KES` |
| `status` | string | Filter by transaction status (PENDING, SUCCESS, FAILED, COMPLETE, etc.). | `SUCCESS` |
| `report` | string | Filter by transaction report type (collection, withdraw, deposit). | `collection` |
| `externalId` | string | Search by exact external ID. | `ImpadlTdest25` |
| `secureId` | string | Search by exact secure ID. | `6S4mi15UgO2xYkjjyqH4gA==` |
| `sourceOfFunds` | string | Filter by source of funds (MPESA, CARD, CRYPTO, korapay, flutterwave, etc.). | `MPESA` |
| `startDate` | integer | Filter transactions from this Unix timestamp (inclusive). | `1704067200` |
| `endDate` | integer | Filter transactions up to this Unix timestamp (inclusive). | `1704153600` |
| `page` | integer | Page number for pagination (default: 1). | `1` |
| `page_size` | integer | Number of results per page (default: 20, max: 100). | `20` |

## Request Examples

### Example 1: Search by Merchant and Status

```bash
curl -X GET "https://api.example.com/api/v1/transactions/search?merchantId=merchant123&status=SUCCESS&page=1&page_size=20" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Example 2: Search by Phone Number

```bash
curl -X GET "https://api.example.com/api/v1/transactions/search?phone=254712345678" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Example 3: Search by Amount and Currency

```bash
curl -X GET "https://api.example.com/api/v1/transactions/search?amount=1000&currency=KES" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Example 4: Search by Date Range

```bash
curl -X GET "https://api.example.com/api/v1/transactions/search?startDate=1704067200&endDate=1704153600&status=SUCCESS" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Example 5: Complex Search with Multiple Filters

```bash
curl -X GET "https://api.example.com/api/v1/transactions/search?merchantId=merchant123&phone=254712345678&amount=1000&currency=KES&status=SUCCESS&report=collection&sourceOfFunds=MPESA&page=1&page_size=50" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## Response Format

### Success Response (200 OK)

```json
{
  "transactions": [
    {
      "id": 1234,
      "impalaMerchantId": "merchant123",
      "transactionStatus": "SUCCESS",
      "transactionReport": "collection",
      "currency": "KES",
      "amount": 1000,
      "msisdn": "254712345678",
      "netAmount": 1000.0,
      "secureId": "6S4mi15UgO2xYkjjyqH4gA==",
      "sourceOfFunds": "MPESA",
      "externalId": "ImpadlTdest25",
      "callbackUrl": "https://merchant.com/callback",
      "dateAdded": 1704067200,
      "merchantRequestID": "AG_20240101_123456",
      "checkoutRequestID": "ws_CO_01012024123456",
      "responseCode": "0",
      "responseDescription": "Success",
      "callbackStatus": "SENT"
    }
  ],
  "pagination": {
    "current_page": 1,
    "per_page": 20,
    "total_pages": 5,
    "total_items": 100
  }
}
```

### Error Responses

#### 401 Unauthorized

```json
{
  "error": "Missing authentication details"
}
```

#### 400 Bad Request

```json
{
  "error": "Invalid request format",
  "details": "Invalid page number"
}
```

#### 500 Internal Server Error

```json
{
  "error": "Failed to search transactions",
  "details": "Database connection error"
}
```

## Response Fields

### Transaction Object

| Field | Type | Description |
|-------|------|-------------|
| `id` | integer | Unique transaction ID |
| `impalaMerchantId` | string | Merchant identifier |
| `transactionStatus` | string | Current transaction status (PENDING, SUCCESS, FAILED, COMPLETE, etc.) |
| `transactionReport` | string | Transaction type (collection, withdraw, deposit) |
| `currency` | string | Currency code (KES, USD, XOF, UGX, etc.) |
| `amount` | integer | Transaction amount |
| `msisdn` | string | Phone number associated with the transaction |
| `netAmount` | float | Net amount after fees |
| `secureId` | string | Secure transaction identifier |
| `sourceOfFunds` | string | Payment source (MPESA, CARD, CRYPTO, etc.) |
| `externalId` | string | External transaction identifier |
| `callbackUrl` | string | Merchant callback URL |
| `dateAdded` | integer | Unix timestamp when transaction was created |
| `merchantRequestID` | string | Merchant request identifier |
| `checkoutRequestID` | string | Checkout request identifier |
| `responseCode` | string | Response code from payment provider |
| `responseDescription` | string | Response description |
| `callbackStatus` | string | Callback delivery status |

### Pagination Object

| Field | Type | Description |
|-------|------|-------------|
| `current_page` | integer | Current page number |
| `per_page` | integer | Number of items per page |
| `total_pages` | integer | Total number of pages |
| `total_items` | integer | Total number of matching transactions |

## Transaction Status Values

| Status | Description |
|--------|------------|
| `PENDING` | Transaction is pending processing |
| `SUCCESS` | Transaction completed successfully |
| `FAILED` | Transaction failed |
| `COMPLETE` | Transaction completed (synonym for SUCCESS) |
| `processing` | Transaction is being processed |

## Transaction Report Types

| Type | Description |
|------|-------------|
| `collection` | Collection/payment received |
| `withdraw` | Withdrawal/payout |
| `deposit` | Deposit transaction |

## Source of Funds Values

Common values include:
- `MPESA` - M-Pesa mobile money
- `CARD` - Card payment
- `CRYPTO` - Cryptocurrency payment
- `ECITIZEN` - eCitizen payment

## Notes

1. **Phone Number Search**: The phone parameter uses partial matching (LIKE query), so you can search with partial phone numbers.

2. **Date Range**: Both `startDate` and `endDate` should be Unix timestamps (seconds since epoch).

3. **Pagination**: 
   - Default page size is 20
   - Maximum page size is 100
   - Results are ordered by `dateAdded` in descending order (newest first)

4. **Merchant ID**: If you're authenticated, the `merchantId` parameter is automatically set from your token. You can still override it by providing it explicitly in the query string.

5. **Combining Filters**: All filters are combined with AND logic. All conditions must be met for a transaction to appear in results.

6. **Empty Results**: If no transactions match your criteria, you'll receive an empty array with pagination metadata showing `total_items: 0`.

## Rate Limiting

This endpoint may be subject to rate limiting. Check response headers for rate limit information:
- `X-RateLimit-Limit`: Maximum number of requests per time window
- `X-RateLimit-Remaining`: Remaining requests in current window
- `X-RateLimit-Reset`: Time when the rate limit resets

## Support

For issues or questions, please contact support or refer to the main API documentation.

