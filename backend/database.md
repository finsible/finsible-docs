# Database Schema

> **PostgreSQL Database Design for Finsible Financial Management**

The Finsible database is designed with PostgreSQL to provide robust, scalable, and efficient data storage for financial management operations. The schema follows normalization principles while optimizing for performance and data integrity.

## 🗃️ **Schema Overview**

### **Core Tables Structure**

```
├── Users & Authentication
│   ├── users                    # User accounts and profiles
│   └── user_sessions           # Active user sessions
├── Financial Accounts
│   ├── accounts                # User financial accounts
│   └── account_types           # Account type definitions
├── Transaction Management
│   ├── transactions           # All financial transactions
│   ├── transaction_types      # Transaction type enumeration
│   └── transfer_details       # Additional transfer information
├── Categorization
│   ├── categories             # Transaction categories
│   └── category_domains       # Category domain groupings
└── System & Audit
    ├── sync_logs              # Data synchronization tracking
    ├── audit_logs             # System activity logs
    └── user_preferences       # User-specific settings
```

## 👤 **User Management Schema**

### **users**

Primary user account information with OAuth integration.

```sql
CREATE TABLE users (
    id                BIGSERIAL PRIMARY KEY,
    google_id         VARCHAR(255) UNIQUE NOT NULL,
    email             VARCHAR(255) UNIQUE NOT NULL,
    name              VARCHAR(255) NOT NULL,
    picture_url       VARCHAR(500),
    account_created   TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    last_logged_in    TIMESTAMP,
    is_active         BOOLEAN NOT NULL DEFAULT true,
    created_at        TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at        TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

    -- Indexes
    CONSTRAINT idx_users_google_id UNIQUE (google_id),
    CONSTRAINT idx_users_email UNIQUE (email)
);

-- Additional indexes for performance
CREATE INDEX idx_users_active ON users(is_active);
CREATE INDEX idx_users_last_login ON users(last_logged_in);
```

### **user_sessions**

Track active user sessions for security and analytics.

```sql
CREATE TABLE user_sessions (
    id              BIGSERIAL PRIMARY KEY,
    user_id         BIGINT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    session_token   VARCHAR(500) UNIQUE NOT NULL,
    device_info     JSONB,
    ip_address      INET,
    expires_at      TIMESTAMP NOT NULL,
    created_at      TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    last_accessed   TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

    -- Indexes
    CONSTRAINT idx_sessions_token UNIQUE (session_token)
);

CREATE INDEX idx_sessions_user_id ON user_sessions(user_id);
CREATE INDEX idx_sessions_expires ON user_sessions(expires_at);
```

## 💰 **Account Management Schema**

### **account_types**

Predefined account types for categorization.

```sql
CREATE TABLE account_types (
    id          SERIAL PRIMARY KEY,
    code        VARCHAR(50) UNIQUE NOT NULL,
    name        VARCHAR(100) NOT NULL,
    description TEXT,
    color       VARCHAR(7), -- Hex color code
    icon        VARCHAR(50),
    is_active   BOOLEAN NOT NULL DEFAULT true,

    -- Constraints
    CONSTRAINT chk_account_type_color CHECK (color ~ '^#[0-9A-Fa-f]{6}$')
);

-- Insert default account types
INSERT INTO account_types (code, name, description, color) VALUES
('CASH', 'Cash', 'Physical cash holdings', '#4CAF50'),
('CHECKING', 'Checking Account', 'Bank checking account', '#2196F3'),
('SAVINGS', 'Savings Account', 'Bank savings account', '#FF9800'),
('CREDIT_CARD', 'Credit Card', 'Credit card account', '#F44336'),
('INVESTMENT', 'Investment Account', 'Investment and trading account', '#9C27B0'),
('LOAN', 'Loan Account', 'Loan and debt account', '#795548');
```

### **accounts**

User financial accounts with balance tracking.

```sql
CREATE TABLE accounts (
    id               BIGSERIAL PRIMARY KEY,
    user_id          BIGINT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    account_type_id  INTEGER NOT NULL REFERENCES account_types(id),
    name             VARCHAR(255) NOT NULL,
    balance          DECIMAL(19,2) NOT NULL DEFAULT 0.00,
    initial_balance  DECIMAL(19,2) NOT NULL DEFAULT 0.00,
    is_custom        BOOLEAN NOT NULL DEFAULT false,
    description      TEXT,
    is_active        BOOLEAN NOT NULL DEFAULT true,

    -- Loan-specific fields
    loan_amount      DECIMAL(19,2),
    remaining_amount DECIMAL(19,2),
    interest_rate    DECIMAL(5,4),

    created_at       TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at       TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

    -- Constraints
    CONSTRAINT chk_balance_precision CHECK (balance >= -999999999999999.99 AND balance <= 999999999999999.99),
    CONSTRAINT chk_loan_amount_positive CHECK (loan_amount IS NULL OR loan_amount > 0),
    CONSTRAINT chk_interest_rate_valid CHECK (interest_rate IS NULL OR (interest_rate >= 0 AND interest_rate <= 100))
);

-- Indexes for performance
CREATE INDEX idx_accounts_user_id ON accounts(user_id);
CREATE INDEX idx_accounts_type ON accounts(account_type_id);
CREATE INDEX idx_accounts_active ON accounts(is_active);
CREATE INDEX idx_accounts_user_active ON accounts(user_id, is_active);
```

