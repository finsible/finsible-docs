# Backend Setup & Installation

> **Spring Boot Development Environment Setup**

This guide covers setting up the Finsible Spring Boot backend development environment, including database configuration, security setup, and API development tools.

## 📋 **Prerequisites**

### **Required Software**

| Software       | Version                 | Purpose                         |
| -------------- | ----------------------- | ------------------------------- |
| **Java JDK**   | 17 or higher            | Backend runtime                 |
| **Maven**      | 3.8+                    | Build and dependency management |
| **PostgreSQL** | 13+                     | Primary database                |
| **Git**        | Latest                  | Version control                 |
| **IDE**        | IntelliJ IDEA / Eclipse | Development environment         |

### **Recommended Tools**

-   **Docker** - For containerized database setup
-   **Postman/Insomnia** - API testing and development
-   **pgAdmin** - PostgreSQL administration
-   **JMeter** - Performance testing

## 🚀 **Installation Steps**

### **1. Clone Repository**

```bash
# Clone the backend repository
git clone https://github.com/finsible/finsible-backend.git
cd finsible-backend
```

### **2. Database Setup**

#### **Option A: Docker (Recommended)**

```bash
# Start PostgreSQL with Docker
docker run --name finsible-postgres \\
  -e POSTGRES_USER=finsible \\
  -e POSTGRES_PASSWORD=your_password \\
  -e POSTGRES_DB=finsible_db \\
  -p 5432:5432 \\
  -d postgres:13
```

#### **Option B: Local PostgreSQL**

```sql
-- Create database and user
CREATE DATABASE finsible_db;
CREATE USER finsible WITH ENCRYPTED PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE finsible_db TO finsible;
```

### **3. Configuration Setup**

Create `application-dev.properties`:

```properties
# Database Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/finsible_db
spring.datasource.username=finsible
spring.datasource.password=your_password

# JPA Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# JWT Configuration
app.jwt.secret=your-jwt-secret-key
app.jwt.expiration=86400000

# Google OAuth Configuration
app.google.client-id=your-google-client-id
```

### **4. Build and Run**

```bash
# Install dependencies and build
mvn clean install

# Run development server
mvn spring-boot:run -Dspring-boot.run.profiles=dev

# Or run with specific port
mvn spring-boot:run -Dspring-boot.run.profiles=dev -Dserver.port=8080
```

## 🔧 **Development Environment**

### **IDE Configuration**

#### **IntelliJ IDEA**

1. **Import Project** - Open as Maven project
2. **Set JDK** - Configure Java 17+ SDK
3. **Enable Annotation Processing** - For Lombok and JPA
4. **Install Plugins** - Spring Boot, JPA Buddy, Database Navigator

#### **Eclipse**

1. **Import Maven Project** - Use Import → Existing Maven Projects
2. **Set Compiler** - Java 17+ compliance level
3. **Install STS** - Spring Tool Suite for enhanced Spring support

### **Database Tools**

#### **pgAdmin Setup**

```bash
# Docker pgAdmin (optional)
docker run --name finsible-pgadmin \\
  -e PGADMIN_DEFAULT_EMAIL=admin@finsible.app \\
  -e PGADMIN_DEFAULT_PASSWORD=admin \\
  -p 8081:80 \\
  -d dpage/pgadmin4
```

Access at `http://localhost:8081`

### **API Testing Setup**

#### **Postman Collection**

-   Import Finsible API collection
-   Configure environment variables
-   Set up authentication workflows

## 🏗️ **Project Structure**

```
finsible-backend/
├── src/main/java/com/finsible/
│   ├── FinsibleApplication.java          # Main application class
│   ├── config/                           # Configuration classes
│   │   ├── SecurityConfig.java           # Spring Security configuration
│   │   ├── JwtConfig.java               # JWT configuration
│   │   └── DatabaseConfig.java          # Database configuration
│   ├── controller/                       # REST controllers
│   │   ├── AuthController.java          # Authentication endpoints
│   │   ├── AccountController.java       # Account management
│   │   ├── TransactionController.java   # Transaction operations
│   │   └── CategoryController.java      # Category management
│   ├── service/                         # Business logic services
│   │   ├── AuthService.java            # Authentication service
│   │   ├── AccountService.java         # Account operations
│   │   ├── TransactionService.java     # Transaction processing
│   │   └── SyncService.java            # Synchronization engine
│   ├── repository/                      # Data access layer
│   │   ├── UserRepository.java         # User data access
│   │   ├── AccountRepository.java      # Account data access
│   │   └── TransactionRepository.java  # Transaction data access
│   ├── model/                          # Entity models
│   │   ├── User.java                   # User entity
│   │   ├── Account.java                # Account entity
│   │   ├── Transaction.java            # Transaction entity
│   │   └── Category.java               # Category entity
│   ├── dto/                            # Data Transfer Objects
│   │   ├── request/                    # Request DTOs
│   │   └── response/                   # Response DTOs
│   ├── security/                       # Security components
│   │   ├── JwtAuthenticationFilter.java # JWT filter
│   │   ├── JwtTokenProvider.java       # JWT utilities
│   │   └── UserPrincipal.java          # User principal
│   └── util/                           # Utility classes
├── src/main/resources/
│   ├── application.properties          # Main configuration
│   ├── application-dev.properties      # Development config
│   ├── application-prod.properties     # Production config
│   └── db/migration/                   # Database migrations
└── src/test/java/                      # Test classes
```

## 🧪 **Testing Setup**

### **Unit Testing**

```bash
# Run unit tests
mvn test

# Run with coverage
mvn test jacoco:report
```

### **Integration Testing**

```bash
# Run integration tests
mvn test -Dtest=**/*IntegrationTest

# Run with TestContainers
mvn test -Dspring.profiles.active=test
```

## 🔒 **Security Configuration**

### **JWT Setup**

```bash
# Generate JWT secret
openssl rand -base64 64
```

### **Google OAuth Setup**

1. **Google Cloud Console** - Create OAuth2 credentials
2. **Configure Redirect URIs** - Add backend callback URLs
3. **Environment Variables** - Set client ID and secret

## 🚀 **Deployment Preparation**

### **Docker Configuration**

```dockerfile
# Dockerfile for backend
FROM openjdk:17-jdk-slim
COPY target/finsible-backend-*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

### **Production Configuration**

```properties
# application-prod.properties
spring.datasource.url=${DATABASE_URL}
spring.jpa.hibernate.ddl-auto=validate
logging.level.com.finsible=INFO
```

---

**📝 Note**: This setup guide covers the essential development environment configuration. Refer to the [Development Guide](development.md) for detailed development workflows and best practices.
