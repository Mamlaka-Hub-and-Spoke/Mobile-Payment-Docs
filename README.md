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

- **Mobile APIs:** 100 requests per minute
- **Card Transactions:** 50 requests per minute
- **Balance APIs:** 200 requests per minute
- **Transaction Status:** 300 requests per minute

---

## Support

For technical support or questions about this API, please contact:
- **Email:** collins@mam-laka.com
- **Documentation Version:** 3.0.0
- **Last Updated:** February 2025
