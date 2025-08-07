# Expense Categories

<div class="api-endpoint">
  <span class="api-method get">GET</span>
  <strong>Host:</strong> <code>www.finsible.app</code><br>
  <strong>Path:</strong> <code>/categories?type=expense</code><br>
  <strong>Status:</strong> <code>200 OK</code><br>
  <strong>Authentication:</strong> Required (JWT Bearer Token)
</div>

## Overview

Retrieves all available expense categories organized by domain. These categories help users classify their spending for better budget management and financial insights.

## Request Parameters

| Parameter | Type     | Required | Description         |
| --------- | -------- | -------- | ------------------- |
| `type`    | `string` | Yes      | Must be `"expense"` |

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
    "message": "Expense categories",
    "data": {
        "type": "EXPENSE",
        "categories": [
            {
                "id": 101001,
                "name": "Groceries",
                "color": "orange",
                "domain": "ESSENTIALS"
            },
            {
                "id": 101002,
                "name": "Utilities",
                "color": "red",
                "domain": "ESSENTIALS"
            },
            {
                "id": 101003,
                "name": "Transport",
                "color": "blue",
                "domain": "ESSENTIALS"
            },
            {
                "id": 101004,
                "name": "Food & Dining",
                "color": "green",
                "domain": "ESSENTIALS"
            },
            {
                "id": 102001,
                "name": "Shopping",
                "color": "pink",
                "domain": "LIFESTYLE"
            },
            {
                "id": 102002,
                "name": "Entertainment",
                "color": "yellow",
                "domain": "LIFESTYLE"
            },
            {
                "id": 103001,
                "name": "Insurance Premiums",
                "color": "purple",
                "domain": "FINANCIAL"
            },
            {
                "id": 103002,
                "name": "Loan EMIs",
                "color": "gray",
                "domain": "FINANCIAL"
            },
            {
                "id": 104001,
                "name": "Gifts & Celebrations",
                "color": "orange",
                "domain": "SOCIAL"
            },
            {
                "id": 104002,
                "name": "Donations",
                "color": "blue",
                "domain": "SOCIAL"
            },
            {
                "id": 104003,
                "name": "Lend",
                "color": "red",
                "domain": "SOCIAL"
            }
        ]
    }
}
```

## Response Fields

| Field                      | Type      | Description                      |
| -------------------------- | --------- | -------------------------------- |
| `success`                  | `boolean` | Indicates successful response    |
| `message`                  | `string`  | Human-readable response message  |
| `data.type`                | `string`  | Category type (always "EXPENSE") |
| `data.categories`          | `array`   | List of expense categories       |
| `data.categories[].id`     | `number`  | Unique category identifier       |
| `data.categories[].name`   | `string`  | Display name of the category     |
| `data.categories[].color`  | `string`  | UI color theme for the category  |
| `data.categories[].domain` | `string`  | Category domain/group            |

## Expense Domains

| Domain       | Description                    | Examples                              |
| ------------ | ------------------------------ | ------------------------------------- |
| `ESSENTIALS` | Necessary living expenses      | Groceries, Utilities, Transport, Food |
| `LIFESTYLE`  | Discretionary spending         | Shopping, Entertainment               |
| `FINANCIAL`  | Financial obligations          | Insurance, Loan EMIs                  |
| `SOCIAL`     | Social and charitable expenses | Gifts, Donations, Lending             |

## Category Breakdown

### 🏠 Essentials (ESSENTIALS)

Essential living expenses that are necessary for daily life.

| ID     | Category      | Color  | Description                         |
| ------ | ------------- | ------ | ----------------------------------- |
| 101001 | Groceries     | Orange | Daily household shopping            |
| 101002 | Utilities     | Red    | Electricity, water, gas bills       |
| 101003 | Transport     | Blue   | Fuel, public transport, maintenance |
| 101004 | Food & Dining | Green  | Restaurants, takeout, meals         |

### 🎭 Lifestyle (LIFESTYLE)

Discretionary spending for personal enjoyment and lifestyle.

| ID     | Category      | Color  | Description                           |
| ------ | ------------- | ------ | ------------------------------------- |
| 102001 | Shopping      | Pink   | Clothing, accessories, personal items |
| 102002 | Entertainment | Yellow | Movies, games, hobbies                |

### 💼 Financial (FINANCIAL)

Financial obligations and commitments.

| ID     | Category           | Color  | Description                              |
| ------ | ------------------ | ------ | ---------------------------------------- |
| 103001 | Insurance Premiums | Purple | Life, health, vehicle insurance          |
| 103002 | Loan EMIs          | Gray   | Home loans, personal loans, credit cards |

### 🤝 Social (SOCIAL)

Social and charitable expenses.

| ID     | Category             | Color  | Description                       |
| ------ | -------------------- | ------ | --------------------------------- |
| 104001 | Gifts & Celebrations | Orange | Birthday gifts, festival expenses |
| 104002 | Donations            | Blue   | Charitable contributions          |
| 104003 | Lend                 | Red    | Money lent to friends/family      |

## Usage Examples

### Android Kotlin Implementation

```kotlin
// API Service
@GET("/categories")
suspend fun getExpenseCategories(
    @Query("type") type: String = "expense"
): CategoriesResponse

