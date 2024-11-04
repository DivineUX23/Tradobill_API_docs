# Tradobill_API_docs
Tradobill API doc
## Endpoints, Request Bodies, Headers, and Responses

### 1. Login Endpoint
**Endpoint:** `/api/auth/login`
**Request Method:** POST
**Request Body:**
```json
{
    "username": "your_username",
    "password": "your_password"
}
```
**Request Headers:**
- `Content-Type: application/json`
**Response:**
- **Success (200):**
  ```json
  {
  "code": 200,
  "status": "ok",
  "message": {
    "success": [
      "Login Succesfull"
    ]
  },
  "data": {
    "user": {
      "id": 7,
      "firstname": null,
      "lastname": null,
      "username": "johndoe",
      "email": "john.doe@example.com",
      "api_key": null,
      "webhook_url": null,
      "redirect_url": null,
      "country_code": "US",
      "mobile": "+11234567890",
      "ref_by": null,
      "balance": "0.00000000",
      "ref_balance": "0.000000",
      "hold_balance": null,
      "trx_password": null,
      "image": null,
      "nuban": null,
      "nuban_ref": null,
      "address": {
        "address": "",
        "state": "",
        "zip": "",
        "country": "United States",
        "city": ""
      },
      "gender": null,
      "dob": null,
      "status": 1,
      "ev": 0,
      "sv": 1,
      "en": null,
      "sn": null,
      "profile_complete": 0,
      "kyc_complete": null,
      "kyc": null,
      "ver_code_send_at": null,
      "ts": 0,
      "tv": 1,
      "tsc": null,
      "vendor": null,
      "api_access": null,
      "ban_reason": null,
      "plan_id": null,
      "earn_at": null,
      "expire_at": null,
      "customer_id": null,
      "customer_tier": null,
      "created_at": "2024-10-24T12:10:25.000000Z",
      "updated_at": "2024-10-24T12:10:25.000000Z"
    },
    "accessToken": "50|Up5K7QkCYZDB8TgHcw3ZKYdI2GgkbFevg1pv8EZn",
    "token_type": "Bearer"
  }
}
  ```
- **Unauthorized (401):**
  ```json
  {
      "code": 401,
      "type": "error",
      "status": "unauthorized",
      "message": "Unauthorized user"
  }
  ```

### 2. Logout Endpoint
**Endpoint:** `/api/auth/logout`
**Request Method:** POST
**Request Headers:**
- `Authorization: Bearer your_access_token`
**Response:**
- **Success (200):**
  ```json
  {
      "code": 200,
      "status": "ok",
      "message": ["success": "Logout successful"]
  }
  ```





## Registration Endpoint

**Endpoint:** `/auth/register`

**PHP Route:** `Route::post('auth/register', 'Api\Auth\RegisterController@register');`

**Request:**

* **Method:** POST
* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "email": "testuser@example.com",
    "mobile": "1234567890",
    "password": "SecurePassword123",
    "password_confirmation": "SecurePassword123",
    "username": "testuser123",
    "country_code": "US", 
    "mobile_code": "+1",
    "country": "United States",
    "agree": true // If required by GeneralSetting
}
```


**Response (Successful Registration - 202):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 202,
    "status": "created",
    "message": {
        "success": [
            "Registration successfull"
        ]
    },
    "data": {
        "access_token": "generated_access_token",
        "token_type": "Bearer",
        "user": {
            "id": 2,  // Example user ID
            "username": "testuser123",
            "email": "testuser@example.com",
            // ... other user details
        }
    }
}
```

**Response (Validation Error - 200):**  *(This should be a 422 Unprocessable Entity)*

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 200, // Incorrect: Should be 422
    "status": "ok", // Incorrect: Should be "error" or similar
    "message": {
        "error": [
            "The email has already been taken.",
            // ...other validation errors
        ]
    }
}
```



**Response (Mobile Number Exists - 409):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 409,
    "status": "conflict",
    "message": {
        "error": [
            "The mobile number already exists"
        ]
    }
}

```



## Send Password Reset Code Endpoint

**Endpoint:** `/auth/password/reset` (This is a suggested endpoint; adjust as needed)

**PHP Route:** `Route::post('auth/password/reset', 'Api\Auth\ForgotPasswordController@sendResetCodeEmail');`


**Request:**

* **Method:** POST
* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "type": "email", // or "username"
    "value": "testuser@example.com" // or "testusername"
}
```

**Response (Success - 200):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 200,
    "status": "ok",
    "message": {
        "success": [
            "Password reset email sent successfully"
        ]
    },
    "data": {
        "email": "testuser@example.com"
    }
}
```

**Response (User Not Found - 200):** *(Should be a 404 Not Found)*

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 200, // Should be 404
    "status": "ok", // Should indicate an error
    "message": {
        "error": [
            "User not found."
        ]
    }
}
```

**Response (Validation Error - 200):** *(Should be a 422 Unprocessable Entity)*

* **Headers:**
    * `Content-Type: application/json`
* **Body:** (Example with email validation error)

```json
{
    "code": 200, // Should be 422
    "status": "ok", // Should indicate an error
    "message": {
        "error": [
            "Email must be a valid email" 
        ]
    }
}
```


**Response (Invalid Selection - 200):** *(Should be a 400 Bad Request)*

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 200, // Should be 400
    "status": "ok",  // Should indicate an error
    "message": {
        "error": [
            "Invalid selection"
        ]
    }
}
```



## Verify Code Endpoint


**Endpoint:** `/auth/password/verify` (Suggested endpoint)

**PHP Route:** `Route::post('auth/password/verify', 'Api\Auth\ForgotPasswordController@verifyCode');`


**Request:**

* **Method:** POST
* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "email": "testuser@example.com",
    "code": "123456"
}

```


**Response (Success - 200):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 200,
    "status": "ok",
    "message": {
        "success": [
            "You can change your password"
        ]
    },
    "data": {
        "token": "123456",
        "email": "testuser@example.com"
    }
}
```


