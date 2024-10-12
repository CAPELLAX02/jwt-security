# JWT Security Implementation with Spring Boot

This is a Spring Boot project that demonstrates the implementation of JWT (JSON Web Token) based authentication and authorization. The project secures endpoints using Spring Security and provides API endpoints for user registration and authentication.

## Project Structure Overview

![Project Structure](src/main/resources/static/jwt-security.jpg)

## Features

- **JWT Authentication:** Implements token-based authentication using JWT.
- **User Registration and Authentication:** Allows users to register and authenticate.
- **Stateless Authentication:** Uses JWT to manage user sessions without server-side storage.
- **Spring Security Integration:** Secures API endpoints and manages role-based access control.

## Technologies Used

- **Spring Boot 3.3.4**
- **Spring Security**
- **PostgreSQL** (as the database)
- **JSON Web Tokens (JWT)**
- **Lombok** (for reducing boilerplate code)
- **JPA/Hibernate** (for ORM)
- **Maven** (for dependency management)

## Setup Instructions

### Prerequisites
- Java 17
- Maven
- PostgreSQL database

### Database Configuration
Ensure that you have PostgreSQL running on your system. You can set up your database configuration in the `application.yml` file:

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/jwt_security_db
    username: postgres
    password: 123456
    driver-class-name: org.postgresql.Driver
  jpa:
    hibernate:
      ddl-auto: create-drop
    show-sql: true
    properties:
      hibernate:
        format_sql: true
    database: postgresql
    database-platform: org.hibernate.dialect.PostgreSQLDialect

JWT_SECRET_KEY: 542376a94fd702ef89315425a86dc157c7dc9eb8cad7cb987d1694e1baaf98bf
```

### Build and Run

1. Clone the repository:

   ```bash
   git clone https://github.com/your-repo/jwt-security.git
   cd jwt-security
   ```

2. Package the application with Maven:

   ```bash
   mvn clean package
   ```

3. Run the application:

   ```bash
   mvn spring-boot:run
   ```

### API Endpoints

- **POST /api/v1/auth/register**: Register a new user.
- **POST /api/v1/auth/authenticate**: Authenticate an existing user.

### Example Register Request

```json
POST /api/v1/auth/register
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "password": "password123"
}
```

### Example Authenticate Request

```json
POST /api/v1/auth/authenticate
{
  "email": "john.doe@example.com",
  "password": "password123"
}
```

### Response

For both register and authenticate endpoints, the response will include a JWT token:

```json
{
  "token": "eyJhbGciOiJIUzI1NiJ9..."
}
```

### Security Configuration

The `SecurityConfiguration` class configures Spring Security to:

- Allow unauthenticated access to `/api/v1/auth/**` (for registration and login).
- Require authentication for all other endpoints.

```java
httpSecurity
  .csrf(AbstractHttpConfigurer::disable)
  .authorizeHttpRequests(auth -> auth
      .requestMatchers("/api/v1/auth/**").permitAll()
      .anyRequest().authenticated()
  )
  .sessionManagement(session -> session
      .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
  )
  .addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class);
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.