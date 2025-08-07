# List Accounts

<div class="api-endpoint">
  <span class="api-method get">GET</span>
  <strong>Host:</strong> <code>www.finsible.app</code><br>
  <strong>Path:</strong> <code>/accounts/all</code><br>
  <strong>Status:</strong> <code>200 OK</code><br>
  <strong>Authentication:</strong> Required (JWT Bearer Token)
</div>

## Overview

Retrieves all user accounts including various account types like cash, investments, loans, and deposits. This endpoint provides the foundation for account-based transaction management.

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
    "message": "Launch app: List of accounts",
    "data": {
        "accounts": [
            {
                "id": 901001,
                "name": "Cash",
                "balance": 0.0,
                "accountType": "CASH",
                "isCustom": false,
                "description": "Physical cash in wallet or at home"
            },
            {
                "id": 901002,
                "name": "Mutual Funds",
                "balance": 0.0,
                "accountType": "MUTUAL_FUNDS",
                "isCustom": false,
                "description": "SIP investments and lump sum mutual funds"
            },
            {
                "id": 901003,
                "name": "Stocks",
                "balance": 0.0,
                "accountType": "STOCKS",
                "isCustom": false,
                "description": "Individual stocks bought through demat account"
            },
            {
                "id": 901004,
                "name": "Fixed Deposits",
                "balance": 0.0,
                "accountType": "FD",
                "isCustom": false,
                "description": "Money locked in fixed deposits with banks"
            },
            {
                "id": 901005,
                "name": "Loans",
                "loanAmount": 0.0,
                "remainingAmount": 0.0,
                "accountType": "LOANS",
                "isCustom": false,
                "description": "Personal loans, home loans, or money borrowed"
            }
        ]
    },
    "cache": true,
    "cacheTtlMinutes": 1440
}
```

## Response Fields

| Field                             | Type      | Description                         |
| --------------------------------- | --------- | ----------------------------------- |
| `success`                         | `boolean` | Indicates successful response       |
| `message`                         | `string`  | Human-readable response message     |
| `data.accounts`                   | `array`   | List of user accounts               |
| `data.accounts[].id`              | `number`  | Unique account identifier           |
| `data.accounts[].name`            | `string`  | Display name of the account         |
| `data.accounts[].balance`         | `number`  | Current account balance             |
| `data.accounts[].accountType`     | `string`  | Type of account (enum)              |
| `data.accounts[].isCustom`        | `boolean` | Whether account is user-created     |
| `data.accounts[].description`     | `string`  | Account description                 |
| `data.accounts[].loanAmount`      | `number`  | Total loan amount (loans only)      |
| `data.accounts[].remainingAmount` | `number`  | Remaining loan balance (loans only) |
| `cache`                           | `boolean` | Whether response should be cached   |
| `cacheTtlMinutes`                 | `number`  | Cache TTL (24 hours)                |

## Account Types

| Type            | Code           | Description                |
| --------------- | -------------- | -------------------------- |
| Cash            | `CASH`         | Physical cash holdings     |
| Mutual Funds    | `MUTUAL_FUNDS` | Investment in mutual funds |
| Stocks          | `STOCKS`       | Stock market investments   |
| Fixed Deposits  | `FD`           | Bank fixed deposits        |
| Loans           | `LOANS`        | Borrowed money/debt        |
| Savings Account | `SAVINGS`      | Bank savings account       |
| Credit Card     | `CREDIT_CARD`  | Credit card account        |

## Usage Examples

### Android Kotlin Implementation

```kotlin
// Repository method
suspend fun getAllAccounts(): Result<AccountsResponse> {
    return try {
        val response = apiService.getAllAccounts()
        Result.success(response)
    } catch (e: Exception) {
        Result.failure(e)
    }
}

// ViewModel usage
class AccountsViewModel @Inject constructor(
    private val repository: AccountRepository
) : ViewModel() {

    private val _accounts = MutableLiveData<List<Account>>()
    val accounts: LiveData<List<Account>> = _accounts

    fun loadAccounts() {
        viewModelScope.launch {
            repository.getAllAccounts()
                .onSuccess { response ->
                    _accounts.value = response.data.accounts
                }
                .onFailure { error ->
                    // Handle error
                }
        }
    }
}
```

### Data Classes

```kotlin
data class AccountsResponse(
    val success: Boolean,
    val message: String,
    val data: AccountsData,
    val cache: Boolean,
    val cacheTtlMinutes: Int
)

data class AccountsData(
    val accounts: List<Account>
)

data class Account(
    val id: Long,
    val name: String,
    val balance: Double,
    val accountType: AccountType,
    val isCustom: Boolean,
    val description: String,
    val loanAmount: Double? = null,
    val remainingAmount: Double? = null
)

enum class AccountType {
    CASH, MUTUAL_FUNDS, STOCKS, FD, LOANS,
    SAVINGS, CREDIT_CARD
}
```

## Error Handling

-   **401 Unauthorized**: Invalid or expired JWT token
-   **403 Forbidden**: Insufficient permissions
-   **500 Internal Server Error**: Server-side issues
-   **Network Errors**: Handle connectivity issues

## Caching Strategy

-   **Cache Duration**: 24 hours (1440 minutes)
-   **Cache Key**: User-specific account data
-   **Invalidation**: On account creation/modification
-   **Offline Support**: Use cached data when offline
