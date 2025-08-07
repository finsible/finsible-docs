# Income Categories

<div class="api-endpoint">
  <span class="api-method get">GET</span>
  <strong>Host:</strong> <code>www.finsible.app</code><br>
  <strong>Path:</strong> <code>/categories?type=income</code><br>
  <strong>Status:</strong> <code>200 OK</code><br>
  <strong>Authentication:</strong> Required (JWT Bearer Token)
</div>

## Overview

Retrieves all available income categories organized by domain. These categories help users classify their income sources for better financial tracking and reporting.

## Request Parameters

| Parameter | Type     | Required | Description        |
| --------- | -------- | -------- | ------------------ |
| `type`    | `string` | Yes      | Must be `"income"` |

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
    "message": "Income categories",
    "data": {
        "type": "INCOME",
        "categories": [
            {
                "id": 201001,
                "name": "Salary",
                "color": "green",
                "domain": "PRIMARY"
            },
            {
                "id": 201002,
                "name": "Business",
                "color": "blue",
                "domain": "PRIMARY"
            },
            {
                "id": 202001,
                "name": "Dividends",
                "color": "purple",
                "domain": "PASSIVE"
            },
            {
                "id": 202002,
                "name": "Interest",
                "color": "yellow",
                "domain": "PASSIVE"
            },
            {
                "id": 202003,
                "name": "Rental Income",
                "color": "orange",
                "domain": "PASSIVE"
            },
            {
                "id": 203001,
                "name": "Freelance",
                "color": "pink",
                "domain": "IRREGULAR"
            },
            {
                "id": 203002,
                "name": "Gifts Received",
                "color": "gray",
                "domain": "IRREGULAR"
            },
            {
                "id": 203003,
                "name": "Borrowed Funds",
                "color": "red",
                "domain": "IRREGULAR"
            }
        ]
    }
}
```

## Response Fields

| Field                      | Type      | Description                     |
| -------------------------- | --------- | ------------------------------- |
| `success`                  | `boolean` | Indicates successful response   |
| `message`                  | `string`  | Human-readable response message |
| `data.type`                | `string`  | Category type (always "INCOME") |
| `data.categories`          | `array`   | List of income categories       |
| `data.categories[].id`     | `number`  | Unique category identifier      |
| `data.categories[].name`   | `string`  | Display name of the category    |
| `data.categories[].color`  | `string`  | UI color theme for the category |
| `data.categories[].domain` | `string`  | Category domain/group           |

## Income Domains

| Domain      | Description                         | Examples                  |
| ----------- | ----------------------------------- | ------------------------- |
| `PRIMARY`   | Regular, predictable income sources | Salary, Business revenue  |
| `PASSIVE`   | Income from investments and assets  | Dividends, Interest, Rent |
| `IRREGULAR` | One-time or unpredictable income    | Freelance, Gifts, Loans   |

## Category Colors

| Color    | Hex Code  | Usage                   |
| -------- | --------- | ----------------------- |
| `green`  | `#4CAF50` | Primary income (salary) |
| `blue`   | `#2196F3` | Business income         |
| `purple` | `#9C27B0` | Investment returns      |
| `yellow` | `#FFEB3B` | Interest income         |
| `orange` | `#FF9800` | Rental income           |
| `pink`   | `#E91E63` | Freelance work          |
| `gray`   | `#9E9E9E` | Gifts and misc          |
| `red`    | `#F44336` | Borrowed funds          |

## Usage Examples

### Android Kotlin Implementation

```kotlin
// API Service
@GET("/categories")
suspend fun getIncomeCategories(
    @Query("type") type: String = "income"
): CategoriesResponse

// Repository
suspend fun getIncomeCategories(): Result<List<Category>> {
    return try {
        val response = apiService.getIncomeCategories()
        Result.success(response.data.categories)
    } catch (e: Exception) {
        Result.failure(e)
    }
}

// ViewModel
class IncomeCategoriesViewModel @Inject constructor(
    private val repository: CategoryRepository
) : ViewModel() {

    private val _categories = MutableStateFlow<List<Category>>(emptyList())
    val categories = _categories.asStateFlow()

    fun loadIncomeCategories() {
        viewModelScope.launch {
            repository.getIncomeCategories()
                .onSuccess { categories ->
                    _categories.value = categories
                }
        }
    }
}
```

### Jetpack Compose UI

```kotlin
@Composable
fun IncomeCategoryGrid(
    categories: List<Category>,
    onCategorySelected: (Category) -> Unit
) {
    LazyVerticalGrid(
        columns = GridCells.Fixed(2),
        contentPadding = PaddingValues(16.dp),
        horizontalArrangement = Arrangement.spacedBy(12.dp),
        verticalArrangement = Arrangement.spacedBy(12.dp)
    ) {
        items(categories) { category ->
            CategoryCard(
                category = category,
                onClick = { onCategorySelected(category) }
            )
        }
    }
}

@Composable
fun CategoryCard(
    category: Category,
    onClick: () -> Unit
) {
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .clickable { onClick() },
        colors = CardDefaults.cardColors(
            containerColor = Color(category.colorInt)
        )
    ) {
        Column(
            modifier = Modifier.padding(16.dp),
            horizontalAlignment = Alignment.CenterHorizontally
        ) {
            Text(
                text = category.name,
                style = MaterialTheme.typography.bodyLarge,
                color = Color.White,
                textAlign = TextAlign.Center
            )
            Text(
                text = category.domain,
                style = MaterialTheme.typography.bodySmall,
                color = Color.White.copy(alpha = 0.8f),
                textAlign = TextAlign.Center
            )
        }
    }
}
```

### Data Classes

```kotlin
data class CategoriesResponse(
    val success: Boolean,
    val message: String,
    val data: CategoriesData
)

data class CategoriesData(
    val type: String,
    val categories: List<Category>
)

data class Category(
    val id: Long,
    val name: String,
    val color: String,
    val domain: String
) {
    val colorInt: Int
        get() = when (color) {
            "green" -> 0xFF4CAF50.toInt()
            "blue" -> 0xFF2196F3.toInt()
            "purple" -> 0xFF9C27B0.toInt()
            "yellow" -> 0xFFFFEB3B.toInt()
            "orange" -> 0xFFFF9800.toInt()
            "pink" -> 0xFFE91E63.toInt()
            "gray" -> 0xFF9E9E9E.toInt()
            "red" -> 0xFFF44336.toInt()
            else -> 0xFF757575.toInt()
        }
}
```

## Best Practices

-   **Caching**: Cache categories locally for offline access
-   **UI Grouping**: Group categories by domain for better UX
-   **Color Consistency**: Use provided colors throughout the app
-   **Search/Filter**: Implement search for large category lists
-   **Custom Categories**: Allow users to create custom income categories
-   **Analytics**: Track category usage for insights
