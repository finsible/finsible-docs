# Create Transaction

<div class="api-endpoint">
  <span class="api-method post">POST</span>
  <strong>Host:</strong> <code>www.finsible.app</code><br>
  <strong>Path:</strong> <code>/transactions</code><br>
  <strong>Status:</strong> <code>201 Created</code><br>
  <strong>Authentication:</strong> Required (JWT Bearer Token)
</div>

## Overview

Creates a new financial transaction (income, expense, or transfer) in the user's account. This endpoint supports all transaction types and automatically updates account balances.

## Request Headers

```http
Authorization: Bearer <jwt_token>
Content-Type: application/json
```

## Request Body

### Income Transaction

```json
{
    "type": "INCOME",
    "amount": 5000.0,
    "description": "Monthly salary",
    "categoryId": 201001,
    "accountId": 901001,
    "date": "2025-08-08T10:30:00Z",
    "tags": ["salary", "monthly"],
    "notes": "August 2025 salary payment"
}
```

### Expense Transaction

```json
{
    "type": "EXPENSE",
    "amount": 150.75,
    "description": "Grocery shopping",
    "categoryId": 101001,
    "accountId": 901001,
    "date": "2025-08-08T14:30:00Z",
    "tags": ["groceries", "essential"],
    "location": {
        "name": "Supermarket XYZ",
        "latitude": 40.7128,
        "longitude": -74.006
    }
}
```

### Transfer Transaction

```json
{
    "type": "TRANSFER",
    "amount": 1000.0,
    "description": "Investment transfer",
    "categoryId": 302001,
    "fromAccountId": 901001,
    "toAccountId": 901002,
    "date": "2025-08-08T16:00:00Z",
    "fee": 10.0
}
```

## Request Fields

| Field           | Type     | Required | Description                                       |
| --------------- | -------- | -------- | ------------------------------------------------- |
| `type`          | `string` | Yes      | Transaction type: `INCOME`, `EXPENSE`, `TRANSFER` |
| `amount`        | `number` | Yes      | Transaction amount (positive value)               |
| `description`   | `string` | Yes      | Transaction description                           |
| `categoryId`    | `number` | Yes      | Category ID from categories API                   |
| `accountId`     | `number` | Yes\*    | Account ID (for income/expense)                   |
| `fromAccountId` | `number` | Yes\*    | Source account ID (for transfers)                 |
| `toAccountId`   | `number` | Yes\*    | Destination account ID (for transfers)            |
| `date`          | `string` | Yes      | Transaction date (ISO 8601 format)                |
| `tags`          | `array`  | No       | Array of tag strings                              |
| `notes`         | `string` | No       | Additional notes                                  |
| `location`      | `object` | No       | Location information                              |
| `fee`           | `number` | No       | Transaction fee (for transfers)                   |

\*Required fields depend on transaction type

## Response Schema

### Success Response

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

### Error Response

```json
{
    "success": false,
    "message": "Validation failed",
    "data": {
        "errors": [
            {
                "field": "amount",
                "code": "INVALID_AMOUNT",
                "message": "Amount must be greater than 0"
            },
            {
                "field": "categoryId",
                "code": "INVALID_CATEGORY",
                "message": "Category not found or not valid for transaction type"
            }
        ]
    },
    "cache": false,
    "cacheTtlMinutes": null
}
```

## Response Fields

| Field                          | Type     | Description                           |
| ------------------------------ | -------- | ------------------------------------- |
| `data.transaction.id`          | `number` | Unique transaction identifier         |
| `data.transaction.type`        | `string` | Transaction type                      |
| `data.transaction.amount`      | `number` | Transaction amount                    |
| `data.transaction.description` | `string` | Transaction description               |
| `data.transaction.category`    | `object` | Category information                  |
| `data.transaction.account`     | `object` | Account information (updated balance) |
| `data.transaction.date`        | `string` | Transaction date                      |
| `data.transaction.tags`        | `array`  | Array of tags                         |
| `data.transaction.createdAt`   | `string` | Creation timestamp                    |
| `data.transaction.updatedAt`   | `string` | Last update timestamp                 |
| `data.transaction.syncStatus`  | `string` | Sync status with server               |
| `data.updatedBalances`         | `array`  | Balance changes for affected accounts |

