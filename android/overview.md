# Overview

<!-- Modern Hero Section -->
<div class="hero-section">
    <h1 class="hero-title">True Native Experience</h1>
    <p class="hero-description">
       The Finsible Android client is a modern, native Android application built with Jetpack Compose and MVVM architecture. It provides a comprehensive financial management experience with offline-first capabilities and intelligent synchronization.
    </p>
</div>

## Technical Overview

<div style="display: flex; gap: 32px; flex-wrap: wrap; align-items: flex-start;">

<div style="flex: 1; min-width: 280px;">

#### Architecture & Design

| Aspect             | Details                                |
| ------------------ | -------------------------------------- |
| **Language**       | Kotlin 100%                            |
| **UI Framework**   | Jetpack Compose with Material Design 3 |
| **Architecture**   | MVVM with Repository Pattern           |
| **Minimum SDK**    | API 26 (Android 8.0)                   |
| **Target SDK**     | API 34 (Android 14)                    |
| **Compile SDK**    | API 36 (Android 16)                    |
| **Gradle Version** | `8.11.1`                               |
| **AGP Version**    | `8.5.2`                                |

</div>

<div style="flex: 1; min-width: 280px;">

#### Key Technologies

| Category                 | Technology                                 |
| ------------------------ | ------------------------------------------ |
| **Local Database**       | ObjectBox (high-performance local storage) |
| **Networking**           | Retrofit with OkHttp (API communication)   |
| **Dependency Injection** | Dagger Hilt                                |
| **Authentication**       | Google Sign-In, JWT token management       |
| **Serialization**        | Kotlinx Serialization (JSON parsing)       |

</div>
</div>

## Key Features

<div class="contact-section" style="margin-bottom: 0 !important;">

<!-- Financial Management -->
<div class="info-card help">
  <div class="info-card-content">
    <div class="info-card-header">
      <div class="info-card-icon help">
        <!-- Coins icon -->
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none"
          stroke="currentColor" stroke-width="2" stroke-linecap="round"
          stroke-linejoin="round" style="color: #ffffff;">
          <circle cx="8" cy="8" r="6" />
          <circle cx="16" cy="16" r="6" />
        </svg>
      </div>
      <h3 class="info-card-title">Financial Management</h3>
    </div>
    <div class="info-card-list">
      <div class="info-card-item enhanced">
        <div class="item-primary">
          <!-- List icon -->
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round" class="item-icon">
            <path d="M8 6h13M8 12h13M8 18h13" />
            <circle cx="3" cy="6" r="1" />
            <circle cx="3" cy="12" r="1" />
            <circle cx="3" cy="18" r="1" />
          </svg>
          Transaction Tracking
        </div>
        <div class="item-secondary">
          Create, edit, and categorize income, expenses, and transfers
        </div>
      </div>
      <div class="info-card-item enhanced">
        <div class="item-primary">
          <!-- Tag icon -->
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round" class="item-icon">
            <path
              d="M20.59 13.41 11 3H4v7l9.59 9.59a2 2 0 0 0 2.83 0l4.17-4.17a2 2 0 0 0 0-2.83Z" />
            <path d="M6 6h.01" />
          </svg>
          Smart Categories
        </div>
        <div class="item-secondary">
          Color-coded categorization with automatic filtering by transaction type
        </div>
      </div>
      <div class="info-card-item enhanced">
        <div class="item-primary">
          <!-- Wallet icon -->
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round" class="item-icon">
            <path d="M20 6H4a2 2 0 0 0-2 2v12h20V8a2 2 0 0 0-2-2Z" />
            <path d="M16 14h.01" />
          </svg>
          Account Management
        </div>
        <div class="item-secondary">
          Multiple account types with real-time balance tracking
        </div>
      </div>
      <div class="info-card-item enhanced">
        <div class="item-primary">
          <!-- Arrow left-right icon -->
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round" class="item-icon">
            <path d="M16 3h5v5" />
            <path d="M8 21H3v-5" />
            <path d="M21 3 14 10" />
            <path d="M3 21l7-7" />
          </svg>
          Transfer System
        </div>
        <div class="item-secondary">
          Seamless money transfers between accounts with fee tracking
        </div>
      </div>
    </div>
  </div>
