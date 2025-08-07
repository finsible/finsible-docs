# Mock Server Setup

Since the Finsible backend is not currently deployed, the Android app uses mock responses for development and testing. This guide explains how to configure and use the mock server functionality.

## Overview

The mock server system provides:

-   ✅ **Realistic API responses** for all endpoints
-   ✅ **Configurable delays** to simulate network conditions
-   ✅ **Error simulation** for testing error handling
-   ✅ **Offline development** without backend dependency
-   ✅ **Consistent test data** for predictable testing

## Configuration

### Enable Mock Mode

Add to `secrets.properties`:

```properties
# Mock Server Configuration
USE_MOCK_SERVER=true
MOCK_DELAY_MS=1000
MOCK_ERROR_RATE=0.1
MOCK_NETWORK_ERROR_RATE=0.05
```

### Build Configuration

In `build.gradle.kts`:

```kotlin
android {
    buildTypes {
        debug {
            buildConfigField("boolean", "USE_MOCK_SERVER", "true")
            buildConfigField("long", "MOCK_DELAY_MS", "1000L")
        }
        release {
            buildConfigField("boolean", "USE_MOCK_SERVER", "false")
            buildConfigField("long", "MOCK_DELAY_MS", "0L")
        }
    }
}
```

## Mock Response Structure

### Directory Layout

```
app/src/main/assets/mock_responses/
├── auth/
│   ├── google_signin_success.json
│   ├── google_signin_error.json
│   └── refresh_token.json
├── accounts/
│   ├── accounts_list.json
│   ├── account_create_success.json
│   └── account_update_success.json
├── categories/
│   ├── income_categories.json
│   ├── expense_categories.json
│   ├── transfer_categories.json
│   └── category_create_success.json
├── transactions/
│   ├── transactions_list.json
│   ├── transaction_create_success.json
│   ├── transaction_update_success.json
│   └── transaction_delete_success.json
├── user/
│   ├── profile.json
│   └── profile_update_success.json
└── errors/
    ├── validation_error.json
    ├── unauthorized_error.json
    └── server_error.json
```

### Response Files

#### Authentication Responses

**`auth/google_signin_success.json`**

```json
{
    "success": true,
    "message": "You are successfully logged in.",
    "data": {
        "isNewUser": false,
        "userId": "100593673127133532461",
        "email": "user@example.com",
        "name": "Test User",
        "picture": "https://lh3.googleusercontent.com/a/default-user=s96-c",
        "accountCreated": "2025-01-01T10:00:00.000Z",
        "lastLoggedIn": "2025-08-08T12:00:00.000Z",
        "jwt": "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxMDA1OTM2NzMxMjcxMzM1MzI0NjEiLCJpYXQiOjE3NDEwMjU1MzEsImV4cCI6MTc1NjU3NzUzMX0.7ZbfDjdb4t5AZCHnpHrCYSTYolI8CaTB9D6E6-r6mWU6UAE51WHzRwSEs-BjX0dIwrL1DyQ-Z1zDahETrdME7A"
    },
    "cache": false,
    "cacheTtlMinutes": null
}
```

#### Transaction Responses

**`transactions/transaction_create_success.json`**

```json
{
    "success": true,
    "message": "Transaction created successfully",
    "data": {
        "transaction": {
            "id": 123456,
            "type": "EXPENSE",
            "amount": 150.75,
            "description": "Grocery shopping",
            "category": {
                "id": 101001,
                "name": "Groceries",
                "color": "orange",
                "domain": "ESSENTIALS"
            },
            "account": {
                "id": 901001,
                "name": "Cash",
                "balance": 4849.25
            },
            "date": "2025-08-08T14:30:00Z",
            "tags": ["groceries", "essential"],
            "createdAt": "2025-08-08T14:30:15Z",
            "updatedAt": "2025-08-08T14:30:15Z",
            "syncStatus": "SYNCED"
        },
        "updatedBalances": [
            {
                "accountId": 901001,
                "previousBalance": 5000.0,
                "newBalance": 4849.25,
                "change": -150.75
            }
        ]
    },
    "cache": false,
    "cacheTtlMinutes": null
}
```

## Mock Interceptor Implementation

### Base Mock Interceptor

