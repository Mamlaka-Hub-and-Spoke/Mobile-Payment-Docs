# Mamlaka Hub and Spoke Payment API (v2.0.3)

**Author**  
Collins 

**Email**  
collins@mam-laka.com  

 

**Base URL**  
`https://payments.mam-laka.com`

---
#### Mobile APIs

### Step 1: Generate Token

## Authentication

### Step 1: Generate Token
To perform API operations, you need to first generate a token using the username and password issued during the setup process. The token will be used in the Authorization header for subsequent operations.

#### API Endpoint to Generate Token
- **URL**: `{{baseUrl}}/api/v1`
- **Method**: `GET`
- **Headers**: 
  - `Authorization: Basic Y29sbHM6Y29tZXRhcHBtYWlu`
![image](https://github.com/user-attachments/assets/5c9f6e7a-2e12-4f04-af3a-5feaf971ac94)




#### Example cURL Request:
```bash
curl --location 'https://payments.mam-laka.com/api/v1' \
--header 'Authorization: Basic Y29sbHM6Y29tZXRhcHBtYWlu'




```
## API-Documentation

### Initiate STk push ie C2B

**Endpoint:** `{{BASE_URL}}/api/v1/mobile/initiate`

**Method:** `POST`

```json
{
  "impalaMerchantId": "{{username}}",
  "displayName": "AVIATOR", //name you want appear on the customer when making the payment 
  "currency": "KES",
  "amount": 10,
  "payerPhone": "254768899729", 
  "mobileMoneySP": "M-Pesa",
  "externalId": "ImpadlTdest2",
  "callbackUrl": "https://97e6-217-21-116-242.ngrok-free.app/" 
}



```
![image](https://github.com/user-attachments/assets/fef0fb1e-bdec-4ded-9cbc-3e7d83f83a80)

# Sample response 

```json
{
    "message": "Payment initiation successful",
    "secureId": "qdml8553ZeInavKorBHzLA==",
    "transactionId": "ImpadlTdest2",
}

```
# Callback Success  response 

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
# Callback Failed Transaction   Response 

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



### Money Transfer Alias B2C

**Endpoint:** `{{BASE_URL}}/api/v1/mobile/transfer`

**Method:** `POST`

```json
{
    "impalaMerchantId":"{{username}}",
    "currency":"KES",
    "amount":10,
    "recipientPhone":"254768899729", 
    "mobileMoneySP":"M-Pesa",
    "externalId":"joeltest404",
    "callbackUrl":"https://97e6-217-21-116-242.ngrok-free.app/"
}

```
# Sample response 

```json
{
    "message": "Payment initiation successful",
    "secureId": "qdml8553ZeInavKorBHzLA==",
    "transactionId": "a392-45d1-93c1-58f9447915e717138558"
}

```
# Callback Success  response 

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

# Callback Failed  response 

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
### Card Transaction

**Endpoint:** `https://payments.mam-laka.com/api/v1/card/initiate`

**Method:** `POST`

```json
{
   "impalaMerchantId":"{{username}}",
    "currency":"USD",
    "amount":1,
    "mobileMoneySP":"card",
    "externalId":"ImpadlTdest2",
     "redirectUrl":"backtoyoursite.com",
    "callbackUrl":"https://8bd0-2c0f-fe38-2413-2c98-7114-6990-db59-11c.ngrok-free.app/mc/log.php"
}

```
# Sample response 

```json
{
    "cardLink": "http://commetagri.mam-laka.com/uba.php?data=YW1vdW50PTEuMDAmbWVyY2hhbnQ9YXBwJmNhbGxiYWNrPWh0dHBzOi8vOGJkMC0yYzBmLWZlMzgtMjQxMy0yYzk4LTcxMTQtNjk5MC1kYjU5LTExYy5uZ3Jvay1mcmVlLmFwcC9tYy9sb2cucGhwJnJlZGlyZWN0PU9XWEpKRFVNZ1B2aDlYUWhTQnk5eGc9PSZleHRlcm5hbGlkPUltcGFkbFRkZXN0Mg==",
    "message": "card Payment  initiation successful",
    "secureId": "OWXJJDUMgPvh9XQhSBy9xg=="
}

```
# Card Callback Success  response 

```json
{
 "transactionStatus":"COMPLETED",
 "transactionReport":"SUCCESS",
 "currency":"KES",
 "amount":"5.00",
 "netAmount":"5.00",
 "secureId":"8e6bfa77-4f2a-4ff6-af48-ef774f2178ca",
 "externalId":"BoomW",
 "redirectUrl":"bRbHRh6Tmi1mBNUzKKTtzA=="
}

```
# Card Callback Failed  response 

```json
{
 "transactionStatus":"FAILED",
 "transactionReport":"N/A",
 "currency":"KES",
 "amount":"5.00",
 "netAmount":"5.00",
 "secureId":"8e6bfa77-4f2a-4ff6-af48-ef774f2178ca",
 "externalId":"BoomW",
 "redirectUrl":"bRbHRh6Tmi1mBNUzKKTtzA=="
}
```
# Check Transaction Status

This document describes how to check the status of a transaction using the provided API endpoint.

## Endpoint


https://payments.mam-laka.com/api/v1/transaction?merchant=<your_merchant_id>&secureId=<secure_id>

**Note:**

* Replace `<your_merchant_id>` with your actual merchant ID.
* Replace `<secure_id>` with the `secure_id` returned during transaction initialization.

## Example Request

https://payments.mam-laka.com/api/v1/transaction?merchant=impala&secureId=d4XeBKNruNJnvhOMOd8RLg==

## Sample Response (JSON)

```json
{
  "transaction": {
    "amount": 1,
    "callback_url": "[https://f0d6-197-232-22-252.ngrok-free.app](https://f0d6-197-232-22-252.ngrok-free.app)",
    "currency": "USD",
    "date_added": 1741675246,
    "external_id": "ImpadlTdest2",
    "impalaMerchantId": "impala",
    "secure_id": "d4XeBKNruNJnvhOMOd8RLg==",
    "source_of_funds": "card",
    "transaction_report": "collection"
    "transaction_status": "PENDING" //FAILED OR COMPLETED
  }
}
```



