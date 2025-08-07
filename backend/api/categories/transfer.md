# Transfer Categories

<div class="api-endpoint">
  <span class="api-method get">GET</span>
  <strong>Host:</strong> <code>www.finsible.app</code><br>
  <strong>Path:</strong> <code>/categories?type=transfer</code><br>
  <strong>Status:</strong> <code>200 OK</code><br>
  <strong>Authentication:</strong> Required (JWT Bearer Token)
</div>

## Overview

Retrieves all available transfer categories organized by domain. These categories help users classify transfers between accounts, investments, and asset transactions for comprehensive financial management.

## Request Parameters

| Parameter | Type     | Required | Description          |
| --------- | -------- | -------- | -------------------- |
| `type`    | `string` | Yes      | Must be `"transfer"` |

## Request Headers

```http
Authorization: Bearer <jwt_token>
Content-Type: application/json
```

## Response Schema

### Success Response

```json
{
    "success": true,
    "message": "Transfer categories",
    "data": {
        "type": "TRANSFER",
        "categories": [
            {
                "id": 301001,
                "name": "Account Transfer",
                "color": "blue",
                "domain": "ACCOUNT_MANAGEMENT"
            },
            {
                "id": 301002,
                "name": "Cash Withdrawal",
                "color": "orange",
                "domain": "ACCOUNT_MANAGEMENT"
            },
            {
                "id": 301003,
                "name": "Cash Deposit",
                "color": "yellow",
                "domain": "ACCOUNT_MANAGEMENT"
            },
            {
                "id": 301004,
                "name": "Credit Card Payment",
                "color": "red",
                "domain": "ACCOUNT_MANAGEMENT"
            },
            {
                "id": 302001,
                "name": "Investment Purchase",
                "color": "green",
                "domain": "INVESTMENT"
            },
            {
                "id": 302002,
                "name": "Investment Sale",
                "color": "purple",
                "domain": "INVESTMENT"
            },
            {
                "id": 303001,
                "name": "Buy Asset",
                "color": "pink",
                "domain": "ASSET_ACQUISITION"
            },
            {
                "id": 303002,
                "name": "Sell Asset",
                "color": "gray",
                "domain": "ASSET_DISPOSAL"
            }
        ]
    }
}
```

## Response Fields

| Field                      | Type      | Description                       |
| -------------------------- | --------- | --------------------------------- |
| `success`                  | `boolean` | Indicates successful response     |
| `message`                  | `string`  | Human-readable response message   |
| `data.type`                | `string`  | Category type (always "TRANSFER") |
| `data.categories`          | `array`   | List of transfer categories       |
| `data.categories[].id`     | `number`  | Unique category identifier        |
| `data.categories[].name`   | `string`  | Display name of the category      |
| `data.categories[].color`  | `string`  | UI color theme for the category   |
| `data.categories[].domain` | `string`  | Category domain/group             |

## Transfer Domains

| Domain               | Description                  | Use Cases                                  |
| -------------------- | ---------------------------- | ------------------------------------------ |
| `ACCOUNT_MANAGEMENT` | Basic account operations     | Transfers, deposits, withdrawals, payments |
| `INVESTMENT`         | Investment-related transfers | Buying/selling stocks, mutual funds        |
| `ASSET_ACQUISITION`  | Asset purchase transactions  | Real estate, vehicles, equipment           |
| `ASSET_DISPOSAL`     | Asset sale transactions      | Selling assets for cash                    |

## Category Breakdown

### 🏦 Account Management (ACCOUNT_MANAGEMENT)

Basic banking and account operations between different financial accounts.

| ID     | Category            | Color  | Description                    | Common Use Cases                    |
| ------ | ------------------- | ------ | ------------------------------ | ----------------------------------- |
| 301001 | Account Transfer    | Blue   | Moving money between accounts  | Savings to checking, bank transfers |
| 301002 | Cash Withdrawal     | Orange | Withdrawing cash from accounts | ATM withdrawals, bank counter       |
| 301003 | Cash Deposit        | Yellow | Depositing cash into accounts  | Bank deposits, ATM deposits         |
| 301004 | Credit Card Payment | Red    | Paying credit card bills       | Monthly CC payments, debt clearance |

### 📈 Investment (INVESTMENT)

Investment-related transfers for portfolio management.

