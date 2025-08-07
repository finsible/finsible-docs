# API Overview

This section provides comprehensive documentation for all Finsible API endpoints. The API follows RESTful principles and uses JSON for data exchange.

## Base Configuration

### 🌐 Base URL

```
Production: https://finsible.app
Staging: https://staging.finsible.app
Development: https://dev.finsible.app
```

### 🔐 Authentication

All authenticated endpoints require a JWT Bearer token in the Authorization header:

```http
Authorization: Bearer <jwt_token>
```

### 📋 Common Headers

```http
Content-Type: application/json
Accept: application/json
Authorization: Bearer <jwt_token>
User-Agent: Finsible-Android/1.0.0
```

## Response Format

### Standard Response Structure

All API responses follow a consistent format:

```json
{
  "success": boolean,
  "message": "Human readable message",
  "data": object | array | null,
  "cache": boolean,
  "cacheTtlMinutes": number | null
}
```

### Response Fields

| Field             | Type           | Description                             |
| ----------------- | -------------- | --------------------------------------- |
| `success`         | `boolean`      | Indicates if the request was successful |
| `message`         | `string`       | Human-readable status message           |
| `data`            | `any`          | Response payload (varies by endpoint)   |
| `cache`           | `boolean`      | Whether response should be cached       |
| `cacheTtlMinutes` | `number\|null` | Cache expiration time in minutes        |

### HTTP Status Codes

| Status | Meaning               | When Used                      |
| ------ | --------------------- | ------------------------------ |
| `200`  | OK                    | Successful requests            |
| `201`  | Created               | Resource creation              |
| `400`  | Bad Request           | Invalid request data           |
| `401`  | Unauthorized          | Missing/invalid authentication |
| `403`  | Forbidden             | Insufficient permissions       |
| `404`  | Not Found             | Resource not found             |
| `422`  | Unprocessable Entity  | Validation errors              |
| `429`  | Too Many Requests     | Rate limiting                  |
| `500`  | Internal Server Error | Server errors                  |

## API Endpoints Overview

### 🔐 Authentication APIs

| Method | Endpoint             | Description                 | Auth Required |
| ------ | -------------------- | --------------------------- | ------------- |
| `POST` | `/auth/googleSignIn` | Google OAuth authentication | ❌            |
| `POST` | `/auth/refresh`      | Refresh JWT token           | ✅            |
| `POST` | `/auth/logout`       | Logout user                 | ✅            |

**Documentation:**

-   [Google Sign-In Success](auth/google_signin_success.md)
-   [Google Sign-In Error](auth/google_signin_error.md)

### 👤 User Management APIs

| Method   | Endpoint        | Description         | Auth Required |
| -------- | --------------- | ------------------- | ------------- |
| `GET`    | `/user/profile` | Get user profile    | ✅            |
| `PUT`    | `/user/profile` | Update user profile | ✅            |
| `DELETE` | `/user/account` | Delete user account | ✅            |

### 🏦 Account Management APIs

| Method   | Endpoint         | Description            | Auth Required |
| -------- | ---------------- | ---------------------- | ------------- |
| `GET`    | `/accounts/all`  | List all user accounts | ✅            |
| `POST`   | `/accounts`      | Create new account     | ✅            |
| `PUT`    | `/accounts/{id}` | Update account         | ✅            |
| `DELETE` | `/accounts/{id}` | Delete account         | ✅            |

**Documentation:**

-   [List Accounts](api/accounts/list.md)

### 🏷️ Category Management APIs

| Method   | Endpoint                  | Description            | Auth Required |
| -------- | ------------------------- | ---------------------- | ------------- |
| `GET`    | `/categories?type={type}` | Get categories by type | ✅            |
| `POST`   | `/categories`             | Create custom category | ✅            |
| `PUT`    | `/categories/{id}`        | Update category        | ✅            |
| `DELETE` | `/categories/{id}`        | Delete custom category | ✅            |

**Query Parameters for `/categories`:**

-   `type` (required): `income`, `expense`, or `transfer`

