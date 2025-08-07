# Development Guide

This guide provides comprehensive information for developers working on the Finsible Android application.

## <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M14.7 6.3a1 1 0 0 0 0 1.4l1.6 1.6a1 1 0 0 0 1.4 0l3.77-3.77a6 6 0 0 1-7.94 7.94l-6.91 6.91a2.12 2.12 0 0 1-3-3l6.91-6.91a6 6 0 0 1 7.94-7.94l-3.76 3.76z"></path></svg> Development Environment

### Required Tools

| Tool           | Minimum Version  | Recommended Version | Purpose             |
| -------------- | ---------------- | ------------------- | ------------------- |
| Android Studio | Narwhal 2025.1.2 | Latest              | Primary IDE         |
| JDK            | 17               | 21                  | Kotlin compilation  |
| Android SDK    | API 26-35        | Latest              | Android development |
| Git            | 2.30+            | Latest              | Version control     |

### IDE Configuration

#### Android Studio Settings

1. **Code Style**

    - `Settings > Editor > Code Style > Kotlin`
    - Import: `.idea/codeStyles/Project.xml`

2. **Inspections**

    - Enable all Kotlin inspections
    - Enable Android Lint checks
    - Configure custom inspection profiles

3. **Plugins**
    - Kotlin Multiplatform Mobile
    - Compose Multiplatform IDE Support
    - ObjectBox Browser

#### Recommended Extensions

```json
{
    "plugins": ["Kotlin", "Android", "Compose Multiplatform IDE Support", "ObjectBox Browser", "Rainbow Brackets", "GitToolBox"]
}
```

## 📁 Project Structure

### Module Organization

```
FinsibleFrontend/
├── app/                    # Main application module
├── buildSrc/              # Build configuration
├── gradle/                # Gradle wrapper
├── docs/                  # Documentation
└── scripts/               # Development scripts
```

### Source Code Structure

```
app/src/main/java/com/itsjeel01/finsiblefrontend/
├── FinsibleApplication.kt  # Application class
├── common/                 # Shared utilities
│   ├── extensions/             # Kotlin extensions
│   ├── utils/                  # Utility classes
│   └── constants/              # App constants
├── ui/                     # Presentation layer
│   ├── components/             # Reusable UI components
│   │   ├── common/                 # Generic components
│   │   ├── forms/                  # Form components
│   │   └── charts/                 # Chart components
│   ├── screens/               # Screen-specific composables
│   │   ├── auth/                   # Authentication screens
│   │   ├── dashboard/              # Dashboard screens
│   │   ├── transactions/           # Transaction screens
│   │   └── settings/               # Settings screens
│   ├── navigation/            # Navigation configuration
│   └── theme/                # Material Design theme
├── domain/                 # Business logic layer
│   ├── model/                 # Domain models
│   ├── repository/            # Repository interfaces
│   ├── usecase/              # Business use cases
│   └── util/                 # Domain utilities
├── data/                   # Data access layer
│   ├── di/                   # Dependency injection
│   ├── local/               # Local data sources
│   │   ├── entity/              # ObjectBox entities
│   │   ├── dao/                 # Data access objects
│   │   └── database/            # Database configuration
│   ├── remote/              # Remote data sources
│   │   ├── api/                 # Retrofit interfaces
│   │   ├── dto/                 # Data transfer objects
│   │   ├── interceptor/         # Network interceptors
│   │   └── mock/                # Mock implementations
│   ├── repository/          # Repository implementations
│   └── sync/               # Background synchronization
└── di/                     # App-level DI modules
```

## 🔧 Coding Standards

### Kotlin Style Guide

#### Naming Conventions

```kotlin
// Classes: PascalCase
class TransactionRepository

// Functions and variables: camelCase
fun createTransaction()
val accountBalance: Double

// Constants: SCREAMING_SNAKE_CASE
companion object {
    const val MAX_TRANSACTION_AMOUNT = 999_999.99
}

// Private properties: underscore prefix
class ViewModel {
    private val _uiState = MutableStateFlow(UiState())
    val uiState = _uiState.asStateFlow()
}
```

#### Code Organization

```kotlin
class ExampleClass {
    // 1. Companion object
    companion object {
        const val CONSTANT = "value"
    }

    // 2. Properties
    private val property: String

    // 3. Init blocks
    init {
        // initialization
    }

    // 4. Public methods
    fun publicMethod() { }

    // 5. Private methods
    private fun privateMethod() { }
}
```

#### Function Structure

