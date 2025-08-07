# Setup & Installation

This guide will help you set up the Finsible Android development environment and get the project running locally.

## Prerequisites

Before you begin, ensure you have the following installed:

### Required Software

| Software           | Version              | Purpose             |
| ------------------ | -------------------- | ------------------- |
| **Android Studio** | Narwhal (2025.1.2) or later | Primary IDE |
| **JDK**            | 17 or higher         | Kotlin compilation  |
| **Git**            | Latest               | Version control     |
| **Android SDK**    | API levels 26-35     | Android development |

### Hardware Requirements

-   **RAM**: 8GB minimum, 16GB recommended
-   **Storage**: 10GB free space for Android Studio + SDK

## Development Setup

### 1. Clone Repository

```bash
# Clone the repository
git clone https://github.com/finsible/android-client.git
cd FinsibleFrontend
```

### 2. Google OAuth Setup

#### Google Cloud Console Configuration

1. **Create Project**
   - Go to [Google Cloud Console](https://console.cloud.google.com)
   - Create a new project or select existing
   - Enable Google Sign-In API

2. **OAuth Consent Screen**
   - Configure OAuth consent screen
   - Set app domain and privacy policy

3. **Create Credentials**
   - Go to "Credentials" section
   - Click "Create Credentials" → "OAuth 2.0 Client IDs"
   - Select "Android" application type

#### Get SHA-1 Fingerprint

```bash
# Debug keystore
keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android

# Release keystore (if available)
keytool -list -v -keystore path/to/release.keystore -alias your-key-alias
```

#### Configure OAuth Client

-   **Package Name**: `com.itsjeel01.finsiblefrontend`
-   **SHA-1 Certificate**: Use fingerprint from above
-   **Client ID**: Copy generated client ID

### 3. Configure Secrets

Create `secrets.properties` in the project root:

```properties
# API Configuration
BASE_URL="https://finsible.app"

# Google OAuth
SERVER_CLIENT_ID="your-google-oauth-client-id-here.apps.googleusercontent.com"

# Debug Configuration (optional)
DEBUG_MODE=true
LOG_LEVEL="DEBUG"
```

### 4. Android Studio Setup

1. **Import Project**
   - Open Android Studio
   - Click "Open an existing project"
   - Select the FinsibleFrontend directory
   - Click "OK"

2. **SDK Configuration**
   - Go to Tools → SDK Manager
   - Install required SDK versions (26-35)
   - Accept license agreements

3. **Gradle Sync**
   - Wait for automatic Gradle sync
   - Resolve any dependency issues

### 5. Build Project

```bash
# Build debug version
./gradlew assembleDebug

# Build release version (optional)
./gradlew assembleRelease
```

### 6. Run Application

-   Connect Android device or start emulator (API 26+)
-   Click "Run" button in Android Studio
-   Select target device
-   App will install and launch

## Mock Server Configuration

> ⚠️ **Important**: The backend server is not deployed. Use mock responses for development.

Mock JSON responses are located in:
```
app/src/main/assets/mock_responses/
├── auth/
│   ├── google_signin_success.json
│   └── google_signin_error.json
├── categories/
│   ├── categories_list.json
│   ├── category_add_success.json
│   └── category_rename_success.json
└── transactions/
    ├── transactions_list.json
    └── transaction_create_success.json
```

📖 **[Complete Mock Server Setup →](mock-server.md)**

## Troubleshooting

### Common Issues

**Gradle Sync Failed**
- Check internet connection
- Invalidate caches: File → Invalidate Caches and Restart
- Update Android Studio and Gradle plugin

**Google Sign-In Not Working**
- Verify OAuth client ID in `secrets.properties`
- Check SHA-1 fingerprint configuration
- Ensure Google Sign-In API is enabled

**Build Errors**
- Clean project: Build → Clean Project
- Rebuild project: Build → Rebuild Project
- Check Android SDK installation

**Emulator Issues**
- Enable hardware acceleration (Intel HAXM/AMD)
- Allocate sufficient RAM to emulator
- Use recommended system images (API 26+)

## Next Steps

Once setup is complete:

1. 📖 Read the [Development Guide](development.md)
2. 🏗️ Understand the [Architecture](architecture.md)  
3. 🧪 Set up [Testing](testing.md)
4. 🤝 Review [Contributing Guidelines](contributing.md)

## Getting Help

- **Issues**: Create issue on [GitHub](https://github.com/finsible/finsible-frontend/issues)
- **Questions**: Use "question" label
- **Maintainer**: [@alph-a07](https://github.com/alph-a07)