```kotlin
@Singleton
class MockInterceptor @Inject constructor(
    @ApplicationContext private val context: Context
) : Interceptor {

    override fun intercept(chain: Interceptor.Chain): Response {
        if (!BuildConfig.USE_MOCK_SERVER) {
            return chain.proceed(chain.request())
        }

        val request = chain.request()
        val mockResponse = generateMockResponse(request)

        // Simulate network delay
        if (BuildConfig.MOCK_DELAY_MS > 0) {
            Thread.sleep(BuildConfig.MOCK_DELAY_MS)
        }

        return mockResponse
    }

    private fun generateMockResponse(request: Request): Response {
        val path = request.url.encodedPath
        val method = request.method

        // Simulate random errors
        if (shouldSimulateError()) {
            return createErrorResponse(request)
        }

        val responseBody = getMockResponseBody(method, path, request)

        return Response.Builder()
            .request(request)
            .protocol(Protocol.HTTP_1_1)
            .code(200)
            .message("OK")
            .body(responseBody.toResponseBody("application/json".toMediaType()))
            .addHeader("Content-Type", "application/json")
            .build()
    }

    private fun getMockResponseBody(method: String, path: String, request: Request): String {
        return when {
            // Authentication endpoints
            path == "/auth/googleSignIn" && method == "POST" -> {
                if (shouldSimulateAuthError()) {
                    loadMockFile("auth/google_signin_error.json")
                } else {
                    loadMockFile("auth/google_signin_success.json")
                }
            }

            // Category endpoints
            path == "/categories" && method == "GET" -> {
                val type = request.url.queryParameter("type") ?: "expense"
                loadMockFile("categories/${type}_categories.json")
            }

            // Account endpoints
            path == "/accounts/all" && method == "GET" -> {
                loadMockFile("accounts/accounts_list.json")
            }

            // Transaction endpoints
            path == "/transactions" && method == "POST" -> {
                loadMockFile("transactions/transaction_create_success.json")
            }
            path == "/transactions" && method == "GET" -> {
                loadMockFile("transactions/transactions_list.json")
            }

            // User endpoints
            path == "/user/profile" && method == "GET" -> {
                loadMockFile("user/profile.json")
            }

            else -> {
                """{"success": false, "message": "Mock endpoint not implemented", "data": null}"""
            }
        }
    }

    private fun loadMockFile(fileName: String): String {
        return try {
            context.assets.open("mock_responses/$fileName")
                .bufferedReader()
                .use { it.readText() }
        } catch (e: Exception) {
            """{"success": false, "message": "Mock file not found: $fileName", "data": null}"""
        }
    }

    private fun shouldSimulateError(): Boolean {
        return Random.nextFloat() < MockConfig.ERROR_RATE
    }

    private fun shouldSimulateAuthError(): Boolean {
        return Random.nextFloat() < 0.2f // 20% chance of auth error
    }

    private fun createErrorResponse(request: Request): Response {
        val errorCodes = listOf(400, 401, 403, 404, 422, 500)
        val errorCode = errorCodes.random()

        val errorBody = when (errorCode) {
            401 -> loadMockFile("errors/unauthorized_error.json")
            422 -> loadMockFile("errors/validation_error.json")
            500 -> loadMockFile("errors/server_error.json")
            else -> """{"success": false, "message": "Simulated error", "data": null}"""
        }

        return Response.Builder()
            .request(request)
            .protocol(Protocol.HTTP_1_1)
            .code(errorCode)
            .message("Simulated Error")
            .body(errorBody.toResponseBody("application/json".toMediaType()))
            .build()
    }
}
```

### Dynamic Response Generation