**Response (Invalid Token - 200):** *(Should be a 400 Bad Request or 422)*


* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 200, // Should be 400 or 422
    "status": "ok", // Should be "error"
    "message": {
        "error": [
            "Invalid token"
        ]
    }
}
```

**Response (Validation Error - 200):** *(Should be 422)*

* **Headers:**
 * `Content-Type: application/json`
* **Body:** (Example if the `code` is missing)
```json
{
    "code": 200,  // Should be 422
    "status": "ok",  // Should be "error"
    "message": {
        "error": [
            "The code field is required."
        ]
    }
}
```

## Reset Password Endpoint

**Endpoint:** `/auth/password/update` (This is a suggested endpoint; adjust if needed)

**PHP Route:** `Route::post('auth/password/update', 'Api\Auth\ResetPasswordController@reset');`

**Request:**

* **Method:** POST
* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "email": "testuser@example.com",
    "token": "123456",
    "password": "NewSecurePassword123",
    "password_confirmation": "NewSecurePassword123"
}
```

**Response (Success - 200):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 200,
    "status": "ok",  // Consider using "success"
    "message": {
        "success": [ // Or just have a message, not an array
            "Password changed"
        ]
    }
}
```

**Response (Invalid Verification Code - 200):** *(Should be a 400 Bad Request or 422)*

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 200, // Should be 400 or 422
    "status": "ok", // Should indicate an error
    "message": {
        "error": [
            "Invalid verification code"
        ]
    }
}
```


**Response (Validation Error - 200):** *(Should be a 422 Unprocessable Entity)*

* **Headers:**
    * `Content-Type: application/json`
* **Body:**  (Example with a password validation error)

```json
{
    "code": 200, // Should be 422
    "status": "ok", // Should indicate an error
    "message": {
        "error": [
            "The password must be at least 6 characters." // Example
        ]
    },
    "data": null  // This is unnecessary
}
```




Let's break down and document each endpoint in the provided code.

## Authorization Check Endpoint

**Endpoint:** `/auth/authorization` (This is a suggested endpoint – adjust as needed.)

**PHP Route:** `Route::get('auth/authorization', 'Api\AuthorizationController@authorization');` (Assumed GET, but could be POST)

**Request:**

* **Method:** GET (or POST)
* **Headers:**
    * `Authorization: Bearer <access_token>`

**Response (Account Deactivated - 200):** *(Should be a 403 Forbidden)*

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 200, // Should be 403
    "status": "ok", // Should indicate an error
    "message": {
        "success": [ // Shouldn't be "success"
            "Your account has been deactivated"
        ]
    }
}
```

**Response (Email Verification Needed - 200):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 200,
    "status": "ok",
    "message": {
        "success": [
            "Email verification"
        ]
    },
    "data": {
        "verification_url": "http://example.com/api/user/verify/email",
        "verification_method": "POST",
        "resend_url": "http://example.com/api/user/send/verify/code?type=email",
        "resend_method": "GET",
        "verification_type": "email"
    }
}
```

**Response (SMS Verification Needed - 200):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:** (Similar structure to email verification, but with SMS details)

```json
{
    "code": 200,
    "status": "ok",
    "message": {
        "success": [
            "SMS verification"
        ]
    },
    "data": {
        "verification_url": "http://example.com/api/user/verify/sms",
        "verification_method": "POST",
        "resend_url": "http://example.com/api/user/send/verify/code?type=phone",
        "resend_method": "GET",
        "verification_type": "sms"
    }
}

```


**Response (2FA Verification Needed - 200):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 200,
    "status": "ok",
    "message": {
        "success": [
            "Google Authenticator"  // Consider a more descriptive message
        ]
    },
    "data": {
        "verification_url": "http://example.com/api/user/go2fa/verify",
        "verification_method": "POST",
        "verification_type": "2fa"
    }
}
```


## Send Verification Code Endpoint

**Endpoint:** `/user/send/verify/code`

**PHP Route:** `Route::get('user/send/verify/code', 'Api\AuthorizationController@sendVerifyCode');`


**Request:**

* **Method:** GET
* **Headers:**
    * `Authorization: Bearer <access_token>`
* **Query Parameters:**
    * `type`:  `email` or `phone`

**Response (Success - Email - 200):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 200,
    "status": "ok",
    "message": {
        "success": [
            "Email verification code sent successfully"
        ]
    }
}

```

**Response (Success - SMS - 200):** (Similar structure, but message indicates SMS)

```json
{
    "code": 200,
    "status": "ok",
    "message": {
        "success": [
            "SMS verification code sent successfully"
        ]
    }
}
```


**Response (Rate Limit - 200):** *(Should be a 429 Too Many Requests)*

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 200,  // Should be 429
    "status": "ok", // Should be "error" or similar
    "message": {
        "success": [ // Conflicting - Shouldn't be "success"
            "Please Try after 118 Seconds" //  Make this more user-friendly
        ]
    }
}
```

**Response (Sending Failed - 200):** *(Should be a 500 Internal Server Error)*

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
   "code": 200, // Should be 500
   "status": "ok", //  Should be an error status
   "message": {
       "success": [  // Should not be a success message
           "Sending Failed" // Be more specific if possible
       ]
   }
}

```



## Email Verification Endpoint

**Endpoint:** `/user/verify/email`

**PHP Route:** `Route::post('user/verify/email', 'Api\AuthorizationController@emailVerification');`

**Request:**

* **Method:** POST
* **Headers:**
    * `Authorization: Bearer <access_token>`
    * `Content-Type: application/json`
* **Body:**

```json
{
    "email_verified_code": "123456"
}
```

**Response (Success - 200):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 200,
    "status": "ok",
    "message": {
        "success": [
            "Email verified successfully"
        ]
    }
}
```

**Response (Code Mismatch - 200):** *(Should be a 400 Bad Request)*

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 200, // Should be 400
    "status": "ok",  // Should indicate error
    "message": {
        "success":[ // Shouldn't be success
            "Verification code didn't match!"
        ]
    }
}

```

