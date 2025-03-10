# 📘 API Documentation

## Base URL
```
http://127.0.0.1:8000/api/
```

## 🔐 Authentication Endpoints

### 1️⃣ Register a New User
- **URL:** `/register/`
- **Method:** `POST`
- **Description:** Creates a new user account.
- **Request Body (JSON):**
    ```json
    {
        "username": "testuser",
        "email": "test@example.com",
        "password": "strongpassword"
    }
    ```
- **Response (201 Created):**
    ```json
    {
        "message": "Registration successful!",
        "user": {
            "id": 1,
            "username": "testuser",
            "email": "test@example.com",
            "login_streak": 0,
            "ink_bottle_returns": 0
        }
    }
    ```
- **Errors (400 Bad Request):**
    ```json
    {
        "username": ["This field is required."]
    }
    ```

### 2️⃣ Login & Get JWT Token
- **URL:** `/token/`
- **Method:** `POST`
- **Description:** Authenticates user and returns an access and refresh token.
- **Request Body (JSON):**
    ```json
    {
        "username": "testuser",
        "password": "strongpassword"
    }
    ```
- **Response (200 OK):**
    ```json
    {
        "refresh": "eyJhbGciOiJIUzI1...",
        "access": "eyJhbGciOiJIUzI1..."
    }
    ```
- **Errors (401 Unauthorized):**
    ```json
    {
        "detail": "No active account found with the given credentials"
    }
    ```

### 3️⃣ Refresh JWT Token
- **URL:** `/token/refresh/`
- **Method:** `POST`
- **Description:** Refreshes an expired access token.
- **Request Body (JSON):**
    ```json
    {
        "refresh": "eyJhbGciOiJIUzI1..."
    }
    ```
- **Response (200 OK):**
    ```json
    {
        "access": "eyJhbGciOiJIUzI1..."
    }
    ```

## 👤 User Profile Endpoints (Requires Authentication)

### 4️⃣ Get User Profile
- **URL:** `/my/account/`
- **Method:** `GET`
- **Headers:**
    ```makefile
    Authorization: Bearer <access_token>
    ```
- **Response (200 OK):**
    ```json
    {
        "id": 1,
        "username": "testuser",
        "email": "test@example.com",
        "login_streak": 5,
        "ink_bottle_returns": 2
    }
    ```

### 5️⃣ Update User Profile
- **URL:** `/my/account/`
- **Method:** `PUT`
- **Headers:**
    ```makefile
    Authorization: Bearer <access_token>
    ```
- **Request Body (JSON):**
    ```json
    {
        "username": "newusername",
        "email": "newemail@example.com"
    }
    ```
- **Response (200 OK):**
    ```json
    {
        "message": "Profile updated successfully!",
        "user": {
            "id": 1,
            "username": "newusername",
            "email": "newemail@example.com"
        }
    }
    ```

### 6️⃣ Change Password
- **URL:** `/change_password/`
- **Method:** `POST`
- **Headers:**
    ```makefile
    Authorization: Bearer <access_token>
    ```
- **Request Body (JSON):**
    ```json
    {
        "old_password": "oldpassword",
        "new_password": "newstrongpassword"
    }
    ```
- **Response (200 OK):**
    ```json
    {
        "message": "Password updated successfully!"
    }
    ```
- **Errors (400 Bad Request):**
    ```json
    {
        "old_password": ["Incorrect password."]
    }
    ```

## 📩 Contact Endpoint

### 7️⃣ Send a Contact Message
- **URL:** `/contact/`
- **Method:** `POST`
- **Request Body (JSON):**
    ```json
    {
        "name": "John Doe",
        "email": "johndoe@example.com",
        "message": "I need support."
    }
    ```
- **Response (201 Created):**
    ```json
    {
        "message": "Message sent successfully!"
    }
    ```
- **Errors (400 Bad Request):**
    ```json
    {
        "email": ["This field is required."]
    }
    ```

## 📊 Admin Dashboard (Requires Admin Authentication)

### 8️⃣ Get Admin Dashboard Data
- **URL:** `/admin_dash/`
- **Method:** `GET`
- **Headers:**
    ```makefile
    Authorization: Bearer <access_token>
    ```
- **Response (200 OK):**
    ```json
    {
        "total_orders": 150,
        "total_sales": 45000,
        "total_products": 75,
        "total_users": 320
    }
    ```

## ⚡ Testing with Postman
1. Register a user ➝ `POST /register/`
2. Login & get JWT token ➝ `POST /token/`
3. Use JWT token in Authorization header
4. Test protected endpoints like `/my/account/` and `/admin_dash/`

## 📌 Notes
- All protected routes require a valid JWT token.
- Use `Authorization: Bearer <access_token>` for protected routes.
- JWT tokens expire, so use `/token/refresh/` to get a new one.
- Admin-only endpoints should be accessed by superusers.
