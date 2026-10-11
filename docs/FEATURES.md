# Expenso - Comprehensive Features Documentation

## 📋 Table of Contents

1. [Authentication & Security](#-authentication--security)
2. [Transaction Management](#-transaction-management)
3. [Budget Tracking & History](#-budget-tracking--history)
4. [Payment Reminder System](#-payment-reminder-system)
5. [Offline Financial Analytics](#-offline-financial-analytics)
6. [PDF Statement Generation](#-pdf-statement-generation)
7. [Safe Query Runner & Saved Queries](#-safe-query-runner--saved-queries)
8. [Data Backup & Restore Engine](#-data-backup--restore-engine)
9. [Dashboard & Quick Actions](#-dashboard--quick-actions)
10. [Settings & File Location Management](#️-settings--file-location-management)
11. [Platform Support & 16KB Page Alignment](#-platform-support--16kb-page-alignment)
12. [Related Documentation](#-related-documentation)

---

## 🔐 Authentication & Security

### User Registration & Login
- **Email-Based Registration**: Local account creation with strict email validation.
- **PBKDF2 Password Hashing**: Utilizes `PBKDF2WithHmacSHA256` with:
  - **Iterations**: `100,000`
  - **Salt**: 32 cryptographically random bytes via `SecureRandom`
  - **Verification**: Constant-time `MessageDigest.isEqual` to eliminate timing attacks.
- **BIP-39 Style Recovery Phrase**: Generates a secure recovery phrase stored encrypted via `RecoveryPhraseService` for local password reset without remote services.

### Biometric Authentication
- **AndroidX BiometricPrompt**: Native fingerprint and facial recognition.
- **Hardware Keystore Backed**: Encryption keys unlocked via biometric authentication without storing raw credentials in memory.
- **PIN Fallback**: Configurable secure personal identification fallback.

### Session Lifecycle Management
- **Hardware-Backed Session Store**: Active session tokens stored inside `EncryptedSharedPreferences`.
- **Inactivity Timeout**: Enforces automatic logout after **24 hours** of user inactivity.
- **Cryptographic Corruption Recovery**: Deep exception traversal catches `AEADBadTagException` or Keystore invalidation, safely re-initializing storage without crashing.

### Dual-Tier Encryption
- **SQLCipher 4.17.0 (AES-256-CBC)**: Full disk encryption across all SQLite database pages, WAL journals, and indices.
- **AES-256-GCM**: Column-level encryption with 128-bit authentication tags for personal identification and security audit records.
- **Isolated Key Management**: `DatabaseKeyManager` runs on independent `EncryptedSharedPreferences` (`expenso_db_key_prefs`), preventing database lockout during user setting resets.

---

## 💰 Transaction Management

### Transaction Attributes
- **Amount & Currencies**: Stored with multi-currency support and localized formatting (`₹`, `$`, `€`, `£`, `¥`, etc.).
- **Transaction Types**: `INCOME`, `EXPENSE`, `TRANSFER`.
- **Classification**: Category, subcategory, and customizable tags.
- **Metadata**: Payment method, account name, reference/tracking number, location, and optional notes.
- **Receipt Images**: Local file path mapping to receipt attachments.
- **Soft Deletion**: `is_deleted` flag preserves audit trail and enables undo capabilities.

### Reactive Mutation Pipeline
- **`TransactionEventBus`**: Background repository writes emit `TRANSACTION_CREATED`, `TRANSACTION_UPDATED`, or `TRANSACTION_DELETED` events.
- **Zero Polling**: ViewModels receive instant notifications to refresh live data and cash flow calculations without disk polling.

### Search & Filtering
- Filter by transaction type (Income / Expense / Transfer).
- Category filtering with custom category support.
- Date range picker for arbitrary reporting windows.
- Text search across descriptions, notes, and merchant locations.
- Paginated loading via `RecyclerView` with `DiffUtil`.

---

## 📊 Budget Tracking & History

### Budget Features
- **Flexible Recurrence**: Create budgets for `WEEKLY`, `MONTHLY`, or `YEARLY` periods.
- **Category Scoping**: Assign spending limits to individual categories or overall spending.
- **Custom Ordering**: Drag-and-drop budget reordering backed by database `position` column.
- **Threshold Alerts**: Configurable spending percentage alert triggers (e.g. notify when spending reaches 80% of budget limit).

### Automated Budget Reset Engine
- **`BudgetResetWorker`**: Periodic background worker scheduled via AndroidX `WorkManager` (daily execution).
- **Historical Snapshotting**: When a budget period expires, the worker:
  1. Computes total final expenditure in `transactions`.
  2. Archives a permanent snapshot record into `budget_history` (`budget_amount`, `spent_amount`, `spent_percentage`, `final_status`).
  3. Rolls `start_date` and `end_date` forward to the next cycle.
  4. Resets active spent counters.

---

## ⏰ Payment Reminder System

### Scheduled Bill Obligations
- Track recurring utility bills, subscriptions, loans, and credit card payments.
- Attributes: Title, amount, category, due date, due time, recurrence interval, and payment method.
- Supported Recurrence: `NONE`, `DAILY`, `WEEKLY`, `BIWEEKLY`, `MONTHLY`, `QUARTERLY`, `YEARLY`.

### Exact Alarms via `AlarmManager`
- Scheduled using `AlarmManager.setExactAndAllowWhileIdle()` to guarantee delivery during Android Doze mode.
- Runtime permission verification (`SCHEDULE_EXACT_ALARM` / `USE_EXACT_ALARM`) on Android 12+ (API 31+).

### Interactive Notification Actions
- **Mark as Paid**:
  - Automatically records an expense transaction in `transactions`.
  - Increments completion count.
  - Automatically calculates subsequent due date via `RecurrenceEngine`.
  - Logs action record into `reminder_history`.
- **Snooze**: Postpones notification without altering base recurrence schedule.

---

## 📈 Offline Financial Analytics

### Mathematical Metrics (`OfflineAnalyticsEngine.java`)
- **Net Cash Flow**: Total Income - Total Expenses.
- **Savings Rate**: $\frac{\text{Net Balance}}{\text{Income}} \times 100\%$.
- **Category Breakdown**: Percentage distribution of expenses per category.
- **Temporal Variance**: Month-over-Month (MoM) and Year-over-Year (YoY) percentage changes.

### Visualizations
- Interactive MPAndroidChart pie charts for category expenditures.
- Stacked bar charts for monthly income vs expense trends.
- Real-time chart entry formatting executed entirely in memory.

---

## 📄 PDF Statement Generation

### Built-in iText7 Engine (`PdfStatementGenerator.java`)
- Generate formal multi-page PDF statements entirely on-device without third-party web services.
- **Intelligent Space Budgeting**: Calculates remaining vertical height on current page, dynamically preventing awkward splits across table headers and cards.

### Document Sections:
1. **Branded Header**: User details, statement period, generated timestamp.
2. **Financial Overview Card**: Income, Expenses, Net Balance, Savings Rate.
3. **Category Breakdown Table**: Category name, transaction count, expenditure total, percentage.
4. **Itemized Transaction Ledger**: Date, Category, Description, Payment Method, Amount.
5. **Secure Sharing**: Integrated with Android `FileProvider` (`com.offline.expenso.fileprovider`) for private PDF sharing via email, drive, or print services.

---

## 💻 Safe Query Runner & Saved Queries

### Power-User SQL Sandbox (`SecureQueryRunnerActivity.java`)
- Direct read-only SQL query execution over encrypted SQLite tables.
- **AST Security Guardrails**:
  - Rejects dangerous SQL commands (`INSERT`, `UPDATE`, `DELETE`, `DROP`, `ALTER`, `CREATE`, `REPLACE`, `TRUNCATE`, `ATTACH`).
  - Automatic injection of `WHERE user_id = ?` ensures complete multi-tenant user isolation.
  - Caps query results at a hard maximum of **500 rows** to protect device memory.
  - Memory-safe autocloseable cursor wrappers prevent database handle leaks.
- **Saved Queries Feature**:
  - Save frequently run SQL queries with custom titles into the `saved_queries` database table.
  - Includes predefined templates (e.g. Top 10 Expenses, Spending by Category, Monthly Totals).
- **Export Query Results**: Export custom SQL query outputs directly to CSV via Storage Access Framework.

---

## 🔄 Data Backup & Restore Engine

### Multi-Table ZIP Packaging
- Bundles complete database state into a single `.zip` archive containing:
  - `manifest.json`: App version, DB schema version, timestamp, user ID, included table list.
  - `transactions.csv`
  - `budgets.csv`
  - `budget_history.csv`
  - `payment_reminders.csv`
  - `reminder_history.csv`
  - `configurations.csv`
  - `categories.csv`
  - `app_settings.csv`
  - `security_audit.csv`

### Restoration Modes
- **Replace Mode**: Safely purges existing user records and populates directly from backup files.
- **Merge Mode**: Retains existing data and resolves foreign key primary key collisions using [`UserIdRemapper.java`](file:///d:/Projects/expenso/expenso-android/app/src/main/java/com/offline/expenso/data/backup/UserIdRemapper.java).

---

## 🏠 Dashboard & Quick Actions

- **Financial Health Cards**: Real-time balance, income, and expense summary.
- **Shimmer State Loading**: Smooth Material shimmer placeholders while database initializes.
- **Quick Action Row**: Single-tap shortcuts to Add Transaction, Budgets, Reminders, Analytics, and PDF Statements.
- **Recent Activity Ledger**: Immediate view of the latest 5-10 transactions.
- **In-App Update Notifications**: Play Core `AppUpdateManager` integration for seamless app maintenance.

---

## ⚙️ Settings & File Location Management

- **Dynamic Theming**: Light Mode, Dark Mode, or Follow System.
- **Primary Currency Selection**: Support for all major international currencies with automatic symbol resolution.
- **Storage Access Framework (SAF)**:
  - `FileLocationSettingsActivity` lets users designate custom export directories on internal or external storage.
  - Grants persistent folder access via `takePersistableUriPermission`.
  - Fallback to app-scoped documents folder (`context.getExternalFilesDir(Environment.DIRECTORY_DOCUMENTS)`).

---

## 📱 Platform Support & 16KB Page Alignment

- **Android 15+ Compatibility**: Native NDK binaries compiled with 16KB page alignment (`armeabi-v7a`, `arm64-v8a`, `x86`, `x86_64`) preventing ELF alignment crashes on Android 15 devices.
- **Target SDK**: `compileSdk = 36`, `targetSdk = 36`, `minSdk = 24` (Android 7.0+).
- **Zero-Network Guarantee**: Strictly no internet permission requested in `AndroidManifest.xml`.

---

## 📚 Related Documentation

- **[README.md](../README.md)** - Project overview and build runbook.
- **[ARCHITECTURE.md](ARCHITECTURE.md)** - System design and Room v14 schema.
- **[SECURITY.md](SECURITY.md)** - Cryptographic specifications.
- **[USER_GUIDE.md](USER_GUIDE.md)** - User manual.
- **[QUERY_RUNNER.md](QUERY_RUNNER.md)** - SQL Query Runner reference.
