# Mamlaka Hub and Spoke Payment API (v2)

**Author**  
Collins Ochieng  

**Email**  
collins@mam-laka.com  

**Phone**  
+254794940160  
254768899729  

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
    "transactionId": "4950-41ad-9820-8559865ca58950703213"
}

```
# Callback Success  response 

```json
{
  "Body": {
    "stkCallback": {
      "MerchantRequestID": "4950-41ad-9820-8559865ca58950703213",
      "CheckoutRequestID": "ws_CO_21012025132712034768899729",
      "ResultCode": 0,
      "ResultDesc": "The service request is processed successfully.",
      "CallbackMetadata": {
        "Item": [
          {
            "Name": "Amount",
            "Value": 1.00
          },
          {
            "Name": "MpesaReceiptNumber",
            "Value": "NLJ7RT61SV"
          },
          {
            "Name": "TransactionDate",
            "Value": 20250121132712
          },
          {
            "Name": "PhoneNumber",
            "Value": 254768899729
          }
        ]
      }
    }
  }
}

```
# Callback Failed Transaction   Response 

```json
{
    "Body": {
      "stkCallback": {
        "MerchantRequestID": "2697-4453-b07f-ff58b4b1cf417304846",
        "CheckoutRequestID": "ws_CO_01022025203439198725822504",
        "ResultCode": 1032,
        "ResultDesc": "Request cancelled by user"
      }
    }
}
```

- **`transactionId`**: This is a unique identifier returned after initiating a payment. Use it to track the transaction status. It will be the same as the **`MerchantRequestID`** provided in the callback. Use this ID to update your records accordingly.


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
  "Result": {
    "ResultType": 0,
    "ResultCode": 0,
    "ResultDesc": "The service request is processed successfully.",
    "OriginatorConversationID": "a392-45d1-93c1-58f9447915e717138558",
    "ConversationID": "AG_20250201_206051c643ca6389833f",
    "TransactionID": "TB18E731YO",
    "ResultParameters": {
      "ResultParameter": [
        {
          "Key": "ReceiverPartyPublicName",
          "Value": "0768899729 - Collins "
        },
        {
          "Key": "TransactionCompletedDateTime",
          "Value": "01.02.2025 21:28:54"
        },
        {
          "Key": "B2CUtilityAccountAvailableFunds",
          "Value": 6918.7
        },
        {
          "Key": "B2CWorkingAccountAvailableFunds",
          "Value": 0
        },
        {
          "Key": "B2CRecipientIsRegisteredCustomer",
          "Value": "Y"
        },
        {
          "Key": "B2CChargesPaidAccountAvailableFunds",
          "Value": 0
        },
        {
          "Key": "TransactionAmount",
          "Value": 10
        },
        {
          "Key": "TransactionReceipt",
          "Value": "TB18E731YO"
        }
      ]
    },
    "ReferenceData": {
      "ReferenceItem": {
        "Key": "QueueTimeoutURL",
        "Value": "http://internalapi.safaricom.co.ke/mpesa/b2cresults/v1/submit"
      }
    }
  }
}

```

- **`transactionId`**: This is a unique identifier returned after initiating a payment. Use it to track the transaction status. It will be the same as the **`OriginatorConversationID`** provided in the callback. Use this ID to update your records accordingly.





