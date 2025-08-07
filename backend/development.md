# Backend Development Guide

> **Spring Boot Development Workflow and Best Practices**

This guide covers the development workflow, coding standards, and best practices for contributing to the Finsible Spring Boot backend.

## <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M14.7 6.3a1 1 0 0 0 0 1.4l1.6 1.6a1 1 0 0 0 1.4 0l3.77-3.77a6 6 0 0 1-7.94 7.94l-6.91 6.91a2.12 2.12 0 0 1-3-3l6.91-6.91a6 6 0 0 1 7.94-7.94l-3.76 3.76z"></path></svg> **Development Workflow**

### **Branch Strategy**

```bash
# Feature development
git checkout -b feature/user-authentication
git checkout -b feature/transaction-sync
git checkout -b enhancement/performance-optimization

# Bug fixes
git checkout -b bugfix/balance-calculation
git checkout -b hotfix/security-vulnerability
```

### **Development Process**

1. **Create Feature Branch** - Branch from `develop`
2. **Implement Changes** - Follow coding standards
3. **Write Tests** - Unit and integration tests
4. **Documentation** - Update API docs and guides
5. **Code Review** - Submit PR for review
6. **Merge** - Merge after approval and tests pass

## 🏗️ **API Development**

### **Controller Development**

```java
@RestController
@RequestMapping("/api/v1/transactions")
@Validated
public class TransactionController {

    @Autowired
    private TransactionService transactionService;

    @PostMapping
    public ResponseEntity<ApiResponse<TransactionDto>> createTransaction(
            @Valid @RequestBody CreateTransactionRequest request,
            Authentication authentication) {

        UserPrincipal user = (UserPrincipal) authentication.getPrincipal();
        TransactionDto transaction = transactionService.createTransaction(request, user.getId());

        return ResponseEntity.ok(ApiResponse.success(transaction));
    }
}
```

### **Service Layer Implementation**

```java
@Service
@Transactional
public class TransactionService {

    @Autowired
    private TransactionRepository transactionRepository;

    @Autowired
    private AccountService accountService;

    public TransactionDto createTransaction(CreateTransactionRequest request, Long userId) {
        // Validate request
        validateTransactionRequest(request, userId);

        // Create transaction entity
        Transaction transaction = mapToEntity(request, userId);

        // Update account balances
        updateAccountBalances(transaction);

        // Save transaction
        Transaction saved = transactionRepository.save(transaction);

        // Return DTO
        return mapToDto(saved);
    }
}
```

### **Repository Layer**

```java
@Repository
public interface TransactionRepository extends JpaRepository<Transaction, Long> {

    @Query("SELECT t FROM Transaction t WHERE t.user.id = :userId AND t.date BETWEEN :startDate AND :endDate")
    List<Transaction> findByUserIdAndDateRange(
        @Param("userId") Long userId,
        @Param("startDate") LocalDateTime startDate,
        @Param("endDate") LocalDateTime endDate);

    @Query("SELECT SUM(t.amount) FROM Transaction t WHERE t.account.id = :accountId AND t.type = :type")
    BigDecimal calculateBalanceByAccountAndType(
        @Param("accountId") Long accountId,
        @Param("type") TransactionType type);
}
```

## 🗃️ **Database Management**

### **Entity Design**

```java
@Entity
@Table(name = "transactions")
@EntityListeners(AuditingEntityListener.class)
public class Transaction {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, precision = 19, scale = 2)
    private BigDecimal amount;

    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private TransactionType type;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id", nullable = false)
    private User user;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "account_id")
    private Account account;

    @CreatedDate
    @Column(name = "created_at", updatable = false)
    private LocalDateTime createdAt;

    @LastModifiedDate
    @Column(name = "updated_at")
    private LocalDateTime updatedAt;

    // Constructors, getters, setters
}
```

### **Migration Scripts**

