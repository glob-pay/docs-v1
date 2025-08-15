# GlobPay API Documentation

## Overview
GlobPay API provides secure payment processing capabilities with token-based authentication. This API allows merchants to initiate card payments and receive payment links for iframe integration.

## Base URL
```
Sandbox: https://sandbox.globpay.ai/api/v1
Production: https://api.globpay.ai/api/v1
```

## Authentication

### Generate Access Token
Generate a Bearer token using Basic OAuth authentication.

**Endpoint:** `GET /api/v1`

**Headers:**
```
Authorization: Basic bXNpbWJvX3RlY2hub2xvZ2llczptc2ltYm9fdGVjaG5vbG9naWVz
```

**cURL Example:**
```bash
curl --location 'https://sandbox.globpay.ai/api/v1' \
--header 'Authorization: Basic bXNpbWJvX3RlY2hub2xvZ2llczptc2ltYm9fdGVjaG5vbG9naWVz'
```

**Credentials:**
- Username: `msimbo_technologies`
- Password: `msimbo_technologies`

### Successful Response (200)
```json
{
    "expires_at": "2025-08-16T14:33:38+02:00",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE3NTUzNDc2MTgsIm1lcmNoYW50SUQiOiJtc2ltYm9fdGVjaG5vbG9naWVzIiwidXNlcm5hbWUiOiJtc2ltYm9fdGVjaG5vbG9naWVzIn0.SHyy2z4zXJPAZWGc-phZQw1bLp94DyARXrIrQ7s0M5o"
}
```

### Failed Response (401)
```json
{
    "error": "Invalid password"
}
```

## Payment Operations

### Initiate Card Payment
Create a new payment transaction and receive a payment link for iframe integration.

**Endpoint:** `POST /api/v1/globpay/card/initiate`

**Headers:**
```
Content-Type: application/json
Authorization: Bearer {your_token}
```

**Request Body:**
```json
{
  "first_name": "John",
  "last_name": "Doe",
  "email": "john.doe@example.com",
  "address": "123 Main Street",
  "country": "US",
  "city": "New York",
  "state": "NY",
  "zip": "10001",
  "ip_address": "192.168.1.100",
  "phone_number": "+1234567890",
  "amount": 1,
  "currency": "USD",
  "card_number": "4012888888881881",
  "card_expiry_month": "01",
  "card_expiry_year": "2030",
  "card_cvv": "029",
  "redirect_url": "https://webhook.site/d4a26e9e-dc74-4e03-8def-71dc4d9d51bd",
  "webhook_url": "https://webhook.site/d4a26e9e-dc74-4e03-8def-71dc4d9d51bd",
  "order_id": "ORD_123456789335909991374"
}
```

**cURL Example:**
```bash
curl --location 'https://sandbox.globpay.ai/api/v1/globpay/card/initiate' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE3NTUzNDg1MjIsIm1lcmNoYW50SUQiOiJtc2ltYm9fdGVjaG5vbG9naWVzIiwidXNlcm5hbWUiOiJtc2ltYm9fdGVjaG5vbG9naWVzIn0.jnK9O2DXbzb5B05w-Q0vw8tdY7WhiWMtcvFAmyzprj0' \
--data-raw '{
  "first_name": "John",
  "last_name": "Doe",
  "email": "john.doe@example.com",
  "address": "123 Main Street",
  "country": "US",
  "city": "New York",
  "state": "NY",
  "zip": "10001",
  "ip_address": "192.168.1.100",
  "phone_number": "+1234567890",
  "amount": 1,
  "currency": "USD",
  "card_number": "4012888888881881",
  "card_expiry_month": "01",
  "card_expiry_year": "2030",
  "card_cvv": "029",
  "redirect_url": "https://webhook.site/d4a26e9e-dc74-4e03-8def-71dc4d9d51bd",
  "webhook_url": "https://webhook.site/d4a26e9e-dc74-4e03-8def-71dc4d9d51bd",
  "order_id": "ORD_123456789335909991374"
}'
```

### Successful Response (200)
```json
{
    "message": "Payment initiation successful",
    "orderId": "ORD_123456789335909991374s",
    "redirect_url": "https://portal.pay.agency/v1/test/card/checkout/PA6218804317346445",
    "status": "REDIRECT",
    "transactionId": "PA6218804317346445"
}
```

## Iframe Integration

### Server-to-Server (Staging Available)
The `redirect_url` from the payment initiation response can be loaded in an iframe for seamless payment processing.

**HTML Example:**
```html
<iframe 
    src="https://portal.pay.agency/v1/test/card/checkout/PA6218804317346445"
    width="100%" 
    height="600px" 
    frameborder="0">
</iframe>
```

## Error Handling

### HTTP Status Codes

| Status Code | Description | Response Body |
|-------------|-------------|---------------|
| 200 | Success | Payment details or token information |
| 400 | Bad Request | Invalid request parameters |
| 401 | Unauthorized | Invalid credentials or expired token |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Resource not found |
| 409 | Conflict | Duplicate order ID |
| 422 | Unprocessable Entity | Validation errors |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Server error |

### Common Error Responses

#### Invalid Token (401)
```json
{
    "details": "token signature is invalid: signature is invalid",
    "error": "Invalid or expired token"
}
```

#### Duplicate Order ID (409)
```json
{
    "message": "Order ID already exists. Please use a new one.",
    "status": "DUPLICATE_ORDER"
}
```

#### Validation Error (422)
```json
{
    "error": "Validation failed",
    "details": [
        {
            "field": "card_number",
            "message": "Invalid card number format"
        }
    ]
}
```

#### Rate Limit Exceeded (429)
```json
{
    "error": "Rate limit exceeded",
    "retry_after": 60
}
```

## Testing

### Test Card Numbers
- **Visa Test Card**: 4012888888881881
- **Mastercard Test Card**: 5555555555554444
- **American Express Test Card**: 378282246310005

### Test Environment
- Use sandbox URLs for testing
- Test webhook endpoints with [webhook.site](https://webhook.site)
- Monitor responses in Postman or similar tools

## Security Considerations

- Always use HTTPS in production
- Store tokens securely and never expose them in client-side code
- Implement proper token refresh mechanisms
- Validate all input parameters server-side
- Use webhooks for payment status updates
- Implement proper error logging and monitoring

## Support

For technical support or questions:
- **API Documentation**: [GlobPay API Docs](https://documenter.getpostman.com/view/17201336/2sB3B8sD7g)
- **Sandbox Environment**: https://sandbox.globpay.ai
- **Production Environment**: https://api.globpay.ai

---

**Last Updated**: December 2024  
**API Version**: v1  
**Environment**: Sandbox & Production