</div>

<!-- Security & Authentication -->
<div class="info-card help">
  <div class="info-card-content">
    <div class="info-card-header">
      <div class="info-card-icon help">
        <!-- Shield icon -->
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none"
          stroke="currentColor" stroke-width="2" stroke-linecap="round"
          stroke-linejoin="round" style="color: #ffffff;">
          <path
            d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10Z" />
        </svg>
      </div>
      <h3 class="info-card-title">Security & Authentication</h3>
    </div>
    <div class="info-card-list">
      <div class="info-card-item enhanced">
        <div class="item-primary">
          <!-- Google icon -->
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round" class="item-icon">
            <path
              d="M21.35 11.1H12v2.9h5.35A5.99 5.99 0 0 1 12 18a6 6 0 1 1 0-12 5.92 5.92 0 0 1 4.19 1.64l2.06-2.06A9.94 9.94 0 0 0 12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10c0-.63-.07-1.25-.2-1.85Z" />
          </svg>
          Google OAuth
        </div>
        <div class="item-secondary">
          Secure authentication with Google Sign-In
        </div>
      </div>
      <div class="info-card-item enhanced">
        <div class="item-primary">
          <!-- Key icon -->
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round" class="item-icon">
            <path
              d="m21 2-2 2m-7.586 7.586a2 2 0 1 0-2.828 2.828L12 21h2l2-2v-2l2-2V9l2-2-2-2-2 2v2l-2 2h-2l-2 2" />
          </svg>
          JWT Token Management
        </div>
        <div class="item-secondary">
          Automatic token refresh and secure storage
        </div>
      </div>
      <div class="info-card-item enhanced">
        <div class="item-primary">
          <!-- Lock icon -->
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round" class="item-icon">
            <rect x="3" y="11" width="18" height="11" rx="2" ry="2" />
            <path d="M7 11V7a5 5 0 0 1 10 0v4" />
          </svg>
          Local Encryption
        </div>
        <div class="item-secondary">
          Sensitive data protection in local storage
        </div>
      </div>
      <div class="info-card-item enhanced">
        <div class="item-primary">
          <!-- Fingerprint icon -->
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round" class="item-icon">
            <path
              d="M12 11a4 4 0 0 1 4 4v1.5m0-5.5V9a4 4 0 1 0-8 0v2M12 2a10 10 0 0 0-10 10v2a10 10 0 0 0 20 0v-2a10 10 0 0 0-10-10Z" />
          </svg>
          Biometric Security
        </div>
        <div class="item-secondary">
          Fingerprint and face unlock support
        </div>
      </div>
    </div>
  </div>
</div>