```kotlin
// Prefer expression syntax for simple functions
fun isValid(): Boolean = amount > 0 && description.isNotBlank()

// Use block syntax for complex functions
fun processTransaction(transaction: Transaction): Result<Transaction> {
    return try {
        validateTransaction(transaction)
        saveToDatabase(transaction)
        Result.success(transaction)
    } catch (e: Exception) {
        Result.failure(e)
    }
}
```

### Jetpack Compose Guidelines

#### Composable Structure

```kotlin
@Composable
fun TransactionCard(
    transaction: Transaction,
    onEdit: (Transaction) -> Unit,
    onDelete: (Transaction) -> Unit,
    modifier: Modifier = Modifier
) {
    // State declarations first
    var isExpanded by remember { mutableStateOf(false) }

    // UI content
    Card(
        modifier = modifier.fillMaxWidth(),
        onClick = { isExpanded = !isExpanded }
    ) {
        // Content
    }
}
```

#### State Management

```kotlin
// ViewModel state pattern
class TransactionViewModel : ViewModel() {
    private val _uiState = MutableStateFlow(TransactionUiState())
    val uiState: StateFlow<TransactionUiState> = _uiState.asStateFlow()

    fun updateTransaction(transaction: Transaction) {
        _uiState.update { currentState ->
            currentState.copy(
                transaction = transaction,
                isLoading = false
            )
        }
    }
}

// Compose state consumption
@Composable
fun TransactionScreen(viewModel: TransactionViewModel = hiltViewModel()) {
    val uiState by viewModel.uiState.collectAsState()

    LaunchedEffect(Unit) {
        viewModel.loadTransaction()
    }

    TransactionContent(
        uiState = uiState,
        onEvent = viewModel::handleEvent
    )
}
```

#### Performance Best Practices

```kotlin
// Use remember for expensive calculations
@Composable
fun ExpensiveComponent(data: List<Item>) {
    val processedData = remember(data) {
        data.map { it.processExpensively() }
    }

    LazyColumn {
        items(
            items = processedData,
            key = { it.id } // Always provide stable keys
        ) { item ->
            ItemCard(item = item)
        }
    }
}

// Avoid recomposition with stable parameters
@Composable
fun StableComponent(
    data: ImmutableList<Item>, // Use stable collections
    onClick: (Item) -> Unit
) {
    // Component content
}
```

## 🗄️ Database Development

### ObjectBox Entity Design

```kotlin
@Entity
data class TransactionEntity(
    @Id var id: Long = 0,

    // Required fields
    var amount: Double,
    var description: String,
    var date: Date,
    var type: String, // INCOME, EXPENSE, TRANSFER

    // Foreign keys
    var categoryId: Long,
    var accountId: Long,
    var fromAccountId: Long? = null, // For transfers
    var toAccountId: Long? = null,   // For transfers

    // Optional fields
    var notes: String? = null,
    var tags: String? = null, // JSON array as string
    var location: String? = null, // JSON object as string

    // Metadata
    var createdAt: Date = Date(),
    var updatedAt: Date = Date(),
    var syncStatus: String = SyncStatus.PENDING.name,
    var serverId: Long? = null
) {
    // Relations
    lateinit var category: ToOne<CategoryEntity>
    lateinit var account: ToOne<AccountEntity>
}
```

### Repository Pattern

```kotlin
@Singleton
class TransactionRepositoryImpl @Inject constructor(
    private val localDataSource: TransactionLocalDataSource,
    private val remoteDataSource: TransactionRemoteDataSource,
    private val syncEngine: SyncEngine
) : TransactionRepository {

    override suspend fun createTransaction(transaction: Transaction): Result<Transaction> {
        return try {
            // 1. Save locally first (offline-first)
            val entity = transaction.toEntity()
            val savedEntity = localDataSource.insertTransaction(entity)

            // 2. Schedule background sync
            syncEngine.scheduleTransactionSync(savedEntity.id)

            // 3. Return domain model
            Result.success(savedEntity.toDomainModel())
        } catch (e: Exception) {
            Result.failure(e)
        }
    }

    override fun getAllTransactions(): Flow<List<Transaction>> {
        return localDataSource.getAllTransactions()
            .map { entities ->
                entities.map { it.toDomainModel() }
            }
    }
}
```

## 🌐 Network Development

### API Service Definition

```kotlin
interface FinsibleApiService {

    @POST("/auth/googleSignIn")
    suspend fun googleSignIn(
        @Body request: GoogleSignInRequest
    ): ApiResponse<AuthResponse>

    @GET("/transactions")
    suspend fun getTransactions(
        @Query("page") page: Int = 1,
        @Query("limit") limit: Int = 50,
        @Query("type") type: String? = null,
        @Query("from_date") fromDate: String? = null,
        @Query("to_date") toDate: String? = null
    ): ApiResponse<TransactionsResponse>

    @POST("/transactions")
    suspend fun createTransaction(
        @Body request: CreateTransactionRequest
    ): ApiResponse<TransactionResponse>
}
```

