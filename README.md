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

## Dashboard Endpoint

**Endpoint:** `/user/dashboard`

**PHP Route:** `Route::get('user/dashboard', 'Api\UserController@dashboard');`

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
    "data": {
        "ref_balance": 100.50, // Example
        "balance": 500.00,      // Example
        "total_transaction": 25, // Example
        "deposit": 1500.00,    // Example
		"yearLabels": ["January", "February", ...],
		"yearDeposit": [120.00, 50.50, ...],  // Monthly deposit amounts
		"yearPayout": [0.00, 20.00, ...], // Monthly payout amounts
        "plan": { /* ... plan details */ }, // latest user plan details
        "trx": [ /* ... transaction details */ ], // latest 6 transactions
        "ref": 5,  // Referral count
        "last_login": { /* ... details of the last login */},
        "top_earner": [
            {
                "user_id": 5,
                "sums": 1000,
                "user": { /* user information */ }
            },
            // ...
        ],
		"user": {
			// ... user details
		},
        "user_browser_counter": {
            "Chrome": 55,
            "Firefox": 20,
            // ... other browsers
        },
        "user_os_counter": {
            "Windows": 60,
            "macOS": 15,
            // ... other operating systems
        }
    }
}

```

**Explanation:** This endpoint provides a comprehensive overview of the user's dashboard, including balances, transactions, referrals, recent logins, top earners, and usage statistics.



## Submit Profile Endpoint

**Endpoint:** `/user/profile/submit`

**PHP Route:** `Route::post('user/profile/submit', 'Api\UserController@submitProfile');`

**Request:**

* **Method:** POST
* **Headers:**
    * `Authorization: Bearer <access_token>`
    * `Content-Type: application/json` (Or `multipart/form-data` if image included)
* **Body:**

```json
{
    "firstname": "John",
    "lastname": "Doe",
    "address": "123 Main St",
    "state": "CA",
    "zip": "90210",
    "city": "Los Angeles",
    "image": "user_image.jpg" // If updating profile image, sent as multipart data.
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
             "Profile updated successfully."
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
    "status": "ok", // Should indicate an error
    "message": {
        "error": [
            "The firstname field is required."
        ]
    }
}

```


**Explanation:** This endpoint allows users to update their profile information, including their name, address, and profile image.



## Submit Password Endpoint

**Endpoint:** `/user/password/submit`

**PHP Route:** `Route::post('user/password/submit', 'Api\UserController@submitPassword');`

**Request:**

* **Method:** POST
* **Headers:**
    * `Authorization: Bearer <access_token>`
    * `Content-Type: application/json`
* **Body:**

```json
{
    "current_password": "OldPassword123",
    "password": "NewPassword123",
    "password_confirmation": "NewPassword123"
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
    "message": {  // Or just message: "..."
        "success": "Password changes successfully" // Or use the "error" key for wrong password response
    }
}
```


**Response (Password Mismatch or Validation Error - 200):** *(Should be 400 or 422 respectively)*
* **Headers:**
 * `Content-Type: application/json`
* **Body:** (Example: Password Mismatch)
```json
{
    "code": 200, // Should be 400
    "status": "ok", // Should be "error"
    "message": {
        "error":  "The password doesn't match!" // Or ["The password doesn't match!"] for consistency
    }
}

```


**Explanation:**  This endpoint enables users to change their password. It requires the current password for verification.



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
