# Backend Architecture

> **Spring Boot Backend Architecture and Design Patterns**

The Finsible backend follows a layered architecture pattern with clear separation of concerns, implementing modern Spring Boot best practices for scalability, maintainability, and security.

## 🏗️ **Overall Architecture**

### **Layered Architecture Pattern**

```
┌─────────────────────────────────────────────────────────────────┐
│                     Presentation Layer                          │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │   Controllers   │  │   REST APIs     │  │   WebSockets    │ │
│  │   (HTTP/REST)   │  │  (OpenAPI 3.0)  │  │  (Real-time)    │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘ │
├─────────────────────────────────────────────────────────────────┤
│                      Business Layer                             │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │    Services     │  │   Use Cases     │  │   Validators    │ │
│  │ (Business Logic)│  │  (Domain Logic) │  │   (Rules)       │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘ │
├─────────────────────────────────────────────────────────────────┤
│                      Data Access Layer                          │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │  Repositories   │  │   JPA Entities  │  │     DTOs        │ │
│  │   (Spring Data) │  │   (Database)    │  │  (Transfer)     │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘ │
├─────────────────────────────────────────────────────────────────┤
│                     Infrastructure Layer                        │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │   PostgreSQL    │  │   Security      │  │   External      │ │
│  │   (Database)    │  │   (JWT/OAuth)   │  │   APIs          │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

## 🔧 **Core Components**

### **1. Presentation Layer**

#### **REST Controllers**

-   **Responsibility**: Handle HTTP requests, input validation, response formatting
-   **Pattern**: Controller → Service → Repository
-   **Security**: Method-level security with JWT authentication

```java
@RestController
@RequestMapping("/api/v1")
@PreAuthorize("hasRole('USER')")
public class TransactionController {
    // RESTful endpoints for transaction management
}
```

#### **Exception Handling**

-   **Global Exception Handler**: Centralized error handling
-   **Custom Exceptions**: Domain-specific error types
-   **Error Response Format**: Consistent error response structure

### **2. Business Layer**

#### **Service Classes**

-   **Transaction Service**: Core transaction processing logic
-   **Account Service**: Account management and balance calculations
-   **Sync Service**: Data synchronization between clients
-   **Auth Service**: Authentication and authorization logic

#### **Domain Models**

-   **Rich Domain Models**: Business logic encapsulated in entities
-   **Value Objects**: Immutable objects for data integrity
-   **Domain Events**: Event-driven architecture for loose coupling

### **3. Data Access Layer**

#### **JPA Repositories**

-   **Spring Data JPA**: Automatic repository implementation
-   **Custom Queries**: Optimized database queries with @Query
-   **Specifications**: Dynamic query building for complex filters

#### **Entity Relationships**

```
User (1) ←→ (N) Account (1) ←→ (N) Transaction
     ↓                              ↓
Category (N) ←→ (N) Transaction ←→ (1) TransactionType
```

## 🗃️ **Database Design**

### **Entity-Relationship Model**

#### **Core Entities**

-   **User**: User account information and authentication data
-   **Account**: Financial accounts (bank, credit card, cash, investments)
-   **Transaction**: Financial transactions (income, expense, transfer)
-   **Category**: Transaction categorization (hierarchical structure)

#### **Supporting Entities**

-   **SyncLog**: Data synchronization tracking
-   **AuditLog**: System activity and change tracking
-   **UserPreferences**: User-specific configuration and settings

### **Database Schema Highlights**

```sql
-- Users table with OAuth integration
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    google_id VARCHAR(255) UNIQUE NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(255) NOT NULL,
    picture_url VARCHAR(500),
    account_created TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    last_logged_in TIMESTAMP,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