**Response (Validation Error - 200):**  *(Should be 422)*

* **Headers:**
* `Content-Type: application/json`
* **Body:** (Example if `email_verified_code` is missing)

```json

{
    "code": 200, // Should be 422
    "status": "ok", // Should indicate an error
    "message": {
        "error": [
            "The email verified code field is required."
        ]
    }
}
```



## SMS Verification Endpoint (Similar structure to Email Verification)

**Endpoint:** `/user/verify/sms`

**PHP Route:**  `Route::post('user/verify/sms', 'Api\AuthorizationController@smsVerification');`

(Request and Response structures are very similar to email verification, just using `sms_verified_code` instead of `email_verified_code`.)



## 2FA Verification Endpoint


**Endpoint:** `/user/go2fa/verify`

**PHP Route:** `Route::post('user/go2fa/verify', 'Api\AuthorizationController@g2faVerification');`

**Request:**

* **Method:** POST
* **Headers:**
    * `Authorization: Bearer <access_token>`
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": "123456" 
}
```


**Response (Success - 200):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 200,
    "status": "ok",
    "message": {      
        "success": [   // Or just a plain message string
            "Verification successful"
        ]
    }
}
```


**Response (Wrong Code - 200):** *(Should be 400 or 401 Unauthorized)*

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json

{
    "code": 200,  // Should be 400 or 401
    "status": "ok", // Should indicate an error
    "message": {
        "error": [  //  "error" key is better for consistency
            "Wrong verification code"
        ]
    }
}

```
















```markdown
# User API Documentation

This document provides details on the available API endpoints for user functionalities. All endpoints require authentication unless otherwise specified.

## Authentication

All endpoints except `/user/validateusername` require a valid bearer token in the `Authorization` header for authentication.


## Endpoints

### 1. Home

* **Endpoint:** `/user/home`
* **Method:** `GET`
* **Description:** Retrieves user dashboard data, including balances, transactions, tickets, and other statistics.
* **Request Body:** None
* **Response Body:**
```json
{
  "status": "success",
  "message": "home",
  "data": {
    "pageTitle": "Dashboard",
    "widget": {
      "ref_balance": 0,
      "balance": 0,
      "usd_balance": 0, 
      "gbp_balance": 0,
      "total_transaction": 0,
      "total_debit": 0,
      "total_credit": 0,
      "total_ticket": 0
    },
    "data": {
        "gatewayCurrency": [],
        "yearLabels": [],
        "yearDeposit": [],
        "yearPayout": [],
        "ref": 0,
        "top_earner": [],
        "user_browser_counter": {},
        "user_os_counter": {},
        "user_login_counter": {},
        "logins": [],
        "current_login": {},
        "last_login": {},
        "credit": [],
        "debit": [],
        "trxlog": [],
        "deposit": [] 
    }
  }
}
```


### 2. Crypto Trade

* **Endpoint:** `crypto/trade/{id?}`
* **Method:** `GET`
* **Description:**  Retrieves crypto trade data.  If `id` is 'buy' or 'sell', returns available currencies. Otherwise, returns trade history and statistics.
* **Request Body:** None
* **Response Body (if id is not 'buy' or 'sell'):**
```json
{
  "pageTitle": "Trade Crypto Trade",
  "currencies": [],
  "log": [], // Paginated trade history
  "week": 0,
  "month": 0,
  "year": 0,
  "sellPercentage": 0,
  "buyPercentage": 0,
  "total": 0
}
```
* **Response Body (if id is 'buy' or 'sell'):**
```json
{
    "pageTitle": "Trade Crypto Trade",
    "currencies": []
}
```


### 3. API Key

* **Endpoint:** `/user/apikey`
* **Method:** `GET`
* **Description:** Retrieves the user's API key.
* **Request Body:** None
* **Response Body:**
```json
{
    "pageTitle": "API Key",
    "key": "sk_xxxxxxxxxxxx", 
    "user": {} 
}
```


### 4. Generate API Key

* **Endpoint:** `/user/apikey/generate`
* **Method:** `POST`
* **Description:** Generates a new API key for the user.
* **Request Body:** None
* **Response Body:**
```json
{
  "success": "API Key Generated Successfully"
}
```



### 5. API Webhook

* **Endpoint:** `/user/apikey/webhook`
* **Method:** `POST`
* **Description:** Updates the user's webhook and callback URLs.
* **Request Body:**
```json
{
  "webhook": "your_webhook_url",
  "callback": "your_callback_url"
}
```
* **Response Body:**
```json
{
  "success": "Webhook & Callback URL Updated Successfully"
}
```


### 6. QR Code

* **Endpoint:** `/user/qrcode`
* **Method:** `GET`
* **Description:** Retrieves the user's QR code information and transaction history.
* **Request Body:** None
* **Response Body:**
```json
{
    "today": 0,
    "trx": [],
    "pageTitle": "QR Code",
    "url": "qr_code_url",
    "payment": 0,
    "received": 0
}
```


### 7. KYC

* **Endpoint:** `/user/kyc`
* **Method:** `GET`
* **Description:** Retrieves the user's KYC verification status and information.
* **Request Body:** None
* **Response Body:**
```json
{
    "pageTitle": "KYC Verification",
    "user": {
       // User data including KYC information
    }
}
```


### 8. KYC Submit

* **Endpoint:** `/user/kyc/post`
* **Method:** `POST`
* **Description:** Submits KYC verification documents.
* **Request Body:**
```json
{
  "type": "kyc_type",  //e.g., passport, driving license
  "front": "front_image_file",  // File upload
  "back": "back_image_file"    // File upload
}
```

* **Response Body:**
```json
{
  "success": "KYC document submitted successfully"
}
```

### 9. Generate Nuban