**Documentation:**

-   [Income Categories](api/categories/income.md)
-   [Expense Categories](api/categories/expense.md)
-   [Transfer Categories](api/categories/transfer.md)

### 💰 Transaction APIs

| Method   | Endpoint             | Description             | Auth Required |
| -------- | -------------------- | ----------------------- | ------------- |
| `GET`    | `/transactions`      | List transactions       | ✅            |
| `POST`   | `/transactions`      | Create transaction      | ✅            |
| `GET`    | `/transactions/{id}` | Get transaction details | ✅            |
| `PUT`    | `/transactions/{id}` | Update transaction      | ✅            |
| `DELETE` | `/transactions/{id}` | Delete transaction      | ✅            |

**Query Parameters for `/transactions`:**

-   `page` (optional): Page number (default: 1)
-   `limit` (optional): Items per page (default: 50)
-   `type` (optional): Filter by transaction type
-   `from_date` (optional): Start date filter (ISO 8601)
-   `to_date` (optional): End date filter (ISO 8601)
-   `category_id` (optional): Filter by category
-   `account_id` (optional): Filter by account

### 📊 Analytics APIs

| Method | Endpoint                | Description             | Auth Required |
| ------ | ----------------------- | ----------------------- | ------------- |
| `GET`  | `/analytics/summary`    | Financial summary       | ✅            |
| `GET`  | `/analytics/trends`     | Spending trends         | ✅            |
| `GET`  | `/analytics/categories` | Category-wise breakdown | ✅            |

### 🔄 Sync APIs

| Method | Endpoint             | Description            | Auth Required |
| ------ | -------------------- | ---------------------- | ------------- |
| `POST` | `/sync/transactions` | Bulk sync transactions | ✅            |
| `POST` | `/sync/accounts`     | Bulk sync accounts     | ✅            |
| `GET`  | `/sync/status`       | Get sync status        | ✅            |

## Error Handling

### Error Response Format

```json
{
    "success": false,
    "message": "Validation failed",
    "data": {
        "errors": [
            {
                "field": "amount",
                "code": "INVALID_FORMAT",
                "message": "Amount must be a positive number"
            }
        ]
    },
    "cache": false,
    "cacheTtlMinutes": null
}
```

### Common Error Codes

| Code                       | Description                      | Resolution                       |
| -------------------------- | -------------------------------- | -------------------------------- |
| `INVALID_TOKEN`            | JWT token is invalid or expired  | Refresh token or re-authenticate |
| `INSUFFICIENT_PERMISSIONS` | User lacks required permissions  | Check user role and permissions  |
| `VALIDATION_ERROR`         | Request data validation failed   | Fix request data format          |
| `RESOURCE_NOT_FOUND`       | Requested resource doesn't exist | Verify resource ID               |
| `DUPLICATE_RESOURCE`       | Resource already exists          | Use unique identifiers           |
| `RATE_LIMIT_EXCEEDED`      | Too many requests                | Implement exponential backoff    |

## Rate Limiting

### Limits by Endpoint Type

| Endpoint Type    | Limit        | Window   |
| ---------------- | ------------ | -------- |
| Authentication   | 5 requests   | 1 minute |
| Read Operations  | 100 requests | 1 minute |
| Write Operations | 50 requests  | 1 minute |
| Bulk Operations  | 10 requests  | 1 minute |

### Rate Limit Headers

```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1640995200
```

## Caching Strategy

### Cache Headers

```http
Cache-Control: public, max-age=3600
ETag: "abc123"
Last-Modified: Wed, 21 Oct 2025 07:28:00 GMT
```

### Cacheable Endpoints

| Endpoint        | Cache Duration | Invalidation Trigger  |
| --------------- | -------------- | --------------------- |
| `/categories`   | 24 hours       | Category modification |
| `/accounts/all` | 1 hour         | Account changes       |
| `/user/profile` | 30 minutes     | Profile updates       |

## SDK Integration

### Android Kotlin Example

