# Backend & API

> **Spring Boot Financial Management API**

The Finsible backend is a robust Spring Boot application that provides secure RESTful APIs for financial data management. It serves as the central data layer for both Android and web clients, offering comprehensive financial services with enterprise-grade security and performance.

## <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m13 2-2 2.5h3L12 7"/><path d="M10 14v-3"/><path d="M14 14v-3"/><path d="M11 19c.64.35 1.36.35 2 0"/><path d="M4 13a2 2 0 0 1 0-6h16a2 2 0 0 1 0 6"/><path d="M4 19a2 2 0 0 1 0-6h16a2 2 0 0 1 0 6"/></svg> **Technical Overview**

### **Architecture & Design**

-   **Framework**: Spring Boot with Java
-   **Database**: PostgreSQL with JPA/Hibernate
-   **Security**: Spring Security with JWT authentication
-   **API Design**: RESTful architecture with OpenAPI/Swagger documentation
-   **Deployment**: Containerized with Docker, cloud-ready

### **Key Technologies**

-   **Backend**: Spring Boot, Spring Security, Spring Data JPA
-   **Database**: PostgreSQL with connection pooling
-   **Authentication**: JWT tokens with refresh token support
-   **Documentation**: OpenAPI 3.0 (Swagger) for API specification
-   **Testing**: JUnit, Spring Boot Test, TestContainers
-   **Build Tool**: Maven with multi-profile configuration

## <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="18" height="11" x="3" y="11" rx="2" ry="2"/><path d="m7 11V7a5 5 0 0 1 10 0v4"/></svg> **Core Services**

### **Authentication & Security**

-   **JWT Authentication** - Secure token-based authentication system
-   **Google OAuth Integration** - Social login with Google Sign-In
-   **Role-Based Access Control** - Granular permission management
-   **Token Management** - Automatic token refresh and revocation
-   **Data Encryption** - Sensitive data protection at rest and in transit

### **Financial Data Management**

-   **Account Services** - Multi-account management with real-time balances
-   **Transaction Engine** - Comprehensive transaction processing and validation
-   **Category Management** - Dynamic categorization system with custom categories
-   **Transfer Processing** - Inter-account transfers with fee calculation
-   **Balance Reconciliation** - Automatic balance updates and consistency checks

### **Synchronization & Data Integrity**

-   **Real-time Sync** - Live data synchronization across all clients
-   **Conflict Resolution** - Intelligent conflict detection and resolution
-   **Data Validation** - Comprehensive server-side validation
-   **Audit Logging** - Complete transaction and change audit trails
-   **Backup & Recovery** - Automated data backup and recovery procedures

### **Analytics & Reporting**

-   **Spending Analysis** - Category-based spending insights and trends
-   **Budget Tracking** - Budget creation, monitoring, and alerts
-   **Financial Reports** - Comprehensive financial reporting and analytics
-   **Data Export** - Multiple format exports (JSON, CSV, PDF)

## <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M14.7 6.3a1 1 0 0 0 0 1.4l1.6 1.6a1 1 0 0 0 1.4 0l3.77-3.77a6 6 0 0 1-7.94 7.94l-6.91 6.91a2.12 2.12 0 0 1-3-3l6.91-6.91a6 6 0 0 1 7.94-7.94l-3.76 3.76z"></path></svg> **Development Status**

