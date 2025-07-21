# Logistics Delivery Management System (LDMS) - Backend API

A comprehensive RESTful API built with Spring Boot for managing logistics and delivery operations. This system handles user management, driver operations, order processing, and warehouse management with role-based authentication and authorization.

## 🚀 Features

### Core Functionality
- **User Management**: Registration, authentication, and role-based access control
- **Driver Management**: Driver registration, profile management, and assignment
- **Order Management**: Order creation, tracking, status updates, and lifecycle management
- **Warehouse Operations**: Warehouse management and inventory tracking
- **Authentication & Security**: JWT-based authentication with role-based authorization

### Technical Features
- CRUD operations for all entities
- JWT token-based authentication
- Role-based access control (Admin, Customer, Driver)
- Global exception handling
- Input validation
- CORS configuration for cross-origin requests
- MySQL database integration with JPA/Hibernate

## 🛠 Technologies

- **Java**: 17+
- **Spring Boot**: 3.x
- **Spring Security**: JWT Authentication
- **Spring Data JPA**: Database operations
- **MySQL**: Primary database
- **Maven**: Dependency management
- **Jakarta Validation**: Input validation

## 📋 Prerequisites

- JDK 17 or later
- Maven 3.6+
- MySQL Server 8.0+

## ⚙️ Database Configuration

The application connects to a MySQL database. Update the configuration in `src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/your_database_name
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# JPA/Hibernate properties
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# JWT Configuration
app.jwt.expiration-in-ms=3600000

# Server Configuration
server.port=8023
```

## 🚀 Building and Running the Application

### Using Maven Wrapper (Recommended)

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd LDMS_Backend
   ```

2. **Build the project**
   ```bash
   ./mvnw clean install
   ```

3. **Run the application**
   ```bash
   ./mvnw spring-boot:run
   ```

### Using Maven

1. **Build the project**
   ```bash
   mvn clean install
   ```

2. **Run the application**
   ```bash
   mvn spring-boot:run
   ```

The application will start on **port 8023** (http://localhost:8023).

## 📚 API Endpoints

### Authentication Endpoints
```
POST /api/auth/login          - User login
POST /api/auth/register       - User registration
POST /api/auth/refresh        - Refresh JWT token
```

### User Management
```
GET    /api/users             - Get all users (Admin only)
GET    /api/users/{id}        - Get user by ID
PUT    /api/users/{id}        - Update user
DELETE /api/users/{id}        - Delete user (Admin only)
```

### Driver Management
```
GET    /api/drivers           - Get all drivers
GET    /api/drivers/{id}      - Get driver by ID
POST   /api/drivers           - Create new driver
PUT    /api/drivers/{id}      - Update driver
DELETE /api/drivers/{id}      - Delete driver
```

### Order Management
```
GET    /api/orders            - Get all orders
GET    /api/orders/{id}       - Get order by ID
POST   /api/orders            - Create new order
PUT    /api/orders/{id}       - Update order
DELETE /api/orders/{id}       - Delete order
PUT    /api/orders/{id}/status - Update order status
```

### Warehouse Management
```
GET    /api/warehouses        - Get all warehouses
GET    /api/warehouses/{id}   - Get warehouse by ID
POST   /api/warehouses        - Create new warehouse
PUT    /api/warehouses/{id}   - Update warehouse
DELETE /api/warehouses/{id}   - Delete warehouse
```

### Test Endpoints
```
GET    /api/test/public       - Public endpoint (no authentication required)
GET    /api/test/protected    - Protected endpoint (authentication required)
```

## 🔐 Authentication

The API uses JWT (JSON Web Token) for authentication. Include the token in the Authorization header:

```
Authorization: Bearer <your-jwt-token>
```

### User Roles
- **ADMIN**: Full access to all endpoints
- **CUSTOMER**: Access to order-related operations
- **DRIVER**: Access to driver-specific operations

## 📝 Request/Response Examples

### Login Request
```json
{
    "username": "user@example.com",
    "password": "password123"
}
```

### Login Response
```json
{
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "type": "Bearer",
    "username": "user@example.com",
    "roles": ["ROLE_USER"]
}
```

### Order Object Structure
```json
{
    "id": 1,
    "orderNumber": "ORD-001",
    "customerId": 1,
    "driverId": 2,
    "status": "PENDING",
    "orderStage": "WAREHOUSE",
    "createdDate": "2025-01-20T10:30:00",
    "deliveryAddress": "123 Main St, City, State",
    "totalAmount": 99.99
}
```

## 🔧 Error Handling

The API includes comprehensive error handling:

- **400 Bad Request**: Invalid input data
- **401 Unauthorized**: Authentication required
- **403 Forbidden**: Insufficient permissions
- **404 Not Found**: Resource not found
- **500 Internal Server Error**: Server-side errors

### Error Response Format
```json
{
    "timestamp": "2025-01-20T10:30:00",
    "status": 404,
    "error": "Not Found",
    "message": "Order not found with id: 1",
    "path": "/api/orders/1"
}
```

## ✅ Validation

Input validation is implemented using Jakarta Validation:

- Email format validation
- Required field validation
- String length constraints
- Numeric range validation

## 🏗 Project Structure

```
src/main/java/com/msd/spring_boot_rest_api/
├── SpringBootRestApiApplication.java    # Main application class
├── config/
│   └── CorsConfig.java                  # CORS configuration
├── controller/
│   ├── AuthController.java             # Authentication endpoints
│   ├── DriverController.java           # Driver management
│   ├── OrderController.java            # Order management
│   ├── TestController.java             # Test endpoints
│   ├── UserController.java             # User management
│   └── WarehouseController.java        # Warehouse management
├── dto/
│   ├── LoginRequest.java               # Login request DTO
│   └── LoginResponse.java              # Login response DTO
├── exception/
│   ├── GlobalExceptionHandler.java     # Global exception handling
│   └── ResourceNotFoundException.java  # Custom exceptions
├── model/
│   ├── Admin.java                      # Admin entity
│   ├── Customer.java                   # Customer entity
│   ├── Driver.java                     # Driver entity
│   ├── Order.java                      # Order entity
│   ├── OrderedItems.java               # Order items entity
│   ├── OrderStage.java                 # Order stage enum
│   ├── OrderStatus.java                # Order status enum
│   ├── Role.java                       # Role enum
│   └── User.java                       # User entity
├── repository/
│   ├── AdminRepository.java            # Admin data access
│   ├── DriverRepository.java           # Driver data access
│   ├── OrderRepository.java            # Order data access
│   └── UserRepository.java             # User data access
├── security/
│   ├── JwtAuthenticationFilter.java    # JWT filter
│   ├── JwtTokenProvider.java           # JWT utility
│   └── SecurityConfig.java             # Security configuration
└── service/
    ├── JwtUserDetails.java             # JWT user details
    └── OrderService.java               # Order business logic
```

## 🔍 Testing

Run the test suite:

```bash
./mvnw test
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.


## 🚀 Deployment

### Production Considerations

1. **Environment Variables**: Use environment variables for sensitive configuration
2. **Database**: Configure production database settings
3. **Security**: Update JWT secret and expiration times
4. **Logging**: Configure appropriate logging levels
5. **HTTPS**: Enable SSL/TLS in production

### Docker Deployment (Optional)

Create a `Dockerfile`:

```dockerfile
FROM openjdk:17-jdk-slim
VOLUME /tmp
COPY target/*.jar app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

Build and run:

```bash
docker build -t ldms-backend .
docker run -p 8023:8023 ldms-backend
```