## 📊 **Transaction Management Schema**

### **transaction_types**

Enumeration of transaction types.

```sql
CREATE TABLE transaction_types (
    id          SERIAL PRIMARY KEY,
    code        VARCHAR(20) UNIQUE NOT NULL,
    name        VARCHAR(50) NOT NULL,
    description TEXT,

    CONSTRAINT chk_transaction_type_code CHECK (code IN ('INCOME', 'EXPENSE', 'TRANSFER'))
);

INSERT INTO transaction_types (code, name, description) VALUES
('INCOME', 'Income', 'Money received or earned'),
('EXPENSE', 'Expense', 'Money spent or paid out'),
('TRANSFER', 'Transfer', 'Money moved between accounts');
```

### **transactions**

Core transaction records with comprehensive tracking.

```sql
CREATE TABLE transactions (
    id                  BIGSERIAL PRIMARY KEY,
    user_id             BIGINT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    type                VARCHAR(20) NOT NULL CHECK (type IN ('INCOME', 'EXPENSE', 'TRANSFER')),

    -- Account references
    account_id          BIGINT REFERENCES accounts(id), -- For INCOME/EXPENSE
    from_account_id     BIGINT REFERENCES accounts(id), -- For TRANSFER
    to_account_id       BIGINT REFERENCES accounts(id), -- For TRANSFER
    category_id         BIGINT REFERENCES categories(id),

    -- Transaction details
    amount              DECIMAL(19,2) NOT NULL CHECK (amount > 0),
    fee                 DECIMAL(19,2) DEFAULT 0.00 CHECK (fee >= 0),
    description         VARCHAR(500) NOT NULL,
    notes               TEXT,

    -- Timing and location
    transaction_date    TIMESTAMP NOT NULL,
    location_latitude   DECIMAL(10,8),
    location_longitude  DECIMAL(11,8),

    -- Metadata
    tags                TEXT[], -- PostgreSQL array for flexible tagging
    attachments         JSONB,   -- File attachments metadata

    -- Synchronization
    client_id           VARCHAR(100), -- Client-generated ID for sync
    last_sync_at        TIMESTAMP,

    -- Audit fields
    created_at          TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at          TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

    -- Constraints for data integrity
    CONSTRAINT chk_transaction_accounts CHECK (
        (type = 'TRANSFER' AND from_account_id IS NOT NULL AND to_account_id IS NOT NULL AND account_id IS NULL) OR
        (type IN ('INCOME', 'EXPENSE') AND account_id IS NOT NULL AND from_account_id IS NULL AND to_account_id IS NULL)
    ),
    CONSTRAINT chk_transfer_different_accounts CHECK (
        type != 'TRANSFER' OR from_account_id != to_account_id
    ),
    CONSTRAINT chk_location_coordinates CHECK (
        (location_latitude IS NULL AND location_longitude IS NULL) OR
        (location_latitude IS NOT NULL AND location_longitude IS NOT NULL AND
         location_latitude >= -90 AND location_latitude <= 90 AND
         location_longitude >= -180 AND location_longitude <= 180)
    )
);

-- Comprehensive indexing for performance
CREATE INDEX idx_transactions_user_id ON transactions(user_id);
CREATE INDEX idx_transactions_type ON transactions(type);
CREATE INDEX idx_transactions_date ON transactions(transaction_date);
CREATE INDEX idx_transactions_account ON transactions(account_id);
CREATE INDEX idx_transactions_from_account ON transactions(from_account_id);
CREATE INDEX idx_transactions_to_account ON transactions(to_account_id);
CREATE INDEX idx_transactions_category ON transactions(category_id);
CREATE INDEX idx_transactions_user_date ON transactions(user_id, transaction_date);
CREATE INDEX idx_transactions_user_type ON transactions(user_id, type);
CREATE INDEX idx_transactions_sync ON transactions(last_sync_at);
CREATE INDEX idx_transactions_client_id ON transactions(client_id);

-- GIN index for tags array
CREATE INDEX idx_transactions_tags ON transactions USING GIN (tags);
```

## 🏷️ **Categorization Schema**

### **category_domains**

High-level category groupings.

