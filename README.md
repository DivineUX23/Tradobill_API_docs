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
    "methodId": #withdraw_method_id", #example 3
    "amount": 100
}
```
* **Response Body:**
```json
{
    "success": "Withdrawal request created successfully. Proceed to preview."
    "data" :
	{'trx' : "trx_string"}
}
```


### 18. Withdraw Preview

* **Endpoint:** `/user/withdraw/preview`
* **Method:** `GET`
* **Description:** Retrieves the details of a pending withdrawal request.
* **Request Body:**
 ```json
{
    "trx": "trx", #trx string code from previous response
}
```
.
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
* **Request Body:**  
 ```json
{
    "trx": "trx", #trx string code from previous response
}
```

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



# Betting API Documentation

This document details the API endpoints for betting functionalities.  All endpoints require authentication using a bearer token in the `Authorization` header.

## Endpoints


### 1. Get Wallet Details

* **Endpoint:** `/betting/wallet`
* **Method:** `GET`
* **Description:** Retrieves the authenticated user's betting wallet details, including available networks, betting history, and current balance.
* **Request Body:** None
* **Response Body:**
```json
{
  "status": "success",
  "data": {
    "networks": [
        // Array of betting networks. Structure depends on the 'betting.json' file
    ],
    "betting_history": {  // Paginated betting history
        "data": [
            // Array of Order objects representing betting transactions
        ],
        // ... pagination data
    },
    "user_balance": 100  // User's main wallet balance
  }
}

```

### 2. Verify Merchant

* **Endpoint:** `/betting/verify`
* **Method:** `POST`
* **Description:** Verifies a betting merchant/customer ID. Uses the Payscribe API for verification.
* **Request Body:**
```json
{
    "merchant": "merchant_code",  // Betting provider code (e.g., sportybet)
    "number": "customer_id"     // Customer ID with the merchant (User Id wiht the betting company)
}
```
* **Response Body (Success):**
```json
{
    "status": "success",
    "data": {
        "customer_name": "Customer Name"  // Retrieved from Payscribe API
    }
}
```
* **Response Body (Error):**
```json
{
    "status": "error",
    "message": "Invalid Customer ID" // Or other error messages from the Payscribe API
}
```


### 3. Fund Betting Wallet

* **Endpoint:** `/betting/fund`
* **Method:** `POST`
* **Description:** Funds the user's betting wallet using their main wallet balance.
* **Request Body:**
```json
{
    "password": "transaction_password", // User's transaction PIN
    "amount": 50, // Amount to fund
    "customer_name": "Customer Name", // Verified customer name from step 2
    "number": "customer_id",       //  Customer ID/account number with the betting merchant
    "company": "merchant_code"      // Betting provider code
}
```
* **Response Body (Success):**
```json
{
    "status": "success",
    "message": "Transaction successful",
    "data": {
        "transaction_id": "transaction_code",
        "amount": 50, // Funded amount
        "customer_name": "Customer Name",
        "new_balance": 40 // User's new main wallet balance after funding
    }
}
```
* **Response Body (Error):**  Possible error responses:
```json
{
    "status": "error",
    "message": "Invalid transaction password" // Incorrect transaction PIN
}
```
or
```json
{
    "status": "error",
    "message": "Insufficient wallet balance"
}

