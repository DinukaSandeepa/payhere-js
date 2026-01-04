# payhere-js

This repository contains resources related to integrating the PayHere JavaScript SDK (`payhere.js`) for Onsite Checkout experiences.

## Files of Interest

- `LLMs.txt` — A complete copy of the PayHere JavaScript SDK (payhere.js) documentation used as reference for integrations and examples.

## Features

The PayHere JavaScript SDK supports:

- **One-time Payments** - Standard checkout payments
- **Preapproval Payments** - Authorize future payments without immediate charge
- **Recurring Payments** - Set up automatic recurring billing (Weekly, Monthly, Yearly)

## Quick Start

### 1. Include the Script

```html
<script type="text/javascript" src="https://www.payhere.lk/lib/payhere.js"></script>
```

### 2. Configure Payment Object

```javascript
var payment = {
    "sandbox": true,                    // Set to false for production
    "merchant_id": "121XXXX",           // Replace with your Merchant ID
    "return_url": undefined,            // Important for popup mode
    "cancel_url": undefined,            // Important for popup mode
    "notify_url": "https://yoursite.com/notify",
    "order_id": "ORDER123",
    "items": "Product Name",
    "amount": "1000.00",
    "currency": "LKR",
    "hash": "YOUR_GENERATED_HASH",       // Generate server-side!
    "first_name": "John",
    "last_name": "Doe",
    "email": "john@example.com",         // Must be valid email format
    "phone": "0771234567",               // Must be valid phone number
    "address": "No.1, Main Street",
    "city": "Colombo",
    "country": "Sri Lanka"
};
```

### 3. Handle Events & Start Payment

```javascript
payhere.onCompleted = function(orderId) {
    console.log("Payment completed: " + orderId);
};

payhere.onDismissed = function() {
    console.log("Payment dismissed");
};

payhere.onError = function(error) {
    console.log("Error: " + error);
};

payhere.startPayment(payment);
```

## Security Notes

⚠️ **Important Security Considerations:**

- **Never** store your Merchant Secret on the client-side (browser)
- Generate the `hash` parameter on your **server-side** only
- Verify all payment notifications using `md5sig` before processing
- Use HTTPS for your `notify_url` endpoint

## Hash Generation

Generate the hash on your server using:

```
hash = to_upper_case(md5(merchant_id + order_id + amount + currency + to_upper_case(md5(merchant_secret))))
```

**PHP Example:**
```php
$hash = strtoupper(
    md5(
        $merchant_id . 
        $order_id . 
        number_format($amount, 2, '.', '') . 
        $currency .  
        strtoupper(md5($merchant_secret)) 
    ) 
);
```

## Recurring Payments

To enable recurring payments, add these parameters:

```javascript
var payment = {
    // ... other params
    "recurrence": "1 Month",    // Billing frequency: 2 Week, 1 Month, 6 Month, 1 Year
    "duration": "1 Year"        // Total duration: 1 Month, 1 Year, Forever
};
```

## Preapproval Payments

To enable preapproval (authorize without immediate charge):

```javascript
var payment = {
    // ... other params
    "preapprove": true
};
```

## Payment Status Codes

| Code | Status |
|------|--------|
| 2 | Success |
| 0 | Pending |
| -1 | Canceled |
| -2 | Failed |
| -3 | Chargedback |

## Supported Payment Methods

`VISA`, `MASTER`, `AMEX`, `EZCASH`, `MCASH`, `GENIE`, `VISHWA`, `PAYAPP`, `HNB`, `FRIMI`

## Documentation

For complete documentation, see:
- `LLMs.txt` in this repository
- [Official PayHere JavaScript SDK Documentation](https://support.payhere.lk/api-&-mobile-sdk/javascript-sdk)

## Prerequisites

1. **Merchant ID** - Find in Side Menu > Integrations of your PayHere Account
2. **Merchant Secret** - Generate for your domain/app:
   - Go to Side Menu > Integrations
   - Click 'Add Domain/App'
   - Enter your top level domain or app package name
   - Wait for approval (up to 24 hours)
   - Copy your Merchant Secret

---

*Last updated: 4th January 2026*