### Error Handling

```kotlin
sealed class ApiResult<T> {
    data class Success<T>(val data: T) : ApiResult<T>()
    data class Error<T>(val exception: Exception) : ApiResult<T>()
    data class NetworkError<T>(val code: Int, val message: String) : ApiResult<T>()
}

suspend fun <T> safeApiCall(apiCall: suspend () -> T): ApiResult<T> {
    return try {
        ApiResult.Success(apiCall())
    } catch (e: HttpException) {
        ApiResult.NetworkError(e.code(), e.message())
    } catch (e: Exception) {
        ApiResult.Error(e)
    }
}
```

## 🧪 Testing Guidelines

### Unit Testing

```kotlin
class TransactionRepositoryTest {

    @Mock
    private lateinit var localDataSource: TransactionLocalDataSource

    @Mock
    private lateinit var remoteDataSource: TransactionRemoteDataSource

    @Mock
    private lateinit var syncEngine: SyncEngine

    private lateinit var repository: TransactionRepositoryImpl

    @Before
    fun setup() {
        repository = TransactionRepositoryImpl(
            localDataSource = localDataSource,
            remoteDataSource = remoteDataSource,
            syncEngine = syncEngine
        )
    }

    @Test
    fun `createTransaction saves locally and schedules sync`() = runTest {
        // Given
        val transaction = createTestTransaction()
        val entity = transaction.toEntity()
        whenever(localDataSource.insertTransaction(any())).thenReturn(entity)

        // When
        val result = repository.createTransaction(transaction)

        // Then
        assertTrue(result.isSuccess)
        verify(localDataSource).insertTransaction(any())
        verify(syncEngine).scheduleTransactionSync(entity.id)
    }
}
```

### Compose Testing

```kotlin
class TransactionScreenTest {

    @get:Rule
    val composeTestRule = createComposeRule()

    @Test
    fun `displays transaction list correctly`() {
        // Given
        val transactions = listOf(
            createTestTransaction(description = "Coffee"),
            createTestTransaction(description = "Groceries")
        )

        // When
        composeTestRule.setContent {
            TransactionList(transactions = transactions)
        }

        // Then
        composeTestRule.onNodeWithText("Coffee").assertIsDisplayed()
        composeTestRule.onNodeWithText("Groceries").assertIsDisplayed()
    }

    @Test
    fun `transaction creation flow works`() {
        composeTestRule.setContent {
            CreateTransactionScreen()
        }

        // Fill form
        composeTestRule.onNodeWithText("Amount").performTextInput("100.00")
        composeTestRule.onNodeWithText("Description").performTextInput("Test expense")

        // Submit
        composeTestRule.onNodeWithText("Save").performClick()

        // Verify
        composeTestRule.onNodeWithText("Transaction created").assertIsDisplayed()
    }
}
```

## 🚀 Build & Deployment

### Gradle Configuration

#### Build Types

```kotlin
android {
    buildTypes {
        debug {
            isDebuggable = true
            applicationIdSuffix = ".debug"
            buildConfigField("String", "BASE_URL", "\"https://dev.finsible.app\"")
            buildConfigField("boolean", "USE_MOCK_SERVER", "true")
            isMinifyEnabled = false
        }

        staging {
            initWith(debug)
            applicationIdSuffix = ".staging"
            buildConfigField("String", "BASE_URL", "\"https://staging.finsible.app\"")
            buildConfigField("boolean", "USE_MOCK_SERVER", "false")
            isMinifyEnabled = true
            proguardFiles("proguard-android-optimize.txt", "proguard-rules.pro")
        }

        release {
            isDebuggable = false
            buildConfigField("String", "BASE_URL", "\"https://finsible.app\"")
            buildConfigField("boolean", "USE_MOCK_SERVER", "false")
            isMinifyEnabled = true
            isShrinkResources = true
            proguardFiles("proguard-android-optimize.txt", "proguard-rules.pro")
            signingConfig = signingConfigs.getByName("release")
        }
    }
}
```

#### Dependencies Management

```kotlin
// Use version catalogs for dependencies
dependencyResolutionManagement {
    versionCatalogs {
        libs {
            library("hilt-android", "com.google.dagger", "hilt-android").version("2.48")
            library("compose-bom", "androidx.compose", "compose-bom").version("2024.02.00")
            bundle("compose", ["compose-ui", "compose-tooling", "compose-material3"])
        }
    }
}
```

### CI/CD Pipeline

#### GitHub Actions