```
or
```json
{
 "status": "error",
 "message": "Error message from Payscribe API" //  E.g., if the funding request to Payscribe fails
}
```


# Crypto API Documentation

This document details the API endpoints for cryptocurrency functionalities. All endpoints require authentication using a bearer token in the `Authorization` header.  The "Crypto Exchange" feature must also be enabled in the admin settings.

## Endpoints

### 1. Get Crypto Rates

* **Endpoint:** `/crypto/rates`
* **Method:** `GET`
* **Description:** Retrieves the current exchange rates for available cryptocurrencies.
* **Request Body:** None
* **Response Body (Success):**
```json
{
    "status": "success",
    "data": [
        // Array of Cryptocurrency objects with rate information
        {
            "id": 1,
            "name": "Bitcoin",
            "symbol": "BTC",
            "image": "btc.png",
            "buy_rate": 30000,  // Example buy rate
            "sell_rate": 29500, // Example sell rate
            // ... other cryptocurrency details
        },
        // ... other cryptocurrencies
    ]
}
```
* **Response Body (Error):**
```json
{
  "status": "error",
  "message": "Error message" // Details of the error
}
```

### 2. Get Crypto Currencies (Same as Rates)

* **Endpoint:** `/user/currencies`
* **Method:** `GET`
* **Description:** This endpoint appears to be identical to `/user/crypto/rates` and retrieves the same data.
* **Request Body:** None
* **Response Body:** Same as `/user/crypto/rates`


### 3. Get Crypto Wallet

* **Endpoint:** `/user/crypto/wallet/{id}`
* **Method:** `GET`
* **Description:** Retrieves the user's wallet for a specific cryptocurrency. If the wallet doesn't exist, a new one is created using the Coinremitter API.
* **Request Body:** None
* **Response Body (Success):**
```json
{
  "status": "success",
  "data": {
    "wallet": {
      "id": 1,
      "coin_id": 1, // Cryptocurrency ID
      "user_id": 1, // User ID
      "label": "user_label",  // Wallet label
      "address": "crypto_wallet_address",
      "qrcode": "qr_code_url", // QR code image URL
      "balance": 0, // Current wallet balance (in cryptocurrency)
      "status": 1 // Wallet status
    },
    "transactions": [
        // Array of Cryptotrx objects representing transactions for this wallet
        {
            // ... Transaction details
        }
    ],
    "coin": {
        // Cryptocurrency details (same structure as in /rates endpoint)
    }
  }
}
```

* **Response Body (Error):**
```json
{
  "status": "error",
  "message": "Error message" // Details of the error, e.g., if wallet creation fails.
}
```


### 4. Validate Crypto Wallet Address

* **Endpoint:** `/user/crypto/validate/{id}`
* **Method:** `POST`
* **Description:** Validates a cryptocurrency wallet address using the Coinremitter API.
* **Request Body:**
```json
{
  "address": "crypto_wallet_address"
}
```
* **Response Body (Success):**
```json
{
  "status": "success",
  "data": {
    "valid": true,  //  true if the address is valid, false otherwise
    // ... other data returned by Coinremitter
  }
}