## Validation Rules

### Amount Validation

-   Must be a positive number
-   Maximum precision: 2 decimal places
-   Minimum value: 0.01
-   Maximum value: 99,999,999.99

### Description Validation

-   Required field
-   Minimum length: 1 character
-   Maximum length: 255 characters
-   Special characters allowed

### Date Validation

-   Must be valid ISO 8601 format
-   Cannot be more than 30 days in the future
-   Cannot be more than 10 years in the past

### Category Validation

-   Must exist in the system
-   Must match transaction type:
    -   Income categories (200xxx) for INCOME transactions
    -   Expense categories (100xxx) for EXPENSE transactions
    -   Transfer categories (300xxx) for TRANSFER transactions

### Account Validation

-   Must belong to the authenticated user
-   Must be active (not deleted)
-   For transfers: source and destination accounts must be different

## Usage Examples

### Android Kotlin Implementation

```kotlin
// Data classes
data class CreateTransactionRequest(
    val type: TransactionType,
    val amount: Double,
    val description: String,
    val categoryId: Long,
    val accountId: Long? = null,
    val fromAccountId: Long? = null,
    val toAccountId: Long? = null,
    val date: String, // ISO 8601 format
    val tags: List<String> = emptyList(),
    val notes: String? = null,
    val location: LocationData? = null,
    val fee: Double? = null
)

data class LocationData(
    val name: String,
    val latitude: Double,
    val longitude: Double
)

enum class TransactionType {
    INCOME, EXPENSE, TRANSFER
}

// Repository method
class TransactionRepository @Inject constructor(
    private val apiService: FinsibleApiService,
    private val localDataSource: TransactionLocalDataSource
) {
    suspend fun createTransaction(request: CreateTransactionRequest): Result<Transaction> {
        return try {
            // Save locally first (offline-first)
            val localTransaction = localDataSource.insertPendingTransaction(request)

            // Try to sync with server
            val response = apiService.createTransaction(request)

            if (response.success) {
                // Update local transaction with server ID
                localDataSource.updateTransactionWithServerId(
                    localTransaction.id,
                    response.data.transaction.id
                )
                Result.success(response.data.transaction)
            } else {
                Result.failure(ApiException(response.message))
            }
        } catch (e: Exception) {
            // Keep local transaction for later sync
            Result.failure(e)
        }
    }
}
```

### Jetpack Compose Form

```kotlin
@Composable
fun CreateTransactionForm(
    onTransactionCreated: (Transaction) -> Unit,
    viewModel: CreateTransactionViewModel = hiltViewModel()
) {
    val uiState by viewModel.uiState.collectAsState()

    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        // Transaction Type Selection
        TransactionTypeSelector(
            selectedType = uiState.type,
            onTypeSelected = viewModel::setTransactionType
        )

        // Amount Input
        OutlinedTextField(
            value = uiState.amount,
            onValueChange = viewModel::setAmount,
            label = { Text("Amount") },
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Decimal),
            isError = uiState.amountError != null,
            supportingText = uiState.amountError?.let { { Text(it) } }
        )

        // Description Input
        OutlinedTextField(
            value = uiState.description,
            onValueChange = viewModel::setDescription,
            label = { Text("Description") },
            isError = uiState.descriptionError != null,
            supportingText = uiState.descriptionError?.let { { Text(it) } }
        )

        // Category Selection
        CategorySelector(
            categories = uiState.availableCategories,
            selectedCategory = uiState.selectedCategory,
            onCategorySelected = viewModel::setCategory
        )

        // Account Selection
        AccountSelector(
            accounts = uiState.availableAccounts,
            selectedAccount = uiState.selectedAccount,
            onAccountSelected = viewModel::setAccount,
            transactionType = uiState.type
        )

        // Date Picker
        DatePicker(
            selectedDate = uiState.date,
            onDateSelected = viewModel::setDate
        )

        // Tags Input
        TagsInput(
            tags = uiState.tags,
            onTagsChanged = viewModel::setTags
        )

        // Save Button
        Button(
            onClick = { viewModel.createTransaction() },
            modifier = Modifier.fillMaxWidth(),
            enabled = uiState.isValid && !uiState.isLoading
        ) {
            if (uiState.isLoading) {
                CircularProgressIndicator(modifier = Modifier.size(16.dp))
            } else {
                Text("Create Transaction")
            }
        }
    }

    // Handle transaction creation result
    LaunchedEffect(uiState.createdTransaction) {
        uiState.createdTransaction?.let { transaction ->
            onTransactionCreated(transaction)
        }
    }
}
```