-- Accounts with balance tracking
CREATE TABLE accounts (
    id BIGSERIAL PRIMARY KEY,
    user_id BIGINT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    name VARCHAR(255) NOT NULL,
    account_type VARCHAR(50) NOT NULL,
    balance DECIMAL(19,2) NOT NULL DEFAULT 0.00,
    is_custom BOOLEAN NOT NULL DEFAULT false,
    description TEXT,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

-- Transactions with comprehensive tracking
CREATE TABLE transactions (
    id BIGSERIAL PRIMARY KEY,
    user_id BIGINT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    account_id BIGINT REFERENCES accounts(id),
    from_account_id BIGINT REFERENCES accounts(id),
    to_account_id BIGINT REFERENCES accounts(id),
    category_id BIGINT REFERENCES categories(id),
    type VARCHAR(20) NOT NULL CHECK (type IN ('INCOME', 'EXPENSE', 'TRANSFER')),
    amount DECIMAL(19,2) NOT NULL,
    fee DECIMAL(19,2) DEFAULT 0.00,
    description VARCHAR(500) NOT NULL,
    notes TEXT,
    transaction_date TIMESTAMP NOT NULL,
    location_latitude DECIMAL(10,8),
    location_longitude DECIMAL(11,8),
    tags TEXT[], -- PostgreSQL array for tags
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

    -- Constraints
    CONSTRAINT valid_account_for_type CHECK (
        (type = 'TRANSFER' AND from_account_id IS NOT NULL AND to_account_id IS NOT NULL) OR
        (type IN ('INCOME', 'EXPENSE') AND account_id IS NOT NULL)
    )
);
```

## 🔒 **Security Architecture**

### **Authentication Flow**

```
Google OAuth → JWT Token → Request Authorization → Resource Access
     ↓              ↓              ↓                    ↓
1. User Login  2. Token Issue  3. Token Validate  4. Grant Access
```

### **Security Components**

#### **JWT Implementation**

-   **Token Structure**: Header + Payload + Signature
-   **Expiration**: Configurable token lifetime (default: 24 hours)
-   **Refresh Tokens**: Automatic token renewal for seamless UX
-   **Revocation**: Token blacklist for secure logout

#### **Authorization Levels**

-   **User Role**: Standard user access to own data
-   **Admin Role**: System administration capabilities
-   **Service Role**: Inter-service communication

#### **Data Protection**

-   **Field-Level Encryption**: Sensitive data encryption at rest
-   **HTTPS Enforcement**: TLS encryption for data in transit
-   **Input Sanitization**: SQL injection and XSS prevention
-   **Rate Limiting**: API abuse prevention

## 🔄 **Synchronization Engine**

### **Sync Strategy**

```
Client State → Server Validation → Conflict Resolution → State Update
     ↓               ↓                    ↓                  ↓
1. Client Push  2. Server Check  3. Merge Strategy  4. Broadcast Update
```

### **Conflict Resolution**

-   **Last-Write-Wins**: Simple timestamp-based resolution
-   **Vector Clocks**: Distributed conflict detection
-   **Manual Resolution**: User-guided conflict resolution for critical data

### **Sync Optimization**

-   **Delta Sync**: Only sync changed data
-   **Batch Operations**: Bulk data synchronization
-   **Priority Queues**: Critical data synchronized first
-   **Retry Logic**: Automatic retry with exponential backoff

## 📊 **Performance Architecture**

### **Caching Strategy**

```java
@Cacheable(value = "userAccounts", key = "#userId")
public List<AccountDto> getUserAccounts(Long userId) {
    // Cached account data for faster access
}

@CacheEvict(value = "userAccounts", key = "#userId")
public void invalidateUserCache(Long userId) {
    // Cache invalidation on data changes
}
```

### **Database Optimization**

-   **Connection Pooling**: HikariCP for efficient connection management
-   **Query Optimization**: Indexed queries and query plan analysis
-   **Lazy Loading**: JPA lazy loading for related entities
-   **Pagination**: Efficient handling of large datasets

### **API Performance**

-   **Response Compression**: GZIP compression for API responses
-   **Async Processing**: Non-blocking operations for heavy tasks
-   **Circuit Breaker**: Resilience patterns for external service calls
-   **Metrics Collection**: Performance monitoring and alerting

## 🧪 **Testing Architecture**

### **Testing Pyramid**

```
Unit Tests (70%) → Integration Tests (20%) → E2E Tests (10%)
       ↓                    ↓                    ↓
  Fast Feedback        Component Testing    Full System Test
```

### **Testing Strategy**

-   **Unit Tests**: Service and repository layer testing
-   **Integration Tests**: Database and API endpoint testing
-   **Contract Tests**: API contract validation
-   **Performance Tests**: Load testing and benchmarking

### **Test Infrastructure**

-   **TestContainers**: Containerized test dependencies
-   **Test Profiles**: Isolated test configurations
-   **Mock Services**: External service mocking
-   **Test Data Factory**: Consistent test data generation

## 🚀 **Deployment Architecture**

### **Containerization**

```dockerfile
# Multi-stage build for optimized production image
FROM openjdk:17-jdk-slim AS builder
WORKDIR /app
COPY . .
RUN ./mvnw clean package -DskipTests

FROM openjdk:17-jre-slim
COPY --from=builder /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### **Cloud Architecture**

-   **Horizontal Scaling**: Auto-scaling based on load
-   **Load Balancing**: Traffic distribution across instances
-   **Health Checks**: Application health monitoring
-   **Blue-Green Deployment**: Zero-downtime deployments

### **Monitoring & Observability**

-   **Application Metrics**: Performance and business metrics
-   **Logging**: Structured logging with correlation IDs
-   **Distributed Tracing**: Request flow tracking
-   **Alerting**: Proactive issue detection and notification

---

**📝 Note**: This architecture documentation provides a comprehensive overview of the backend design. For implementation details, refer to the [Development Guide](development.md).
