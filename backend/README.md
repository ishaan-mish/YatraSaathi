# YatraSaathi Backend API

Backend API for YatraSaathi travel companion website built with Node.js, Express, and MongoDB Atlas.

## Architecture

This backend follows the **MVC (Model-View-Controller)** architecture pattern for better modularity and scalability:

- **Models**: Data schemas and database interactions (`models/`)
- **Views**: Not applicable for REST API (responses are JSON)
- **Controllers**: Business logic and request handling (`controllers/`)
- **Routes**: API endpoint definitions (`routes/`)
- **Middleware**: Authentication, validation, and error handling (`middleware/`)

## Features

- User registration (signup) with comprehensive validation
- User authentication (login) with JWT tokens
- Protected routes with JWT middleware
- Input validation using express-validator
- Password hashing with bcryptjs
- Error handling middleware
- MongoDB Atlas cloud database integration

## Prerequisites

- Node.js (v14 or higher)
- npm or yarn
- MongoDB Atlas account and cluster

## Setup Instructions

### 1. Install Dependencies

```bash
cd backend
npm install
```

### 2. Configure Environment Variables

1. Copy the `.env.example` file to `.env`:
   ```bash
   cp .env.example .env
   ```

2. Open `.env` and configure the following variables:

   **MongoDB Atlas Setup:**
   - Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
   - Create a free cluster (if you don't have one)
   - Click "Connect" → "Connect your application"
   - Copy the connection string
   - Replace `<password>` with your database user password
   - Replace `<database_name>` with your database name (e.g., `yatrasaathi`)
   
   Example:
   ```
   MONGODB_URI=mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/yatrasaathi?retryWrites=true&w=majority
   ```

   **JWT Secret:**
   - Generate a strong random string for JWT_SECRET
   - You can use: `node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"`
   - Or use any strong random string

   Example:
   ```
   JWT_SECRET=your_super_secret_jwt_key_min_32_characters_long
   ```

### 3. Start the Server

**Development mode (with auto-reload):**
```bash
npm run dev
```

**Production mode:**
```bash
npm start
```

The server will start on `http://localhost:5000` (or the PORT specified in `.env`).

## API Endpoints

### Base URL
```
http://localhost:5000/api
```

### 1. Health Check
- **GET** `/api/health`
- Returns server status

### 2. User Signup
- **POST** `/api/auth/signup`
- **Body:**
  ```json
  {
    "firstName": "John",
    "middleName": "Michael",
    "lastName": "Doe",
    "email": "john.doe@example.com",
    "phoneNumber": "+1234567890",
    "age": 25,
    "gender": "Male",
    "residenceCity": "New York",
    "residenceCountry": "United States",
    "password": "SecurePass123"
  }
  ```
- **Response:**
  ```json
  {
    "success": true,
    "message": "User registered successfully",
    "data": {
      "user": { ... },
      "token": "jwt_token_here"
    }
  }
  ```

### 3. User Login
- **POST** `/api/auth/login`
- **Body:**
  ```json
  {
    "email": "john.doe@example.com",
    "password": "SecurePass123"
  }
  ```
- **Response:**
  ```json
  {
    "success": true,
    "message": "Login successful",
    "data": {
      "user": { ... },
      "token": "jwt_token_here"
    }
  }
  ```

### 4. Get Current User (Protected)
- **GET** `/api/auth/me`
- **Headers:**
  ```
  Authorization: Bearer <token>
  ```
- **Response:**
  ```json
  {
    "success": true,
    "data": {
      "user": { ... }
    }
  }
  ```

## Testing with Thunder Client

### Setting up Thunder Client

1. Install Thunder Client extension in VS Code
2. Create a new request collection named "YatraSaathi API"

### Test Scenarios

#### 1. Health Check
- Method: GET
- URL: `http://localhost:5000/api/health`
- Expected: 200 OK with server status

#### 2. Signup
- Method: POST
- URL: `http://localhost:5000/api/auth/signup`
- Headers: `Content-Type: application/json`
- Body (raw JSON):
  ```json
  {
    "firstName": "John",
    "middleName": "Michael",
    "lastName": "Doe",
    "email": "john.doe@example.com",
    "phoneNumber": "+1234567890",
    "age": 25,
    "gender": "Male",
    "residenceCity": "New York",
    "residenceCountry": "United States",
    "password": "SecurePass123"
  }
  ```
- Save the `token` from response for subsequent requests

#### 3. Login
- Method: POST
- URL: `http://localhost:5000/api/auth/login`
- Headers: `Content-Type: application/json`
- Body (raw JSON):
  ```json
  {
    "email": "john.doe@example.com",
    "password": "SecurePass123"
  }
  ```
- Save the `token` from response

#### 4. Get Current User (Protected Route)
- Method: GET
- URL: `http://localhost:5000/api/auth/me`
- Headers: 
  - `Authorization: Bearer <your_token_here>`
  - `Content-Type: application/json`

### Test Error Cases

1. **Signup with existing email** - Should return 400
2. **Signup with invalid data** - Should return 400 with validation errors
3. **Login with wrong password** - Should return 401
4. **Login with non-existent email** - Should return 401
5. **Access protected route without token** - Should return 401
6. **Access protected route with invalid token** - Should return 401

## Project Structure

```
backend/
├── controllers/          # Business logic
│   └── authController.js # Authentication controller
├── middleware/           # Custom middleware
│   ├── auth.js          # JWT authentication middleware
│   └── validation.js    # Input validation middleware
├── models/              # Database models
│   └── User.js          # User schema and model
├── routes/              # API routes
│   └── authRoutes.js    # Authentication routes
├── .env                 # Environment variables (not in git)
├── .env.example         # Environment variables template
├── .gitignore           # Git ignore file
├── package.json         # Dependencies and scripts
├── server.js            # Express server setup
└── README.md            # This file
```

## Error Handling

The API returns standardized error responses:

```json
{
  "success": false,
  "message": "Error message here",
  "errors": [...] // For validation errors
}
```

Status codes:
- `200`: Success
- `201`: Created (successful signup)
- `400`: Bad Request (validation errors)
- `401`: Unauthorized (authentication failed)
- `404`: Not Found
- `500`: Internal Server Error

## Security Features

- Passwords are hashed using bcryptjs before storage
- JWT tokens for secure authentication
- Password field excluded from user queries by default
- Input validation on all user inputs
- CORS enabled for cross-origin requests

## Next Steps

After testing the backend with Thunder Client:

1. ✅ Verify all endpoints work correctly
2. ✅ Test error cases
3. ✅ Verify database connections
4. ✅ Check JWT token generation and validation
5. Move to frontend development

## Support

For issues or questions, please refer to the main project documentation.