* **Endpoint:** `/user/generate/nuban`
* **Method:** `POST`
* **Description:** Generates a virtual account number (Nuban) for the user.
* **Request Body:** None. Provider is determined by the server's configuration.
* **Response Body (Success):**
```json
{
  "ok": true,
  "status": "success",
  "message": "Account number created successfully" 
}
```
* **Response Body (Error):**
```json
{
  "ok": false,
  "status": "danger",
  "message": "Error message"
}
```




### 10. Swap Currency

* **Endpoint:** `/user/swap`
* **Method:** `GET`
* **Description:** Retrieves the user's currency swap history.
* **Request Body:** None
* **Response Body:**
```json
{
    "pageTitle": "Swap Currency",
    "log": []
}
```



### 11. Swap Currency Rate

* **Endpoint:** `/user/swap/rate`
* **Method:** `POST`
* **Description:** Retrieves the exchange rate for a currency swap.
* **Request Body:**
```json
{
  "from": "currency_code", // E.g., USD
  "to": "currency_code",   // E.g., NGN
  "amount": 100           // Amount to swap
}
```
* **Response Body:**
```json
{
    "ok": true,
    "status": "success",
    "message": "calculated_amount currency_code" // E.g., "57000 NGN"
}

```


### 12. Swap Currency Post

* **Endpoint:** `/user/swap`
* **Method:** `POST`
* **Description:** Executes a currency swap.
* **Request Body:**
```json
{
  "from": "currency_code",
  "to": "currency_code",
  "amount": 100
}
```
* **Response Body:**
```json
{
  "success": "Currency swap successful"
}
```


### 13. P2P Transfer

* **Endpoint:** `/user/p2p`
* **Method:** `GET`
* **Description:** Retrieves the user's P2P transfer history.
* **Request Body:** None
* **Response Body:**
```json
{
  "pageTitle": "P2P Transfer",
  "p2plog": []
}
```


### 14. P2P Transfer Post

* **Endpoint:** `/user/p2p`
* **Method:** `POST`
* **Description:** Executes a P2P transfer.
* **Request Body:**
```json
{
  "pin": "transaction_password",
  "recipient": "recipient_username_or_email",
  "amount": 100,
  "wallet": "balance" or "ref_wallet"
}
```
* **Response Body:**
```json
{
  "success": "P2P fund transfer completed successfully"
}
```


### 15. Login Earn

* **Endpoint:** `/user/login-earn`
* **Method:** `POST`
* **Description:** Claims the daily login bonus.
* **Request Body:** None.
* **Response Body (Success):**
```json
{
    "success": "Daily income earned successfully. Please come back tomorrow to earn more."
}
```

* **Response Body (Error):**
```json

{
    "error": "Sorry, you can’t earn daily passive income at the moment. Come back tomorrow." // Or other error message
}
```



### 16. Withdraw Money (GET)

* **Endpoint:** `/user/withdraw`
* **Method:** `GET`
* **Description:** Retrieves available withdrawal methods and the user's withdrawal history.
* **Request Body:** None
* **Response Body:**
```json
{
    "pageTitle": "Withdraw Money",
    "withdrawMethod": [],
    "withdraws": []
}
```

### 17. Withdraw Money (POST)

* **Endpoint:** `/user/withdraw`
* **Method:** `POST`
* **Description:** Creates a new withdrawal request.
* **Request Body:**
```json
{
    "methodId": { "id": withdraw_method_id }, 
    "amount": 100
}
```
* **Response Body:**
```json
{
    "success": "Withdrawal request created successfully. Proceed to preview."
}
```


### 18. Withdraw Preview

* **Endpoint:** `/user/withdraw/preview`
* **Method:** `GET`
* **Description:** Retrieves the details of a pending withdrawal request.
* **Request Body:** None (Uses session data for the withdrawal TRX).
* **Response Body:**
```json
{
    "pageTitle": "Withdraw Preview",
    "withdraw": {
        // Withdrawal details
    }
}

```



### 19. Withdraw Submit

* **Endpoint:** `/user/withdraw/submit`
* **Method:** `POST`
* **Description:** Submits a withdrawal request for processing.
* **Request Body:**  Depends on the chosen withdrawal method. May include fields like account number, wallet address, etc.
* **Response Body:**
```json
{
    "success": "Withdraw request sent successfully"
}
```



### 20. Gift Card History

* **Endpoint:** `/user/giftcard/history`
* **Method:** `GET`
* **Description:** Retrieves the user's gift card purchase history.
* **Request Body:** None
* **Response Body:**
```json
{
    "pageTitle": "Giftcard History",
    "log": []
}
```



### 21. Gift Card Details

* **Endpoint:** `/user/giftcard/details/{id}`
* **Method:** `GET`
* **Description:** Retrieves the details of a specific gift card purchase.
* **Request Body:** None.
* **Response Body:**
```json
{
  "pageTitle": "Giftcard Details",
  "card": {}
}
```



### 22. Gift Card Redeem Code

* **Endpoint:** `/user/giftcard/redeem/{id}`
* **Method:** `GET`
* **Description:** Retrieves the redemption code for a specific gift card.
* **Request Body:** None.
* **Response Body:**
```json
{
    "code": 200,
    "status": "ok",
    "data": {} // Gift card details including redemption code
}
```




### 23. Deposit History

* **Endpoint:** `/user/deposit/history`
* **Method:** `GET`
* **Description:** Retrieves the user's deposit history.
* **Request Body:** None
* **Response Body:**

```json
{
  "pageTitle": "Deposit History",
  "gatewayCurrency": [],
  "deposits": []
}
```



### 24. Show 2FA Form

* **Endpoint:** `/user/2fa/form`
* **Method:** `GET`
* **Description:** Displays the 2FA setup form with the QR code and secret key.
* **Request Body:** None
* **Response Body:**

```json
{
  "pageTitle": "2FA Setting",
  "secret": "your_secret_key",
  "qrCodeUrl": "qr_code_url"
}
```