| ID     | Category            | Color  | Description         | Common Use Cases                    |
| ------ | ------------------- | ------ | ------------------- | ----------------------------------- |
| 302001 | Investment Purchase | Green  | Buying investments  | Stock purchases, mutual fund SIPs   |
| 302002 | Investment Sale     | Purple | Selling investments | Stock sales, redeeming mutual funds |

### 🏠 Asset Acquisition (ASSET_ACQUISITION)

Purchasing physical or digital assets.

| ID     | Category  | Color | Description       | Common Use Cases                 |
| ------ | --------- | ----- | ----------------- | -------------------------------- |
| 303001 | Buy Asset | Pink  | Purchasing assets | Real estate, vehicles, equipment |

### 💰 Asset Disposal (ASSET_DISPOSAL)

Selling assets for monetary gain.

| ID     | Category   | Color | Description    | Common Use Cases              |
| ------ | ---------- | ----- | -------------- | ----------------------------- |
| 303002 | Sell Asset | Gray  | Selling assets | Property sales, vehicle sales |

## Usage Examples

### Android Kotlin Implementation

```kotlin
// API Service
@GET("/categories")
suspend fun getTransferCategories(
    @Query("type") type: String = "transfer"
): CategoriesResponse

// Transfer-specific Repository
class TransferCategoryRepository @Inject constructor(
    private val apiService: ApiService,
    private val localDb: CategoryDao
) {
    suspend fun getTransferCategories(): Result<List<Category>> {
        return try {
            val response = apiService.getTransferCategories()
            val categories = response.data.categories

            // Cache locally for offline use
            localDb.insertTransferCategories(categories)

            Result.success(categories)
        } catch (e: Exception) {
            // Fallback to cached data
            val cachedCategories = localDb.getTransferCategories()
            if (cachedCategories.isNotEmpty()) {
                Result.success(cachedCategories)
            } else {
                Result.failure(e)
            }
        }
    }

    // Get categories by domain
    suspend fun getCategoriesByDomain(domain: String): List<Category> {
        return localDb.getTransferCategoriesByDomain(domain)
    }
}
```

### Transfer Transaction UI

```kotlin
@Composable
fun TransferCategorySelection(
    categories: List<Category>,
    selectedCategory: Category?,
    onCategorySelected: (Category) -> Unit
) {
    val groupedCategories = categories.groupBy { it.domain }

    LazyColumn(
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        groupedCategories.forEach { (domain, domainCategories) ->
            item {
                DomainCard(
                    domain = domain,
                    categories = domainCategories,
                    selectedCategory = selectedCategory,
                    onCategorySelected = onCategorySelected
                )
            }
        }
    }
}

@Composable
fun DomainCard(
    domain: String,
    categories: List<Category>,
    selectedCategory: Category?,
    onCategorySelected: (Category) -> Unit
) {
    Card(
        modifier = Modifier.fillMaxWidth()
    ) {
        Column(
            modifier = Modifier.padding(16.dp)
        ) {
            Text(
                text = domain.getDomainDisplayName(),
                style = MaterialTheme.typography.titleMedium,
                modifier = Modifier.padding(bottom = 12.dp)
            )

            categories.forEach { category ->
                TransferCategoryItem(
                    category = category,
                    isSelected = category.id == selectedCategory?.id,
                    onClick = { onCategorySelected(category) }
                )
            }
        }
    }
}

@Composable
fun TransferCategoryItem(
    category: Category,
    isSelected: Boolean,
    onClick: () -> Unit
) {
    Row(
        modifier = Modifier
            .fillMaxWidth()
            .clickable { onClick() }
            .padding(vertical = 8.dp),
        verticalAlignment = Alignment.CenterVertically
    ) {
        RadioButton(
            selected = isSelected,
            onClick = onClick,
            colors = RadioButtonDefaults.colors(
                selectedColor = Color(category.colorInt)
            )
        )

        Spacer(modifier = Modifier.width(12.dp))

        Box(
            modifier = Modifier
                .size(12.dp)
                .background(
                    color = Color(category.colorInt),
                    shape = CircleShape
                )
        )

        Spacer(modifier = Modifier.width(12.dp))

        Text(
            text = category.name,
            style = MaterialTheme.typography.bodyLarge
        )
    }
}
```

### Transfer Flow Management