```sql
-- V1__Create_users_table.sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    google_id VARCHAR(255) UNIQUE NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(255) NOT NULL,
    picture_url VARCHAR(500),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

-- V2__Create_accounts_table.sql
CREATE TABLE accounts (
    id BIGSERIAL PRIMARY KEY,
    user_id BIGINT NOT NULL REFERENCES users(id),
    name VARCHAR(255) NOT NULL,
    account_type VARCHAR(50) NOT NULL,
    balance DECIMAL(19,2) NOT NULL DEFAULT 0.00,
    is_custom BOOLEAN NOT NULL DEFAULT false,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

## 🔒 **Security Implementation**

### **JWT Authentication**

```java
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    @Autowired
    private JwtTokenProvider tokenProvider;

    @Autowired
    private UserDetailsService userDetailsService;

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                   HttpServletResponse response,
                                   FilterChain filterChain) throws ServletException, IOException {

        String token = getTokenFromRequest(request);

        if (StringUtils.hasText(token) && tokenProvider.validateToken(token)) {
            String username = tokenProvider.getUsernameFromToken(token);
            UserDetails userDetails = userDetailsService.loadUserByUsername(username);

            UsernamePasswordAuthenticationToken authentication =
                new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
            SecurityContextHolder.getContext().setAuthentication(authentication);
        }

        filterChain.doFilter(request, response);
    }
}
```

### **API Security Configuration**

```java
@Configuration
@EnableWebSecurity
@EnableMethodSecurity(prePostEnabled = true)
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.cors().and().csrf().disable()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            .and()
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/v1/auth/**").permitAll()
                .requestMatchers("/swagger-ui/**", "/v3/api-docs/**").permitAll()
                .anyRequest().authenticated()
            )
            .addFilterBefore(jwtAuthenticationFilter(), UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }
}
```

## 🧪 **Testing Strategy**

### **Unit Testing**

```java
@ExtendWith(MockitoExtension.class)
class TransactionServiceTest {

    @Mock
    private TransactionRepository transactionRepository;

    @Mock
    private AccountService accountService;

    @InjectMocks
    private TransactionService transactionService;

    @Test
    void createTransaction_ValidRequest_ReturnsTransactionDto() {
        // Given
        CreateTransactionRequest request = createValidRequest();
        Long userId = 1L;
        Transaction savedTransaction = createMockTransaction();

        when(transactionRepository.save(any(Transaction.class))).thenReturn(savedTransaction);

        // When
        TransactionDto result = transactionService.createTransaction(request, userId);

        // Then
        assertThat(result).isNotNull();
        assertThat(result.getAmount()).isEqualTo(request.getAmount());
        verify(transactionRepository).save(any(Transaction.class));
    }
}
```

### **Integration Testing**

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@TestPropertySource(locations = "classpath:application-test.properties")
@Testcontainers
class TransactionControllerIntegrationTest {

    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:13")
            .withDatabaseName("finsible_test")
            .withUsername("test")
            .withPassword("test");

    @Autowired
    private TestRestTemplate restTemplate;

    @Test
    void createTransaction_ValidRequest_ReturnsCreatedTransaction() {
        // Given
        CreateTransactionRequest request = createValidRequest();
        HttpHeaders headers = createAuthHeaders();
        HttpEntity<CreateTransactionRequest> entity = new HttpEntity<>(request, headers);

        // When
        ResponseEntity<ApiResponse> response = restTemplate.postForEntity(
            "/api/v1/transactions", entity, ApiResponse.class);

        // Then
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(response.getBody().isSuccess()).isTrue();
    }
}
```

## 📊 **Performance Optimization**

### **Database Optimization**

```java
// Use @BatchSize for collection fetching
@OneToMany(mappedBy = "user", fetch = FetchType.LAZY)
@BatchSize(size = 20)
private List<Transaction> transactions;

// Optimize queries with @EntityGraph
@EntityGraph(attributePaths = {"account", "category"})
@Query("SELECT t FROM Transaction t WHERE t.user.id = :userId")
List<Transaction> findByUserIdWithAccountAndCategory(@Param("userId") Long userId);

// Use pagination for large datasets
@Query("SELECT t FROM Transaction t WHERE t.user.id = :userId")
Page<Transaction> findByUserId(@Param("userId") Long userId, Pageable pageable);
```

### **Caching Strategy**

```java
@Service
public class AccountService {

    @Cacheable(value = "accounts", key = "#userId")
    public List<AccountDto> getUserAccounts(Long userId) {
        return accountRepository.findByUserId(userId)
                .stream()
                .map(this::mapToDto)
                .collect(Collectors.toList());
    }

    @CacheEvict(value = "accounts", key = "#userId")
    public void invalidateUserAccountsCache(Long userId) {
        // Cache will be evicted
    }
}
```

## 📖 **API Documentation**

### **OpenAPI Configuration**

```java
@Configuration
public class OpenApiConfig {

    @Bean
    public OpenAPI customOpenAPI() {
        return new OpenAPI()
                .info(new Info()
                        .title("Finsible API")
                        .version("1.0.0")
                        .description("Financial Management API"))
                .addSecurityItem(new SecurityRequirement().addList("bearerAuth"))
                .components(new Components()
                        .addSecuritySchemes("bearerAuth",
                                new SecurityScheme()
                                        .type(SecurityScheme.Type.HTTP)
                                        .scheme("bearer")
                                        .bearerFormat("JWT")));
    }
}
```

### **Controller Documentation**

```java
@Operation(summary = "Create a new transaction",
           description = "Creates a new financial transaction for the authenticated user")
@ApiResponses(value = {
    @ApiResponse(responseCode = "200", description = "Transaction created successfully"),
    @ApiResponse(responseCode = "400", description = "Invalid request data"),
    @ApiResponse(responseCode = "401", description = "Authentication required")
})
@PostMapping
public ResponseEntity<ApiResponse<TransactionDto>> createTransaction(
        @Valid @RequestBody CreateTransactionRequest request,
        Authentication authentication) {
    // Implementation
}
```

## 🔄 **Deployment**

### **Docker Configuration**

```dockerfile
FROM openjdk:17-jdk-slim

WORKDIR /app

COPY target/finsible-backend-*.jar app.jar

EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:8080/actuator/health || exit 1

ENTRYPOINT ["java", "-jar", "app.jar"]
```

### **Production Configuration**

```yaml
# docker-compose.yml
version: '3.8'
services:
    app:
        build: .
        ports:
            - '8080:8080'
        environment:
            - SPRING_PROFILES_ACTIVE=prod
            - DATABASE_URL=jdbc:postgresql://db:5432/finsible
        depends_on:
            - db

    db:
        image: postgres:13
        environment:
            - POSTGRES_DB=finsible
            - POSTGRES_USER=finsible
            - POSTGRES_PASSWORD=${DB_PASSWORD}
        volumes:
            - postgres_data:/var/lib/postgresql/data

volumes:
    postgres_data:
```

---

**📝 Note**: This development guide provides comprehensive coverage of backend development practices. For setup instructions, see the [Setup Guide](setup.md).
