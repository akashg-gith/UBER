# API Documentation

## Endpoints

---

### 1. `/users/register`

#### Description

This endpoint is used to register a new user. It accepts user details such as `fullname`, `email`, and `password`, hashes the password, and stores the user in the database. Upon successful registration, it returns a JSON Web Token (JWT) and the user details.

---

#### HTTP Method

`POST`

---

#### Request Body

The request body must be in JSON format and include the following fields:

| Field                | Type   | Required | Description                                        |
| -------------------- | ------ | -------- | -------------------------------------------------- |
| `fullname`           | Object | Yes      | Contains `firstname` and `lastname`.               |
| `fullname.firstname` | String | Yes      | The first name of the user (minimum 3 characters). |
| `fullname.lastname`  | String | No       | The last name of the user (minimum 3 characters).  |
| `email`              | String | Yes      | The email address of the user (must be valid).     |
| `password`           | String | Yes      | The password for the user (minimum 6 characters).  |

#### Example Request Body

```json
{
  "fullname": {
    "firstname": "John",
    "lastname": "Doe"
  },
  "email": "john.doe@example.com",
  "password": "password123"
}
```

---

#### Response

##### Success Response

- **Status Code:** `201 Created`
- **Description:** The user was successfully registered.
- **Response Body:**
  ```json
  {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "_id": "643f1c2e8f1b2c001c8e4f1a",
      "fullname": {
        "firstname": "John",
        "lastname": "Doe"
      },
      "email": "john.doe@example.com"
    }
  }
  ```

---

### 2. `/users/login`

#### Description

This endpoint is used to authenticate a user. It accepts the user's email and password, verifies the credentials, and returns a JSON Web Token (JWT) along with the user details upon successful authentication.

---

#### HTTP Method

`POST`

---

#### Request Body

The request body must be in JSON format and include the following fields:

| Field      | Type   | Required | Description                                    |
| ---------- | ------ | -------- | ---------------------------------------------- |
| `email`    | String | Yes      | The email address of the user (must be valid). |
| `password` | String | Yes      | The password for the user.                     |

#### Example Request Body

```json
{
  "email": "john.doe@example.com",
  "password": "password123"
}
```

---

#### Response

##### Success Response

- **Status Code:** `200 OK`
- **Description:** The user was successfully authenticated.
- **Response Body:**
  ```json
  {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "_id": "643f1c2e8f1b2c001c8e4f1a",
      "fullname": {
        "firstname": "John",
        "lastname": "Doe"
      },
      "email": "john.doe@example.com"
    }
  }
  ```

---

### 3. `/users/profile`

#### Description

This endpoint is used to retrieve the profile of the currently authenticated user. The user must provide a valid JWT token in the request headers.

---

#### HTTP Method

`GET`

---

#### Headers

| Header          | Type   | Required | Description                     |
| --------------- | ------ | -------- | ------------------------------- |
| `Authorization` | String | Yes      | Bearer token for authentication |

---

#### Response

##### Success Response

- **Status Code:** `200 OK`
- **Description:** The user's profile was successfully retrieved.
- **Response Body:**
  ```json
  {
    "_id": "643f1c2e8f1b2c001c8e4f1a",
    "fullname": {
      "firstname": "John",
      "lastname": "Doe"
    },
    "email": "john.doe@example.com"
  }
  ```

##### Error Responses

1. **Unauthorized**
   - **Status Code:** `401 Unauthorized`
   - **Description:** The user is not authenticated.
   - **Response Body:**
     ```json
     {
       "message": "Unauthorized"
     }
     ```

---

### 4. `/users/logout`

#### Description

This endpoint is used to log out the currently authenticated user. It clears the authentication token from cookies and blacklists the token to prevent further use.

---

#### HTTP Method

`GET`

---

#### Headers

| Header          | Type   | Required | Description                     |
| --------------- | ------ | -------- | ------------------------------- |
| `Authorization` | String | Yes      | Bearer token for authentication |

---

#### Response

##### Success Response