```kotlin
data class TransferTransaction(
    val fromAccountId: Long,
    val toAccountId: Long,
    val amount: Double,
    val categoryId: Long,
    val description: String,
    val date: LocalDateTime
)

class TransferViewModel @Inject constructor(
    private val categoryRepository: TransferCategoryRepository,
    private val accountRepository: AccountRepository,
    private val transactionRepository: TransactionRepository
) : ViewModel() {

    private val _transferCategories = MutableStateFlow<List<Category>>(emptyList())
    val transferCategories = _transferCategories.asStateFlow()

    private val _selectedCategory = MutableStateFlow<Category?>(null)
    val selectedCategory = _selectedCategory.asStateFlow()

    fun loadTransferCategories() {
        viewModelScope.launch {
            categoryRepository.getTransferCategories()
                .onSuccess { categories ->
                    _transferCategories.value = categories
                }
        }
    }

    fun selectCategory(category: Category) {
        _selectedCategory.value = category
    }

    fun executeTransfer(transfer: TransferTransaction) {
        viewModelScope.launch {
            transactionRepository.createTransfer(transfer)
                .onSuccess {
                    // Handle success
                }
                .onFailure { error ->
                    // Handle error
                }
        }
    }

    // Smart category suggestion based on accounts
    fun suggestCategory(fromAccount: Account, toAccount: Account): Category? {
        val categories = _transferCategories.value

        return when {
            fromAccount.accountType == AccountType.CASH &&
            toAccount.accountType == AccountType.SAVINGS -> {
                categories.find { it.id == 301003L } // Cash Deposit
            }
            fromAccount.accountType == AccountType.SAVINGS &&
            toAccount.accountType == AccountType.CASH -> {
                categories.find { it.id == 301002L } // Cash Withdrawal
            }
            fromAccount.accountType == AccountType.SAVINGS &&
            toAccount.accountType == AccountType.MUTUAL_FUNDS -> {
                categories.find { it.id == 302001L } // Investment Purchase
            }
            else -> {
                categories.find { it.id == 301001L } // Default: Account Transfer
            }
        }
    }
}
```

### Extension Functions

```kotlin
// Domain display names
fun String.getDomainDisplayName(): String {
    return when (this) {
        "ACCOUNT_MANAGEMENT" -> "🏦 Account Management"
        "INVESTMENT" -> "📈 Investment Operations"
        "ASSET_ACQUISITION" -> "🏠 Asset Purchase"
        "ASSET_DISPOSAL" -> "💰 Asset Sale"
        else -> this
    }
}

// Category validation for transfer types
fun Category.isValidForTransfer(fromAccount: Account, toAccount: Account): Boolean {
    return when (domain) {
        "ACCOUNT_MANAGEMENT" -> true // Always valid
        "INVESTMENT" -> {
            fromAccount.isLiquidAccount() || toAccount.isInvestmentAccount()
        }
        "ASSET_ACQUISITION" -> {
            fromAccount.isLiquidAccount()
        }
        "ASSET_DISPOSAL" -> {
            toAccount.isLiquidAccount()
        }
        else -> false
    }
}

// Account type helpers
fun Account.isLiquidAccount(): Boolean {
    return accountType in listOf(
        AccountType.CASH,
        AccountType.SAVINGS,
        AccountType.CHECKING
    )
}

fun Account.isInvestmentAccount(): Boolean {
    return accountType in listOf(
        AccountType.MUTUAL_FUNDS,
        AccountType.STOCKS
    )
}
```

## Smart Category Suggestions

The app can suggest appropriate transfer categories based on account types:

| From Account | To Account   | Suggested Category         |
| ------------ | ------------ | -------------------------- |
| Savings      | Cash         | Cash Withdrawal            |
| Cash         | Savings      | Cash Deposit               |
| Savings      | Mutual Funds | Investment Purchase        |
| Stocks       | Savings      | Investment Sale            |
| Savings      | Credit Card  | Credit Card Payment        |
| Any          | Any          | Account Transfer (default) |

## Best Practices

-   **Smart Suggestions**: Auto-suggest categories based on account types
-   **Validation**: Validate category selection against account types
-   **UI Grouping**: Group categories by domain for better organization
-   **Quick Actions**: Provide shortcuts for common transfers
-   **Transaction History**: Track transfer patterns for better suggestions
-   **Offline Support**: Cache categories for offline transfer creation
-   **Color Coding**: Use consistent color themes across transfer UI
-   **Confirmation**: Always confirm transfer details before execution