```kotlin
class DynamicMockInterceptor @Inject constructor(
    @ApplicationContext private val context: Context,
    private val mockDataGenerator: MockDataGenerator
) : Interceptor {

    override fun intercept(chain: Interceptor.Chain): Response {
        if (!BuildConfig.USE_MOCK_SERVER) {
            return chain.proceed(chain.request())
        }

        val request = chain.request()
        val response = when {
            request.isTransactionCreate() -> {
                generateTransactionCreateResponse(request)
            }
            request.isAccountUpdate() -> {
                generateAccountUpdateResponse(request)
            }
            else -> {
                getStaticMockResponse(request)
            }
        }

        return response
    }

    private fun generateTransactionCreateResponse(request: Request): Response {
        val requestBody = request.body?.let { body ->
            val buffer = Buffer()
            body.writeTo(buffer)
            buffer.readUtf8()
        }

        val transactionRequest = Json.decodeFromString<CreateTransactionRequest>(requestBody ?: "{}")
        val mockTransaction = mockDataGenerator.generateTransaction(transactionRequest)

        val responseJson = Json.encodeToString(ApiResponse.success(mockTransaction))

        return createSuccessResponse(request, responseJson)
    }
}

@Singleton
class MockDataGenerator @Inject constructor() {

    fun generateTransaction(request: CreateTransactionRequest): TransactionResponse {
        return TransactionResponse(
            transaction = Transaction(
                id = Random.nextLong(100000, 999999),
                type = request.type,
                amount = request.amount,
                description = request.description,
                category = getCategoryById(request.categoryId),
                account = getAccountById(request.accountId ?: 0),
                date = request.date,
                tags = request.tags,
                createdAt = Clock.System.now().toString(),
                updatedAt = Clock.System.now().toString(),
                syncStatus = "SYNCED"
            ),
            updatedBalances = listOf(
                BalanceUpdate(
                    accountId = request.accountId ?: 0,
                    previousBalance = 5000.0,
                    newBalance = 5000.0 - request.amount,
                    change = -request.amount
                )
            )
        )
    }
}
```

## Testing with Mock Server

### Unit Tests

```kotlin
@Test
fun `mock server returns success response`() = runTest {
    // Given
    val mockInterceptor = MockInterceptor(context)
    val okHttp = OkHttpClient.Builder()
        .addInterceptor(mockInterceptor)
        .build()

    val retrofit = Retrofit.Builder()
        .baseUrl("https://finsible.app/")
        .client(okHttp)
        .addConverterFactory(KotlinxSerializationConverterFactory.create())
        .build()

    val apiService = retrofit.create(FinsibleApiService::class.java)

    // When
    val response = apiService.getAllAccounts()

    // Then
    assertTrue(response.success)
    assertNotNull(response.data)
}
```

### Integration Tests

```kotlin
@RunWith(AndroidJUnit4::class)
class MockServerIntegrationTest {

    @get:Rule
    val composeTestRule = createComposeRule()

    @Before
    fun setup() {
        // Enable mock server for tests
        MockConfig.USE_MOCK_SERVER = true
    }

    @Test
    fun loginFlowWithMockServer() {
        composeTestRule.setContent {
            FinsibleApp()
        }

        // Tap login button
        composeTestRule.onNodeWithText("Sign in with Google").performClick()

        // Verify navigation to dashboard (mock auth success)
        composeTestRule.waitForIdle()
        composeTestRule.onNodeWithText("Dashboard").assertIsDisplayed()
    }
}
```

## App Inspection Alternative

Use Android Studio's Network Inspector for runtime mocking:

### Setup App Inspection Rules

1. **Open App Inspection**

    ```
    View → Tool Windows → App Inspection
    ```

2. **Add Network Rule**

    - Select your app process
    - Open Network Inspector tab
    - Click "Rules" → "Add Rule"

3. **Configure Rule**
    ```
    URL Pattern: https://finsible.app/auth/googleSignIn
    Method: POST
    Response Code: 200
    Response Body: {JSON response here}
    ```

### Dynamic Rule Configuration

```kotlin
// Configure rules programmatically (for testing)
object NetworkInspectorRules {

    fun setupAuthRules() {
        addRule(
            pattern = "*/auth/googleSignIn",
            method = "POST",
            response = loadAssetFile("mock_responses/auth/google_signin_success.json")
        )
    }

    fun setupCategoryRules() {
        addRule(
            pattern = "*/categories*",
            method = "GET",
            response = { request ->
                val type = extractQueryParam(request, "type")
                loadAssetFile("mock_responses/categories/${type}_categories.json")
            }
        )
    }
}
```

## Mock Data Management

### Version Control

```gitignore
# Include mock responses in version control
!/app/src/main/assets/mock_responses/
```

### Data Consistency

```kotlin
object MockDataStore {
    private val users = mutableMapOf<String, User>()
    private val transactions = mutableMapOf<String, MutableList<Transaction>>()

    fun addTransaction(userId: String, transaction: Transaction) {
        transactions.getOrPut(userId) { mutableListOf() }.add(transaction)
    }

    fun getUserTransactions(userId: String): List<Transaction> {
        return transactions[userId] ?: emptyList()
    }

    // Maintain state across mock requests
    fun updateAccountBalance(accountId: String, change: Double) {
        // Update account balance in mock data
    }
}
```