```kotlin
class ApiClient @Inject constructor(
    private val retrofit: Retrofit,
    private val tokenManager: TokenManager
) {
    private val apiService = retrofit.create(FinsibleApiService::class.java)

    suspend fun getAllAccounts(): Result<List<Account>> {
        return try {
            val response = apiService.getAllAccounts()
            if (response.success) {
                Result.success(response.data.accounts)
            } else {
                Result.failure(ApiException(response.message))
            }
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
}
```

### Error Handling

```kotlin
sealed class ApiResult<T> {
    data class Success<T>(val data: T) : ApiResult<T>()
    data class Error<T>(
        val message: String,
        val code: String?,
        val httpCode: Int?
    ) : ApiResult<T>()
}

suspend fun <T> safeApiCall(
    apiCall: suspend () -> ApiResponse<T>
): ApiResult<T> {
    return try {
        val response = apiCall()
        if (response.success) {
            ApiResult.Success(response.data)
        } else {
            ApiResult.Error(
                message = response.message,
                code = null,
                httpCode = null
            )
        }
    } catch (e: HttpException) {
        ApiResult.Error(
            message = "Network error: ${e.message}",
            code = "NETWORK_ERROR",
            httpCode = e.code()
        )
    } catch (e: Exception) {
        ApiResult.Error(
            message = "Unknown error: ${e.message}",
            code = "UNKNOWN_ERROR",
            httpCode = null
        )
    }
}
```

## Mock Server Setup

For development purposes, use mock responses when the backend is not available.

### Mock Configuration

```kotlin
object MockConfig {
    const val USE_MOCK_SERVER = BuildConfig.DEBUG
    const val MOCK_DELAY_MS = 1000L
    const val MOCK_SUCCESS_RATE = 0.9f // 90% success rate
}
```

### Mock Interceptor

```kotlin
class MockInterceptor @Inject constructor(
    @ApplicationContext private val context: Context
) : Interceptor {

    override fun intercept(chain: Interceptor.Chain): Response {
        if (!MockConfig.USE_MOCK_SERVER) {
            return chain.proceed(chain.request())
        }

        val request = chain.request()
        val mockResponse = getMockResponse(request.url.encodedPath)

        return Response.Builder()
            .request(request)
            .protocol(Protocol.HTTP_1_1)
            .code(200)
            .message("OK")
            .body(mockResponse.toResponseBody("application/json".toMediaType()))
            .build()
    }

    private fun getMockResponse(path: String): String {
        val mockFile = when {
            path.contains("/auth/googleSignIn") -> "mock_responses/auth/google_signin_success.json"
            path.contains("/categories") -> {
                val type = extractQueryParam(path, "type")
                "mock_responses/categories/${type}_categories.json"
            }
            path.contains("/accounts/all") -> "mock_responses/accounts/accounts_list.json"
            else -> "mock_responses/default_response.json"
        }

        return context.assets.open(mockFile).bufferedReader().use { it.readText() }
    }
}
```

## Best Practices

### 📱 Client-Side Implementation

1. **Offline-First Architecture**

    - Cache responses locally
    - Sync when network available
    - Provide offline functionality

2. **Error Handling**

    - Implement retry mechanisms
    - Show user-friendly error messages
    - Log errors for debugging

3. **Security**

    - Store tokens securely
    - Validate SSL certificates
    - Implement certificate pinning

4. **Performance**
    - Use appropriate timeouts
    - Implement request deduplication
    - Compress large payloads

### 🔧 Development Tools

1. **API Testing**

    - Use Postman collections
    - Implement automated API tests
    - Monitor API performance

2. **Documentation**

    - Keep API docs updated
    - Provide code examples
    - Document error scenarios

3. **Monitoring**
    - Track API usage metrics
    - Monitor error rates
    - Set up alerting

## Next Steps

-   [Authentication Implementation](auth/google_signin_success.md)
-   [Account Management](api/accounts/list.md)
-   [Category System](api/categories/income.md)
-   [Transaction Management](api/transactions/create.md)
