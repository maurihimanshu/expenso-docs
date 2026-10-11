# Expenso - Architecture Guide

## 📋 Table of Contents

1. [System Architecture Overview](#️-system-architecture-overview)
2. [Architecture Principles](#-architecture-principles)
3. [Clean Layer Responsibilities](#-clean-layer-responsibilities)
4. [Data Flow Architecture & Event Bus](#-data-flow-architecture--event-bus)
5. [Component & Activity Architecture](#️-component--activity-architecture)
6. [Database Architecture & Schema (v14)](#️-database-architecture--schema-v14)
7. [Database Migration History (v1 to v14)](#-database-migration-history-v1-to-v14)
8. [Security & Cryptography Architecture](#-security--cryptography-architecture)
9. [Application Lifecycle & Two-Phase Startup](#-application-lifecycle--two-phase-startup)
10. [Background Scheduling & Workers](#-background-scheduling--workers)
11. [Package Structure](#-package-structure)
12. [Performance & Android 15/16 Compatibility](#-performance--android-1516-compatibility)
13. [Related Documentation](#-related-documentation)

---

## 🏗️ System Architecture Overview

Expenso implements **Clean Architecture** combined with **Model-View-ViewModel (MVVM)**, an asynchronous two-phase initialization lifecycle, and an in-memory reactive event bus (`TransactionEventBus`). It operates strictly **100% offline** with zero remote servers, cloud sync, or external analytics SDKs.

```mermaid
flowchart TD
    subgraph Principles["🎯 Architectural Principles"]
        P1["100% Offline Autonomy"]
        P2["Dual-Tier Cryptography"]
        P3["Layer Separation & Inversion"]
        P4["Reactive Mutation Propagation"]
    end
    
    subgraph Presentation_Layer["📱 Presentation Layer"]
        A["15 Android Activities"]
        B["10 AndroidX ViewModels"]
        C["Dialogs & BottomSheets"]
        D["RecyclerView Adapters"]
    end
    
    subgraph Domain_Layer["🔧 Domain Layer"]
        E["Pure Domain Entities"]
        F["Domain Use Cases"]
        G["Repository Interfaces"]
        H["Domain Services & Engines"]
    end
    
    subgraph Data_Layer["💾 Data Layer"]
        I["Secure Repositories"]
        J["SQLCipher 4.17.0 DB expenso_secure.db"]
        K["11 Room DAOs"]
        L["Multi-Table Backup & Restore Engine"]
        M["CentralizedFileLocationManager SAF"]
    end
    
    P1 --> P2
    P2 --> P3
    P3 --> P4
    
    A --> B
    B --> F
    F --> G
    G --> I
    I --> J
    J --> K
    I --> L
    I --> M
```

---

## 📐 Architecture Principles

1. **Strict Offline Independence**: No `INTERNET` permission in `AndroidManifest.xml`. All data processing, storage, analytics, and PDF generation execute locally on device.
2. **Dual-Tier Hardware-Backed Security**: SQLCipher 4.17.0 encrypts SQLite database pages on disk. Sensitive user profile credentials and security audit records use AES-256-GCM backed by Android Hardware Keystore (`AndroidKeyStore`).
3. **Decoupled Key Management**: `DatabaseKeyManager` maintains the SQLCipher passphrase in an isolated storage file (`expenso_db_key_prefs`) backed by its own dedicated MasterKey (`expenso_db_master_key`), preventing user preference resets from corrupting database accessibility.
4. **Reactive Mutation Propagation**: When repositories insert, update, or delete data, they notify `TransactionEventBus`. ViewModels observe this bus and update their `LiveData` reactively without continuous disk polling.
5. **Deterministic Two-Phase Startup**: Cold starts are divided into a fast synchronous path on the main thread and a heavy asynchronous path on a background thread pool, synchronized via `CountDownLatch` and `InitializationListener`.

---

## 🎯 Clean Layer Responsibilities

### 1. Presentation Layer (`com.offline.expenso.presentation`)
- **Activities & Dialogs**: Render UI, handle user touch events, and observe ViewModels.
- **ViewModels**: Extend `BaseViewModel`, encapsulate UI state within private `MutableLiveData`, and expose read-only `LiveData`.
- **Adapters**: Bind datasets to Material Design 3 `RecyclerView` components using `DiffUtil` for smooth 60 FPS transitions.
- **Theming**: `ThemeManager` applies Light, Dark, or System themes before layout inflation.

### 2. Domain Layer (`com.offline.expenso.domain`)
- **Entities**: Clean business models (`Budget`, `BudgetHistory`, `DomainTransaction`, `Money`, `User`, `Category`, `Configuration`, `PasswordResetToken`, `RecoveryPhrase`, `StatementData`).
- **Use Cases**: Encapsulate single business actions (`ComputeBudgetSpendingUseCase`, `StatementGeneratorUseCase`).
- **Domain Services**: Business rules including `RecurrenceEngine` (due date arithmetic), `AuthenticationService`, `PasswordResetService`, `RecoveryPhraseService`, and `ConfigurationValidationService`.
- **Repository Contracts**: Pure interfaces defining data requirements without framework dependencies (`TransactionRepository`, `BudgetRepository`, `UserRepository`).

### 3. Data Layer (`com.offline.expenso.data`)
- **Repositories**: Coordinate between Room DAOs, local preferences, and cryptography (`TransactionSecureRepository`, `BudgetRepository`, `UserSecureRepository`, `PaymentReminderRepository`, etc.).
- **Persistence (`AppDatabase`)**: Room database encrypted by SQLCipher 4.17.0. Current version: 14.
- **Backup & Restore**: Multi-table CSV export and import into compressed `.zip` archives with foreign-key collision resolution (`UserIdRemapper`).
- **File Management**: `CentralizedFileLocationManager` handles Storage Access Framework (SAF) tree URIs with fallback to app-scoped document directories.

---

## 🔄 Data Flow Architecture & Event Bus

Mutations follow a clean unidirectional data flow:

```mermaid
sequenceDiagram
    participant View as Activity / Fragment
    participant VM as ViewModel
    participant UC as Domain Use Case
    participant Repo as Secure Repository
    participant Bus as TransactionEventBus
    participant DB as AppDatabase (SQLCipher)

    View->>VM: User initiates action (e.g. Save Expense)
    VM->>UC: Execute business logic
    UC->>Repo: insertTransaction(DomainTransaction)
    Repo->>DB: Room DAO insert (Encrypted at rest)
    DB-->>Repo: Insert successful
    Repo->>Bus: postEvent(TRANSACTION_CREATED)
    Bus-->>VM: Notify active observer ViewModels
    VM-->>View: LiveData emits updated state
    View-->>View: UI re-renders with new entry
```

---

## 🏛️ Component & Activity Architecture

Expenso employs an activity-based navigation architecture where [`DashboardActivity`](file:///d:/Projects/expenso/expenso-android/app/src/main/java/com/offline/expenso/presentation/ui/activities/DashboardActivity.java) acts as the launcher and primary host:

```mermaid
flowchart TB
    subgraph Launcher_Host["🚀 Launcher & Host"]
        Dash["DashboardActivity<br/>Session Verification & Financial Overview"]
    end

    subgraph Core_Activities["💳 Core Financial Activities"]
        Txn["TransactionsActivity"]
        TxnDet["TransactionDetailsActivity"]
        Bud["BudgetsActivity"]
        Rem["RemindersActivity"]
        Ana["AnalyticsActivity"]
        Stmt["StatementActivity"]
    end

    subgraph Management_Activities["⚙️ Management & Utility Activities"]
        Auth["AuthenticationActivity"]
        Prof["ProfileActivity"]
        Conf["ConfigurationActivity"]
        Back["DataBackupActivity"]
        Set["SettingsActivity"]
        FileLoc["FileLocationSettingsActivity"]
        Query["SecureQueryRunnerActivity"]
        AllAct["AllActivitiesActivity"]
    end

    Dash --> Txn
    Txn --> TxnDet
    Dash --> Bud
    Dash --> Rem
    Dash --> Ana
    Dash --> Stmt
    Dash --> Back
    Dash --> Set
    Dash --> Query
    Dash --> AllAct
    
    Set --> Prof
    Set --> Conf
    Set --> FileLoc
    Dash -.->|Unauthenticated| Auth
```

---

## 🗄️ Database Architecture & Schema (v14)

- **Database Engine**: Room 2.6.1 + SQLCipher 4.17.0 (AES-256-CBC)
- **Database File**: `expenso_secure.db`
- **Schema Location**: `app/schemas/com.offline.expenso.data.local.database.AppDatabase/`

### Complete Entity-Relationship Model

```mermaid
erDiagram
    users ||--o{ transactions : "owns"
    users ||--o{ budgets : "sets"
    users ||--o{ payment_reminders : "schedules"
    users ||--o{ saved_queries : "saves"
    budgets ||--o{ budget_history : "archives"
    payment_reminders ||--o{ reminder_history : "logs"

    users {
        string id PK
        string email UK
        string fullNameEncrypted
        string passwordHashEncrypted
        string phoneNumberEncrypted
        long dateOfBirth
        string profilePictureUrlEncrypted
        boolean isEmailVerified
        boolean isPhoneVerified
        boolean isBiometricEnabled
        string preferredCurrency
        string currency_symbol
        string preferredLanguage
        long createdAt
        long updatedAt
        long lastLoginAt
        boolean isActive
        string recoveryPhraseHashEncrypted
        string recoveryPhraseEncrypted
    }

    transactions {
        string id PK
        string user_id FK
        string amount
        string currency_code
        string exchange_rate
        string type
        string category_id
        string subcategory
        string description
        string notes
        long transaction_date
        long transaction_time
        string account_name
        string payment_method
        string reference_number
        string location
        string tags
        boolean is_recurring
        string recurring_pattern
        string receipt_image_path
        boolean is_tax_deductible
        long created_at
        long updated_at
        boolean is_deleted
        int version
    }

    budgets {
        string id PK
        string user_id
        string name
        string description
        string category_id
        string category_name
        string budget_amount
        string spent_amount
        string period
        string start_date
        string end_date
        string status
        boolean is_active
        boolean is_recurring
        long created_at
        long updated_at
    }

    budget_history {
        string id PK
        string budget_id
        string user_id
        string budget_name
        string category_id
        string category_name
        string period
        string period_start_date
        string period_end_date
        string budgeted_amount
        string spent_amount
        string remaining_amount
        double spent_percentage
        string final_status
        long created_at
    }

    payment_reminders {
        string id PK
        string user_id FK
        string title
        string category
        string amount
        string currency_code
        long due_date
        string due_time
        string repeat_type
        int repeat_interval
        string repeat_unit
        long next_due_date
        long notification_time
        boolean notification_enabled
        int notification_id
        string status
        boolean is_active
        long snooze_until
        string notes
        string payment_method
        boolean auto_create_transaction
        long created_at
        long updated_at
        long last_notified_at
        int completion_count
    }

    reminder_history {
        string id PK
        string user_id FK
        string reminder_id FK
        string transaction_id FK
        string title
        string category
        string amount
        string currency_code
        string payment_method
        long original_due_date
        long paid_date
        string status
        int days_overdue
        string notes
        string remarks
        string actual_amount_paid
        long created_at
        boolean was_recurring
        int occurrence_number
    }

    configurations {
        string id PK
        string display_name
        string configuration_type
        int position
        long created_at
        long updated_at
        string created_by
    }

    saved_queries {
        string id PK
        string user_id FK
        string title
        string sql_query
        boolean is_predefined
        long created_at
    }
```

---

## 📜 Database Migration History (v1 to v14)

| Migration | Versions | Core Changes |
| :--- | :--- | :--- |
| `MIGRATION_5_6` | 5 → 6 | Added `recoveryPhraseHashEncrypted` and `recoveryPhraseEncrypted` to `users`. |
| `MIGRATION_6_7` | 6 → 7 | Created `budgets` table with recurring spending attributes. |
| **`MIGRATION_7_8`** | **7 → 8** | **Major Architecture Refactor**: Prior to version 8, transactions used individual column AES encryption (`amount_encrypted`, `description_encrypted`, `notes_encrypted`, etc.). This prevented SQLite index lookups, sorting, filtering, and aggregate queries. Because SQLCipher already transparently encrypts the entire SQLite database file at rest with page-level AES-256, column encryption on transactions was redundant and harmful. Migration 7→8 safely decrypted all records and recreated `transactions` with plain typed columns. |
| `MIGRATION_8_9` | 8 → 9 | Added `is_recurring` to `budgets` and created `budget_history` for archived period snapshots. |
| `MIGRATION_9_10` | 9 → 10 | Added composite indices to `configurations` table for faster type lookup. |
| `MIGRATION_10_11` | 10 → 11 | Created `payment_reminders` and `reminder_history` tables for bill alerts. |
| `MIGRATION_11_12` | 11 → 12 | Simplified `configurations` schema (reduced redundant columns), mapped `currency_symbol` directly into `users` (`₹`, `$`, `€`, `£`, `¥`, etc.). |
| `MIGRATION_12_13` | 12 → 13 | Added `position INTEGER NOT NULL DEFAULT 0` to `configurations` for drag-and-drop category ordering. |
| `MIGRATION_13_14` | 13 → 14 | Created `saved_queries` table for Query Runner persistence. |

---

## 🔐 Security & Cryptography Architecture

```mermaid
flowchart TD
    subgraph Keystore_Layer["Hardware Keystore Layer"]
        K_APP["expenso_master_key<br/>(AndroidKeyStore)"]
        K_DB["expenso_db_master_key<br/>(AndroidKeyStore)"]
    end

    subgraph Key_Storage["Key Storage Layer"]
        PREF_APP["EncryptedSharedPreferences<br/>app_prefs.xml"]
        PREF_DB["DatabaseKeyManager Isolated Prefs<br/>expenso_db_key_prefs.xml"]
    end

    subgraph Operational_Crypto["Operational Cryptography"]
        AES["AESEncryptionManager<br/>AES-256-GCM 128-bit tag"]
        PASS["PBKDF2WithHmacSHA256<br/>100,000 iterations, 32-byte salt"]
        SQLC["SQLCipher 4.17.0 Engine<br/>AES-256-CBC per page"]
    end

    K_APP --> PREF_APP
    K_DB --> PREF_DB
    PREF_APP --> AES
    PREF_APP --> PASS
    PREF_DB -->|Passphrase| SQLC
```

### Key Security Safeguards:
1. **Isolated Passphrase Management (`DatabaseKeyManager.java`)**: Ensures database availability is decoupled from user session state.
2. **Timing Attack Protection**: Password verification uses `MessageDigest.isEqual()` for constant-time byte comparisons.
3. **SafeQueryRunner AST Sandbox**: Direct SQL execution blocks destructive keywords (`DROP`, `DELETE`, `ALTER`, `INSERT`, `UPDATE`), injects mandatory `user_id` filtering, and caps results at 500 rows.

---

## ⏱️ Application Lifecycle & Two-Phase Startup

To prevent cold-start UI freezes while initializing heavy cryptographic providers:

```mermaid
sequenceDiagram
    participant OS as Android OS
    participant App as ExpensoApplication
    participant Main as Main Thread
    participant BG as Background Executor
    participant UI as DashboardActivity

    OS->>App: onCreate()
    App->>Main: Phase 1: Initialize SessionManager & ThemeManager
    Main-->>App: UI theme applied immediately
    App->>BG: Phase 2: Asynchronous heavy setup
    App->>UI: Launch DashboardActivity
    UI->>App: awaitInitialization(15, SECONDS)
    BG->>BG: Setup Keystore & AESEncryptionManager
    BG->>BG: Open SQLCipher AppDatabase v14
    BG->>BG: Initialize Repositories & Schedulers
    BG->>App: initializationLatch.countDown()
    App-->>UI: Latch released / listener triggered
    UI->>UI: Populate financial cards & live data
```

---

## ⏰ Background Scheduling & Workers

1. **Daily Budget Resets (`BudgetResetWorker.java`)**:
   - Scheduled via AndroidX `WorkManager` daily periodic request.
   - Evaluates active recurring budgets against current time.
   - Moves spent amounts to `budget_history` and rolls dates forward.
2. **Exact Payment Reminders (`ReminderAlarmScheduler.java`)**:
   - Uses `AlarmManager.setExactAndAllowWhileIdle()` on Android 6.0+.
   - Checks `AlarmManager.canScheduleExactAlarms()` on Android 12+ (API 31+).
   - Direct notification actions handled by `ReminderActionReceiver`.

---

## 📦 Package Structure

```
com.offline.expenso/
├── ExpensoApplication.java                 # Lifecycle & init orchestrator
├── core/                                   # Platform tools
│   ├── logging/                            # AppLogger
│   ├── security/                           # BiometricManager
│   ├── threading/                          # AppExecutorService
│   └── updates/                            # AppUpdateManager
├── data/                                   # Data Layer
│   ├── analytics/                          # OfflineAnalyticsEngine
│   ├── backup/                             # Multi-table backup/restore
│   ├── budget/                             # BudgetResetWorker
│   ├── events/                             # TransactionEventBus
│   ├── file/                               # CentralizedFileLocationManager
│   ├── local/dao/                          # Room DAOs
│   ├── local/database/                     # AppDatabase, SafeQueryRunner
│   ├── local/preferences/                  # SecurePreferencesManager
│   ├── mappers/                            # Domain ↔ Entity mappers
│   ├── reminder/                           # RecurrenceEngine & Alarms
│   ├── repositories/                       # Secure repositories
│   ├── security/                           # Cryptography managers
│   └── statement/                          # PdfStatementGenerator
├── domain/                                 # Domain Layer
│   ├── entities/                           # Domain models
│   ├── enums/                              # Business enums
│   ├── repositories/                       # Repository interfaces
│   ├── services/                           # Domain logic services
│   ├── usecases/                           # Clean use cases
│   └── value_objects/                      # Money
└── presentation/                           # Presentation Layer
    ├── common/                             # Base classes & shimmer
    ├── theming/                            # ThemeManager
    └── ui/                                 # Activities, ViewModels, Adapters
```

---

## ⚡ Performance & Android 15/16 Compatibility

1. **16KB Memory Page Size**: NDK packaging configured for 16KB ELF alignment across all target ABIs (`armeabi-v7a`, `arm64-v8a`, `x86`, `x86_64`) for Android 15+ kernels.
2. **In-Memory Reactive Caching**: Repositories cache computed balances, eliminating redundant SQL transactions on repeated view visits.
3. **85% Jacoco Line Coverage**: CI pipeline enforces coverage threshold on unit tests (`./gradlew testDebugUnitTest jacocoTestCoverageVerification`).

---

## 📚 Related Documentation

- **[README.md](../README.md)** - Project overview and quick start.
- **[SECURITY.md](SECURITY.md)** - Cryptographic implementation details.
- **[USER_GUIDE.md](USER_GUIDE.md)** - Complete user manual.
- **[QUERY_RUNNER.md](QUERY_RUNNER.md)** - Safe Query Runner guide and schema.
- **[FEATURES.md](FEATURES.md)** - Full feature specifications.