# Google Sign-In Success

<div class="api-endpoint">
  <span class="api-method post">POST</span>
  <strong>Host:</strong> <code>www.finsible.app</code><br>
  <strong>Path:</strong> <code>/auth/googleSignIn</code><br>
  <strong>Status:</strong> <code>200 OK</code>
</div>

## Overview

Successful Google OAuth authentication response that returns user information and JWT token for subsequent API calls.

## Response Schema

### Success Response

```json
{
    "message": "You are successfully logged in.",
    "success": true,
    "data": {
        "isNewUser": true,
        "userId": "100593673127133532461",
        "email": "pateljeel123789@gmail.com",
        "name": "Patel Jeel",
        "picture": "https://lh3.googleusercontent.com/a-/ALV-UjVdM6CGAftnvEWofumC-j0HyOdhVQ7A3sDf2Cf8MvQcbwBg4npg=s96-c",
        "accountCreated": "2025-03-03T23:42:10.984369",
        "lastLoggedIn": "2025-03-03T23:42:10.984369",
        "jwt": "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxMDA1OTM2NzMxMjcxMzM1MzI0NjEiLCJpYXQiOjE3NDEwMjU1MzEsImV4cCI6MTc1NjU3NzUzMX0.7ZbfDjdb4t5AZCHnpHrCYSTYolI8CaTB9D6E6-r6mWU6UAE51WHzRwSEs-BjX0dIwrL1DyQ-Z1zDahETrdME7A"
    },
    "cache": false,
    "cacheTtlMinutes": null
}
```

## Response Fields

| Field                 | Type           | Description                                             |
| --------------------- | -------------- | ------------------------------------------------------- |
| `message`             | `string`       | Human-readable success message                          |
| `success`             | `boolean`      | Indicates successful authentication                     |
| `data`                | `object`       | User authentication data                                |
| `data.isNewUser`      | `boolean`      | Whether this is a first-time user registration          |
| `data.userId`         | `string`       | Unique user identifier from Google                      |
| `data.email`          | `string`       | User's Google email address                             |
| `data.name`           | `string`       | User's display name from Google profile                 |
| `data.picture`        | `string`       | URL to user's Google profile picture                    |
| `data.accountCreated` | `string`       | ISO 8601 timestamp of account creation                  |
| `data.lastLoggedIn`   | `string`       | ISO 8601 timestamp of last login                        |
| `data.jwt`            | `string`       | JWT token for API authentication (expires in ~180 days) |
| `cache`               | `boolean`      | Whether response should be cached                       |
| `cacheTtlMinutes`     | `number\|null` | Cache TTL in minutes                                    |

## Usage Notes

-   The JWT token should be stored securely and included in subsequent API requests
-   Token expiration should be handled gracefully in the client
-   The `isNewUser` flag can be used to trigger onboarding flows
-   Profile picture URLs may have size parameters that can be modified

## Security Considerations

-   JWT tokens should be stored in secure storage (EncryptedSharedPreferences)
-   Tokens should be validated on each app launch
-   Implement proper token refresh mechanisms
-   Never log or expose JWT tokens in debug builds