```sql
CREATE TABLE category_domains (
    id          SERIAL PRIMARY KEY,
    code        VARCHAR(50) UNIQUE NOT NULL,
    name        VARCHAR(100) NOT NULL,
    description TEXT,
    color       VARCHAR(7), -- Hex color code

    CONSTRAINT chk_domain_color CHECK (color ~ '^#[0-9A-Fa-f]{6}$')
);

-- Insert category domains
INSERT INTO category_domains (code, name, description, color) VALUES
-- Income domains
('PRIMARY', 'Primary Income', 'Regular predictable income sources', '#4CAF50'),
('PASSIVE', 'Passive Income', 'Income from investments and assets', '#8BC34A'),
('IRREGULAR', 'Irregular Income', 'One-time or unpredictable income', '#CDDC39'),

-- Expense domains
('ESSENTIALS', 'Essential Expenses', 'Necessary living expenses', '#FF5722'),
('LIFESTYLE', 'Lifestyle Expenses', 'Discretionary spending', '#FF9800'),
('FINANCIAL', 'Financial Obligations', 'Financial commitments', '#F44336'),
('SOCIAL', 'Social Expenses', 'Social and charitable expenses', '#E91E63'),

-- Transfer domains
('ACCOUNT_MANAGEMENT', 'Account Management', 'Basic account operations', '#2196F3'),
('INVESTMENT', 'Investment Operations', 'Investment transactions', '#3F51B5'),
('ASSET_ACQUISITION', 'Asset Acquisition', 'Asset purchase transactions', '#9C27B0'),
('ASSET_DISPOSAL', 'Asset Disposal', 'Asset sale transactions', '#673AB7');
```

### **categories**

Detailed transaction categories.

```sql
CREATE TABLE categories (
    id              BIGSERIAL PRIMARY KEY,
    domain_id       INTEGER NOT NULL REFERENCES category_domains(id),
    code            VARCHAR(50) NOT NULL,
    name            VARCHAR(100) NOT NULL,
    description     TEXT,
    color           VARCHAR(7), -- Hex color code
    icon            VARCHAR(50),
    transaction_type VARCHAR(20) NOT NULL CHECK (transaction_type IN ('INCOME', 'EXPENSE', 'TRANSFER')),
    is_system       BOOLEAN NOT NULL DEFAULT true,
    is_active       BOOLEAN NOT NULL DEFAULT true,

    -- Hierarchical structure support
    parent_id       BIGINT REFERENCES categories(id),
    sort_order      INTEGER DEFAULT 0,

    created_at      TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at      TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT chk_category_color CHECK (color ~ '^#[0-9A-Fa-f]{6}$'),
    CONSTRAINT idx_categories_domain_code UNIQUE (domain_id, code)
);

-- Indexes
CREATE INDEX idx_categories_domain ON categories(domain_id);
CREATE INDEX idx_categories_type ON categories(transaction_type);
CREATE INDEX idx_categories_active ON categories(is_active);
CREATE INDEX idx_categories_parent ON categories(parent_id);
CREATE INDEX idx_categories_type_active ON categories(transaction_type, is_active);
```

## 🔄 **Synchronization Schema**

### **sync_logs**

Track data synchronization between clients and server.

```sql
CREATE TABLE sync_logs (
    id              BIGSERIAL PRIMARY KEY,
    user_id         BIGINT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    client_id       VARCHAR(100) NOT NULL,
    operation_type  VARCHAR(20) NOT NULL CHECK (operation_type IN ('PUSH', 'PULL', 'CONFLICT')),
    entity_type     VARCHAR(50) NOT NULL,
    entity_id       BIGINT,
    conflict_data   JSONB,
    status          VARCHAR(20) NOT NULL CHECK (status IN ('SUCCESS', 'FAILED', 'PENDING')),
    error_message   TEXT,
    created_at      TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT idx_sync_logs_user_client UNIQUE (user_id, client_id, created_at)
);

CREATE INDEX idx_sync_logs_user ON sync_logs(user_id);
CREATE INDEX idx_sync_logs_status ON sync_logs(status);
CREATE INDEX idx_sync_logs_operation ON sync_logs(operation_type);
CREATE INDEX idx_sync_logs_entity ON sync_logs(entity_type, entity_id);
```

## 📋 **Audit & System Schema**

### **audit_logs**

Comprehensive audit trail for system activities.

```sql
CREATE TABLE audit_logs (
    id              BIGSERIAL PRIMARY KEY,
    user_id         BIGINT REFERENCES users(id),
    action          VARCHAR(50) NOT NULL,
    entity_type     VARCHAR(50) NOT NULL,
    entity_id       BIGINT,
    old_values      JSONB,
    new_values      JSONB,
    ip_address      INET,
    user_agent      TEXT,
    created_at      TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_audit_logs_user ON audit_logs(user_id);
CREATE INDEX idx_audit_logs_action ON audit_logs(action);
CREATE INDEX idx_audit_logs_entity ON audit_logs(entity_type, entity_id);
CREATE INDEX idx_audit_logs_created ON audit_logs(created_at);
```

