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
