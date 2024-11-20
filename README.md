# Tradobill_API_docs
Tradobill API doc

# NOTE: 

## ALL ENDPOINTS MUST HAVE THE FOLLOWING:

## 1. Request Headers:
```
Content-Type: application/json
Accept: application/json

```


# Endpoints, Request Bodies, Headers, and Responses

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


# Registration API Documentation

This document details the API endpoints for user registration.

## Endpoints


### 1. Register User

* **Endpoint:** `/user/register`
* **Method:** `POST`
* **Description:** Registers a new user. Sends a verification code to the user's email address.
* **Request Body:**
```json
{
    "email": "user_email@example.com",
    "mobile": "1234567890", // Without country code and mobile code
    "mobile_code": "+1",
    "password": "password",
    "password_confirmation": "password",
    "username": "username123",
    "country_code": "US",
    "country": "United States",
    "captcha": "captcha_response", // If captcha is enabled
    "agree": true // If terms and conditions are enabled
}
```

* **Response Body (Success - Verification Code Sent):**
```json
{
    "code": 200,
    "status": "ok",
    "message": {
        "success": "Verification code sent successfully"
    }
}
```

* **Response Body (Error - Validation):**
```json
{
    "code": 422,
    "status": "error",
    "message": {
        "error": [
            // ... validation error messages
        ]
    }
}
```
* **Response Body (Error - Email Send):**
```json
{
    "code": 500,
    "status": "error",
    "message": {
        "error": "Could not send verification email. Please check your email or contact support."
    }
}
```




### 2. Resend Verification Code

* **Endpoint:** `/user/resend-code`
* **Method:** `POST`
* **Description:** Resends the email verification code. This endpoint is typically used if the user didn't receive the initial verification email.
* **Request Body:**
```json
{
 "email": "user_email@example.com"
}
```


* **Response Body (Success - Verification Code Sent):**
```json
{
    "code": 200,
    "status": "ok",
    "message": {
        "success": "Verification code sent successfully"
    }
}
```

* **Response Body (Error - Validation):**
```json
{
    "code": 422,
    "status": "error",
    "message": {
        "error": [
            // ... validation error messages
        ]
    }
}
```
* **Response Body (Error - Email Send):**
```json
{
    "code": 500,
    "status": "error",
    "message": {
        "error": "Could not send verification email. Please check your email or contact support."
    }
}
```



### 3. Verify Email

* **Endpoint:** `/user/verify-email`
* **Method:** `POST`
* **Description:** Verifies the user's email address using the verification code.  If successful, marks the user's email as verified and logs them in.

* **Request Body:**
```json
{
 "email": "user_email@example.com",
 "ver_code": "123456" // Verification code received via email
}

```
* **Response Body (Success):**


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


* **Response Body (Error - Validation or Invalid Code):** Standard error responses as in other endpoints.