// Repository with Caching
class CategoryRepository @Inject constructor(
    private val apiService: ApiService,
    private val localDb: CategoryDao
) {
    suspend fun getExpenseCategories(): Result<List<Category>> {
        return try {
            // Try local cache first
            val cachedCategories = localDb.getExpenseCategories()
            if (cachedCategories.isNotEmpty()) {
                return Result.success(cachedCategories)
            }

            // Fetch from API
            val response = apiService.getExpenseCategories()
            val categories = response.data.categories

            // Cache locally
            localDb.insertCategories(categories)

            Result.success(categories)
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
}
```

### Jetpack Compose UI with Domain Grouping

```kotlin
@Composable
fun ExpenseCategoriesScreen(
    categories: List<Category>,
    onCategorySelected: (Category) -> Unit
) {
    val groupedCategories = categories.groupBy { it.domain }

    LazyColumn(
        contentPadding = PaddingValues(16.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        groupedCategories.forEach { (domain, domainCategories) ->
            item {
                DomainSection(
                    domain = domain,
                    categories = domainCategories,
                    onCategorySelected = onCategorySelected
                )
            }
        }
    }
}

@Composable
fun DomainSection(
    domain: String,
    categories: List<Category>,
    onCategorySelected: (Category) -> Unit
) {
    Column {
        Text(
            text = domain.getDomainTitle(),
            style = MaterialTheme.typography.titleMedium,
            modifier = Modifier.padding(bottom = 8.dp)
        )

        LazyRow(
            horizontalArrangement = Arrangement.spacedBy(12.dp)
        ) {
            items(categories) { category ->
                CategoryChip(
                    category = category,
                    onClick = { onCategorySelected(category) }
                )
            }
        }
    }
}

@Composable
fun CategoryChip(
    category: Category,
    onClick: () -> Unit
) {
    FilterChip(
        onClick = onClick,
        label = { Text(category.name) },
        colors = FilterChipDefaults.filterChipColors(
            containerColor = Color(category.colorInt),
            labelColor = Color.White
        )
    )
}
```

### Budget Analysis Extension

```kotlin
// Extension for budget analysis
fun List<Category>.getEssentialCategories(): List<Category> {
    return filter { it.domain == "ESSENTIALS" }
}

fun List<Category>.getDiscretionaryCategories(): List<Category> {
    return filter { it.domain in listOf("LIFESTYLE", "SOCIAL") }
}

// Domain title mapping
fun String.getDomainTitle(): String {
    return when (this) {
        "ESSENTIALS" -> "🏠 Essential Expenses"
        "LIFESTYLE" -> "🎭 Lifestyle & Entertainment"
        "FINANCIAL" -> "💼 Financial Obligations"
        "SOCIAL" -> "🤝 Social & Charitable"
        else -> this
    }
}
```

### Budget Tracking Integration

```kotlin
data class BudgetCategory(
    val category: Category,
    val budgetLimit: Double,
    val currentSpent: Double
) {
    val percentageUsed: Float
        get() = if (budgetLimit > 0) (currentSpent / budgetLimit).toFloat() else 0f

    val isOverBudget: Boolean
        get() = currentSpent > budgetLimit
}

@Composable
fun BudgetProgressCard(budget: BudgetCategory) {
    Card(
        modifier = Modifier.fillMaxWidth()
    ) {
        Column(
            modifier = Modifier.padding(16.dp)
        ) {
            Row(
                modifier = Modifier.fillMaxWidth(),
                horizontalArrangement = Arrangement.SpaceBetween
            ) {
                Text(
                    text = budget.category.name,
                    style = MaterialTheme.typography.titleMedium
                )
                Text(
                    text = "${budget.currentSpent.formatCurrency()} / ${budget.budgetLimit.formatCurrency()}",
                    style = MaterialTheme.typography.bodyMedium,
                    color = if (budget.isOverBudget) Color.Red else Color.Gray
                )
            }

            LinearProgressIndicator(
                progress = budget.percentageUsed.coerceAtMost(1f),
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(top = 8.dp),
                color = if (budget.isOverBudget) Color.Red else Color(budget.category.colorInt)
            )
        }
    }
}
```

## Best Practices

-   **Domain-Based UI**: Group categories by domain for better user experience
-   **Budget Integration**: Use categories for budget planning and tracking
-   **Smart Suggestions**: Suggest categories based on transaction patterns
-   **Custom Categories**: Allow users to create domain-specific custom categories
-   **Analytics**: Provide spending insights by category and domain
-   **Color Consistency**: Maintain color themes across the entire app
-   **Offline Support**: Cache categories for offline transaction creation