### 25. Create 2FA

* **Endpoint:** `/user/2fa/create`
* **Method:** `POST`
* **Description:** Enables 2FA for the user.
* **Request Body:**
```json
{
  "key": "your_secret_key",
  "code": "google_authenticator_code"
}
```
* **Response Body:**
```json
{
  "success": "Google authenticator activated successfully"
}
```



### 26. Disable 2FA

* **Endpoint:** `/user/2fa/disable`
* **Method:** `POST`
* **Description:** Disables 2FA for the user.
* **Request Body:**
```json
{
  "code": "google_authenticator_code"
}
```
* **Response Body:**
```json
{
  "success": "Two-factor authenticator deactivated successfully"
}
```



### 27. Transactions History

* **Endpoint:** `/user/transactions`
* **Method:** `GET`
* **Description:**  Retrieves the user's transaction history. Supports filtering by `trx_type` and `remark`.
* **Request Body:**  (Optional query parameters for filtering)
```
trx_type=+/-   // Filter by transaction type (credit/debit)
remark=deposit/withdrawal/etc. // Filter by transaction remark
```

* **Response Body:**

```json
{
 "pageTitle": "Transactions",
 "transactions": [],
 "remarks": []
}
```



### 28. Download Attachment

* **Endpoint:** `/user/attachment/{fileHash}`
* **Method:** `GET`
* **Description:** Downloads an attachment file.
* **Request Body:** None.
* **Response Body:** File download.


### 29. User Data (GET)

* **Endpoint:** `/user/data`
* **Method:** `GET`
* **Description:** Retrieves user data for profile completion. Redirects to `/user/home` if profile is already complete.
* **Request Body:** None
* **Response Body:**
```json
{
  "pageTitle": "Update User Details",
  "user": {} 
}
```

### 30. User Data (POST)

* **Endpoint:** `/user/data`
* **Method:** `POST`
* **Description:** Submits user data to complete the profile. Redirects to `/user/home` on successful submission.
* **Request Body:**
```json
{
    "firstname": "John",
    "lastname": "Doe",
    "image": "user_image_file",
    "address": "street address",
    "state": "state",
    "zip": "zip code",
    "city": "city"
}
```
* **Response Body:**
```json
{
    "success": "Registration process completed successfully",
    "redirect": "user.home"
}
```


### 31. Transaction Password Verification

* **Endpoint:** `/user/trxpass`
* **Method:** `POST`
* **Description:** Verifies the user's transaction password.
* **Request Body:**
```json
{
    "password": "user_transaction_password"
}
```
* **Response Body:**
```json
{
    "ok": true,
    "status": "success",
    "message": "The password is correct!"
}
```
or
```json
{
    "ok": false,
    "status": "danger",
    "message": "The transaction password doesn't match!"
}
```



### 32. Validate Username

* **Endpoint:** `/user/validateusername`
* **Method:** `POST`
* **Description:** Validates a given username against existing users.  This endpoint does *not* require authentication.
* **Request Body:**
```json
{
  "account": "username"
}
```
* **Response Body (if username exists):**
```json
{
  "ok": true,
  "status": "success",
  "message": "User Full Name",
  "image": "user_profile_image_url"
}

```

* **Response Body (if username does not exist):**
```json
{
  "ok": false,
  "status": "danger",
  "message": "Invalid Username!"
}
```

```


```markdown
# Profile API Documentation

This document details the API endpoints for managing user profiles. All endpoints require authentication using a bearer token in the `Authorization` header.

## Endpoints

### 1. Get Downlines

* **Endpoint:** `/profile/downlines`
* **Method:** `GET`
* **Description:** Retrieves the authenticated user's downlines (referrals) and related referral bonus transactions.
* **Request Body:** None
* **Response Body:**
```json
{
  "status": "success",
  "data": {
    "referrals": { // Paginated list of referrals
        "data": [
            // Array of User objects representing the referrals
        ],
        // ...pagination data
    },
    "transactions": { // Paginated list of referral bonus transactions
        "data": [
            // Array of Transaction objects 
        ],
        // ...pagination data
    }
  }
}
```

### 2. Get Profile Details

* **Endpoint:** `/profile/details`
* **Method:** `GET`
* **Description:** Retrieves the authenticated user's profile details, including 2FA setup information.
* **Request Body:** None
* **Response Body:**
```json
{
    "status": "success",
    "data": {
        "user": {
            // User object with profile information (id, username, email, firstname, lastname, etc.)
        },
        "countries": [
            // Array of country objects (name, code)
        ],
        "2fa": {
            "secret": "your_secret_key", // For 2FA setup
            "qr_code_url": "qr_code_url"   // QR code URL for 2FA setup
        }
    }
}
```

### 3. Update Profile

* **Endpoint:** `/profile/update`
* **Method:** `POST`
* **Description:** Updates the authenticated user's profile information.
* **Request Body:**
```json
{
    "firstname": "John", // Optional
    "lastname": "Doe",    // Optional
    "image": "image_file",  // Optional. File upload
    "gender": "male/female", // Optional
    "dob": "YYYY-MM-DD",   // Optional
    "address": "123 Main St", // Optional
    "state": "CA",          // Optional
    "zip": "90210",        // Optional
    "city": "Los Angeles",  // Optional
    "country": "US",        // Optional
    "sn": true/false,       // Optional. SMS notifications
    "en": true/false        // Optional. Email notifications
}
```
* **Response Body:**
```json
{
    "status": "success",
    "message": "Profile updated successfully", // Or a specific message based on what was updated
    "data": {
        // Updated User object
    }
}
```


### 4. Update Password

* **Endpoint:** `/profile/password/update`
* **Method:** `POST`
* **Description:** Updates the authenticated user's password.
* **Request Body:**
```json
{
    "current_password": "current_password",
    "password": "new_password",
    "password_confirmation": "new_password"
}
```
* **Response Body:**
```json
{
    "status": "success",
    "message": "Password updated successfully"
}
```


