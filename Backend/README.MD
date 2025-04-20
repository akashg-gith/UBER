# API Documentation

## Endpoint: `/users/register`

### Description

This endpoint is used to register a new user. It accepts user details such as `fullname`, `email`, and `password`, hashes the password, and stores the user in the database. Upon successful registration, it returns a JSON Web Token (JWT) and the user details.

---

### HTTP Method

`POST`

---

### Request Body

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

### Response

#### Success Response

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

#### Error Responses

1. **Validation Error**

   - **Status Code:** `400 Bad Request`
   - **Description:** One or more fields are invalid.
   - **Response Body:**
     ```json
     {
       "errors": [
         {
           "msg": "First Name must be 3 characters long",
           "param": "fullname.firstname",
           "location": "body"
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
       "error": "Email already exists"
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

### Notes

- Ensure that the `Content-Type` header is set to `application/json` when making the request.
- The `password` is hashed before being stored in the database for security purposes.
- A valid JWT is returned upon successful registration, which can be used for authentication in subsequent requests.

---

### Example cURL Request

```bash
curl -X POST http://localhost:3000/users/register \
-H "Content-Type: application/json" \
-d '{
  "fullname": {
    "firstname": "John",
    "lastname": "Doe"
  },
  "email": "john.doe@example.com",
  "password": "password123"
}'
```
