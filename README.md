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
