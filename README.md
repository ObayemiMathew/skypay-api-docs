# SkyPay Payments API Documentation

## Overview
SkyPay API allows developers to create, verify, and refund payments securely.  
It uses REST conventions and JSON for requests and responses.  
This documentation helps developers integrate SkyPay quickly and efficiently.

---

## Base URL
```
https://api.skypay.com/v1
```

All endpoints should be appended to the base URL.  
Example:
```
GET /payments → https://api.skypay.com/v1/payments
```

---

## Authentication
SkyPay uses **Bearer Token authentication**. Include your API key in every request.

### Example:
```
Authorization: Bearer YOUR_SECRET_KEY
```

### Invalid or missing API key response:
```json
{
  "error": "Invalid API key",
  "code": 401
}
```

---

# Endpoints

## 1. Initialize Payment
### `POST /payments`

Creates a new payment session and returns a checkout URL.

### Request Body
```json
{
  "amount": 5000,
  "currency": "NGN",
  "email": "user@example.com"
}
```

### Successful Response
```json
{
  "status": "success",
  "payment_id": "pay_98332",
  "authorization_url": "https://checkout.skypay.com/pay_98332"
}
```

---

## 2. Verify Payment
### `GET /payments/{payment_id}`

Retrieve the status of a payment.

### Example Request:
```
GET /payments/pay_98332
```

### Successful Response:
```json
{
  "status": "success",
  "payment_id": "pay_98332",
  "amount": 5000,
  "currency": "NGN",
  "paid": true
}
```

---

## 3. List Transactions
### `GET /transactions?page=1&limit=10`

Returns a paginated list of transactions.

### Query Parameters
| Name | Type | Description |
|------|------|-------------|
| `page` | integer | Page number |
| `limit` | integer | Number of items per page |

### Successful Response:
```json
{
  "page": 1,
  "limit": 10,
  "total": 52,
  "data": [
    {
      "payment_id": "pay_98332",
      "amount": 5000,
      "currency": "NGN"
    }
  ]
}
```

---

## 4. Refund Payment
### `POST /refunds`

Refund a previously completed payment.

### Request Body
```json
{
  "payment_id": "pay_98332",
  "amount": 2000
}
```

### Successful Response:
```json
{
  "status": "success",
  "refund_id": "ref_5443",
  "refunded_amount": 2000
}
```

---

# Error Codes

| Code | Meaning |
|------|---------|
| **400** | Bad request |
| **401** | Unauthorized |
| **403** | Forbidden |
| **404** | Not found |
| **429** | Too many requests |
| **500** | Server error |

### Example Error Response:
```json
{
  "error": "Missing amount field",
  "code": 400
}
```

---

# Webhooks

SkyPay sends webhook events to notify your server when payments are completed.

### Example Webhook Endpoint:
```
POST https://yourapp.com/webhooks/skypay
```

### Webhook Payload:
```json
{
  "event": "payment.success",
  "payment_id": "pay_98332",
  "amount": 5000
}
```

### Processing Webhooks
1. Receive the POST request  
2. Parse JSON payload  
3. Verify the event type  
4. Update your database accordingly  

---

# Rate Limits
SkyPay API allows:
```
60 requests per minute
```

### Rate Limit Response:
```json
{
  "error": "Too many requests",
  "code": 429
}
```

---

# Support
For assistance, contact:  
**support@skypay.com**