### Mock Data Seeding

```kotlin
class MockDataSeeder {

    fun seedInitialData() {
        // Create sample users
        createSampleUser()

        // Create sample transactions
        createSampleTransactions()

        // Create sample categories
        createSampleCategories()
    }

    private fun createSampleTransactions() {
        val transactions = listOf(
            Transaction(
                id = 1,
                type = "EXPENSE",
                amount = 45.50,
                description = "Coffee shop",
                categoryId = 101004, // Food & Dining
                date = "2025-08-07T09:15:00Z"
            ),
            Transaction(
                id = 2,
                type = "INCOME",
                amount = 5000.00,
                description = "Monthly salary",
                categoryId = 201001, // Salary
                date = "2025-08-01T00:00:00Z"
            )
        )

        // Save to mock data store
        transactions.forEach { transaction ->
            MockDataStore.addTransaction("test-user", transaction)
        }
    }
}
```

## Configuration Options

### Mock Server Settings

```kotlin
object MockConfig {
    // Basic configuration
    var USE_MOCK_SERVER = BuildConfig.USE_MOCK_SERVER
    var DELAY_MS = BuildConfig.MOCK_DELAY_MS

    // Error simulation
    var ERROR_RATE = 0.1f // 10% error rate
    var NETWORK_ERROR_RATE = 0.05f // 5% network errors

    // Response customization
    var INCLUDE_REQUEST_ID = true
    var INCLUDE_TIMESTAMP = true
    var REALISTIC_BALANCES = true

    // Performance settings
    var CACHE_RESPONSES = true
    var LOG_REQUESTS = BuildConfig.DEBUG
}
```

### Environment-Specific Mocking

```kotlin
sealed class MockEnvironment(
    val errorRate: Float,
    val delayMs: Long,
    val realisticData: Boolean
) {
    object Development : MockEnvironment(
        errorRate = 0.1f,
        delayMs = 1000L,
        realisticData = true
    )

    object Testing : MockEnvironment(
        errorRate = 0.0f,
        delayMs = 0L,
        realisticData = false
    )

    object Demo : MockEnvironment(
        errorRate = 0.02f,
        delayMs = 500L,
        realisticData = true
    )
}
```

## Best Practices

### 📝 Mock Response Guidelines

1. **Realistic Data**: Use realistic values and formats
2. **Consistent IDs**: Maintain ID consistency across responses
3. **Error Scenarios**: Include various error response files
4. **Data Relationships**: Ensure foreign key relationships are valid
5. **Timestamps**: Use realistic timestamp values

### 🔧 Development Workflow

1. **Start with Mocks**: Begin development with mock responses
2. **Gradual Migration**: Replace mocks with real API calls incrementally
3. **Error Testing**: Test error scenarios using mock error responses
4. **Performance Testing**: Use delay simulation for performance testing
5. **Documentation**: Keep mock responses updated with API documentation

### 🧪 Testing Strategy

1. **Unit Tests**: Test components in isolation with mocked dependencies
2. **Integration Tests**: Test complete flows with mock server
3. **Error Handling**: Test error scenarios with simulated failures
4. **Performance**: Test with various network delay configurations
5. **Offline Scenarios**: Test app behavior with network errors

## Troubleshooting

### Common Issues

**Mock responses not loading**

-   Check file paths in assets folder
-   Verify JSON syntax in response files
-   Enable logging to see interceptor activity

**Inconsistent data**

-   Maintain data relationships in mock responses
-   Use consistent ID formats across files
-   Update related data when simulating state changes

**Performance issues**

-   Reduce mock delay for faster testing
-   Cache frequently accessed mock responses
-   Optimize large JSON response files

### Debug Tools

```kotlin
class MockDebugger {

    fun logMockRequest(request: Request) {
        if (MockConfig.LOG_REQUESTS) {
            Log.d("MockServer", "Request: ${request.method} ${request.url}")
        }
    }

    fun validateMockResponse(response: String): Boolean {
        return try {
            Json.parseToJsonElement(response)
            true
        } catch (e: Exception) {
            Log.e("MockServer", "Invalid JSON response: $e")
            false
        }
    }
}
```

This mock server setup enables comprehensive development and testing of the Finsible Android app without requiring a deployed backend server.
