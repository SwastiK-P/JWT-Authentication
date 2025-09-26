# JWT Authentication with Spring Boot and MongoDB

A complete JWT authentication system built with Spring Boot 3, Spring Security 6, and MongoDB.

## Features

- User registration and login
- JWT token generation and validation
- Role-based access control (USER, ADMIN)
- MongoDB integration for user data persistence
- Password encryption using BCrypt
- RESTful API endpoints
- Input validation

## Prerequisites

- Java 17 or higher
- Maven 3.6+
- MongoDB (running on localhost:27017)

## Project Structure

```
src/main/java/com/example/jwtsecurity/
├── config/
│   └── WebSecurityConfig.java      # Security configuration
├── controller/
│   ├── AuthController.java         # Authentication endpoints
│   └── TestController.java         # Protected test endpoints
├── dto/
│   ├── LoginRequest.java          # Login request DTO
│   ├── SignUpRequest.java         # Registration request DTO
│   ├── JwtResponse.java           # JWT response DTO
│   └── MessageResponse.java       # Generic message response DTO
├── model/
│   ├── User.java                  # User entity
│   └── Role.java                  # Role entity
├── repository/
│   ├── UserRepository.java        # User repository
│   └── RoleRepository.java        # Role repository
├── security/
│   ├── AuthEntryPointJwt.java     # JWT authentication entry point
│   ├── AuthTokenFilter.java       # JWT authentication filter
│   ├── JwtUtils.java              # JWT utility class
│   ├── UserDetailsImpl.java       # User details implementation
│   └── UserDetailsServiceImpl.java # User details service
├── service/
│   └── AuthService.java           # Authentication service
└── JwtSecurityApplication.java    # Main application class
```

## Getting Started

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd jwt-auth
   ```

2. **Start MongoDB**
   Make sure MongoDB is running on localhost:27017

3. **Configure application properties**
   Update `src/main/resources/application.properties` if needed:
   ```properties
   spring.data.mongodb.database=jwtauth
   app.jwt.secret=mySecretKey123456789012345678901234567890
   app.jwt.expiration=86400000
   ```

4. **Build and run the application**
   ```bash
   mvn clean install
   mvn spring-boot:run
   ```

   The application will start on http://localhost:8080

## API Endpoints

### Authentication Endpoints

#### Register a new user
```http
POST /api/auth/signup
Content-Type: application/json

{
    "username": "testuser",
    "email": "test@example.com",
    "password": "password123",
    "role": ["user"]
}
```

#### Login
```http
POST /api/auth/signin
Content-Type: application/json

{
    "username": "testuser",
    "password": "password123"
}
```

Response:
```json
{
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "tokenType": "Bearer",
    "id": "64a1b2c3d4e5f6789012345",
    "username": "testuser",
    "email": "test@example.com",
    "roles": ["ROLE_USER"]
}
```

### Protected Endpoints

#### Public access
```http
GET /api/test/all
```

#### User access (requires authentication)
```http
GET /api/test/user
Authorization: Bearer <jwt-token>
```

#### Admin access (requires ADMIN role)
```http
GET /api/test/admin
Authorization: Bearer <jwt-token>
```

## Usage Examples

### 1. Register a new user
```bash
curl -X POST http://localhost:8080/api/auth/signup \
  -H "Content-Type: application/json" \
  -d '{
    "username": "john",
    "email": "john@example.com",
    "password": "password123",
    "role": ["user"]
  }'
```

### 2. Login and get JWT token
```bash
curl -X POST http://localhost:8080/api/auth/signin \
  -H "Content-Type: application/json" \
  -d '{
    "username": "john",
    "password": "password123"
  }'
```

### 3. Access protected endpoint
```bash
curl -X GET http://localhost:8080/api/test/user \
  -H "Authorization: Bearer YOUR_JWT_TOKEN_HERE"
```

## Configuration

### JWT Configuration
- **Secret Key**: Used for signing JWT tokens
- **Expiration**: Token expiration time (default: 24 hours)
- **Refresh Expiration**: Refresh token expiration (default: 30 days)

### MongoDB Configuration
- **Host**: localhost (default)
- **Port**: 27017 (default)
- **Database**: jwtauth

## Security Features

- Password encryption using BCrypt
- JWT token-based authentication
- Role-based authorization
- CORS support
- Stateless session management
- Automatic role initialization on startup

## Database Collections

The application creates the following MongoDB collections:
- `users`: Stores user information
- `roles`: Stores available roles (USER, ADMIN)

## Error Handling

The application includes proper error handling for:
- Invalid credentials
- Duplicate username/email
- Expired JWT tokens
- Unauthorized access attempts
- Validation errors

## Development

To extend this application:
1. Add new roles in `Role.ERole` enum
2. Create new protected endpoints with `@PreAuthorize` annotations
3. Implement additional user fields in the User model
4. Add refresh token functionality if needed

## Testing

Run the tests with:
```bash
mvn test
```

## License

This project is licensed under the MIT License.
