# Expenso - User Guide

## 📋 Table of Contents

1. [Getting Started](#-getting-started)
2. [Authentication & Account Security](#-authentication--account-security)
3. [Dashboard & Financial Overview](#-dashboard--financial-overview)
4. [Managing Transactions](#-managing-transactions)
5. [Budget Tracking & History](#-budget-tracking--history)
6. [Payment Reminder System](#-payment-reminder-system)
7. [Offline Financial Analytics](#-offline-financial-analytics)
8. [PDF Statement Generation](#-pdf-statement-generation)
9. [Data Backup & Restore](#-data-backup--restore)
10. [Query Runner & Saved Queries](#-query-runner--saved-queries)
11. [Settings, Profile & Storage Location](#️-settings-profile--storage-location)
12. [Troubleshooting & FAQs](#-troubleshooting--faqs)

---

## 🚀 Getting Started

### First Launch & Registration
Expenso is **100% offline** and operates without an internet connection. On your first launch:
1. **Create an Account**:
   - Enter your email address (used locally for profile identification).
   - Enter a secure password (hashed locally using PBKDF2-HMAC-SHA256 with 100,000 iterations).
   - Tap **Register**.
2. **Setup Account Recovery**:
   - Expenso generates a secret recovery phrase. Write this down in a secure place. If you forget your password, this phrase is the only way to reset your account offline.
3. **Configure Biometrics**:
   - Opt in to enable Fingerprint or Face Unlock via Android BiometricPrompt for instantaneous, secure login.

---

## 🔐 Authentication & Account Security

### Login Methods
- **Biometric Unlock**: Quick authentication using device hardware security.
- **Master Password**: Secure local credential verification using constant-time hash comparisons.
- **Automatic 24-Hour Timeout**: For your privacy, Expenso locks your active session after 24 hours of inactivity.
- **Offline Password Reset**: If you forget your password, tap "Forgot Password" on the login screen and verify your secret recovery phrase.

---

## 🏠 Dashboard & Financial Overview

The Dashboard (`DashboardActivity.java`) is your financial control center:

- **Net Financial Cards**:
  - **Net Balance**: Total income minus total expenditures.
  - **Total Income**: All incoming funds.
  - **Total Expenses**: All recorded spending.
- **Budget Health Overview**: Real-time progress bars indicating active category budgets and threshold alerts (e.g. 80% spending warning).
- **Upcoming Bill Reminders**: High-priority alert banner for payment obligations due soon.
- **Quick Action Bar**:
  - Add Transaction (+)
  - Budgets
  - Reminders
  - Analytics
  - Generate PDF Statement
- **Recent Transactions Ledger**: Displays the 5 most recent transactions with immediate tap-to-inspect details.

---

## 💰 Managing Transactions

Recorded transactions are stored locally inside the SQLCipher-encrypted database.

### Adding a Transaction
1. Tap the **+** floating action button or open **Transactions → Add Transaction**.
2. Select **Type**: `EXPENSE`, `INCOME`, or `TRANSFER`.
3. Enter the **Amount** and verify your localized currency (`₹`, `$`, `€`, `£`, etc.).
4. Choose a **Category** (e.g. Food, Transport, Rent, Salary).
5. (Optional) Provide description, payment method (Cash, Card, UPI), transaction date/time, and custom tags (e.g. `business,travel`).
6. Tap **Save**. The reactive `TransactionEventBus` immediately updates the dashboard and analytics.

### Filtering & Searching Transactions
- Open **Transactions** (`TransactionsActivity.java`).
- Tap the **Filter** icon to open the filter bottom sheet:
  - Filter by type (`Income`, `Expense`, `Transfer`).
  - Filter by category.
  - Filter by payment method.
  - Custom date range picker.
- Use the **Search Bar** to instantly find entries by keyword or merchant name.
- **Swipe Actions**: Swipe left or right on a transaction to edit or soft-delete it.

---

## 📊 Budget Tracking & History

### Setting Up a Budget
1. Navigate to **Budgets** (`BudgetsActivity.java`).
2. Tap **Create Budget**.
3. Select an expense category and enter your target spending limit.
4. Select the **Period**:
   - `WEEKLY`: Resets automatically every 7 days.
   - `MONTHLY`: Resets on the first day of each calendar month.
   - `YEARLY`: Resets annually.
5. Set an **Alert Threshold** (e.g. 80%) to receive in-app notifications before exceeding your budget.

### Automated Budget Reset Worker
Expenso utilizes AndroidX `WorkManager` (`BudgetResetWorker.java`) running in the background every 24 hours:
- When a period expires, the worker calculates total expenditure for that window.
- The completed period is archived permanently into `budget_history`.
- The budget's date window rolls forward to the next cycle and spending counters reset.
- Tap **View Budget History** on any budget card to inspect past monthly performance.

### Drag-and-Drop Ordering
- Long-press any budget card in `BudgetsActivity` and drag vertically to customize your display order.

---

## ⏰ Payment Reminder System

Never miss a bill or recurring subscription with Expenso's built-in payment reminder engine (`RemindersActivity.java`).

### Scheduling a Payment Reminder
1. Navigate to **Reminders** from the Dashboard or navigation drawer.
2. Tap **Add Reminder**.
3. Enter Title (e.g. "Electricity Bill", "Netflix Subscription"), Amount, and Category.
4. Set the **Due Date** and **Due Time**.
5. Select **Recurrence Interval**:
   - `NONE` (One-time bill)
   - `DAILY`, `WEEKLY`, `BIWEEKLY`, `MONTHLY`, `QUARTERLY`, or `YEARLY`.
6. Enable **Notifications** and toggle **Auto Create Transaction** if you want the app to automatically log an expense when paid.
7. Tap **Save Reminder**.

### Exact Alarms & Interactive Notifications
- Expenso registers exact hardware alarms via `AlarmManager.setExactAndAllowWhileIdle()` so alerts fire even when your phone is in deep sleep (Doze mode).
- When the notification arrives, tap actions directly in your notification tray:
  - **Mark as Paid**: Instantly logs the expense transaction, advances recurring bills to their next due date via `RecurrenceEngine`, and records a completion log in `reminder_history`.
  - **Snooze**: Postpones the alarm by your configured snooze window.

---

## 📈 Offline Financial Analytics

Open **Analytics** (`AnalyticsActivity.java`) for deep financial visualization:

1. **Cash Flow Summary**:
   - Net balance, total income, total spending, and calculated **Savings Rate (%)**.
2. **Category Distribution (Pie Chart)**:
   - Interactive MPAndroidChart pie chart displaying your expenditure share per category.
3. **Spending Trends (Bar Chart)**:
   - Compare monthly expenditures across time.
4. **Month-over-Month (MoM) & Year-over-Year (YoY) Variances**:
   - Automatic percentage calculations showing whether your spending in a category increased or decreased compared to the previous period.

---

## 📄 PDF Statement Generation

Generate clean, formal financial statements completely offline using the built-in iText7 engine (`StatementActivity.java`):

1. Open **Statement** from the Dashboard or Settings.
2. Select your desired **Statement Period**:
   - Last 7 Days, This Month, Last Month, This Year, or Custom Date Range.
3. Tap **Generate Statement**.
4. The engine executes intelligent space budgeting to compile:
   - Executive header and financial overview card.
   - Categorized spending breakdown table.
   - Comprehensive itemized transaction ledger.
5. Once generated, tap **Share PDF** or **Open PDF** to view, print, or attach your statement via Android `FileProvider`.

---

## 🔄 Data Backup & Restore

Protect your financial records using Expenso's multi-table backup engine (`DataBackupActivity.java`).

### Creating a Backup
1. Go to **Settings → Data Backup** (or open via All Activities).
2. Tap **Backup Now**.
3. Expenso generates a compressed `.zip` archive containing:
   - `manifest.json` with version metadata.
   - 8 individual CSV files (`transactions.csv`, `budgets.csv`, `payment_reminders.csv`, `configurations.csv`, etc.).
4. The backup is saved to your configured export location (default: `Documents/Expenso/Backups/`).

### Restoring a Backup
1. On the **Data Backup** screen, tap **Restore Backup**.
2. Select your `.zip` (or legacy `.csv`) backup file.
3. Choose your **Restoration Mode**:
   - **Merge Mode (Recommended)**: Imports backup records while preserving existing transactions, using `UserIdRemapper` to prevent ID collisions.
   - **Replace Mode**: Wipes current tables and restores a clean copy from the backup.
4. Tap **Confirm Restore**.

---

## 💻 Query Runner & Saved Queries

For advanced users and developers, Expenso includes a read-only SQL console (`SecureQueryRunnerActivity.java`).

### Running SQL Queries
1. Open **Secure Query Runner**.
2. Type a custom `SELECT` statement against the `transactions` table (e.g. `SELECT description, amount FROM transactions WHERE type = 'EXPENSE'`).
3. Tap **Run Query**.
4. Results appear in a scrollable, formatted table.

### Security Sandbox Rules
- Strictly read-only: `INSERT`, `UPDATE`, `DELETE`, and `DROP` are rejected.
- Multi-account isolation: Queries are automatically scoped to your active `user_id`.
- Output is capped at 500 rows to safeguard device memory.

### Saved Queries
- Tap the **Saved Queries** icon to browse pre-built query templates (Top Expenses, Spending by Category, Payment Method Breakdown).
- Tap **Save Query** to name and store your custom queries for quick future access.
- Tap **Export CSV** to save query outputs to local storage.

---

## ⚙️ Settings, Profile & Storage Location

### Theme & Currency
- **Theme Selection**: Choose between Light Mode, Dark Mode, or System Default.
- **Preferred Currency**: Set your primary currency and symbol (`₹`, `$`, `€`, `£`, `¥`, etc.).

### Custom Storage Location (SAF)
- Open **Settings → File Storage Location** (`FileLocationSettingsActivity.java`).
- Tap **Choose Custom Directory** to grant persistent Android Storage Access Framework (SAF) permissions to an external SD card or specific folder.
- All exported backups, CSVs, and PDF statements will automatically save to this directory.

### Profile & Account Deletion
- Open **Settings → Profile**.
- Update profile details or change your master password.
- **Delete Account & Wipe Data**: Permanently erases all local database records, cryptographic keys, and cached sessions.

---

## ❓ Troubleshooting & FAQs

### Q: Why does Expenso not require Internet permission?
**A:** Expenso is engineered specifically for privacy. All processing (SQL database, analytics, PDF rendering, alarms) executes locally. Your financial data is physically incapable of leaving your device.

### Q: What happens if I forget my password?
**A:** Because Expenso is 100% offline, there is no remote "forgot password email". You must use the secret recovery phrase generated during setup to reset your password.

### Q: My payment reminder notifications are not ringing. What should I check?
**A:** On Android 12+ (API 31+), ensure that **Exact Alarms** are allowed under Android **Settings → Apps → Expenso → Alarms & Reminders**. Also verify that **Battery Optimization** is set to "Unrestricted" or "Optimized" rather than "Restricted".

### Q: Can I transfer my data to a new phone?
**A:** Yes! Create a backup in **Data Backup**, transfer the resulting `Expenso_Backup_*.zip` file to your new device via USB or local transfer, and tap **Restore Backup** on your new phone.

---

**Expenso** - Secure, Modern, Offline-First Personal Finance.
