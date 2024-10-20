# Salad Executor API Documentation

## Base URL
`https://saladexecutor.pythonanywhere.com`

## Endpoints

### 1. Generate Keys
Generate new keys for user accounts.

- **URL:** `/v0/generate`
- **Method:** `POST`
- **Auth Required:** Yes (Admin key required)

#### Request Body
```json
{
  "admin": "AdminSecretKey",
  "length": "weekly|monthly|lifetime",
  "amount": 1
}
```

- `admin`: The secret admin key (required)
- `length`: Duration of the key (default: "lifetime")
  - "weekly": 7 days
  - "monthly": 30 days
  - "lifetime": Very long duration
- `amount`: Number of keys to generate (default: 1)

#### Response
```json
{
  "message": "Keys generated successfully!",
  "keys": "uuid1,uuid2,uuid3"
}
```

#### Status Codes
- 201: Created
- 401: Unauthorized (if admin key is incorrect)

### 2. Register Account
Register a new user account.

- **URL:** `/v0/register`
- **Method:** `POST`
- **Auth Required:** No

#### Request Body
```json
{
  "username": "string",
  "password": "string",
  "email": "string"
}
```

#### Response
```json
{
  "message": "User registered successfully"
}
```

#### Status Codes
- 201: Created
- 409: Conflict (if username already exists)

### 3. Login
Authenticate a user.

- **URL:** `/v0/login`
- **Method:** `POST`
- **Auth Required:** No

#### Request Body
```json
{
  "username": "string",
  "password": "string"
}
```

#### Response
```json
{
  "message": "Successfully logged in"
}
```

#### Status Codes
- 200: OK
- 401: Unauthorized (missing credentials)
- 403: Forbidden (incorrect credentials)

### 4. Redeem Key
Redeem a key to extend account expiry.

- **URL:** `/v0/redeem`
- **Method:** `POST`
- **Auth Required:** No

#### Request Body
```json
{
  "username": "string",
  "key": "uuid-string"
}
```

#### Response
```json
{
  "message": "Key redeemed successfully"
}
```

#### Status Codes
- 200: OK
- 401: Unauthorized (invalid key)
- 422: Unprocessable Entity (missing key)

### 5. Access Script Hub
Fetch scripts from the script hub.

- **URL:** `/access/scripthub`
- **Method:** `GET`
- **Auth Required:** Yes

#### Query Parameters
- `username`: User's username
- `password`: User's password
- `q`: Search query (default: "script")

#### Response
Returns JSON data from scriptblox.com API.

#### Status Codes
- 200: OK
- 401: Unauthorized (missing credentials)
- 403: Forbidden (incorrect credentials)

### 6. Initialize
Fetch initialization data.

- **URL:** `/access/init`
- **Method:** `GET`
- **Auth Required:** Yes

#### Query Parameters
- `username`: User's username
- `password`: User's password

#### Response
Returns plain text content from a GitHub repository.

#### Status Codes
- 200: OK
- 401: Unauthorized (missing credentials)
- 403: Forbidden (incorrect credentials)

## Notes
- All endpoints return JSON responses unless otherwise specified.
- The `/v0/generate` endpoint requires a secret admin key for authentication. This key is not disclosed in the documentation for security reasons.
- User sessions are logged for login, key redemption, and access to scripthub and init endpoints.
- The API uses UUID for key generation.
- Account expiry times are managed in seconds since epoch.