- **Status Code:** `200 OK`
- **Description:** The user was successfully logged out.
- **Response Body:**
  ```json
  {
    "message": "Logged out"
  }
  ```

##### Error Responses

1. **Unauthorized**
   - **Status Code:** `401 Unauthorized`
   - **Description:** The user is not authenticated.
   - **Response Body:**
     ```json
     {
       "message": "Unauthorized"
     }
     ```

---

### Notes

- Ensure that the `Authorization` header is set to `Bearer <token>` for `/users/profile` and `/users/logout`.
- The `/users/logout` endpoint blacklists the token, making it unusable for future requests.

---

### Example cURL Requests

#### `/users/profile`

```bash
curl -X GET http://localhost:3000/users/profile \
-H "Authorization: Bearer <your_token>"
```

#### `/users/logout`

```bash
curl -X GET http://localhost:3000/users/logout \
-H "Authorization: Bearer <your_token>"
```

### 5. `/captains/register`

#### Description

This endpoint is used to register a new captain. It accepts captain details such as `fullname`, `email`, `password`, and `vehicle` information, hashes the password, and stores the captain in the database. Upon successful registration, it returns a JSON Web Token (JWT) and the captain details.

---

#### HTTP Method

`POST`

---

#### Request Body

The request body must be in JSON format and include the following fields:

```json
{
  "fullname": {
    "firstname": "Michael", // Required, must be at least 3 characters long
    "lastname": "Smith" // Optional, must be at least 3 characters long if provided
  },
  "email": "michael.smith@example.com", // Required, must be a valid email
  "password": "securePassword123", // Required, must be at least 6 characters long
  "vehicle": {
    "color": "Red", // Required, must be at least 3 characters long
    "plate": "ABC-1234", // Required, must be at least 3 characters long
    "capacity": 4, // Required, must be a number and at least 1
    "vehicleType": "car" // Required, must be one of ["car", "motocycle", "auto"]
  }
}
```

---

#### Response

##### Success Response

- **Status Code:** `201 Created`
- **Description:** The captain was successfully registered.
- **Response Body:**

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...", // JWT token for authentication
  "captain": {
    "_id": "643f1c2e8f1b2c001c8e4f1a", // Unique ID of the captain
    "fullname": {
      "firstname": "Michael",
      "lastname": "Smith"
    },
    "email": "michael.smith@example.com",
    "vehicle": {
      "color": "Red",
      "plate": "ABC-1234",
      "capacity": 4,
      "vehicleType": "car"
    }
  }
}
```

##### Error Responses

1. **Validation Error**

   - **Status Code:** `400 Bad Request`
   - **Description:** One or more fields are invalid.
   - **Response Body:**

   ```json
   {
     "errors": [
       {
         "msg": "First Name must be 3 characters long", // Example error message
         "param": "fullname.firstname", // Field that caused the error
         "location": "body" // Location of the error
       }
     ]
   }
   ```

2. **Duplicate Email**

   - **Status Code:** `400 Bad Request`
   - **Description:** The email is already registered.
   - **Response Body:**

   ```json
   {
     "message": "Captain already exists"
   }
   ```

3. **Server Error**

   - **Status Code:** `500 Internal Server Error`
   - **Description:** An unexpected error occurred on the server.
   - **Response Body:**

   ```json
   {
     "error": "Internal Server Error"
   }
   ```

---

### 6. `/captains/login`

#### Description

This endpoint is used to authenticate a captain. It accepts the captain's email and password, verifies the credentials, and returns a JSON Web Token (JWT) along with the captain details upon successful authentication.

---

#### HTTP Method

`POST`

---

#### Request Body

```json
{
  "email": "michael.smith@example.com", // Required, must be a valid email
  "password": "securePassword123" // Required, must be at least 6 characters long
}
```

---

#### Response

##### Success Response

- **Status Code:** `200 OK`
- **Description:** The captain was successfully authenticated.
- **Response Body:**

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...", // JWT token for authentication
  "captain": {
    "_id": "643f1c2e8f1b2c001c8e4f1a", // Unique ID of the captain
    "fullname": {
      "firstname": "Michael",
      "lastname": "Smith"
    },
    "email": "michael.smith@example.com",
    "vehicle": {
      "color": "Red",
      "plate": "ABC-1234",
      "capacity": 4,
      "vehicleType": "car"
    }
  }
}
```

