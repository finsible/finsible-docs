# Contributing Guide

Thank you for your interest in contributing to Finsible! This guide outlines the process for contributing to our Android financial management application.

## 🤝 Code of Conduct

By participating in this project, you agree to abide by our Code of Conduct:

-   **Be respectful** and inclusive in all interactions
-   **Be constructive** when providing feedback
-   **Be collaborative** and help others learn
-   **Be patient** with newcomers and questions
-   **Be professional** in all communications

## 🚀 Getting Started

### Prerequisites

Before you begin, ensure you have:

-   [Android Studio](https://developer.android.com/studio) Narwhal (2025.1.2) or later
-   [JDK 17](https://openjdk.org/projects/jdk/17/) or higher
-   [Git](https://git-scm.com/) for version control
-   Basic knowledge of Kotlin and Android development
-   Familiarity with Jetpack Compose (helpful but not required)

### Development Setup

1. **Fork the Repository**

    ```bash
    # Fork the repo on GitHub, then clone your fork
    git clone https://github.com/YOUR_USERNAME/finsible-frontend.git
    cd finsible-frontend
    ```

2. **Set Up Upstream Remote**

    ```bash
    git remote add upstream https://github.com/finsible/finsible-frontend.git
    ```

3. **Configure Environment**

    ```bash
    # Copy example secrets file
    cp secrets.properties.example secrets.properties
    # Edit secrets.properties with your configuration
    ```

4. **Build and Test**

    ```bash
    # Build the project
    ./gradlew build

    # Run tests
    ./gradlew test
    ```

📖 **Detailed setup instructions: [Setup Guide](setup.md)**

## 🔄 Development Workflow

### Branch Strategy

We use a simplified Git Flow:

```
main
├── develop
├── feature/user-authentication
├── feature/transaction-filtering
├── bugfix/balance-calculation
└── hotfix/critical-security-fix
```

### Branch Naming Convention

| Type              | Pattern                   | Example                                |
| ----------------- | ------------------------- | -------------------------------------- |
| **Feature**       | `feature/description`     | `feature/dark-theme`                   |
| **Bug Fix**       | `bugfix/description`      | `bugfix/crash-on-startup`              |
| **Enhancement**   | `enhancement/description` | `enhancement/performance-optimization` |
| **Hotfix**        | `hotfix/description`      | `hotfix/security-vulnerability`        |
| **Documentation** | `docs/description`        | `docs/api-documentation`               |

### Creating a Feature

1. **Create Branch**

    ```bash
    # Update develop branch
    git checkout develop
    git pull upstream develop

    # Create feature branch
    git checkout -b feature/your-feature-name
    ```

2. **Make Changes**

    - Write code following our [coding standards](development.md#coding-standards)
    - Add tests for new functionality
    - Update documentation as needed

3. **Commit Changes**

    ```bash
    # Stage changes
    git add .

    # Commit with descriptive message
    git commit -m "feat: add user profile settings screen"
    ```

4. **Push and Create PR**

    ```bash
    # Push to your fork
    git push origin feature/your-feature-name

    # Create Pull Request on GitHub
    ```

## 📝 Commit Convention

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

### Commit Message Format

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Commit Types

| Type       | Description             | Example                                          |
| ---------- | ----------------------- | ------------------------------------------------ |
| `feat`     | New feature             | `feat: add transaction filtering`                |
| `fix`      | Bug fix                 | `fix: resolve balance calculation error`         |
| `docs`     | Documentation           | `docs: update API documentation`                 |
| `style`    | Code style changes      | `style: format code with ktlint`                 |
| `refactor` | Code refactoring        | `refactor: extract transaction validation logic` |
| `perf`     | Performance improvement | `perf: optimize database queries`                |
| `test`     | Add or update tests     | `test: add unit tests for ViewModel`             |
| `build`    | Build system changes    | `build: update Gradle to 8.0`                    |
| `ci`       | CI configuration        | `ci: add automated testing workflow`             |
| `chore`    | Maintenance tasks       | `chore: update dependencies`                     |

### Examples

```bash
# Feature addition
git commit -m "feat(auth): add Google Sign-In integration"

# Bug fix
git commit -m "fix(transactions): resolve duplicate transaction creation"

# Documentation
git commit -m "docs(api): add expense categories documentation"

# Breaking change
git commit -m "feat!: migrate to new authentication system

BREAKING CHANGE: AuthService.login() method signature changed"
```

## 🏗️ Code Standards

### Architecture Guidelines

-   **Follow MVVM pattern** with Repository and Use Cases
-   **Use Dependency Injection** with Dagger Hilt
-   **Implement offline-first** architecture
-   **Write testable code** with clear separation of concerns

### Kotlin Style Guide

```kotlin
// ✅ Good
class TransactionRepository @Inject constructor(
    private val localDataSource: TransactionLocalDataSource,
    private val remoteDataSource: TransactionRemoteDataSource
) {
    suspend fun createTransaction(transaction: Transaction): Result<Transaction> {
        return try {
            val result = localDataSource.insertTransaction(transaction)
            Result.success(result)
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
}

// ❌ Avoid
class TransactionRepository {
    fun createTransaction(transaction: Transaction) {
        // Direct database access without error handling
        database.insert(transaction)
    }
}
```

### Jetpack Compose Guidelines

```kotlin
// ✅ Good
@Composable
fun TransactionCard(
    transaction: Transaction,
    onEdit: (Transaction) -> Unit,
    modifier: Modifier = Modifier
) {
    Card(
        modifier = modifier.fillMaxWidth(),
        onClick = { onEdit(transaction) }
    ) {
        Column(
            modifier = Modifier.padding(16.dp)
        ) {
            Text(
                text = transaction.description,
                style = MaterialTheme.typography.bodyLarge
            )
            Text(
                text = transaction.amount.formatCurrency(),
                style = MaterialTheme.typography.bodyMedium
            )
        }
    }
}

// ❌ Avoid
@Composable
fun TransactionCard(transaction: Transaction) {
    // Missing modifier parameter
    // No click handling
    // Hardcoded styling
}
```

## 🧪 Testing Requirements

### Test Coverage Expectations

| Component        | Minimum Coverage |
| ---------------- | ---------------- |
| **Domain Layer** | 90%              |
| **Data Layer**   | 85%              |
| **UI Layer**     | 70%              |

### Required Tests

1. **Unit Tests** for all business logic

    ```kotlin
    class CreateTransactionUseCaseTest {
        @Test
        fun `should create transaction successfully`() {
            // Test implementation
        }
    }
    ```

2. **Integration Tests** for repository implementations

    ```kotlin
    @RunWith(AndroidJUnit4::class)
    class TransactionRepositoryTest {
        @Test
        fun `should save and retrieve transactions`() {
            // Test implementation
        }
    }
    ```

3. **UI Tests** for critical user flows
    ```kotlin
    class CreateTransactionScreenTest {
        @Test
        fun `should create transaction when form is valid`() {
            // Test implementation
        }
    }
    ```

### Running Tests

```bash
# Run all tests
./gradlew test

# Run specific test class
./gradlew test --tests "TransactionRepositoryTest"

# Generate coverage report
./gradlew jacocoTestReport
```

## 📋 Pull Request Process

### PR Requirements

Before submitting a Pull Request, ensure:

-   [ ] **Code builds successfully**
-   [ ] **All tests pass**
-   [ ] **Code coverage meets requirements**
-   [ ] **No lint errors or warnings**
-   [ ] **Documentation updated** (if applicable)
-   [ ] **Self-review completed**

### PR Template

```markdown
## Description

Brief description of changes made.

## Type of Change

-   [ ] Bug fix (non-breaking change)
-   [ ] New feature (non-breaking change)
-   [ ] Breaking change (fix or feature causing existing functionality to change)
-   [ ] Documentation update

## Testing

-   [ ] Unit tests added/updated
-   [ ] Integration tests added/updated
-   [ ] Manual testing completed

## Screenshots (if applicable)

Include screenshots of UI changes.

## Additional Notes

Any additional information for reviewers.
```

### Review Process

1. **Automated Checks**

    - CI build passes
    - Tests pass
    - Code coverage meets thresholds
    - Lint checks pass

2. **Code Review**

    - At least one team member review
    - Address all review comments
    - Maintain code quality standards

3. **Testing**

    - Manual testing of changes
    - Regression testing
    - Performance impact assessment

4. **Approval & Merge**
    - Required approvals received
    - All checks pass
    - Squash and merge to develop

## 🐛 Bug Reports

### Bug Report Template

When reporting bugs, please include:

```markdown
## Bug Description

Clear description of the bug.

## Steps to Reproduce

1. Step one
2. Step two
3. Step three

## Expected Behavior

What should happen.

## Actual Behavior

What actually happens.

## Environment

-   Android version:
-   Device model:
-   App version:

## Screenshots/Logs

Include relevant screenshots or log output.
```

### Severity Levels

| Level        | Description            | Response Time |
| ------------ | ---------------------- | ------------- |
| **Critical** | App crashes, data loss | 24 hours      |
| **High**     | Major feature broken   | 3 days        |
| **Medium**   | Minor feature issue    | 1 week        |
| **Low**      | Cosmetic issue         | 2 weeks       |

## 💡 Feature Requests

### Feature Request Template

```markdown
## Feature Description

Clear description of the proposed feature.

## Problem Statement

What problem does this solve?

## Proposed Solution

How should this feature work?

## Alternatives Considered

Other solutions you've considered.

## Additional Context

Screenshots, mockups, or references.
```

### Feature Evaluation Criteria

-   **User Impact**: How many users benefit?
-   **Technical Feasibility**: Implementation complexity
-   **Maintenance Cost**: Long-term support requirements
-   **Strategic Alignment**: Fits product roadmap

## 📚 Documentation

### Documentation Guidelines

-   **Keep it current** - Update docs with code changes
-   **Be comprehensive** - Cover all important aspects
-   **Use examples** - Include code samples and screenshots
-   **Be clear** - Write for your audience's knowledge level

### Types of Documentation

1. **API Documentation** - Document all endpoints
2. **Architecture Docs** - Explain system design
3. **Setup Guides** - Help new contributors
4. **User Guides** - End-user documentation

## 🎯 Contribution Areas

### Good First Issues

Look for issues labeled `good first issue`:

-   Documentation improvements
-   UI/UX enhancements
-   Test coverage improvements
-   Bug fixes in isolated components

### Areas Needing Help

-   **Testing**: Increase test coverage
-   **Accessibility**: Improve app accessibility
-   **Performance**: Optimize database queries
-   **Internationalization**: Add language support
-   **Documentation**: Keep docs up to date

## 🏆 Recognition

### Contributor Levels

| Level                   | Criteria               | Benefits                         |
| ----------------------- | ---------------------- | -------------------------------- |
| **Contributor**         | 1+ merged PR           | Listed in contributors           |
| **Regular Contributor** | 5+ merged PRs          | Early access to features         |
| **Core Contributor**    | 20+ PRs + code reviews | Voting rights on major decisions |

### Hall of Fame

Outstanding contributors are recognized in our README and documentation.

## 📞 Getting Help

### Communication Channels

-   **GitHub Issues**: Bug reports and feature requests
-   **GitHub Discussions**: General questions and ideas
-   **Email**: [dev@finsible.app](mailto:dev@finsible.app) for private matters

### When to Ask for Help

-   **Stuck on implementation** - Ask before spending too much time
-   **Unsure about approach** - Get feedback early
-   **Need clarification** - Better to ask than assume
-   **Want to contribute** - We're here to help!

## 🙏 Thank You

Your contributions make Finsible better for everyone. Whether you're fixing a typo, adding a feature, or helping with testing, every contribution is valuable and appreciated.

---

**Happy Contributing! 🚀**

For questions about contributing, please reach out through [GitHub Discussions](https://github.com/finsible/finsible-frontend/discussions) or email us at [dev@finsible.app](mailto:dev@finsible.app).
