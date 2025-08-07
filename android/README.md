# Android Client

> **Native Android Financial Management Application**

The Finsible Android client is a modern, native Android application built with Jetpack Compose and MVVM architecture. It provides a comprehensive financial management experience with offline-first capabilities and intelligent synchronization.

## <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="18" height="18" rx="2"/><path d="M9 9h6v6H9z"/></svg> **Technical Overview**

### **Architecture & Design**

-   **Language**: Kotlin 100%
-   **UI Framework**: Jetpack Compose with Material Design 3
-   **Architecture**: MVVM with Repository Pattern
-   **Minimum SDK**: API 26 (Android 8.0)
-   **Target SDK**: API 34
-   **Compile SDK**: API 35

### **Key Technologies**

-   **Database**: ObjectBox for high-performance local storage
-   **Networking**: Retrofit with OkHttp for API communication
-   **Dependency Injection**: Dagger Hilt
-   **Authentication**: Google Sign-In with JWT token management
-   **Serialization**: Kotlinx Serialization for JSON parsing

## <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="14" height="20" x="5" y="2" rx="2" ry="2"/><path d="M12 18h.01"/></svg> **Key Features**

### **<svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="12" x2="12" y1="2" y2="22"/><path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"/></svg> Financial Management**

-   **Transaction Tracking** - Create, edit, and categorize income, expenses, and transfers
-   **Smart Categories** - Color-coded categorization with automatic filtering by transaction type
-   **Account Management** - Multiple account types with real-time balance tracking
-   **Transfer System** - Seamless money transfers between accounts with fee tracking

### **<svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="18" height="11" x="3" y="11" rx="2" ry="2"/><circle cx="12" cy="16" r="1"/><path d="m7 11-2-2v-1a2 2 0 0 1 4 0v1"/></svg> Security & Authentication**

-   **Google OAuth** - Secure authentication with Google Sign-In
-   **JWT Token Management** - Automatic token refresh and secure storage
-   **Local Encryption** - Sensitive data protection in local storage
-   **Biometric Security** - Fingerprint and face unlock support

### **<svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 3v18h18"/><path d="M18.7 8 22 12l-3.3 4"/><path d="M7 8v8"/><path d="M11 8v8"/></svg> Data & Synchronization**

-   **Offline-First Architecture** - Full functionality without internet connection
-   **ObjectBox Database** - High-performance local storage with query optimization
-   **Smart Sync Engine** - Intelligent background synchronization with conflict resolution
-   **Data Integrity** - Comprehensive data validation and consistency checks

### **<svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="13.5" cy="6.5" r=".5" fill="currentColor"/><circle cx="17.5" cy="10.5" r=".5" fill="currentColor"/><circle cx="8.5" cy="7.5" r=".5" fill="currentColor"/><circle cx="6.5" cy="12.5" r=".5" fill="currentColor"/><path d="M12 2C6.5 2 2 6.5 2 12s4.5 10 10 10c.926 0 1.648-.746 1.648-1.688 0-.437-.18-.835-.437-1.125-.29-.289-.438-.652-.438-1.125a1.64 1.64 0 0 1 1.668-1.668h1.996c3.051 0 5.555-2.503 5.555-5.554C21.965 6.012 17.461 2 12 2z"/></svg> User Experience**

-   **Modern UI/UX** - Clean, intuitive interface with Jetpack Compose
-   **Material Design 3** - Latest Material Design principles and components
-   **Adaptive Themes** - Beautiful dark/light themes with system preference support
-   **Responsive Design** - Optimized layouts for phones and tablets
-   **Smooth Animations** - Fluid transitions and micro-interactions
-   **Accessibility** - Comprehensive accessibility features and screen reader support

## <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 3h4a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2h-4"/><polyline points="10,17 15,12 10,7"/><line x1="15" x2="3" y1="12" y2="12"/></svg> **Getting Started**

### **For Users**