##### Error Responses

1. **Invalid Credentials**

   - **Status Code:** `401 Unauthorized`
   - **Description:** The email or password is incorrect.
   - **Response Body:**

   ```json
   {
     "message": "Invalid email or password"
   }
   ```

2. **Validation Error**

   - **Status Code:** `400 Bad Request`
   - **Description:** One or more fields are invalid.
   - **Response Body:**

   ```json
   {
     "errors": [
       {
         "msg": "Invalid Email", // Example error message
         "param": "email", // Field that caused the error
         "location": "body" // Location of the error
       }
     ]
   }
   ```

---

### 7. `/captains/profile`

#### Description

This endpoint is used to retrieve the profile of the currently authenticated captain. The captain must provide a valid JWT token in the request headers.

---

#### HTTP Method

`GET`

---

#### Headers

```json
{
  "Authorization": "Bearer <your_token>" // Required, must be a valid JWT token
}
```

---

#### Response

##### Success Response

- **Status Code:** `200 OK`
- **Description:** The captain's profile was successfully retrieved.
- **Response Body:**

```json
{
  "_id": "643f1c2e8f1b2c001c8e4f1a", // Unique ID of the captain
  "fullname": {
    "firstname": "Michael",
    "lastname": "Smith"
  },
  "email": "michael.smith@example.com",
  "vehicle": {
    "color": "Red",
    "plate": "ABC-1234",
    "capacity": 4,
    "vehicleType": "car"
  }
}
```

##### Error Responses

1. **Unauthorized**

   - **Status Code:** `401 Unauthorized`
   - **Description:** The captain is not authenticated.
   - **Response Body:**

   ```json
   {
     "message": "Unauthorized"
   }
   ```

---

### 8. `/captains/logout`

#### Description

This endpoint is used to log out the currently authenticated captain. It clears the authentication token from cookies and blacklists the token to prevent further use.

---

#### HTTP Method

`GET`

---

#### Headers

```json
{
  "Authorization": "Bearer <your_token>" // Required, must be a valid JWT token
}
```

---

#### Response

##### Success Response

- **Status Code:** `200 OK`
- **Description:** The captain was successfully logged out.
- **Response Body:**

```json
{
  "message": "Logged out successfully"
}
```

##### Error Responses

1. **Unauthorized**

   - **Status Code:** `401 Unauthorized`
   - **Description:** The captain is not authenticated.
   - **Response Body:**

   ```json
   {
     "message": "Unauthorized"
   }
   ```

---

### Notes

- Ensure that the `Authorization` header is set to `Bearer <token>` for `/captains/profile` and `/captains/logout`.
- The `/captains/logout` endpoint blacklists the token, making it unusable for future requests.

---

### Example cURL Requests

#### `/captains/register`

```bash
curl -X POST http://localhost:3000/captains/register \
-H "Content-Type: application/json" \
-d '{
  "fullname": {
    "firstname": "Michael",
    "lastname": "Smith"
  },
  "email": "michael.smith@example.com",
  "password": "securePassword123",
  "vehicle": {
    "color": "Red",
    "plate": "ABC-1234",
    "capacity": 4,
    "vehicleType": "car"
  }
}'
```

#### `/captains/login`

```bash
curl -X POST http://localhost:3000/captains/login \
-H "Content-Type: application/json" \
-d '{
  "email": "michael.smith@example.com",
  "password": "securePassword123"
}'
```

#### `/captains/profile`

```bash
curl -X GET http://localhost:3000/captains/profile \
-H "Authorization: Bearer <your_token>"
```

#### `/captains/logout`

```bash
curl -X GET http://localhost:3000/captains/logout \
-H "Authorization: Bearer <your_token>"
```