### 5. Update Transaction PIN

* **Endpoint:** `/profile/pin/update`
* **Method:** `POST`
* **Description:** Updates the authenticated user's transaction PIN.
* **Request Body:**
```json
{
  "password": "account_password", // Required for verification
  "pin": "new_transaction_pin"
}
```
* **Response Body:**
```json
{
  "status": "success",
  "message": "Transaction PIN updated successfully"
}
```


### 6. Deactivate Account

* **Endpoint:** `/profile/deactivate`
* **Method:** `POST`
* **Description:** Deactivates the authenticated user's account.
* **Request Body:**
```json
{
    "password": "account_password" // Required for verification
}
```
* **Response Body:**
```json
{
    "status": "success",
    "message": "Account deactivated successfully"
}
```

```











## Deposit Methods Endpoint

**Endpoint:** `/deposit/methods`

**PHP Route:** `Route::get('deposit/methods', 'Api\PaymentController@depositMethods');`

**Request:**

* **Method:** GET

**Response (Success - 200):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:** (Example)

```json
{
    "code": 200,
    "status": "ok",
    "message": {  // "message" for success and "error" for errors is recommended for consistency
        "success": [ // Better to just have the message string directly
            "Payment Methods"
        ]
    },
    "data": {
        "methods": [
            {
		"id": 1,
		"method_code": 109,
		"currency": "NGN"
                "method": {
                    "name": "FlutterWave",
                    "image": "paypal.png",
                    // ... other method details
                },
                 // ... other gateway currency details
            },
            // ... more methods
        ],
        "image_path": "http://example.com/assets/images/gateway" // Example image path
    }
}

```

**Explanation:** This endpoint retrieves the available deposit methods and their associated currencies, along with the base image path for gateway icons.


## Deposit Insert Endpoint

**Endpoint:** `/deposit/insert`

**PHP Route:** `Route::post('deposit/insert', 'Api\PaymentController@depositInsert');`

**Request:**

* **Method:** POST
* **Headers:**
    * `Content-Type: application/json`
    * `Authorization: Bearer <access_token>`
* **Body:**

```json
{
    "amount": 100,
    "method_code": 109,
    "currency": "NGN"
}
```

**Response (Success - 202 Accepted):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:** (Example)

```json
{
    "code": 202,
    "status": "created",  //  "created" or "accepted"
    "message": {
        "success": [
            "Deposit Created"
        ]
    },
    "data": {
        "deposit": {
            "trx": "unique_transaction_id",  // Example
            "amount": 100,
            // ... other deposit details
        }
    }
}
```

**Response (Validation Error or Invalid Gateway - 200):** *(Should be 422 or 400)*

* **Headers:**
    * `Content-Type: application/json`
* **Body:** (Example validation error)

```json
{
    "code": 200, // Should be 422 or 400
    "status": "ok", // Should be "error"
    "message": {
        "error": [
            "The amount field is required."  // Example
        ]
    }
}

```

**Response (Deposit Limit Error - 200):** *(Should be 400 Bad Request)*

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 200, // Should be 400
    "status": "ok", // Should be "error"
    "message": {
        "error": [
            "Please follow deposit limit"
        ]
    }
}
```


**Explanation:**  This endpoint initiates a deposit request.  It validates the request, checks for deposit limits, calculates charges, and creates a new deposit record.



## Deposit Confirm Endpoint


**Endpoint:** `/deposit/confirm`

**PHP Route:** `Route::get('deposit/confirm', 'Api\PaymentController@depositConfirm');` (Could also be POST)


**Request:**

* **Method:** GET (or POST)
* **Headers:**
 * `Content-Type: application/json`
* **Query Parameters (or Request Body if POST):**
    * `transaction`: The unique transaction ID (trx)
```json
{
  "transaction": "B66OGQJN473R"
}

```


**Response (Success - 200 OK):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:** (The structure will depend on the payment gateway's process method)


```json
{
    "code": 200,
    "status": "ok",
    "data": {
        "gateway_data": {
            "redirect_url": "https://payment-gateway.com/pay",  // Example
            // ... other gateway-specific data
        }
    }
}

```



**Response (Deposit Not Found - 404):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 404,
    "status": "error",
    "message": {
        "error": [
            "Deposit not found"
        ]
    }
}

```

**Response (Validation Error - 200):** *(Should be 422)*
* **Headers:**
 * `Content-Type: application/json`
* **Body:** (Example)
```json
{
    "code": 200, // Should be 422
    "status": "ok",  // Should be "error"
    "message": {
        "error": [
            "The transaction field is required."
        ]
    }
}
```




**Explanation:** This endpoint confirms the deposit and interacts with the chosen payment gateway to proceed with the payment process. The response may contain a redirect URL or other gateway-specific data.




## Manual Deposit Confirm Endpoint


**Endpoint:** `/deposit/manual/confirm`

**PHP Route:** `Route::get('deposit/manual/confirm', 'Api\PaymentController@manualDepositConfirm');` (Or POST)


**Request:**

* **Method:** GET (or POST)
* **Headers:**
 * `Content-Type: application/json`
* **Query Parameter (or request body):**
    * `transaction`:  The transaction ID


**Response (Success - 200):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:** (Example)

```json
{
    "code": 200,
    "status": "ok",
    "message": {
        "success": [
            "Manual payment details"
        ]
    },
    "data": {
        "deposit": {
             // ... deposit details
        },
        "payment_method": {
            // ... payment method details
        }
    }
}
```


**Response (Deposit Not Found or Validation Error):** (Structures similar to previous examples—404 for not found, 422 for validation errors.)


**Explanation:** This endpoint retrieves details for manual deposit confirmation, providing information about the deposit and the chosen manual payment method.



## Manual Deposit Update Endpoint


**Endpoint:** `/deposit/manual/update`

**PHP Route:** `Route::post('deposit/manual/update', 'Api\PaymentController@manualDepositUpdate');`

**Request:**

* **Method:** POST
* **Headers:**
    * `Content-Type: application/json` (or `multipart/form-data` if file uploads are involved)
    * `Authorization: Bearer <access_token>`

* **Body:** (The body will depend on the `gateway_parameter` for the manual method.)  It might include text fields, file uploads, etc.  Example:

```json
{
    "transaction": "trx_id",
    "payment_reference": "ref123",  // Example custom field
    "receipt": "image_file.jpg"  // Example file upload field
}