1. **Download**: Get the app from [Google Play Store](https://play.google.com/store/apps/details?id=com.itsjeel01.finsiblefrontend)
2. **Sign Up**: Use Google Sign-In for quick and secure account creation
3. **Setup Accounts**: Add your bank accounts, credit cards, and cash accounts
4. **Start Tracking**: Begin recording your income, expenses, and transfers

### **For Developers**

1. **Prerequisites**: Android Studio Narwhal (2025.1.2+), JDK 17+, Android SDK API 26-35
2. **Clone Repository**: `git clone https://github.com/finsible/android-client.git`
3. **Setup Environment**: Configure Google OAuth credentials and secrets
4. **Build & Run**: Sync Gradle and run on device or emulator

## <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z"/><path d="M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z"/></svg> **Documentation**

### **Development Guides**

-   **[<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="3"/><path d="M12 1v6m0 6v6m11-7h-6m-6 0H1"/></svg> Setup & Installation](setup.md)** - Complete development environment setup
-   **[<svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M14.7 6.3a1 1 0 0 0 0 1.4l1.6 1.6a1 1 0 0 0 1.4 0l3.77-3.77a6 6 0 0 1-7.94 7.94l-6.91 6.91a2.12 2.12 0 0 1-3-3l6.91-6.91a6 6 0 0 1 7.94-7.94l-3.76 3.76z"></path></svg> Development Guide](development.md)** - Development workflow and best practices
-   **[<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m21.44 11.05-9.19 9.19a6 6 0 0 1-8.49-8.49l9.19-9.19a4 4 0 0 1 5.66 5.66L9.64 16.2a2 2 0 0 1-2.83-2.83l8.49-8.49"/></svg> Architecture](architecture.md)** - Detailed architecture overview and patterns
-   **[<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"/><path d="M12 17h.01"/></svg> Testing Guide](testing.md)** - Comprehensive testing strategy and implementation
-   **[<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="m19 8 2 2-2 2"/><path d="m17 12h4"/></svg> Contributing](contributing.md)** - Contribution guidelines and code standards

### **Technical Resources**

-   **[<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="3"/><path d="M12 1v6m0 6v6m11-7h-6m-6 0H1"/></svg> Mock Server Setup](mock-server.md)** - Local development with mock backend
-   **[<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="7" height="9" x="3" y="3" rx="1"/><rect width="7" height="5" x="14" y="3" rx="1"/><rect width="7" height="9" x="14" y="12" rx="1"/><rect width="7" height="5" x="3" y="16" rx="1"/></svg> Project Structure](development.md#project-structure)** - Codebase organization
-   **[<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 3v18m9-9H3"/></svg> UI Components](development.md#ui-components)** - Reusable Compose components
-   **[<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 7v1a3 3 0 0 0 3 3h1m0-4v4m0 0v5a2 2 0 0 0 2 2h2"/><path d="M20 7v1a3 3 0 0 1-3 3h-1m0-4v4m0 0v5a2 2 0 0 1-2 2h-2"/><path d="M10 7h4"/></svg> Database Schema](development.md#database)** - ObjectBox model definitions

## <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="m19 8 2 2-2 2"/><path d="m17 12h4"/></svg> **Contributing**

We welcome contributions to the Android client! Please read our [Contributing Guide](contributing.md) for details on:

-   **Code Style** - Kotlin coding standards and formatting
-   **Pull Request Process** - How to submit changes
-   **Issue Reporting** - Bug reports and feature requests
-   **Development Workflow** - Branch strategy and review process

### **Current Focus Areas**

-   <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 3v18m9-9H3"/></svg> UI/UX improvements and accessibility enhancements
-   <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m13 2-2 2.5h3L12 7"/><path d="M10 14v-3"/><path d="M14 14v-3"/><path d="M11 19c.64.35 1.36.35 2 0"/><path d="M4 13a2 2 0 0 1 0-6h16a2 2 0 0 1 0 6"/><path d="M4 19a2 2 0 0 1 0-6h16a2 2 0 0 1 0 6"/></svg> Performance optimizations and memory management
-   <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"/><path d="M12 17h.01"/></svg> Test coverage expansion and automation
-   <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m17 2 4 4-4 4"/><path d="m3 6 4 4-4 4"/><path d="m7 2 10 20"/></svg> Sync engine improvements and conflict resolution
-   <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 3v18h18"/><path d="m19 9-5 5-4-4-3 3"/></svg> Advanced analytics and reporting features

## <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/></svg> **Support & Contact**

### **Development Team**

-   **Lead Developer**: [alph-a07](https://github.com/alph-a07)
-   **API Design**: [alph-a07](https://github.com/alph-a07)

### **Get Help**

-   **[GitHub Issues](https://github.com/finsible/android-client/issues)** - Report bugs and request features
-   **[Discussions](https://github.com/finsible/android-client/discussions)** - Community discussions and Q&A
-   **[Documentation](setup.md)** - Comprehensive development guides

### **Repository Links**

-   **<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="7" height="9" x="3" y="3" rx="1"/><rect width="7" height="5" x="14" y="3" rx="1"/><rect width="7" height="9" x="14" y="12" rx="1"/><rect width="7" height="5" x="3" y="16" rx="1"/></svg> Source Code**: [finsible/android-client](https://github.com/finsible/android-client)
-   **<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="8" height="4" x="8" y="2" rx="1" ry="1"/><path d="M16 4h2a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2V6a2 2 0 0 1 2-2h2"/></svg> Project Board**: [Development Roadmap](https://github.com/finsible/android-client/projects)
-   **<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"/><path d="M12 17h.01"/></svg> Issue Tracker**: [Bug Reports & Features](https://github.com/finsible/android-client/issues)
-   **<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7,10 12,15 17,10"/><line x1="12" x2="12" y1="15" y2="3"/></svg> Discussions**: [Community Forum](https://github.com/finsible/android-client/discussions)

---

<div align="center">

**Built with** <svg width="18" height="18" viewBox="0 0 24 24" fill="#e74c3c" stroke="#e74c3c" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="animation: heartbeat 1.5s ease-in-out infinite;"><path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/></svg> **for Android developers and financial enthusiasts**

<style>
@keyframes heartbeat {
  0% { transform: scale(1); }
  14% { transform: scale(1.3); }
  28% { transform: scale(1); }
  42% { transform: scale(1.3); }
  70% { transform: scale(1); }
}
</style>

[🏠 Home](/) • [🔍 Overview](../README.md) • [🌐 Web Client](../web/) • [⚡ Backend](../backend/)

</div>