```

* **Response Body (Error - Validation):**
```json
{
    "code": 400,
    "status": "error",
    "message": {
        "error": "Invalid or expired verification code"
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



### 15. Withdraw History (GET)

* **Endpoint:** `/user/withdraw/logs`
* **Method:** `GET`
* **Description:** Retrieves user's withdrawal history.
* **Request Body:** None
* **Response Body:**
```json
{
    "withdraws": []
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

```json
{
    "status": "success",
    "data": {
        "pageTitle": "Transactions",
        "transactions": {
            "current_page": 1,
            "data": [],
            "first_page_url": "https://weekenddreams.tradobill.com/api/user/transactions?page=1",
            "from": null,
            "last_page": 1,
            "last_page_url": "https://weekenddreams.tradobill.com/api/user/transactions?page=1",
            "links": [
                {
                    "url": null,
                    "label": "&laquo; Previous",
                    "active": false
                },
                {
                    "url": "https://weekenddreams.tradobill.com/api/user/transactions?page=1",
                    "label": "1",
                    "active": true
                },
                {
                    "url": null,
                    "label": "Next &raquo;",
                    "active": false
                }
            ],
            "next_page_url": null,
            "path": "https://weekenddreams.tradobill.com/api/user/transactions",
            "per_page": 20,
            "prev_page_url": null,
            "to": null,
            "total": 0
        },
        "remarks": [
            {
                "remark": "airtime"
            },
        ]
    }
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
* **Description:** Submits user data to complete the profile. Redirects to `/user/home` on successful submission. - For users to compelte info after sign up a must
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
* **Method:** `PUT`
* **Description:** Updates the authenticated user's profile information.
* **Request Body:**
```json

{
  "firstname": "John", // User's first name (optional)
  "lastname": "Doe", // User's last name (optional)
  "mobile": "+1234567890", // User's mobile phone number (optional)
  "country_code": "US", // Country code (optional)
  "gender": "male", // Gender (optional, must be "male" or "female")
  "dob": "1990-01-01", // Date of birth (optional, must be a date before today)
  "image": "base64_encoded_image_string", // Profile image (optional, should be a base64-encoded image string if you're sending as JSON)
  "address": "123 Main St", // Address (optional)
  "state": "California", // State (optional)
  "city": "Los Angeles", // City (optional)
  "zip": "90001", // ZIP code (optional)
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







# (Withdrawal) Bank Transfer API Documentation

This document details the API endpoints for bank transfers. All endpoints require authentication using a bearer token in the `Authorization` header, and users must have completed KYC verification.

## Endpoints

### 1. Get Bank Transfer Summary

* **Endpoint:** `/bank-transfer/summary`
* **Method:** `GET`
* **Description:** Retrieves the authenticated user's bank transfer history and monthly summary data.
* **Request Body:** None
* **Response Body:**
```json
{
    "status": "success",
    "data": {
        "transfer_history": [
            // Array of Transaction objects representing bank transfers
        ],
        "monthly_totals": [
             0, // January total
             0, // February total
             // ... totals for other months
        ],
        "months": [
            "January", "February", "March",  // ... all month names
        ]
    }
}
```

### 2. Get Bank List

* **Endpoint:** `bank-transfer/banks`
* **Method:** `GET`
* **Description:** Retrieves a list of supported banks for bank transfers.  The source of bank data depends on the `transfer_provider` setting in the admin panel (Monnify or Strowallet).

* **Request Body:** None

* **Response Body (Success, Using banks.json file if Monnify provider):**
```json
{
 "status": "success",
 "data": [  // Data from banks.json
     {"name": "Access Bank", "code": "044"},
     // ... other bank details
 ]
}

```

* **Response Body (Success, using strowallet if Strowallet provider):**

```json
{
    "status": "success",
    "data": [  // Data from Strowallet API:
        {"bank_name": "Abbey Mortgage Bank", "bank_code": "801"},
        // ... other bank data
    ]
}
```


* **Response Body (Error, if Strowallet API request fails):**
```json
{
 "status": "error",
 "message": "Error fetching bank list"
}
```



### 3. Validate Bank Account

* **Endpoint:** `bank-transfer/validate-account`
* **Method:** `POST`
* **Description:** Validates a bank account number and retrieves the account holder's name. Uses either the Monnify or Strowallet API depending on the admin setting `transfer_provider`.

* **Request Body:**
```json
{
    "bankcode": "bank_code",
    "account": "account_number"
}
```

* **Response Body (Success - Strowallet):**
```json
{
 "status": "success",
 "data": {
     "account_name": "Account Holder Name",
     "session_id": "session_id_from_strowallet"  // Used for subsequent transfer requests
 }
}
```
* **Response Body (Success- Monnify):**
```json
{
    "status": "success",
    "data": {
        "account_name": "Account Holder Name"
        //, "details" : {} // Full monnify details
    }
}
```


* **Response Body (Error):**
```json
{
    "status": "error",
    "message": "Error: Invalid account details or other error message from API"
}
```

* **Response Body (Error - Validation):**  Standard Laravel validation error format.


### 4. Initiate Bank Transfer

* **Endpoint:** `bank-transfer/transfer`
* **Method:** `POST`
* **Description:** Initiates a bank transfer to the specified account.  Uses either Monnify or Strowallet, depending on admin settings.
* **Request Body:**
```json
{
 "bankcode": "bank_code",
 "account": "account_number",
 "amount": 10000, // Amount to transfer
 "narration": "Transfer Description",  // Narration or description for the transfer
 "account_name": "Recipient Name",     //  Account holder's name (from validation step)
 "bank_name": "Bank Name",           // Name of the recipient's bank
 "pin": "user_transaction_pin"    // User's transaction PIN
}

```
* **Response Body (Success):**
```json
{
  "status": "success",
  "message": "Transfer successful",
  "data": {
      "transaction_id": "unique_transaction_id"
  }
}

```

* **Response Body (Error - Validation, Authentication, or Insufficient Balance):** Standard error responses as in other endpoints.

* **Response Body (Error - Transfer API Issues - Monnify):**  
```json
{
 "status": "error",
 "message": "Error: Transfer failed or error message from monnify API"
}
```

* **Response Body (Error - Transfer API Issues - Strowallet):**
```json
{
    "status": "error",
    "message": "Error: Transfer failed or other error message from Strowallet API"
}
```






# Deposite Endpoints

### 1 Deposit Methods Endpoint

**Endpoint:** `/deposit/methods`
**Explanation:** This endpoint retrieves the available deposit methods and their associated currencies, along with the base image path for gateway icons.
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



### 2 Deposit Insert Endpoint

**Endpoint:** `/deposit/insert`

**Explanation:**  This endpoint initiates a deposit request.  It validates the request, checks for deposit limits, calculates charges, and creates a new deposit record.

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




### 3 Deposit Confirm Endpoint


**Endpoint:** `/deposit/confirm`
**Explanation:** This endpoint confirms the deposit and interacts with the chosen payment gateway to proceed with the payment process. The response may contain a redirect URL or other gateway-specific data.

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








### 4 Manual Deposit Confirm Endpoint


**Endpoint:** `/deposit/manual/confirm`

**Explanation:** This endpoint retrieves details for manual deposit confirmation, providing information about the deposit and the chosen manual payment method.

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




### 5 Manual Deposit Update Endpoint


**Endpoint:** `/deposit/manual/update`


**Explanation:** This endpoint updates the manual deposit request with the provided details (which might include uploaded files).  It marks the deposit status as pending and sends notifications.

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

* **Endpoint:** `/crypto/currencies`
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








# Utility (Local) API Documentation

This document details API endpoints for utility bill payments (e.g., electricity). All endpoints require authentication using a bearer token in the `Authorization` header.

## Endpoints


### 1. Get Utility Companies and Purchase History

* **Endpoint:** `utility/buy`
* **Method:** `GET`
* **Description:**  Retrieves the supported utility companies and the authenticated user's utility bill payment history.
* **Request Body:** None
* **Response Body:**
```json
{
    "status": "success",
    "networks": [
        {"name": "ikeja-electric"},
        {"name": "eko-electric"},
        // ... other utility companies
    ],
    "utility_logs": { // Paginated history
        "data": [
            // Array of Order objects representing utility bill payments
        ],
        // ... pagination details
    }
}

```


### 2. Verify Utility Meter Number

* **Endpoint:** `utility/verify`
* **Method:** `POST`
* **Description:** Verifies a utility meter number using the VTPass API.
* **Request Body:**
```json
{
 "metertype": "prepaid" or "postpaid", // Meter type
 "company": "utility_company_code",  // E.g., ikeja-electric, eko-electric, etc.
 "number": "meter_number"             // The meter number to verify.
}

```


* **Response Body (Success):**
```json
{
    "status": "success",
    "message": "Valid Decoder Number", // VTpass uses Decoder terminology even for electricity
    "customer_name": "Customer Name"  // Name associated with the meter number, if returned by VTPass.
}
```

* **Response Body (Error - Validation):**

```json
{
    "message": "The given data was invalid.", // Standard Laravel validation error
    "errors": {
        // ... specific validation errors
    }
}

```


* **Response Body (Error - Verification):**
```json
{
 "status": "error",
 "message": "Error validating meter number"
}
```



### 3. Pay Utility Bill



* **Endpoint:** `utility/purchase`
* **Method:** `POST`
* **Description:** Pays a utility bill using the VTPass API.
* **Request Body:**
```json
{
 "trx_password": "user_transaction_pin",
 "amount": 5000,  //  Bill amount
 "customername": "Customer Name", // Customer name from verification (if applicable)
 "number": "meter_number",
 "metertype": "prepaid" or "postpaid",
 "wallet": "main",  //  Assumed to be 'main' in current code. Not actively used in processing
 "company": "utility_company_code" //  E.g., ikeja-electric
}
```



* **Response Body (Success):**

```json
{
    "status": "success",
    "message": "Transaction Was Successful",
    "order_id": "transaction_id"
}
```

* **Response Body (Error - Validation or Authentication):**
```json

{
 "status": "error",
 "message": "The given data was invalid." // or "The password doesn't match!"
 "errors": { // ...validation errors if applicable }
}

```


* **Response Body (Error - Insufficient Balance):**
```json
{
    "status": "error",
    "message": "Insufficient wallet balance"
}

```



* **Response Body (Error - VTPass API Issues):**

```json
{
  "status": "error",
  "message": "We cant process this request at the moment",
  "error": {  //  Full VTPass API response for debugging
     // ...
  }
}
```








# Notification Preference API Documentation

This document details the API endpoints for managing user notification preferences. All endpoints require authentication using a bearer token in the `Authorization` header.

## Endpoints

### 1. Get Notification Preferences

* **Endpoint:** `/notification/preferences`
* **Method:** `GET`
* **Description:** Retrieves the notification preferences for the authenticated user.
* **Request Body:** None
* **Response Body (Success):**
```json
{
    "status": "success",
    "message": "Notification preferences retrieved successfully",
    "data": {
        "email_notifications": true, // Or false
        "sms_notifications": false,   // Or true
        // "push_notifications": true, //  If this field is present in your User model
        // "notification_settings": {  // If you use granular type-based settings (uncomment code if needed). The strucuture of these detailed settings would depend on your implemented logic. 
        //     // ... type-based settings (transaction, security, etc.)
        // }
    }
}
```

* **Response Body (Error):**
```json
{
 "status": "error",
 "message": "Failed to fetch notification preferences",
 "error": "Error details"
}

```


### 2. Update Notification Preferences

* **Endpoint:** `/notification/preferences`
* **Method:** `PUT`
* **Description:** Updates the notification preferences for the authenticated user.  You can update general preferences (`email_notifications`, `sms_notifications`) and/or detailed type-specific preferences (`notification_settings`).

* **Request Body:**
```json
{
    "email_notifications": true,   // Or false
    "sms_notifications": false,  //  Or true
    // "push_notifications": true, // If applicable
    // "notification_settings": {  // If you're using fine-grained settings per type (uncomment related code if needed)
    //     "transaction": {
    //         "email": true,
    //         "sms": false,
    //         "push": true
    //     },
    //     "security": {
    //         // ...
    //     },
    //     // ... other types
    // }
}

```


* **Response Body (Success):**

```json
{
  "status": "success",
  "message": "Notification preferences updated successfully",
  "data": {  // Updated preferences
       "email_notifications": true,
       "sms_notifications": false,
      //  "push_notifications": true,  // If applicable.
      //   "notification_settings": { //  If applicable
      //      // ... updated settings
      //   }
  }
}
```


* **Response Body (Error - Validation):**

```json
{
    "status": "error",
    "message": "Validation error",
    "errors": {
        // ... validation error messages
    }
}
```
* **Response Body (Error):**

```json
{
    "status": "error",
    "message": "Failed to update notification preferences",
    "error": {error_details}
}
```



### 3. Update Notification Type Settings

* **Endpoint:** `/notification/preferences/type`
* **Method:** ``PUT`
* **Description:** Updates the notification settings for a *specific* notification type (transaction, security, marketing, system).  This endpoint provides a way to manage granular notification settings.  The structure of `notification_settings` would be based on your implemented logic. The commented-out code in the controller suggests a possible structure.



* **Request Body:**
```json
{
    "type": "transaction",  // Or security, marketing, system.  Must be one of the defined types.
    "email": true,
    "sms": false,
   // "push": true // if applicable
}
```



* **Response Body (Success):**

```json
{
    "status": "success",
    "message": "Notification type settings updated successfully",
    "data": {
        "type": "transaction",  // The updated type
        "settings": {        //  The new settings for that type
            "email": true,
            "sms": false,
            //"push": true // if applicable
        }
    }
}

```

* **Response Body (Error - Validation):** Standard validation error response.


* **Response Body (Error):** Standard error response.









# Notification API Documentation

This document details the API endpoints for managing user notifications.  All endpoints except `/notifications/send` require authentication using a bearer token in the `Authorization` header. The `/notifications/send` endpoint is for admin use only and requires admin authentication. This version focuses on optimized data retrieval.

## Endpoints



### 0. Get Unread Notification Count

* **Endpoint:** `/notifications/unread-count`
* **Method:** `GET`
* **Description:** Retrieves the count of unread notifications for the authenticated user.
* **Request Body:** None
* **Response Body:**
```json
{
 "status": "success",
 "data": {
     "unread_count": 5  // Number of unread notifications
 }
}
```



### 1. Get Notifications List (Minimal Data)

* **Endpoint:** `/notifications`
* **Method:** `GET`
* **Description:** Retrieves a paginated list of notifications with minimal information (id, subject, type, created_at, read_status) for the authenticated user.  Also includes the unread notification count.
* **Request Body:** None
* **Response Body:**
```json
{
 "status": "success",
 "data": {
     "notifications": {  // Paginated list of notifications
         "data": [
             {
                "id": 1,
                "subject": "Notification Subject",
                "notification_type": "transaction", // Example type
                "created_at": "2024-07-28T10:00:00.000000Z",
                "read_status": 0 // 0 for unread, 1 for read
             },
             // ... more notifications
         ],
         // ... pagination data (current_page, last_page, per_page, etc.)
     },
     "unread_count": 3 // Total unread notifications for the user
 }
}
```


### 2. Get Notification Detail

* **Endpoint:** `/notifications/{id}`
* **Method:** `GET`
* **Description:** Retrieves the full details of a specific notification. Marks the notification as read if it was previously unread.
* **Request Body:** None (ID passed as a URL parameter)
* **Response Body (Success):**
```json
{
    "status": "success",
    "data": {
        "id": 123,
        "subject": "Notification Subject",
        "message": "Full notification message content.",
        "notification_type": "transaction",
        "sender": "system",  // Or user ID if sent by a user
        "sent_from": "sender email or identifier",
        "sent_to": "recipient email or identifier",
        "created_at": "2024-07-28T12:30:00.000000Z",
        "read_status": 1,
        "read_at": "2024-07-28T13:00:00.000000Z" // Timestamp when marked as read.
    }
}

```

* **Response Body (Error - Not Found):**
```json
{
  "status": "error",
  "message": "Notification not found"
}

```



### 3. Search Notifications (Minimal Data)

* **Endpoint:** `/notifications/search`
* **Method:** `GET`
* **Description:** Searches notifications (with minimal information) based on subject, date range, and notification type.
* **Request Body:** (Query Parameters)
```
search=keyword          // Required. Minimum 3 characters.
date_from=YYYY-MM-DD   // Optional. Start date.
date_to=YYYY-MM-DD     // Optional. End date.
type=notification_type  // Optional. Filter by notification type.
```

* **Response Body (Success):**

```json
{
    "status": "success",
    "data": { // Paginated search results
        "data": [
            // Array of notifications (minimal data) that match the search criteria.
        ],
        //... pagination data
    }
}
```
* **Response Body (Error - Validation):**

```json
{
    "status": "error",
    "message": "Validation error",
    "errors": {
        // ... validation errors
    }
}

```




### 4. Get Notification Types

* **Endpoint:** `/notifications/types`
* **Method:** `GET`
* **Description:** Retrieves the distinct notification types available for the authenticated user.  This can be used to populate filter options in the UI.
* **Request Body:** None
* **Response Body:**
```json
{
 "status": "success",
 "data": [
     "transaction",
     "deposit",
     "withdrawal",
     // ... other notification types
 ]
}
```


### 5. Mark Notification as Read


* **Endpoint:** `/notifications/mark-read`
* **Method:** `POST`
* **Description:**  Marks a specific notification as read.
* **Request Body:**
```json
{
    "notification_id": 2  // Id of the notification
}
```
* **Response Body (Success):**
```json
{
 "status": "success",
 "message": "Notification marked as read"
}
```

* **Response Body (Error - Validation):**
```json
{
    "status": "error",
    "message": "Validation error",
    "errors": {
        // ... validation error messages
    }
}
```

* **Response Body (Error - Not Found):**
```json
{
    "status": "error",
    "message": "Notification not found"
}
```



### 6. Mark All Notifications as Read

* **Endpoint:** `/notifications/mark-all-read`
* **Method:** `POST`
* **Description:** Marks all of the authenticated user's notifications as read.
* **Request Body:** None
* **Response Body:**

```json
{
    "status": "success",
    "message": "All notifications marked as read"
}
```

* **Response Body (Error):**

```json
{
    "status": "error",
    "message": "Error marking all notifications as read",
    "error": {error_details}
}
```



### 7. Delete Notification

* **Endpoint:** `/notifications/{id}`
* **Method:** `DELETE`
* **Description:** Deletes a specific notification for the authenticated user.
* **Request Body:** None (ID in URL)
* **Response Body (Success):**
```json
{
  "status": "success",
  "message": "Notification deleted successfully"
}
```

* **Response Body (Error - Validation or Not Found):** Standard error responses as shown in previous endpoints.


### 8. Send Notifications (Admin Only) (Not changed)

* This endpoint's functionality remains unchanged from the previous version.  Refer to the previous documentation for details.
















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






# Insurance API Documentation

This document details the API endpoints for purchasing insurance products. All endpoints require authentication using a bearer token in the `Authorization` header.

## Endpoints

### 1. Get Insurance Operators/Variations

* **Endpoint:** `/insurance/operators/{code}`
* **Method:** `GET`
* **Description:** Retrieves the variations (plans/options) for a specific insurance provider using the VTPass API.
* **Request Body:** None (`code` is a URL parameter representing the insurance provider's service ID)
* **Response Body (Success):**
```json
{
 "status": true,
 "message": "Service Fetched",
 "image": "image_url_for_insurance_provider",
 "content": { // Response from VTPass API (structure varies based on provider)
     "variations":[
        {
            "name": "Variation/Plan Name",
            "variation_code": "variation_code",
            "variation_amount": 1500,
            // ... other details about the variation
        },
     ],
    // ... other content from VTPass
 }
}
```

### 2. Get Insurance List

* **Endpoint:** `/insurance/list`
* **Method:** `GET`
* **Description:** Retrieves a list of available insurance providers.
* **Request Body:** None
* **Response Body:**
```json
{
 "status": true,
 "message": "Insurance providers fetched successfully",
 "data": [
     {"name": "Third Party Motor Insurance - Universal Insurance", "code": "ui-insure"},
     {"name": "Health Insurance - HMO", "code": "health-insurance-rhl"},
     {"name": "Personal Accident Insurance", "code": "personal-accident-insurance"}
 ]
}
```

### 3. Get Insurance Purchase History

* **Endpoint:** `/insurance/history`
* **Method:** `GET`
* **Description:** Retrieves the authenticated user's insurance purchase history. Supports optional search functionality.
* **Request Body:** (Query parameters)
```
search="transaction_id"  // Optional search term.
per_page=10             // Optional. Number of results per page. Default is 15.
```
* **Response Body:**
```json
{
    "status": true,
    "message": "History fetched successfully",
    "data": { // Paginated history data
        "data": [
            // Array of Order objects representing insurance purchases
        ],
        // ... pagination details (current_page, last_page, etc.)
    }
}
```

### 4. Buy Motor Insurance

* **Endpoint:** `/insurance/buy/motor`
* **Method:** `POST`
* **Description:** Purchases motor insurance using the VTPass API.
* **Request Body:**
```json
{
 "password": "user_transaction_pin",
 "wallet": "main" or "ref", // Wallet to use for payment
 // ...Vehicle and Insurance Details...
 "Chasis_Number": "chassis_number",
 "Contact_Address": "contact_address",
 "Engine_Number": "engine_number",
 "Insured_Name": "insured_name",
 "Vehicle_Color": "vehicle_color",
 "Vehicle_Make": "vehicle_make",
 "Vehicle_Model": "vehicle_model",
 "Year_of_Make": "year_of_make",
 "billersCode": "vehicle_plate_number", // This is used as both billersCode and Plate_Number
 "serviceID": "vtpass_service_id", //  E.g., "ui-insure"
 "variation_code": "vtpass_variation_code",  // From /operators response
 "variation_name": "insurance_plan_name",  //  Name of the selected insurance plan
 "phone": "phone_number",
 "amount": 15000 // Insurance price

}
```



* **Response Body (Success):**
```json
{
 "status": true,
 "message": "Transaction was successful",
 "data": {
     "order_id": "transaction_id",
     "transaction": { // Transaction details from VTPass API
        // ...
     }
 }
}
```

* **Response Body (Error - Validation, Authentication, Balance, or VTPass API Issues):** Standard error responses.


### 5. Buy Personal Insurance

* **Endpoint:** `/insurance/buy/personal`
* **Method:** `POST`
* **Description:** Purchases personal accident insurance using the VTPass API.

* **Request Body:**
```json
{
    "password": "user_transaction_pin",
    "wallet": "main" or "ref", // Wallet to use for payment
    "billersCode": "full_name", // Customer name
    "address": "contact_address",
    "dob": "date_of_birth", // YYYY-MM-DD format
    "next_kin_name": "next_of_kin_name",
    "next_kin_phone": "next_of_kin_phone",
    "business_occupation": "business/occupation",
    "serviceID": "vtpass_service_id", // E.g. "personal-accident-insurance"
    "variation_code": "vtpass_variation_code", // From `/operators` response
    "variation_name": "insurance_plan_name", // Name of insurance plan
    "phone": "phone_number",
    "amount": 5000 // Insurance Price
}
```

* **Response Body (Success):**
```json
{
    "status": true,
    "message": "Transaction was successful",
    "data": {
        "order_id": "transaction_id",
        "transaction": {/* ...Details from VTPass */},
        "insurance_details": {
            "full_name": "customer_full_name",
            "policy_type": "Personal Insurance",
            "coverage_start": "YYYY-MM-DD", // Today's date
            "next_of_kin": "next_of_kin_name",
            "policy_number": "policy_number_from_vtpass_or_transaction_id"
        }
    }
}

```

* **Response Body (Error):** Standard error responses for validation, authentication, insufficient balance, and VTPass API issues.










# (Prefrences) Site API Documentation

This document details general site-related API endpoints. Some endpoints require authentication while others do not.

## Endpoints

### 1. Get Rates (Crypto & Gift Cards)

* **Endpoint:** `/rates`
* **Method:** `GET`
* **Description:** Retrieves the current rates for cryptocurrencies and gift cards.
* **Request Body:** None
* **Response Body:**
```json
{
    "status": true,
    "data": {
        "coins": [
            // Array of Cryptocurrency objects with rate information
        ],
        "giftcards": [
            // Array of Giftcard objects with their types and rates
        ]
    }
}
```

### 2. Get Page Content

* **Endpoint:** `/pages/{slug}`
* **Method:** `GET`
* **Description:** Retrieves the content of a specific page based on its slug.
* **Request Body:** None (slug in URL)
* **Response Body:**
```json
{
  "status": true,
  "data": {
    "title": "Page Title",
    "sections": [
        // Array of page sections (content depends on your setup)
    ]
  }
}
```

### 3. Subscribe to Newsletter

* **Endpoint:** `/subscribe`
* **Method:** `POST`
* **Description:** Subscribes a user to the newsletter.
* **Request Body:**
```json
{
  "email": "user_email@example.com"
}
```
* **Response Body (Success):**
```json
{
  "status": true,
  "message": "Thanks for subscribing"
}
```
* **Response Body (Error - Validation):**
```json
{
 "status": false,
 "errors": {
     "email": [
         "The email has already been taken." // Or other validation errors
     ]
 }
}
```

### 4. Submit Contact Form

* **Endpoint:** `/contact`
* **Method:** `POST`
* **Description:** Submits a contact form message. Creates a support ticket.  Authentication is not required, but if authenticated, the `user_id` is associated with the ticket.

* **Request Body:**
```json
{
 "name": "User Name",
 "email": "user_email@example.com",
 "subject": "Contact Form Subject",
 "message": "Contact form message content"
}
```
* **Response Body (Success):**
```json
{
    "status": true,
    "message": "Ticket created successfully",
    "data": {
        "ticket_id": "generated_ticket_number"
    }
}

```

* **Response Body (Error - Validation):**
```json
{
 "status": false,
 "errors": {
     // ... validation error messages
 }
}
```


### 5. Track Order

* **Endpoint:** `/track/order`
* **Method:** `POST`
* **Description:** Tracks an order by email and order ID.
* **Request Body:**
```json
{
 "email": "user_email@example.com",
 "orderid": "order_id"
}

```
* **Response Body (Success):**
```json
{
    "status": true,
    "message": "Order Found",
    "data": {
        "orders": [  // Array of matching orders (there might be multiple with the same order ID)
            {
                "product_name": "Product Name",
                "price": 100,
                "quantity": 1,
                "currency": "USD",
                "status": "pending",   // Or other status values
                "created_at": "2024-07-29T14:00:00.000000Z"
            }
            // ...more orders if applicable
        ]
    }
}

```


* **Response Body (Error - Validation, Invalid Email, or Order Not Found):**  Standard error responses as in other endpoints.



### 6. Blog Details

* **Endpoint:** `/blog/{id}/{slug}`
* **Method:** `GET`
* **Description:** Retrieves details of a specific blog post. Increments the view count.
* **Request Body:** None  (ID and slug in the URL)

* **Response Body:**
```json
{
  "status": true,
  "data": {
    "blog": { // The requested blog post data
        // ... blog details from the 'frontends' table
    },
    "recent_blogs": [ // Array of recent blog posts (excluding the current one)
        // ... data for recent blog posts
    ]
  }
}
```


### 7.  Other Endpoints

The following endpoints are listed in the routes but are not defined in the provided controller code:

* `/blog` (GET):  Should retrieve a list of blog posts.
* `/policy/{slug}/{id}` (GET): Should retrieve content for a specific policy page.
* `/cookie-policy` (GET):  Should retrieve the cookie policy content.
* `/language/{lang}` (POST): Should handle language switching.







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