### **user_preferences**

User-specific application settings and preferences.

```sql
CREATE TABLE user_preferences (
    id              BIGSERIAL PRIMARY KEY,
    user_id         BIGINT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    preference_key  VARCHAR(100) NOT NULL,
    preference_value TEXT,
    data_type       VARCHAR(20) NOT NULL CHECK (data_type IN ('STRING', 'NUMBER', 'BOOLEAN', 'JSON')),
    created_at      TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at      TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT idx_user_preferences_unique UNIQUE (user_id, preference_key)
);

CREATE INDEX idx_user_preferences_user ON user_preferences(user_id);
```

## 🔧 **Database Functions & Triggers**

### **Balance Update Trigger**

Automatically update account balances on transaction changes.

```sql
-- Function to update account balance
CREATE OR REPLACE FUNCTION update_account_balance()
RETURNS TRIGGER AS $$
BEGIN
    -- Handle INSERT
    IF TG_OP = 'INSERT' THEN
        IF NEW.type = 'INCOME' THEN
            UPDATE accounts SET balance = balance + NEW.amount WHERE id = NEW.account_id;
        ELSIF NEW.type = 'EXPENSE' THEN
            UPDATE accounts SET balance = balance - NEW.amount WHERE id = NEW.account_id;
        ELSIF NEW.type = 'TRANSFER' THEN
            UPDATE accounts SET balance = balance - NEW.amount WHERE id = NEW.from_account_id;
            UPDATE accounts SET balance = balance + NEW.amount WHERE id = NEW.to_account_id;
        END IF;
        RETURN NEW;
    END IF;

    -- Handle UPDATE
    IF TG_OP = 'UPDATE' THEN
        -- Reverse old transaction
        IF OLD.type = 'INCOME' THEN
            UPDATE accounts SET balance = balance - OLD.amount WHERE id = OLD.account_id;
        ELSIF OLD.type = 'EXPENSE' THEN
            UPDATE accounts SET balance = balance + OLD.amount WHERE id = OLD.account_id;
        ELSIF OLD.type = 'TRANSFER' THEN
            UPDATE accounts SET balance = balance + OLD.amount WHERE id = OLD.from_account_id;
            UPDATE accounts SET balance = balance - OLD.amount WHERE id = OLD.to_account_id;
        END IF;

        -- Apply new transaction
        IF NEW.type = 'INCOME' THEN
            UPDATE accounts SET balance = balance + NEW.amount WHERE id = NEW.account_id;
        ELSIF NEW.type = 'EXPENSE' THEN
            UPDATE accounts SET balance = balance - NEW.amount WHERE id = NEW.account_id;
        ELSIF NEW.type = 'TRANSFER' THEN
            UPDATE accounts SET balance = balance - NEW.amount WHERE id = NEW.from_account_id;
            UPDATE accounts SET balance = balance + NEW.amount WHERE id = NEW.to_account_id;
        END IF;
        RETURN NEW;
    END IF;

    -- Handle DELETE
    IF TG_OP = 'DELETE' THEN
        IF OLD.type = 'INCOME' THEN
            UPDATE accounts SET balance = balance - OLD.amount WHERE id = OLD.account_id;
        ELSIF OLD.type = 'EXPENSE' THEN
            UPDATE accounts SET balance = balance + OLD.amount WHERE id = OLD.account_id;
        ELSIF OLD.type = 'TRANSFER' THEN
            UPDATE accounts SET balance = balance + OLD.amount WHERE id = OLD.from_account_id;
            UPDATE accounts SET balance = balance - OLD.amount WHERE id = OLD.to_account_id;
        END IF;
        RETURN OLD;
    END IF;

    RETURN NULL;
END;
$$ LANGUAGE plpgsql;

-- Create trigger
CREATE TRIGGER trigger_update_account_balance
    AFTER INSERT OR UPDATE OR DELETE ON transactions
    FOR EACH ROW
    EXECUTE FUNCTION update_account_balance();
```

### **Updated At Trigger**

Automatically update timestamp fields.

```sql
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = CURRENT_TIMESTAMP;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Apply to all tables with updated_at column
CREATE TRIGGER trigger_users_updated_at BEFORE UPDATE ON users FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
CREATE TRIGGER trigger_accounts_updated_at BEFORE UPDATE ON accounts FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
CREATE TRIGGER trigger_transactions_updated_at BEFORE UPDATE ON transactions FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
CREATE TRIGGER trigger_categories_updated_at BEFORE UPDATE ON categories FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
```

---

**📝 Note**: This database schema provides a comprehensive foundation for financial data management with performance optimization, data integrity, and scalability in mind.