<!-- Data & Synchronization -->
<div class="info-card help">
  <div class="info-card-content">
    <div class="info-card-header">
      <div class="info-card-icon help">
        <!-- Cloud sync icon -->
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none"
          stroke="currentColor" stroke-width="2" stroke-linecap="round"
          stroke-linejoin="round" style="color: #ffffff;">
          <path d="M16 16.5a4 4 0 0 0 0-8 5 5 0 0 0-9.9 1A3.5 3.5 0 0 0 7 16.5h9Z" />
          <path d="M12 16V10" />
          <polyline points="10 13 12 10 14 13" />
        </svg>
      </div>
      <h3 class="info-card-title">Data &amp; Synchronization</h3>
    </div>
    <div class="info-card-list">
      <div class="info-card-item enhanced">
        <div class="item-primary">
          <!-- Offline cloud icon -->
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round" class="item-icon">
            <path d="M16 16.5a4 4 0 0 0 0-8 5 5 0 0 0-9.9 1A3.5 3.5 0 0 0 7 16.5h9Z" />
            <line x1="7" y1="16" x2="17" y2="16" />
            <line x1="7" y1="18" x2="17" y2="18" />
          </svg>
          Offline-First Architecture
        </div>
        <div class="item-secondary">
          Full functionality without internet connectivity
        </div>
      </div>
      <div class="info-card-item enhanced">
        <div class="item-primary">
          <!-- Database icon -->
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round" class="item-icon">
            <ellipse cx="12" cy="5" rx="8" ry="3" />
            <path d="M4 5v6c0 1.1 3.6 2 8 2s8-.9 8-2V5" />
            <path d="M4 11v6c0 1.1 3.6 2 8 2s8-.9 8-2v-6" />
          </svg>
          ObjectBox Database
        </div>
        <div class="item-secondary">
          High-performance NoSQL database for offline data storage
        </div>
      </div>
      <div class="info-card-item enhanced">
        <div class="item-primary">
          <!-- Sync arrows icon -->
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round" class="item-icon">
            <polyline points="23 4 23 10 17 10" />
            <polyline points="1 20 1 14 7 14" />
            <path d="M3 10a9 9 0 0 1 14-7.7L23 10" />
            <path d="M21 14a9 9 0 0 1-14 7.7L1 14" />
          </svg>
          Smart Sync Engine
        </div>
        <div class="item-secondary">
          Intelligent background synchronization with conflict resolution
        </div>
      </div>
      <div class="info-card-item enhanced">
        <div class="item-primary">
          <!-- Check shield icon -->
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round" class="item-icon">
            <path
              d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10Z" />
            <polyline points="9 12 12 15 17 8" />
          </svg>
          Data Integrity
        </div>
        <div class="item-secondary">
          Comprehensive data validation and consistency checks
        </div>
      </div>
    </div>
  </div>
</div>

<!-- User Experience -->
<div class="info-card help">
  <div class="info-card-content">
    <div class="info-card-header">
      <div class="info-card-icon help">
        <!-- Light bulb icon -->
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none"
          stroke="currentColor" stroke-width="2" stroke-linecap="round"
          stroke-linejoin="round" style="color: #ffffff;">
          <path d="M9 18h6M10 22h4M12 2a6 6 0 0 0-4 10.9c.2.2.4.4.6.7V18h6v-4.4c.2-.3.4-.5.6-.7A6 6 0 0 0 12 2Z" />
        </svg>
      </div>
      <h3 class="info-card-title">User Experience</h3>
    </div>
    <div class="info-card-list">
      <div class="info-card-item enhanced">
        <div class="item-primary">
          <!-- Layout grid icon -->
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round" class="item-icon">
            <rect x="3" y="3" width="8" height="8" />
            <rect x="13" y="3" width="8" height="8" />
            <rect x="3" y="13" width="8" height="8" />
            <rect x="13" y="13" width="8" height="8" />
          </svg>
          Modern UI/UX
        </div>
        <div class="item-secondary">
          Clean, intuitive interface with Jetpack Compose
        </div>
      </div>
      <div class="info-card-item enhanced">
        <div class="item-primary">
          <!-- Palette icon -->
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round" class="item-icon">
            <path d="M12 22a8.38 8.38 0 0 0 4.06-1A8.5 8.5 0 1 0 7 5.94" />
            <circle cx="12" cy="12" r="3" />
          </svg>
          Material Design 3
        </div>
        <div class="item-secondary">
          Latest Material Design principles and components
        </div>
      </div>
      <div class="info-card-item enhanced">
        <div class="item-primary">
          <!-- Dark/light mode icon -->
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round" class="item-icon">
            <circle cx="12" cy="12" r="5" />
            <path
              d="M12 1v2M12 21v2M21 12h2M1 12H3M16.95 7.05l1.41 1.41M5.64 18.36l1.41 1.41M16.95 16.95 18.36 15.54M5.64 5.64 7.05 7.05" />
          </svg>
          Adaptive Themes
        </div>
        <div class="item-secondary">
          Beautiful dark/light themes with system preference support
        </div>
      </div>
      <div class="info-card-item enhanced">
        <div class="item-primary">
          <!-- Animation icon -->
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round"
            stroke-linejoin="round" class="item-icon">
            <path d="M12 2v20M2 12h20M4.93 4.93l14.14 14.14M19.07 4.93 4.93 19.07" />
          </svg>
          Smooth Animations
        </div>
        <div class="item-secondary">
          Fluid transitions and micro-interactions
        </div>
      </div>
    </div>
  </div>