```



**Response (Success - 200 OK):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:**
```json
{
    "code": 200,
    "status": "ok",
    "message": {
        "success": [  // Or just return the message string directly
            "Deposit request sent successfully"
        ]
    }
}

```

**Response (Deposit Not Found or Validation Errors):** (Structures similar to previous examples - 404 or 422)




**Explanation:** This endpoint updates the manual deposit request with the provided details (which might include uploaded files).  It marks the deposit status as pending and sends notifications.



```json
```



## Countries Endpoint

**Endpoint:** `/countries` (Suggest adding an appropriate prefix like `/basic/countries`)

**PHP Route:** `Route::get('countries', 'Api\BasicController@countries');`

**Request:**

* **Method:** GET

**Response (Success - 200):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:** (Example – the actual content will depend on the `country.json` file)

```json
[
    {
        "country": "United States",
        "dial_code": "+1",
        "code": "US"
    },
    {
        "country": "Canada",
        "dial_code": "+1",
        "code": "CA"
    },
    // ... more countries
]
```



## General Setting Endpoint

**Endpoint:** `/general-setting` (Suggest prefixing, e.g., `/basic/general-setting`)

**PHP Route:** `Route::get('general-setting', 'Api\BasicController@generalSetting');`

**Request:**

* **Method:** GET

**Response (Success - 200):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:** (Example – the structure will reflect your `GeneralSetting` model)


```json
{
    "code": 200,
    "status": "ok",
    "message": {
        "success": [
            "General setting data"
        ]
    },
    "data": {
        "general_setting": {
            "site_name": "Example Site",
            // ... other general settings
        }
    }
}
```

## Unauthenticated Endpoint  (This seems like a helper function, not a real endpoint)

This function likely doesn't represent a real API endpoint, but rather a helper function to return a standardized unauthorized response.  If it *is* an endpoint, it would typically be triggered by an authentication middleware when a user tries to access a protected resource without a valid token.

**Example Usage (in middleware):**

```php
return response()->json($this->unauthenticate());
```



## Languages Endpoint

**Endpoint:** `/languages`  (Suggest prefix, e.g., `/basic/languages`)

**PHP Route:** `Route::get('languages', 'Api\BasicController@languages');`

**Request:**

* **Method:** GET

**Response (Success - 200):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:** (Example)


```json
{
    "code": 200,
    "status": "ok",
    "data": {
        "languages": [
            {
                "id": 1,
                "name": "English",
                "code": "en",
                // ... other language details
            },
            {
                "id": 2,
                "name": "Spanish",
                "code": "es",
                // ... other language details
             }
            // ... more languages
        ],
        "image_path": "http://example.com/assets/images/lang" // example
    }
}

```



## Language Data Endpoint

**Endpoint:** `/language/{code}` (Suggest prefix, e.g., `/basic/language/{code}`)

**PHP Route:** `Route::get('language/{code}', 'Api\BasicController@languageData');`


**Request:**

* **Method:** GET
* **Path Parameter:**
    * `code`: The language code (e.g., "en", "es")


**Response (Success - 200):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:** (The structure will depend on the content of the language JSON file.)

```json
{
    "code": 200,
    "status": "ok",
    "message": {  // or data: {} would be more conventional
        "language_data": {
            "key1": "Translated Value 1",
            "key2": "Translated Value 2",
            // ... more translated strings
        }
    }
}
```


**Response (Language Not Found - 404):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "code": 404,
    "status": "error",
    "message": {
        "error": [
            "Language not found"
        ]
    }
}

```












######NOT Guaranteed to work










## Withdraw Methods Endpoint

**Endpoint:** `/user/withdraw/methods`

**PHP Route:** `Route::get('user/withdraw/methods', 'Api\UserController@withdrawMethods');`

**Request:**

* **Method:** GET
* **Headers:**
    * `Authorization: Bearer <access_token>`

**Response (Success - 200):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:** (Example)

```json
{
    "code": 200,
    "status": "ok",
    "message": {
        "success": [  // Better to just use the message string directly
            "Withdraw methods"
        ]
    },
    "data": {
        "methods": [
            {
                "id": 1,
                "name": "Bank Transfer",
                "image": "bank.png", // Example
                // ... other method details
            },
            // ... more methods
        ],
        "image_path": "http://example.com/assets/images/withdraw/method" // Example
    }
}
```


**Explanation:** This endpoint retrieves the available withdrawal methods.



## Withdraw Store Endpoint

**Endpoint:** `/user/withdraw/store`

**PHP Route:** `Route::post('user/withdraw/store', 'Api\UserController@withdrawStore');`

**Request:**

* **Method:** POST
* **Headers:**
    * `Authorization: Bearer <access_token>`
    * `Content-Type: application/json`
* **Body:**

```json
{
    "method_code": 1, // ID of the withdrawal method
    "amount": 200
}

```

**Response (Success - 202):**
* **Headers:**
    * `Content-Type: application/json`
* **Body:** (Example)
```json
{
    "code": 202,
    "status": "created",
    "message": {
        "success": [
            "Withdraw request stored successfully"
        ]
    },
    "data": {
        "trx": "withdraw_trx_id",
        // ... other withdraw details
    }
}
```


**Response (Method Not Found - 404):** (Structure similar to previous examples)

**Response (Insufficient Balance, Amount Limit Errors, Validation Errors - 200):** *(Should be 400 or 422)*

* **Headers:**
    * `Content-Type: application/json`