```yaml
name: Android CI

on:
    push:
        branches: [main, develop]
    pull_request:
        branches: [main]

jobs:
    test:
        runs-on: ubuntu-latest
        steps:
            - uses: actions/checkout@v3

            - name: Set up JDK 17
              uses: actions/setup-java@v3
              with:
                  java-version: '17'
                  distribution: 'temurin'

            - name: Cache Gradle packages
              uses: actions/cache@v3
              with:
                  path: |
                      ~/.gradle/caches
                      ~/.gradle/wrapper
                  key: ${{ runner.os }}-gradle-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}

            - name: Run tests
              run: ./gradlew test

            - name: Generate test report
              run: ./gradlew jacocoTestReport

            - name: Upload coverage to Codecov
              uses: codecov/codecov-action@v3
```

## 🐛 Debugging

### Logging Strategy

```kotlin
// Use structured logging
object Logger {
    private const val TAG = "Finsible"

    fun d(message: String, tag: String = TAG) {
        if (BuildConfig.DEBUG) {
            Log.d(tag, message)
        }
    }

    fun e(message: String, throwable: Throwable? = null, tag: String = TAG) {
        Log.e(tag, message, throwable)
        // Send to crash reporting in release builds
        if (!BuildConfig.DEBUG) {
            // Crashlytics.recordException(throwable)
        }
    }
}

// Usage
Logger.d("Transaction created: ${transaction.id}")
Logger.e("Failed to sync transaction", exception)
```

### Database Debugging

```kotlin
// ObjectBox Browser integration
if (BuildConfig.DEBUG) {
    val started = AndroidObjectBrowser(boxStore).start(this)
    Log.i("ObjectBoxBrowser", "Started: $started")
}
```

### Network Debugging

```kotlin
// OkHttp logging interceptor
if (BuildConfig.DEBUG) {
    val loggingInterceptor = HttpLoggingInterceptor().apply {
        level = HttpLoggingInterceptor.Level.BODY
    }
    okHttpClient.addInterceptor(loggingInterceptor)
}
```

## 📋 Code Review Guidelines

### Checklist

#### Architecture & Design

-   [ ] Follows MVVM architecture pattern
-   [ ] Proper separation of concerns
-   [ ] Appropriate use of dependency injection
-   [ ] Repository pattern correctly implemented

#### Code Quality

-   [ ] Code is readable and well-documented
-   [ ] Follows Kotlin coding standards
-   [ ] No code smells or anti-patterns
-   [ ] Proper error handling

#### Performance

-   [ ] No memory leaks
-   [ ] Efficient database queries
-   [ ] Proper use of coroutines
-   [ ] Optimized for main thread

#### Testing

-   [ ] Unit tests for business logic
-   [ ] UI tests for critical flows
-   [ ] Edge cases covered
-   [ ] Mock dependencies properly

#### Security

-   [ ] No hardcoded secrets
-   [ ] Proper input validation
-   [ ] Secure data storage
-   [ ] Network security implemented

### Review Process

1. **Self Review**: Review your own PR before submitting
2. **Automated Checks**: Ensure CI passes
3. **Peer Review**: At least one team member review
4. **Testing**: Manual testing of critical paths
5. **Documentation**: Update docs if needed

## 🔧 Development Tools

### Useful Gradle Tasks

```bash
# Clean build
./gradlew clean

# Debug build
./gradlew assembleDebug

# Run unit tests
./gradlew test

# Run instrumentation tests
./gradlew connectedAndroidTest

# Generate test coverage report
./gradlew jacocoTestReport

# Lint check
./gradlew lint

# Dependency updates
./gradlew dependencyUpdates
```

### IDE Live Templates

Create custom live templates for common patterns:

```kotlin
// Composable template
@Composable
fun $NAME$(
    modifier: Modifier = Modifier
) {
    $END$
}

// ViewModel template
class $NAME$ViewModel @Inject constructor(

) : ViewModel() {

    private val _uiState = MutableStateFlow($NAME$UiState())
    val uiState: StateFlow<$NAME$UiState> = _uiState.asStateFlow()

    $END$
}
```

### Development Scripts

```bash
#!/bin/bash
# scripts/setup-dev.sh
echo "Setting up development environment..."

# Install Git hooks
cp scripts/pre-commit .git/hooks/
chmod +x .git/hooks/pre-commit

# Create secrets file if not exists
if [ ! -f secrets.properties ]; then
    cp secrets.properties.example secrets.properties
    echo "Please configure secrets.properties"
fi

echo "Development environment ready!"
```

This development guide provides the foundation for consistent, high-quality development on the Finsible Android application. Follow these guidelines to maintain code quality and team productivity.