</div>

</div>     

## Contributing

We welcome contributions to the Android client! Please read our [Contributing Guide](/android/contributing.md) for comprehensive details on:

-   **Code Style** - Kotlin coding standards and formatting
-   **Pull Request Process** - How to submit changes
-   **Issue Reporting** - Bug reports and feature requests
-   **Development Workflow** - Branch strategy and review process

### **Current Focus Areas**

-   UI/UX improvements and accessibility enhancements
-   Performance optimizations and memory management
-   Test coverage expansion and automation
-   Sync engine improvements and conflict resolution
-   Advanced analytics and reporting features

## **Support & Contact**

<div class="contact-section">

<!-- Development Team -->
<div class="info-card team">
    <div class="info-card-content">
        <div class="info-card-header">
            <div class="info-card-icon team">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="color: #ffffff;">
                    <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/>
                    <circle cx="9" cy="7" r="4"/>
                    <path d="M23 21v-2a4 4 0 0 0-3-3.87"/>
                    <path d="M16 3.13a4 4 0 0 1 0 7.75"/>
                </svg>
            </div>
            <h3 class="info-card-title">Development Team</h3>
        </div>
        <div class="info-card-list">
            <div class="info-card-item row">
                <span class="info-card-label">Lead Developer</span>
                <a href="https://github.com/alph-a07" class="info-card-link primary">@alph-a07</a>
            </div>
            <div class="info-card-item row">
                <span class="info-card-label">API Design</span>
                <a href="https://github.com/alph-a07" class="info-card-link secondary">@alph-a07</a>
            </div>
        </div>
    </div>
</div>

<!-- Get Help -->
<div class="info-card help">
    <div class="info-card-content">
        <div class="info-card-header">
            <div class="info-card-icon help">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="color: #ffffff;">
                    <circle cx="12" cy="12" r="3"/>
                    <path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"/>
                </svg>
            </div>
    <h3 class="info-card-title">Get Help</h3>
        </div>
        <div class="info-card-list">
            <div class="info-card-item enhanced">
                <div class="item-primary">
                    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="item-icon">
                        <circle cx="12" cy="12" r="10"/>
                        <path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"/>
                        <path d="M12 17h.01"/>
                    </svg>
                    <a href="https://github.com/finsible/android-client/issues">GitHub Issues</a>
                </div>
                <div class="item-secondary">Report bugs and request features</div>
            </div>
            <div class="info-card-item enhanced">
                <div class="item-primary">
                    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="item-icon">
                        <path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/>
                        <path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/>
                    </svg>
                    Documentation
                </div>
                <div class="item-secondary">Comprehensive development guides</div>
            </div>
        </div>
    </div>
</div>
</div>

---

<div class="repository-links">
    <!-- Repository Links -->
    <div class="repository-buttons">
        <a href="android/" class="repository-button android">
            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <rect width="14" height="20" x="5" y="2" rx="2" ry="2"/>
                <path d="M12 18h.01"/>
            </svg>
            Android Repository
        </a>
        <a href="web/" class="repository-button web">
            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <circle cx="12" cy="12" r="10"/>
                <path d="M2 12h20"/>
                <path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/>
            </svg>
            Web Repository
        </a>
        <a href="backend/" class="repository-button backend">
            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M6 3h12l4 6-10 13L2 9l4-6z"/>
                <path d="M11 3 8 9l4 13 4-13-3-6"/>
                <path d="M2 9h20"/>
            </svg>
            Backend Repository
        </a>
    </div>
    <div class="footer-message">
        <div class="footer-message-content">
            <p class="footer-message-text">
                Built with 
                <svg width="24" height="24" viewBox="0 0 24 24" fill="#e74c3c" stroke="#e74c3c" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="heartbeat-icon">
                    <path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/>
                </svg>
                for android developers and financial enthusiasts
            </p>
        </div>
    </div>
</div>