* **Body:** (Example: Insufficient Balance)

```json
{
    "code": 200,  // Should be 400
    "status": "ok", // Should be "error"
    "message": {
        "error": [
            "You do not have sufficient balance for withdraw."
        ]
    }
}
```


**Explanation:**  This endpoint creates a withdrawal request.  It performs validation checks (amount limits, balance, etc.) before storing the request.


## Withdraw Confirm Endpoint

**Endpoint:** `/user/withdraw/confirm`

**PHP Route:** `Route::post('user/withdraw/confirm', 'Api\UserController@withdrawConfirm');`

**Request:**

* **Method:** POST
* **Headers:**
    * `Authorization: Bearer <access_token>`
    * `Content-Type: application/json` (or `multipart/form-data`)
* **Body:**  (The request body will vary depending on the chosen withdrawal method and its required user data. It may include text fields, file uploads, or 2FA codes). Example:

```json
{
    "transaction": "unique_transaction_id",
    "bank_account_number": "1234567890", // Example custom field
    "swift_code": "ABCD1234",          // Example custom field
    "authenticator_code": "123456"    // if 2FA enabled
}

```


**Response (Success - 200):**
* **Headers:**
    * `Content-Type: application/json`
* **Body:**
```json
{
    "code": 200,
    "status": "ok",
    "message": {
        "success": [
            "Withdraw request sent successfully"
        ]
    }
}
```

**Response (Withdraw Not Found, Validation Error, Wrong 2FA, Insufficient Balance - 200):** *(Use appropriate 4xx status codes)*


**Explanation:** This endpoint confirms a withdrawal request. It handles additional user input based on the withdrawal method and processes the request, including 2FA verification if enabled.




## Withdraw Log Endpoint

**Endpoint:** `/user/withdraw/log`

**PHP Route:** `Route::get('user/withdraw/log', 'Api\UserController@withdrawLog');`

**Request:**

* **Method:** GET
* **Headers:**
    * `Authorization: Bearer <access_token>`


**Response (Success - 200):** (Structure very similar to other paginated responses)



## Deposit History, Transactions, Orders, Cashbacks:

These endpoints follow similar patterns as the previous ones. They handle requests related to viewing deposit history, transactions, orders, and cashbacks.  Ensure consistent status codes and response structures.



## Cashback Balance

**Endpoint:** `/user/cashback/balance`

**PHP Route:** `Route::get('user/cashback/balance', 'Api\UserController@cashbackbalance');`

**Request:**

* **Method:** GET
* **Headers:**
    * `Authorization: Bearer <access_token>`

**Response (Success - 200):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:** (Example)
```json
{
    "code": 200,
    "status": "ok",
    "data": {
        "balance": 35.20,
    }
}
```



## Order Details Endpoint
**Endpoint:** `/user/order/{order_number}`

**PHP Route:** `Route::get('user/order/{order_number}', 'Api\UserController@orderDetails');`

**Request:**

* **Method:** GET
* **Headers:**
    * `Authorization: Bearer <access_token>`
* **Path Parameter:**
    * `order_number`: The unique order number.

**Response (Success - 200):**
(Structure very similar to other success responses)





## Airtime Endpoint

**Endpoint:** `/user/airtime` (Consider a more specific endpoint name like `/user/airtime/providers` or `/user/airtime/history`)

**PHP Route:** `Route::get('user/airtime', 'Api\User\AirtimeController@airtime');`

**Request:**

* **Method:** GET
* **Headers:**
    * `Authorization: Bearer <access_token>`

**Response (Success - 200 OK):**

* **Headers:**
    * `Content-Type: application/json`
* **Body:**

```json
{
    "networks": [
        {
            "name": "MTN",
            "logo": "mtn.png",
            "networkid": "mtn"
        },
        {
            "name": "AIRTEL",
            "logo": "airtel.jpeg",
            "networkid": "airtel"
        },
        {
            "name": "GLO",
            "logo": "glo.jpeg",
            "networkid": "glo"
        },
        {
            "name": "9MOBILE",
            "logo": "9mobile.jpeg",
            "networkid": "etisalat"
        }
    ],
    "airtimelog": {  // This will be the paginated airtime purchase history
        "current_page": 1,
        "data": [
            // ... airtime purchase records
        ],
        // ... pagination details
    }
}
```

**Explanation:** This endpoint returns a list of available airtime network providers and the user's airtime purchase history.  It would be helpful to include the `image_path` in the response for the network logos.



## Buy Airtime (Local) Endpoint

**Endpoint:** `/user/buy-airtime/local`

**PHP Route:** `Route::post('user/buy-airtime/local', 'Api\User\AirtimeController@buy_airtime_post_local');`

**Request:**

* **Method:** POST
* **Headers:**
    * `Authorization: Bearer <access_token>`
    * `Content-Type: application/json`
* **Body:**

```json
{
    "amount": 50,
    "operator": "mtn", // networkid
    "wallet": "main", // or other wallet type
    "phone": "1234567890",  // Phone number to recharge
    "password": "user_transaction_password"
}
```


**Responses:** *(All responses currently use a 200 status code, even for errors, which should be corrected.)*


**Response (Success - 200):** *(Should be 201 Created)*
* Body:
```json
{
    "ok": true,
    "status": "success",
    "message": "Transaction Was Successful",
    "orderid": "unique_order_id"
}
```

**Response (Incorrect Password, Insufficient Balance, Processing Error - 400):**

* **Body:** Example (insufficient balance)
```json
{
    "ok": false,
    "status": "danger",
    "message": "Insufficient wallet balance"
}
```

**Response (VTpass API Error - 400):**

* **Body:** (Example)
```json
{
    "ok": false,
    "status": "danger",
    "message": "VTpass error message" // More specific error messages from VTpass would be helpful.
}
```


**Explanation:**  This endpoint handles the purchase of airtime. It uses the `buy_airtime_post_vtpass` method internally, so the responses and explanations are combined.
