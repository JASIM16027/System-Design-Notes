# Token-Based and JWT Authentication

Let me explain both of these authentication methods in detail.

## Token-Based Authentication

**Token-based authentication** is a security protocol where users receive a unique encrypted token after successfully logging in, which they then use to authenticate subsequent requests instead of sending credentials repeatedly.

### How It Works:

1. **Login**: User sends credentials (username/password) to the server
2. **Token Generation**: Server validates credentials and generates a unique token
3. **Token Storage**: Client stores the token (usually in browser storage or memory)
4. **Authenticated Requests**: Client includes the token in headers for each API request
5. **Validation**: Server validates the token and processes the request
6. **Logout**: Token is deleted from client storage

### Advantages:
- **Stateless**: Server doesn't need to store session data
- **Scalable**: Works well across multiple servers
- **Cross-domain/CORS**: Can be used across different domains
- **Mobile-friendly**: Works seamlessly with mobile applications
- **Security**: Credentials aren't sent with every request

### Disadvantages:
- Token theft risk if not properly secured
- Larger payload size compared to session IDs
- Token revocation can be complex

## JWT (JSON Web Token) Authentication

**JWT** is a specific implementation and standard (RFC 7519) for token-based authentication. It's a compact, URL-safe token format that contains encoded JSON data.

### JWT Structure:

A JWT consists of three parts separated by dots (`.`):

```
xxxxx.yyyyy.zzzzz
```

1. **Header**: Contains token type and hashing algorithm
2. **Payload**: Contains claims (user data and metadata)
3. **Signature**: Ensures token hasn't been tampered with

### Example JWT:
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

### Breaking Down Each Part:

**1. Header** (decoded):
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

**2. Payload** (decoded):
```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022,
  "exp": 1516242622
}
```

Common claims include:
- `sub` (subject): User identifier
- `iat` (issued at): Timestamp of creation
- `exp` (expiration): When token expires
- `iss` (issuer): Who created the token
- Custom claims: Any additional data

**3. Signature**:
Created by encoding the header and payload, then signing with a secret key:
```
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret
)
```

### JWT Flow Example:

```
1. User Login
   Client → POST /login {username, password} → Server

2. Token Generation
   Server validates → Creates JWT → Returns token
   Server → {token: "eyJhbG..."} → Client

3. Authenticated Request
   Client → GET /api/profile 
   Headers: Authorization: Bearer eyJhbG...
   → Server

4. Token Validation
   Server → Verifies signature → Decodes payload → Returns data
```

### Types of JWT:

1. **Access Token**: Short-lived (15 mins - 1 hour), used for API requests
2. **Refresh Token**: Long-lived (days/weeks), used to get new access tokens

### Security Considerations:

**Best Practices:**
- Store tokens securely (httpOnly cookies or secure storage)
- Use HTTPS always
- Implement token expiration
- Use refresh token rotation
- Never store sensitive data in payload (it's only encoded, not encrypted)
- Validate tokens on every request
- Use strong secret keys

**Storage Options:**
- **localStorage**: Easy but vulnerable to XSS attacks
- **sessionStorage**: Cleared when tab closes
- **httpOnly cookies**: More secure, protected from JavaScript access
- **Memory**: Most secure but lost on refresh

### JWT vs Session-Based Authentication:

| Aspect | JWT | Session |
|--------|-----|---------|
| Storage | Client-side | Server-side |
| Scalability | Excellent (stateless) | Requires shared storage |
| Data Access | Immediate (decoded) | Requires database lookup |
| Revocation | Difficult | Easy |
| Size | Larger | Smaller (just ID) |

### When to Use JWT:

- **Good for**: Microservices, mobile apps, SPAs, APIs, cross-domain authentication
- **Consider alternatives for**: Applications requiring immediate token revocation, very high security requirements

### Common Implementation (Pseudocode):

```javascript
// Login
POST /api/login
{
  username: "user",
  password: "pass"
}

// Server generates JWT
const token = jwt.sign(
  { userId: user.id, role: user.role },
  SECRET_KEY,
  { expiresIn: '1h' }
);

// Client uses token
GET /api/protected
Headers: {
  Authorization: "Bearer eyJhbG..."
}

// Server verifies
const decoded = jwt.verify(token, SECRET_KEY);
// decoded = { userId: 123, role: "admin", iat: ..., exp: ... }
```

Both token-based authentication and JWT are fundamental to modern web security, with JWT being the most popular standardized implementation you'll encounter in contemporary applications.
