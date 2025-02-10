# Mamlaka Hub and Spoke Payment API (v2.0.1)

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
- **Method**: `POST`
- **Headers**: 
  - `Authorization: Basic Y29sbHM6Y29tZXRhcHBtYWlu`

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
# Sample response 

```json
{
    "message": "Payment initiation successful",
    "secureId": "qdml8553ZeInavKorBHzLA==",
    "transactionId": ImpadlTdest2",
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