```

* **Response Body (Error):**
```json
{
    "status": "error",
    "errors": { // Validation errors if the 'address' field is missing or invalid }
}
```
or
```json
{
  "status": "error",
  "message": "Error, Please check network" // If the Coinremitter API returns an error.
}
```



### 5. Calculate Exchange Rate

* **Endpoint:** `/user/crypto/exchange/{id}`
* **Method:** `POST`
* **Description:** Calculates the amount of cryptocurrency a user would receive for a given USD amount.
* **Request Body:**
```json
{
  "amount": 100 // Amount in USD
}
```

* **Response Body (Success):**
```json
{
    "status": "success",
    "data": {
        "fiat_amount": 100, // The USD amount provided in the request
        "crypto_amount": 0.0025, // Calculated crypto amount based on rates
        "fiat_symbol": "USD",
        "crypto_symbol": "BTC", // Example (will vary based on the coin)
        // ... other data from the Coinremitter API
    }
}
```


* **Response Body (Error):**

```json
{
    "status": "error",
    "errors": { // Validation errors (if any)}
}
```

or errors for invalid source wallet, or if the amount exceeds the min/max limits.



### 6. Swap Crypto for USD

* **Endpoint:** `/crypto/swap/{id}`
* **Method:** `POST`
* **Description:** Swaps a specified amount of cryptocurrency for USD, adding the USD to the user's main balance.
* **Request Body:**
```json
{
    "amount": 100,      // Amount in USD to swap
    "source": "wallet_address", // User's crypto wallet address
    "swappin": "transaction_password" // User's transaction PIN
}
```


* **Response Body (Success):**
```json
{
 "status": "success",
 "message": "Swap completed successfully",
 "data": {
     "amount_swapped": 0.0025,  // Amount of crypto swapped
     "received_amount": 98,     //  USD amount received after swap (might be less than 'amount' due to swap fees or rates)
     "new_balance":  198         // User's updated main wallet balance
 }
}

```



* **Response Body (Error):** Can include validation errors, "Invalid transaction PIN," "Invalid Source Wallet," "Insufficient wallet balance," or errors from the Coinremitter API.





```
These API endpoints provide functionalities for managing cryptocurrencies, including retrieving rates, managing wallets, validating addresses, calculating exchange amounts, and swapping crypto for USD.  Be sure to handle potential errors appropriately in your application.





# Buy Crypto API Documentation

This document details the API endpoints for buying cryptocurrency. All endpoints require authentication using a bearer token in the `Authorization` header.  Additionally, the user must have completed KYC verification and the "Buy Crypto" feature must be enabled in the admin settings.

## Endpoints

### 1. Get Coin Details

* **Endpoint:** `/crypto/details`
* **Method:** `POST`
* **Description:** Retrieves details for a specific cryptocurrency, including its current rate and the platform's buy rate.
* **Request Body:**
```json
{
  "coin": "coin_id", // The ID of the cryptocurrency
  "amount": 100 // The amount of USD the user wants to spend (used for calculating crypto amount)
}
```

* **Response Body (Success):**
```json
{
    "status": "success",
    "data": {
        "rate": 0.000025, // The current market rate from Coinremitter (if crypto_auto is 1) or the platform's rate
        "our_rate": 0.000026  // The platform's buy rate for the cryptocurrency
    }
}
```
* **Response Body (Error):**
```json
{
    "status": "error",
    "errors": { // Validation errors (if any) }
}
```
or
```json
{
    "status": "error",
    "message": "Unable to calculate asset rate" // If fetching the rate from Coinremitter fails (and crypto_auto is 1)
}
```


### 2. Buy Crypto (Process)

* **Endpoint:** `/crypto/buy`
* **Method:** `POST`
* **Description:** Processes a cryptocurrency purchase.
* **Request Body:**
```json
{
  "pin": "user_transaction_pin",
  "coin": "coin_id",
  "amount": 100, // Amount in USD the user wants to spend
  "wallet": "main" or "referral" // Wallet to deduct funds from
}
```
* **Response Body (Success - Automated Purchase):**  If `crypto_auto` is enabled in the admin settings:
```json
{
  "status": "success",
  "data": {
    "coin": "0.0025 BTC",  // Amount of crypto purchased
    "usd": 100,              // USD equivalent
    "fiat": 100,             // Total fiat spent (including any fees - currently not implemented)
    "auto": true            // Indicates automated purchase
  }
}

```

* **Response Body (Success - Manual Purchase):** If `crypto_auto` is disabled:
```json
{
  "status": "success",
  "data": {
    "coin": {/* Coin details */},
    "trx": "transaction_code",     // Transaction ID
    "usd": 100,                   // USD equivalent
    "fiat": 102,                  // Total fiat spent (including fees if applicable)
    "auto": false                  // Indicates manual purchase
  }
}

```

* **Response Body (Error):**
```json
{
 "status": "error",
 "errors": { // Validation errors (if any) }
}
```

or other error responses like "Invalid transaction PIN" or "Insufficient wallet balance."



### 3. Confirm Manual Purchase

* **Endpoint:** `/crypto/confirm`
* **Method:** `POST`
* **Description:** Confirms a manual cryptocurrency purchase by providing wallet address and payment proof. This endpoint is used only if `crypto_auto` is *disabled*.
* **Request Body:**
```json
{
    "trx": "transaction_code", // The transaction ID returned from the /process endpoint
    "walletaddress": "user_crypto_wallet_address",
    "proof": "proof_of_payment_image" // File upload
}
```
* **Response Body (Success):**
```json
{
 "status": "success",
 "message": "Transaction submitted successfully"
}
```
* **Response Body (Error):**
```json
{
    "status": "error",
    "errors": { // Validation errors (if any) }  
}
```
or other server-side errors if the file upload fails or there's a problem updating the order.







# Sell Crypto API Documentation

This document details the API endpoints for selling cryptocurrency.  All endpoints require authentication using a bearer token in the `Authorization` header.  Additionally, KYC verification must be completed, and the "Sell Crypto" feature must be enabled in the admin settings.

## Endpoints


### 1. Get Coin Details (for Selling)

* **Endpoint:** `/sell/coin-details`
* **Method:** `POST`
* **Description:** Retrieves details for a specific cryptocurrency, including the current rate and platform's sell rate.
* **Request Body:**
```json
{
    "coin": "cryptocurrency_id",
    "amount": 1  // Amount of cryptocurrency to sell (used for rate calculation)
}
```

* **Response Body (Success):**
```json
{
  "ok": true,
  "status": "success",
  "message": "Asset Price Calculated",
  "currency": "BTC", // Cryptocurrency symbol
  "rate": 29500,    //  Current market rate from Coinremitter (if crypto_auto is 1) or the platform's rate
  "ourrate": 29000 // Platform's sell rate for the cryptocurrency
}

```

* **Response Body (Error if crypto_auto is 1 and rate lookup fails):**
```json
{
  "ok": false,
  "status": "error",
  "message": "Sorry, we cant calculate asset rate at the moment."
}

```



### 2. Sell Crypto (Process)

* **Endpoint:** `/sell/process`
* **Method:** `POST`
* **Description:**  Initiates the process of selling cryptocurrency.  The process can be automated (using Coinremitter) or manual, depending on the `crypto_auto` setting in the admin panel.
* **Request Body:**
```json
{
  "pin": "user_transaction_pin",
  "coin": "cryptocurrency_id",
  "amount": 0.0025, // Amount of cryptocurrency to sell, in crypto units
  "wallet": "main" // Wallet to credit the USD equivalent to (Currently only 'main' is supported in code)
}
```


* **Response Body (Success - Automated Sell,  crypto_auto = 1):**
```json
{
  "ok": true,
  "status": "success",
  "message": "Trade Invoice Created Successfully",
  "data": {  // Data from Coinremitter API
      "invoice_id": "invoice_code",  // Use this for checking invoice status
      // ... other Coinremitter data
  },
  "auto": true
}

```

* **Response Body (Success - Manual Sell, crypto_auto = 0):**
```json
{
  "ok": true,
  "status": "success",
  "message": "Trade Invoice Created Successfully",
  "coin": { /* Cryptocurrency details */ },  
  "trx": "transaction_code",  // Use this for manual confirmation
  "data": {/* Order details */},  
  "auto": false 
}
```

* **Response Body (Error from Coinremitter when crypto_auto = 1):**
```json
{
    "ok": false,
    "status": "error",
    "message": "Error message from Coinremitter"
}
```
* **Response Body (General Error):**
```json
{
    "ok": false,
    "status": "error",
    "message": "Error message" // Server-side or other errors
}

```


### 3. Check Sell Order Status (Automated)

* **Endpoint:** `/sell/confirm`
* **Method:** `POST`
* **Description:** Checks the status of an automated sell order (when `crypto_auto` is enabled).  Use the `invoice` ID from the `/sell-crypto/process` response.
* **Request Body:**

```json
{
 "coin": "crypto_symbol",  // E.g., BTC
 "invoice": "invoice_id_from_coinremitter"
}
```

* **Response Body (Success - Status updated):**

```json
{
 "ok": true,
 "status": "success",
 "message": "Invoice Is paid" //  Or other status messages from Coinremitter like "pending", "expired," etc.
}
```
* **Response Body (Error):**
```json
{
    "ok": false,
    "status": "error",
    "message": "Invoice Is unpaid"  // or any other message as per response.
}
```



### 4. Confirm Manual Sell Order

* **Endpoint:** `/sell/confirm-manual`
* **Method:** `POST`
* **Description:**  Confirms a manual sell order (when `crypto_auto` is disabled) by providing the transaction hash and payment proof.
* **Request Body:**

```json
{
    "trx": "transaction_code",   // The `trx` ID returned from the `/process` endpoint for manual orders
    "trxhash": "transaction_hash",  // The transaction hash from the blockchain
    "proof": "proof_of_payment_image" // File upload (optional)
}

```
* **Response Body:**

```json
{
  "notify": [
    {
      "success": "Transaction submitted successfully."
    }
  ]
}
```







# Internet/SME Data API Documentation

This document details the API endpoints for purchasing internet data plans. All endpoints require authentication using a bearer token in the `Authorization` header.

## Endpoints


### 1. Get Internet Data Operators

* **Endpoint:** `/internet/operators`
* **Method:** `GET`
* **Description:** Retrieves the available internet data operators.
* **Request Body:** None
* **Response Body:**

```json
{
  "status": true,
  "message": "Network Fetched",
  "data": {
    "code": "00", 
    "response": [
      {"name": "MTN"},
      {"name": "Airtel"},
      {"name": "Glo"},
      {"name": "9Mobile"}
    ]
  }
}

```



### 2. Get Operator Plans



* **Endpoint:** `/internet/operator-plans/{networkid}`
* **Method:** `GET`
* **Description:** Retrieves the available data plans for a specific operator using their Network ID.  Reads data from the `n3tdata.json` file.

* **Request Body:** None
* **Response Body (Success):**
```json
{
    "status": true,
    "message": "Plans retrieved successfully",
    "data": {
        "networkid": "1",  // The requested network ID
        "plans": [ // Array of plans for this network (from n3tdata.json).  Structure will depend on your json file
            {
                "network": "MTN",
                "plan_name": "Data Plan 1",
                "plan_id": "mtn_plan_1",
                "data_amount": "1GB",
                "validity": "30 Days",
                "price": 1000  
            },
            // ... other plans for the requested network
        ]
    }
}

```
* **Response Body (Network Not Found):**

```json
{
    "status": false,
    "message": "Network not found"
}
```




### 3. Get Internet Packages and Transaction History

* **Endpoint:** `/internet/packages`
* **Method:** `GET`
* **Description:** Retrieves the available internet packages (same as `/internet/operators`) and the authenticated user's internet purchase history.
* **Request Body:** None

* **Response Body:**
```json
{
 "status": true,
 "message": "Internet packages retrieved successfully",
 "data": {
     "networks": [
             {"name": "MTN", "logo":"mtn.png", "networkid": "1"},
             {"name": "AIRTEL", "logo":"airtel.jpeg", "networkid":"2"},
             {"name": "GLO", "logo":"glo.jpeg", "networkid":"3"},
             {"name": "9MOBILE", "logo":"9mobile.jpeg", "networkid":"4"}
     ],
     "transactions": { // Paginated transaction history.
         "data": [
             // ... Array of Order objects for internet data purchases.
         ],
         // ... pagination info
     }
 }
}
```



### 4. Purchase Internet Data

* **Endpoint:** `/internet/purchase`
* **Method:** `POST`
* **Description:** Purchases an internet data plan using the N3T Data API.
* **Request Body:**
```json
{
  "password": "user_transaction_pin",
  "amount": 1500,           // Price of the data plan
  "data_plan": "plan_id",    //  The ID of the data plan (from the /operator-plans response).
  "networkname": "MTN",      // The name of the network. Important: should match the network in n3tdata.json
  "phone": "recipient_phone_number",
  "wallet": "main",          //  (Currently, only the 'main' wallet is used in the code.)
  "networkid": "1"            // Network ID. Important: should match the network IDs in the $networks array of the getOperatorPlans method.
}

```



* **Response Body (Success):**

```json
{
    "status": true,
    "message": "Successful", // Message from N3T Data API
    "data": {
        "order_id": "request_id",     // Request ID/Transaction ID
        "order_details": {/* ... full response from N3T Data API */} 
    }
}

```

* **Response Body (Error - Validation, Authentication, Balance):**
```json
{
  "status": false,
  "message": "Error message" //  Validation errors, incorrect password, or insufficient balance.
}
```



* **Response Body (Error - N3T Data API Issues):**
```json
{
 "status": false,
 "message": "Error message from N3T Data API"  // Includes the API's error message.
}
```











# Airtime API Documentation

This document details the API endpoints for purchasing airtime. All endpoints require authentication using a bearer token in the `Authorization` header.

## Endpoints

### 1. Get Available Networks

* **Endpoint:** `/airtime/networks`
* **Method:** `GET`
* **Description:** Retrieves the available airtime networks.
* **Request Body:** None
* **Response Body:**
```json
{
  "status": "success",
  "data": [
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
  ]
}

```

### 2. Airtime Purchase History

* **Endpoint:** `/airtime/history`
* **Method:** `GET`
* **Description:** Retrieves the authenticated user's airtime purchase history.
* **Request Body:** None
* **Response Body:**
```json
{
    "status": "success",
    "data": { // Paginated data
        "data": [
            {
                // ... Order details for each airtime purchase
            }
        ],
        // ... pagination information (current_page, last_page, etc.)
    }
}
```



### 3. Purchase Airtime

* **Endpoint:** `/airtime/purchase`
* **Method:** `POST`
* **Description:** Purchases airtime for a specified phone number.  Integrates with the VTPass API.
* **Request Body:**
```json
{
  "password": "user_transaction_pin",
  "amount": 100,  // Amount of airtime to purchase
  "operator": "mtn", // Network ID (e.g., mtn, airtel, glo, etisalat)
  "wallet": "main",  // Wallet to deduct funds from (only 'main' wallet is currently handled in code)
  "phone": "phone_number"  //  Recipient's phone number
}
```
* **Response Body (Success):**
```json
{
    "status": "success",
    "message": "Transaction successful",
    "data": {
        "transaction_id": "unique_transaction_id"
    }
}

```

* **Response Body (Error - Invalid Transaction Password):**
```json
{
 "status": "error",
 "message": "Invalid transaction password"
}
```

* **Response Body (Error - Insufficient Balance):**
```json
{
  "status": "error",
  "message": "Insufficient wallet balance"
}

```


* **Response Body (Error - VTPass API Issues):**  The following error response structure is used for different VTPass-related issues:
```json
{
    "status": "error",
    "message": "Service temporarily unavailable"  // Or other VTPass-related error message.
}

```






# Cable TV API Documentation

This document details the API endpoints for Cable TV functionalities.  All endpoints require authentication using a bearer token in the `Authorization` header.

## Endpoints


### 1. Get Cable TV Operators/Packages

* **Endpoint:** `/cabletv/operators`
* **Method:** `POST`
* **Description:** Retrieves the available packages for a specific cable TV provider.  Uses the VTPass API.
* **Request Body:**
```json
{
  "decoder": "dstv" //  The cable TV provider code (e.g., dstv, gotv, startimes, showmax).  This field determines the provider.
}
```

* **Response Body (Success):**
```json
{
    "status": true,
    "message": "Network operators fetched successfully",
    "data": { // The 'content' field from the VTPass API response
        "status": true,
        "message": "Network Fetched",
        "image": "image_url",
        "content": [
            // ... Array of available packages for the chosen provider
            {
                "name": "Package Name",
                "variation_code": "package_code",
                "variation_amount": 1500,  // Price of the package
                // ... other package details
            }
        ]
    }
}

```

* **Response Body (Error):**
```json
{
  "status": false,
  "message": "Failed to fetch network operators",
  "error": "Error details" // More specific error information if available.
}

```



### 2. Get Cable TV Networks and Purchase History

* **Endpoint:** `/cabletv/buy`
* **Method:** `GET`
* **Description:** Retrieves the available cable TV networks and the user's purchase history.
* **Request Body:** None
* **Response Body (Success):**

```json
{
    "status": true,
    "message": "CableTV data retrieved successfully",
    "data": {
        "networks": [
            {"name": "dstv"},
            {"name": "gotv"},
            {"name": "startimes"},
            {"name": "showmax"}
        ],
        "history": {  // Paginated purchase history
            "data": [
                // Array of Order objects representing Cable TV purchases.
            ],
            // ... pagination details (current_page, last_page, etc.)
        }
    }
}

```

* **Response Body (Error):**
```json
{
    "status": false,
    "message": "Failed to retrieve CableTV data",
    "error": "Error details"  //  Specific error message
}
```



### 3. Verify Cable TV Account/Decoder Number

* **Endpoint:** `/cabletv/verify`
* **Method:** `POST`
* **Description:** Verifies a cable TV account or decoder number using the VTPass API.
* **Request Body:**
```json
{
 "decoder": "dstv", // Provider code
 "number": "decoder_number_or_account_id" //  User's decoder number or account ID.
}
```

* **Response Body (Success - Valid Number):**
```json
{
 "ok": true,
 "status": "success",
 "message": "Valid Decoder Number",
 "content": "Customer Name" // Customer's name associated with the account/decoder.
}
```

* **Response Body (Error - Invalid Number):**
```json
{
    "ok": false,
    "status": "danger",
    "message": "Invalid Decoder Number"
}
```

* **Response Body (Error):**
```json
{
    "status": false,
    "message": "Verification failed",
    "error": "Error details"  // Details of the error, if available.
}

```


### 4. Purchase Cable TV Subscription

* **Endpoint:** `/cabletv/purchase`
* **Method:** `POST`
* **Description:** Purchases a cable TV subscription using the VTPass API.
* **Request Body:**
```json
{
  "password": "user_transaction_pin",
  "plan": "variation_code|amount",  //  Package code and amount, separated by a pipe (|)
  "customername": "Customer Name", // Customer's name (from verification)
  "number": "decoder_number",    // Decoder number or account ID
  "wallet": "main",             // Funding wallet (currently 'main' is assumed in code)
  "decoder": "dstv"             // Provider code
}
```

* **Response Body (Success):**
```json
{
  "ok": true,
  "status": "success",
  "message": "Transaction Was Successful",
  "orderid": "transaction_id" 
}
```


* **Response Body (Error - Validation):**
```json
{
  "status": false,
  "message": "Validation failed",
  "errors": {
    // Validation errors
  }
}
```

* **Response Body (Error - Authentication or Balance):**
```json
{
  "status": false,
  "message": "Invalid transaction password"  // or "Insufficient wallet balance"
}
```



* **Response Body (Error - VTPass API Issues):** These errors will have slightly different formats depending on where the error occurred within the VTPass API interaction.  Example:
```json
{
    "ok": false,
    "status": "danger",
    "message": "Error details from VTPass or server side" 
}
```


### 5. Cable TV Purchase History (Duplicate)

* **Endpoint:** `/cabletv/history`
* **Method:** `GET`
* **Description:** This endpoint is a duplicate of the `/cabletv/buy` (GET) endpoint and serves the same function.
* **Request Body:** None
* **Response Body:** Same as `/cabletv/buy` (GET).  This redundancy should be removed.



```





# Education Bills API Documentation

This document details the API endpoints for purchasing educational services (e.g., WAEC, JAMB). All endpoints require authentication using a bearer token in the `Authorization` header.

## Endpoints

### 1. Get Education Services and Purchase History

* **Endpoint:** `/education/buy`
* **Method:** `GET`
* **Description:** Retrieves the available education services and the user's purchase history.
* **Request Body:** None
* **Response Body (Success):**
```json
{
    "status": true,
    "message": "Education bills data retrieved successfully",
    "data": {
        "networks": [
            {"name": "WAEC Registration", "code": "waec-registration"},
            {"name": "WAEC Result Pin Checker", "code": "waec"},
            {"name": "JAMB PIN Vending", "code": "jamb"}
        ],
        "history": { // Paginated purchase history
            "data": [
              // ... Order objects representing education purchases
            ],
            // ... pagination details
        }
    }
}

```

* **Response Body (Error):**
```json
{
  "status": false,
  "message": "Failed to retrieve education bills data",
  "error": "Error details" //  Specific error message
}
```



### 2. Get Education Service Operators/Packages

* **Endpoint:** `/education/operators`
* **Method:** `POST`
* **Description:** Retrieves the available packages or variations for a specific educational service using the VTPass API.
* **Request Body:**
```json
{
    "code": "service_code"  // E.g., "waec", "jamb", "waec-registration"
}

```

* **Response Body (Success):**
```json
{
 "status": true,
 "message": "Network Fetched",
 "image": "image_url",
 "content": [  // Variations or packages available for the service
     {
         "name": "Package name",
         "variation_code": "variation_code",  // Use this in the purchase request
         "variation_amount": 1000, // Price
         //... other variation details
     },
      // ... more variations
 ],
 "fee": "convenience_fee", // Convenience fee, if any
 "serviceid": "service_id" // Service ID
}
```



* **Response Body (Error - Validation):**
```json
{
    "status": false,
    "message": "Validation failed",
    "errors": {
        // ... validation errors
    }
}
```

* **Response Body (Error - VTPass API or Server):**
```json
{
  "status": false,
  "message": "Failed to fetch operators",
  "error": "Error details"
}
```




### 3. Verify JAMB Number

* **Endpoint:** `/education/verify`
* **Method:** `POST`
* **Description:** Verifies a JAMB registration number using the VTPass API.  Currently only supports JAMB verification.
* **Request Body:**

```json
{
  "verifyjamb": "jamb_registration_number" 
}

```



* **Response Body (Success - Valid Number):**
```json
{
 "ok": true,
 "status": "success",
 "message": "Valid Jamb Number",
 "content": "Customer Name"  //  (if available from VTPass)
}

```

* **Response Body (Error - Invalid Number):**
```json
{
  "ok": false,
  "status": "danger",
  "message": "Invalid Jamb Number"
}

```

* **Response Body (Error-Validation):**
```json
{
    "status": false,
    "message": "Validation failed",
    "errors": {
        // ... validation errors
    }
}
```



### 4. Purchase Education Service

* **Endpoint:** `/education/purchase`
* **Method:** `POST`
* **Description:** Purchases an education service (WAEC, JAMB, etc.) using the VTPass API.
* **Request Body:**

```json
{
    "password": "user_transaction_pin",
    "network": "network_code",  // E.g., waec, jamb, waec-registration
    "plan": "variation_code|amount", // The 'variation_code' from /operators and the price, separated by |
    "serviceid": "service_id"       //  The 'serviceID' from /operators 
}
```

* **Response Body (Success):**
```json
{
 "ok": true,
 "status": "success",
 "message": "Transaction Was Successfull",
 "orderid": "transaction_id"
}

```

* **Response Body (Error - Validation or Authentication):**
```json
{
 "status": false,
 "message": "Validation failed" // Or "Invalid transaction password"
 "errors": { // ...validation errors if applicable }
}
```
* **Response Body (Error - Insufficient Balance):**

```json
{
    "status": false,
    "message": "Insufficient wallet balance"
}
```


* **Response Body (Error - VTPass API Issues):**  Similar to CableTV, these responses can vary slightly:
```json
{
    "ok": false,
    "status": "danger",
    "message": "Error message from VTPass or the server"
}
```


### 5. Education Purchase History (Duplicate)

* **Endpoint:** `/education/history`
* **Method:** `GET`
* **Description:**  This is a duplicate of the `/education/buy` (GET) endpoint's history section. The `/education/buy` endpoint already returns purchase history, so this endpoint is redundant.
* **Request Body:** None
* **Response Body:**  Same as the `history` part of the `/education/buy` response. This redundancy should be removed.

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






# Gift Card API Documentation

This document details the API endpoints for buying and selling gift cards. All endpoints require authentication using a bearer token in the `Authorization` header, and users must have completed KYC verification.

## Endpoints

### 1. Get Available Gift Cards, Transactions, and Vouchers

* **Endpoint:** `/giftcards`
* **Method:** `GET`
* **Description:** Retrieves available gift cards, the user's gift card transaction history, and voucher information.
* **Request Body:** None
* **Response Body:**
```json
{
  "status": "success",
  "data": {
    "currencies": [
      // ... Array of available Giftcard objects
      {
          "id": 1,
          "name": "Amazon", // Gift card name
          "image": "amazon.png", // Gift card image
          // ...other gift card details
      }
    ],
    "transactions": { // Paginated transaction history
        "data": [
            // ... Array of Giftcardsale objects
            {
                // Details of each transaction
            }

        ],
        // ... pagination details
    },
    "vouchers": { // Paginated voucher information
        "data": [
            // Array of Voucher objects
        ],
        // ...pagination details
    }
  }
}
```

### 2. Get Gift Card Types

* **Endpoint:** `/giftcards/types`
* **Method:** `POST`
* **Description:** Retrieves the available types (e.g., denominations, regions) for a specific gift card.
* **Request Body:**
```json
{
    "card_id": "giftcard_id",
    "amount": 100  // Amount in USD for which to get types
}

```

* **Response Body (Success):**
```json
{
  "status": "success",
  "data": {
    "card": { // Giftcard details
        // ...
    },
    "types": [
      // ... Array of Giftcardtype objects for the selected card
        {
            "id": 1,
            "name": "$100 - USA", // Example gift card type
            "card_id": 1,       // Gift card this type belongs to
            "rate": 0.85,      // Exchange rate for selling
            "buy_rate": 0.9,  //  Exchange rate for buying
            // ... other type details
        }
    ],
    "amount": 100
  }
}
```

* **Response Body (Error - No Types Found):**
```json
{
  "status": "error",
  "message": "No giftcard types available for {card_name} at the moment."
}
```


### 3. Sell Gift Card

* **Endpoint:** `/giftcards/sell`
* **Method:** `POST`
* **Description:** Submits a request to sell a gift card.
* **Request Body:**

```json
{
    "card_id": "gift_card_id",
    "type_id": "gift_card_type_id",
    "amount": 100, // USD value of the gift card
    "card_type": "Physical" or "Digital",
    "code": "gift_card_code", // Required only if card_type is Digital
    "front_image": "front_image_file", // Required only if card_type is Physical. File upload.
    "back_image": "back_image_file"   // Required only if card_type is Physical. File upload.
}
```

* **Response Body:**
```json
{
  "status": "success",
  "message": "Giftcard Exchange Request Sent Successfully",
  "data": { // The created Giftcardsale object
     // ... details of the sale request
  }
}
```


### 4. Buy Gift Card

* **Endpoint:** `/giftcards/buy`
* **Method:** `POST`
* **Description:** Submits a request to buy a gift card.
* **Request Body:**
```json
{
    "card_id": "gift_card_id",
    "type_id": "gift_card_type_id",
    "amount": 100,
    "card_type": "Physical" or "Digital", // Not actually used in the current code for buying, but included in request
    "country" : "country_name", // Not actually used in the current code for buying, but included in request
    "currency": "currency_code"  // Not actually used in the current code for buying, but included in request
}
```
* **Response Body:**
```json
{
 "status": "success",
 "message": "Giftcard Purchase Request Successful",
 "data": {
     // ... Giftcardsale details
 }
}

```


### 5. Sell Gift Card History

* **Endpoint:** `/giftcards/history/sell`
* **Method:** `GET`
* **Description:** Retrieves the authenticated user's gift card selling history.
* **Request Body:** None

* **Response Body:**

```json
{
  "status": "success",
  "data": {   // Paginated data
      "data": [  // Array of Giftcardsale objects (sell transactions)
          // ...
      ],
      // ...pagination info
  }
}

```


### 6. Buy Gift Card History

* **Endpoint:** `/giftcards/history/buy`
* **Method:** `GET`
* **Description:** Retrieves the authenticated user's gift card buying history.
* **Request Body:** None
* **Response Body:**
```json
{
    "status": "success",
    "data": {  // Paginated data
        "data": [ // Array of Giftcardsale objects (buy transactions)
            // ...
        ],
        //... pagination info
    }
}

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