### ViewModel Implementation

```kotlin
class CreateTransactionViewModel @Inject constructor(
    private val transactionRepository: TransactionRepository,
    private val categoryRepository: CategoryRepository,
    private val accountRepository: AccountRepository
) : ViewModel() {

    private val _uiState = MutableStateFlow(CreateTransactionUiState())
    val uiState: StateFlow<CreateTransactionUiState> = _uiState.asStateFlow()

    fun createTransaction() {
        val state = _uiState.value
        if (!state.isValid) return

        viewModelScope.launch {
            _uiState.value = state.copy(isLoading = true)

            val request = CreateTransactionRequest(
                type = state.type,
                amount = state.amount.toDouble(),
                description = state.description,
                categoryId = state.selectedCategory?.id ?: 0,
                accountId = state.selectedAccount?.id,
                date = state.date.toISOString(),
                tags = state.tags
            )

            transactionRepository.createTransaction(request)
                .onSuccess { transaction ->
                    _uiState.value = state.copy(
                        isLoading = false,
                        createdTransaction = transaction
                    )
                }
                .onFailure { error ->
                    _uiState.value = state.copy(
                        isLoading = false,
                        error = error.message
                    )
                }
        }
    }
}

data class CreateTransactionUiState(
    val type: TransactionType = TransactionType.EXPENSE,
    val amount: String = "",
    val description: String = "",
    val selectedCategory: Category? = null,
    val selectedAccount: Account? = null,
    val date: LocalDate = LocalDate.now(),
    val tags: List<String> = emptyList(),
    val availableCategories: List<Category> = emptyList(),
    val availableAccounts: List<Account> = emptyList(),
    val isLoading: Boolean = false,
    val error: String? = null,
    val createdTransaction: Transaction? = null
) {
    val isValid: Boolean
        get() = amount.isNotBlank() &&
                description.isNotBlank() &&
                selectedCategory != null &&
                selectedAccount != null &&
                amountError == null &&
                descriptionError == null

    val amountError: String?
        get() = when {
            amount.isBlank() -> "Amount is required"
            amount.toDoubleOrNull() == null -> "Invalid amount format"
            amount.toDouble() <= 0 -> "Amount must be greater than 0"
            else -> null
        }

    val descriptionError: String?
        get() = when {
            description.isBlank() -> "Description is required"
            description.length > 255 -> "Description too long"
            else -> null
        }
}
```

## Error Codes

| Code                    | Description                            | Resolution                    |
| ----------------------- | -------------------------------------- | ----------------------------- |
| `INVALID_AMOUNT`        | Amount validation failed               | Check amount format and range |
| `INVALID_CATEGORY`      | Category doesn't exist or wrong type   | Use valid category ID         |
| `INVALID_ACCOUNT`       | Account doesn't exist or access denied | Use valid account ID          |
| `INSUFFICIENT_BALANCE`  | Not enough balance for transfer        | Check account balance         |
| `DUPLICATE_TRANSACTION` | Similar transaction exists             | Review for duplicates         |
| `INVALID_DATE`          | Date format or range invalid           | Use valid ISO 8601 date       |

## Best Practices

-   **Offline Support**: Store transactions locally when offline
-   **Validation**: Validate on client before sending to server
-   **Duplicate Prevention**: Check for similar recent transactions
-   **Auto-categorization**: Suggest categories based on description
-   **Balance Updates**: Update local balances immediately for UX
-   **Error Handling**: Provide clear error messages to users
-   **Sync Strategy**: Retry failed transactions automatically
