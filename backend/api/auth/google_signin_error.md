# Google Sign-In Error

<div class="api-endpoint">
  <span class="api-method post">POST</span>
  <strong>Host:</strong> <code>www.finsible.app</code><br>
  <strong>Path:</strong> <code>/auth/googleSignIn</code><br>
  <strong>Status:</strong> <code>200 OK</code>
</div>

## Overview

Error response when Google OAuth authentication fails. The endpoint returns a 200 status with error details in the response body.

## Response Schema

### Error Response

```json
{
    "message": "Could not log you in. Please try again.",
    "success": false,
    "data": null,
    "cache": false,
    "cacheTtlMinutes": 10000
}
```

## Response Fields

| Field             | Type      | Description                                     |
| ----------------- | --------- | ----------------------------------------------- |
| `message`         | `string`  | Human-readable error message for user display   |
| `success`         | `boolean` | Always `false` for error responses              |
| `data`            | `null`    | No user data returned on authentication failure |
| `cache`           | `boolean` | Whether response should be cached               |
| `cacheTtlMinutes` | `number`  | Cache TTL for error responses                   |

## Common Error Scenarios

### Authentication Failures

-   Invalid Google ID token
-   Expired Google credentials
-   Network connectivity issues
-   Server-side authentication errors

### Client-Side Handling

-   Display user-friendly error message
-   Provide retry mechanism
-   Log technical details for debugging
-   Fallback to manual login if available

## Error Recovery

```kotlin
// Example error handling in Android
when (response.success) {
    false -> {
        // Show error message to user
        showErrorMessage(response.message)

        // Log for debugging
        Log.e("Auth", "Google Sign-In failed: ${response.message}")

        // Allow user to retry
        enableRetryButton()
    }
}
```

## Best Practices

-   **User Experience**: Show clear, actionable error messages
-   **Retry Logic**: Implement exponential backoff for retries
-   **Logging**: Log errors for debugging but avoid sensitive data
-   **Fallback**: Provide alternative authentication methods if possible
-   **Offline Handling**: Cache authentication state for offline scenarios