> **👥 Development Team**: The backend is actively maintained by [maitry291](https://github.com/maitry291) with API design collaboration from [alph-a07](https://github.com/alph-a07).

### **Current Architecture Focus**

-   <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m21.44 11.05-9.19 9.19a6 6 0 0 1-8.49-8.49l9.19-9.19a4 4 0 0 1 5.66 5.66L9.64 16.2a2 2 0 0 1-2.83-2.83l8.49-8.49"/></svg> **API Design** - RESTful endpoint design and specification
-   <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="18" height="11" x="3" y="11" rx="2" ry="2"/><path d="m7 11V7a5 5 0 0 1 10 0v4"/></svg> **Security Implementation** - Authentication and authorization layers
-   <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 7v1a3 3 0 0 0 3 3h1m0-4v4m0 0v5a2 2 0 0 0 2 2h2"/><path d="M20 7v1a3 3 0 0 1-3 3h-1m0-4v4m0 0v5a2 2 0 0 1-2 2h-2"/><path d="M10 7h4"/></svg> **Database Design** - Efficient schema design and optimization
-   <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m17 2 4 4-4 4"/><path d="m3 6 4 4-4 4"/><path d="m7 2 10 20"/></svg> **Sync Engine** - Real-time data synchronization mechanisms
-   <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 3v18h18"/><path d="m19 9-5 5-4-4-3 3"/></svg> **Performance Optimization** - Query optimization and caching strategies

## <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 3v18h18"/><path d="m19 9-5 5-4-4-3 3"/></svg> **API Documentation**

### **Available Endpoints**

#### **<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="18" height="11" x="3" y="11" rx="2" ry="2"/><path d="m7 11V7a5 5 0 0 1 10 0v4"/></svg> Authentication APIs**

-   **[Google Sign-In Success](api/auth/google_signin_success.md)** - Successful OAuth authentication
-   **[Google Sign-In Error](api/auth/google_signin_error.md)** - Authentication error handling

#### **<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"/></svg> Account Management APIs**

-   **[List User Accounts](api/accounts/list.md)** - Retrieve all user accounts with balances

#### **<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="2" r="1"/><path d="M12 4v12"/><path d="M8 8l4-4 4 4"/></svg> Category Management APIs**

-   **[Income Categories](api/categories/income.md)** - Retrieve income categorization options
-   **[Expense Categories](api/categories/expense.md)** - Retrieve expense categorization options
-   **[Transfer Categories](api/categories/transfer.md)** - Retrieve transfer categorization options

#### **<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 2v20M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"/></svg> Transaction APIs**

-   **[Create Transaction](api/transactions/create.md)** - Create new financial transactions

### **API Standards**

-   **Base URL**: `https://api.finsible.app`
-   **Authentication**: JWT Bearer tokens required for protected endpoints
-   **Response Format**: Consistent JSON structure with success/error indicators
-   **Error Handling**: Standardized HTTP status codes and error messages
-   **Rate Limiting**: Implemented to prevent abuse and ensure fair usage

## <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z"/><path d="M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z"/></svg> **Documentation** _(Coming Soon)_

### **Development Guides**

-   **[<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="3"/><path d="M12 1v6m0 6v6m11-7h-6m-6 0H1"/></svg> Setup & Installation](setup.md)** - Backend development environment setup
-   **[<svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M14.7 6.3a1 1 0 0 0 0 1.4l1.6 1.6a1 1 0 0 0 1.4 0l3.77-3.77a6 6 0 0 1-7.94 7.94l-6.91 6.91a2.12 2.12 0 0 1-3-3l6.91-6.91a6 6 0 0 1 7.94-7.94l-3.76 3.76z"></path></svg> Development Guide](development.md)** - Development workflow and best practices
-   **[<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m21.44 11.05-9.19 9.19a6 6 0 0 1-8.49-8.49l9.19-9.19a4 4 0 0 1 5.66 5.66L9.64 16.2a2 2 0 0 1-2.83-2.83l8.49-8.49"/></svg> Architecture](architecture.md)** - Backend architecture and design patterns
-   **[<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 7v1a3 3 0 0 0 3 3h1m0-4v4m0 0v5a2 2 0 0 0 2 2h2"/><path d="M20 7v1a3 3 0 0 1-3 3h-1m0-4v4m0 0v5a2 2 0 0 1-2 2h-2"/><path d="M10 7h4"/></svg> Database Schema](database.md)** - Database design and entity relationships

### **API Reference**

-   **[<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="8" height="4" x="8" y="2" rx="1" ry="1"/><path d="M16 4h2a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2V6a2 2 0 0 1 2-2h2"/></svg> API Overview](api/README.md)** - Complete API documentation and usage guides
-   **OpenAPI Specification** - Interactive API documentation with Swagger UI
-   **Authentication Guide** - JWT implementation and security best practices
-   **Rate Limiting** - API usage limits and optimization strategies

## <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="m19 8 2 2-2 2"/><path d="m17 12h4"/></svg> **Contributing**

> **Collaboration**: Backend contributions are handled by the maintainer [maitry291](https://github.com/maitry291). API design and specifications are collaboratively developed with [alph-a07](https://github.com/alph-a07).

### **Development Areas**

-   <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="18" height="11" x="3" y="11" rx="2" ry="2"/><path d="m7 11V7a5 5 0 0 1 10 0v4"/></svg> **Security Enhancements** - Authentication and authorization improvements
-   <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m13 2-2 2.5h3L12 7"/><path d="M10 14v-3"/><path d="M14 14v-3"/><path d="M11 19c.64.35 1.36.35 2 0"/><path d="M4 13a2 2 0 0 1 0-6h16a2 2 0 0 1 0 6"/><path d="M4 19a2 2 0 0 1 0-6h16a2 2 0 0 1 0 6"/></svg> **Performance Optimization** - Database and API performance tuning
-   <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 3v18h18"/><path d="m19 9-5 5-4-4-3 3"/></svg> **Analytics Features** - Advanced financial analytics and reporting
-   <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m17 2 4 4-4 4"/><path d="m3 6 4 4-4 4"/><path d="m7 2 10 20"/></svg> **Sync Engine** - Real-time synchronization improvements
-   <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"/><path d="M12 17h.01"/></svg> **Testing Coverage** - Comprehensive test suite expansion

### **API Design Process**

-   **Collaborative Design** - API specifications designed jointly by both maintainers
-   **Documentation First** - API documentation precedes implementation
-   **Version Control** - Semantic versioning for API changes
-   **Backward Compatibility** - Maintaining compatibility across client versions

## <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#42a585" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/></svg> **Support & Contact**

### **Development Team**

-   **Backend Lead**: [maitry291](https://github.com/maitry291)
-   **API Design**: [alph-a07](https://github.com/alph-a07)

### **Repository Information**

-   **📂 Repository**: [finsible/finsible-backend](https://github.com/finsible/finsible-backend)
-   **🔄 Status**: Active development
-   **🤝 Collaboration**: Coordinated development with API design collaboration
-   **📋 Issues**: Managed in repository issue tracker

### **API Resources**

-   **📖 Documentation**: Complete API reference and guides
-   **🔧 Status Page**: API service status and uptime monitoring
-   **📊 Performance**: Real-time API performance metrics
-   **🔒 Security**: Security policies and vulnerability reporting

---

<div align="center">

**Powering Finsible with secure and scalable backend services**

[🏠 Home](/) • [🔍 Overview](../README.md) • [📱 Android](../android/) • [🌐 Web](../web/)

</div>
