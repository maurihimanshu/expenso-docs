# Expenso - Comprehensive Features Documentation

## 📋 Table of Contents

1. [Authentication & Security](#authentication--security)
2. [Transaction Management](#transaction-management)
3. [Budget Tracking](#budget-tracking)
4. [Financial Analytics](#financial-analytics)
5. [Dashboard & Overview](#dashboard--overview)
6. [Settings & Configuration](#settings--configuration)
7. [Data Export & Backup](#data-export--backup)
8. [User Interface](#user-interface)

---

## 🔐 Authentication & Security

### User Registration
- **Email-based Registration**: Secure user account creation with email validation
- **Password Requirements**: Strong password enforcement with complexity validation
- **Password Hashing**: PBKDF2WithHmacSHA256 algorithm with Argon2 for additional security
- **Email Encryption**: User email addresses encrypted using AES-256-GCM before storage
- **Unique Passbook ID**: Each user gets a unique identifier for data isolation

### Authentication Methods

#### Biometric Authentication
- **Fingerprint Recognition**: Uses Android BiometricPrompt API
- **Face ID Support**: Facial recognition authentication (when available)
- **Hardware-backed Security**: Leverages Android Keystore for secure biometric storage
- **Fallback to PIN**: Automatic fallback if biometric fails
- **Secure Session Creation**: Session tokens generated after successful authentication

#### PIN Authentication
- **6-Digit PIN**: Enhanced security with 6-digit personal identification number
- **Pattern Detection**: Detects weak patterns (sequential, repeated digits)
- **Brute Force Protection**: Progressive delays after failed attempts
- **Attempt Limiting**: Automatic lockout after maximum failed attempts
- **Secure Storage**: PIN hash stored encrypted, never plain text

### Session Management
- **Encrypted Session Storage**: Session data stored in EncryptedSharedPreferences
- **Auto-logout**: Configurable session timeout for security
- **Session Token Generation**: UUID-based globally unique session tokens
- **Device-specific Sessions**: Each device maintains independent session
- **Secure Session Recovery**: Automatic recovery from cryptographic corruption

### Security Features
- **Multi-layer Encryption**: Field-level + database-level encryption
- **Android Keystore Integration**: Hardware-backed key storage when available
- **Root Detection**: Prevents execution on compromised devices
- **Debug Detection**: Blocks debugging attempts in production
- **Audit Logging**: Complete security event tracking

---

## 💰 Transaction Management

### Transaction Types
- **Income Transactions**: Track money received (salary, freelance, gifts, etc.)
- **Expense Transactions**: Track money spent (bills, groceries, entertainment, etc.)
- **Transaction Status**: Active, deleted (soft delete support)

### Transaction Properties
- **Amount**: Money value object with currency support (stored as plain text in encrypted database)
- **Description**: User-defined transaction description (stored as plain text in encrypted database)
- **Category**: Categorization for better organization
- **Payment Method**: Track payment method (Cash, Card, UPI, etc.)
- **Date & Time**: Precise transaction timestamp
- **User Association**: Transactions linked to specific user passbook
- **Storage Note**: Transaction fields stored as plain text; database file encrypted by SQLCipher

### Transaction Operations

#### Creating Transactions
- **Add Transaction Dialog**: Material Design dialog with form validation
- **Currency Selection**: Multi-currency support with dynamic currency conversion
- **Category Selection**: Choose from predefined or custom categories
- **Date Picker**: Intuitive date selection with calendar interface
- **Amount Validation**: Real-time amount format validation
- **Duplicate Detection**: Optional duplicate transaction detection

#### Editing Transactions
- **Edit Transaction Dialog**: Pre-filled form with existing transaction data
- **Update Support**: Modify transaction properties after creation
- **Validation**: Same validation rules as creation
- **History Preservation**: Maintain transaction modification history

#### Deleting Transactions
- **Soft Delete**: Transactions marked as deleted, not permanently removed
- **Confirmation Dialog**: User confirmation before deletion
- **Reversible**: Ability to restore deleted transactions (future feature)
- **Cascade Updates**: Budget calculations automatically update after deletion

### Transaction Filtering & Search

#### Filter Options
- **Type Filter**: Filter by Income or Expense
- **Category Filter**: Filter by transaction category
- **Payment Method Filter**: Filter by payment method
- **Date Range Filter**: Filter transactions within specific date range
- **Search Query**: Text search across descriptions and categories

#### Sorting Options
- **By Date**: Chronological (newest/oldest first)
- **By Amount**: Highest/lowest amount first
- **By Category**: Alphabetical category sorting

#### Pagination
- **Efficient Loading**: Paginated transaction lists (20 items per page)
- **Infinite Scroll**: Load more transactions as user scrolls
- **Performance Optimized**: Reduces memory usage for large datasets

### Transaction Display
- **Recent Transactions**: Quick access to latest transactions on Dashboard
- **Transaction List**: Full transaction history in TransactionsActivity
- **Transaction Details**: Detailed view with all transaction properties
- **Color Coding**: Visual distinction between income (green) and expense (red)
- **Currency Formatting**: Proper currency symbol and decimal formatting

---

## 📊 Budget Tracking

### Budget Creation
- **Budget Name**: Custom budget name for identification
- **Budget Description**: Optional description for budget purpose
- **Budget Amount**: Target spending amount with currency support
- **Category Association**: Link budget to specific transaction category
- **Period Selection**: Choose budget period (Monthly, Weekly, Yearly)
- **Start & End Dates**: Define budget active period

### Budget Types
- **Category Budgets**: Spending limits for specific categories
- **Period-based Budgets**: Recurring budgets with automatic period management
- **Custom Period Budgets**: User-defined date range budgets

### Budget Status Tracking

#### Budget States
- **On Track**: Spending within normal range (< 80% of budget)
- **Near Limit**: Approaching budget limit (≥ 80% but ≤ 100%)
- **Over Budget**: Exceeded budget amount (> 100%)
- **Expired**: Budget period has ended
- **Paused**: Budget temporarily disabled
- **Draft**: Budget created but not yet active

#### Real-time Calculations
- **Spent Amount Calculation**: Automatic calculation from transactions
- **Remaining Budget**: Live calculation of remaining budget amount
- **Percentage Spent**: Visual percentage indicator
- **Days Remaining**: Countdown to budget period end

### Budget Monitoring
- **Dashboard Integration**: Budget overview cards on main dashboard
- **Budget List View**: All budgets displayed with status indicators
- **Budget Details**: Detailed view with spending breakdown
- **Visual Indicators**: Color-coded status (green/orange/red)
- **Spending Alerts**: Notifications when approaching budget limit

### Budget Operations
- **Create Budget**: Material dialog for budget creation
- **Edit Budget**: Modify budget properties
- **Delete Budget**: Remove budget (with confirmation)
- **Pause/Resume**: Temporarily disable/enable budgets
- **Budget History**: Track budget performance over time

### Budget Analytics
- **Category Performance**: Compare budgeted vs actual spending per category
- **Period Comparison**: Compare spending across different periods
- **Trend Analysis**: Visual trends for budget adherence

---

## 📈 Financial Analytics

### Financial Summary
Comprehensive financial overview for selected time periods (Week, Month, Year):

#### Overview Metrics
- **Total Income**: Sum of all income transactions
- **Total Expenses**: Sum of all expense transactions
- **Net Balance**: Income minus expenses
- **Transaction Count**: Total number of transactions
- **Average Transaction**: Average transaction amount
- **Largest Income**: Highest single income transaction
- **Largest Expense**: Highest single expense transaction

#### Category Analysis
- **Category Breakdown**: Spending by category with percentages
- **Top Categories**: Categories with highest spending
- **Category Comparison**: Visual comparison across categories
- **Income Sources**: Analysis of income by source/category

### Spending Trends
Time-based spending analysis with customizable periods:

#### Time Period Selection
- **Week View**: Daily breakdown for past 7 days
  - Daily spending trends
  - Day-by-day comparison
  - Transaction counts per day
- **Month View**: Monthly summary (YYYY-MM format)
  - Total spending for selected month
  - Income vs expense comparison
  - Transaction count for period
- **Year View**: Monthly trends for past 12 months
  - Monthly spending trends
  - Monthly income trends
  - Comparative analysis across months

#### Trend Visualization
- **Line Charts**: Spending trends over time
- **Bar Charts**: Category-wise spending comparison
- **Pie Charts**: Category distribution visualization
- **Interactive Charts**: Tap to view detailed data points

### Category Analysis
Deep dive into spending patterns by category:

#### Category Metrics
- **Spending per Category**: Total amount spent in each category
- **Percentage Distribution**: Category spending as percentage of total
- **Transaction Count**: Number of transactions per category
- **Average per Transaction**: Average transaction amount per category
- **Top Spending Categories**: Ranked list of highest spending categories

#### Category Insights
- **Category Trends**: How spending in each category changes over time
- **Comparison Analysis**: Compare spending across different time periods
- **Category Alerts**: Identify categories with unusual spending patterns

### Analytics Features
- **Time Period Filtering**: Filter analytics data by Week/Month/Year
- **Currency Support**: Analytics displayed in user's selected currency
- **Date Range Selection**: Custom date ranges for flexible analysis
- **Real-time Updates**: Analytics update automatically when transactions change
- **Offline Computation**: All analytics calculated locally, no internet required

---

## 🏠 Dashboard & Overview

### Dashboard Components

#### Financial Overview Cards
- **Current Balance**: Net balance (Income - Expenses)
- **Total Income**: Sum of all income transactions
- **Total Expenses**: Sum of all expense transactions
- **Currency Display**: All amounts shown in user's selected currency

#### Recent Transactions
- **Quick Access**: View last 5-10 recent transactions
- **Transaction Preview**: Amount, description, category, and date
- **Quick Actions**: Tap to view transaction details
- **View All Link**: Navigate to full transaction list

#### Budget Overview
- **Active Budgets**: Display of active budgets with status
- **Budget Status Indicators**: Visual status (On Track, Near Limit, Over Budget)
- **Quick Budget Access**: Tap to navigate to budget management
- **Spending Progress**: Visual progress indicators for each budget

#### Quick Actions
- **Add Transaction FAB**: Floating action button for quick transaction entry
- **Activity Navigation Cards**: Quick access to main features:
  - Transactions Activity
  - Budgets Activity
  - Analytics Activity
  - Settings Activity
  - Profile Activity
  - Configuration Activity

### Dashboard Features
- **Welcome Message**: Personalized greeting with user name
- **Real-time Updates**: Live data updates as transactions change
- **Pull to Refresh**: Manual refresh capability
- **Responsive Layout**: Adaptive layout for different screen sizes
- **Theme Support**: Dashboard adapts to light/dark theme

### Navigation
- **Bottom Navigation**: Quick access to main activities
- **Activity Cards**: Visual cards for feature navigation
- **Deep Linking**: Support for URL-based navigation (future feature)
- **Back Stack Management**: Proper activity back stack handling

---

## ⚙️ Settings & Configuration

### User Settings

#### Profile Management
- **User Profile**: View and edit user information
- **Email Management**: Update email address (with re-encryption)
- **Password Change**: Secure password update with verification
- **Account Deletion**: Option to delete user account and all data

#### Security Settings
- **Biometric Setup**: Enable/disable biometric authentication
- **PIN Configuration**: Change PIN with security validation
- **Session Timeout**: Configure automatic logout timer
- **Security Audit**: View security event logs

#### App Preferences
- **Currency Selection**: Choose default currency for transactions
- **Theme Selection**: Light mode, Dark mode, or System default
- **Notification Preferences**: Configure app notifications and reminders
- **Language Selection**: App language preference (future feature)

### System Configuration

#### Configuration Activity
- **Configuration Management**: Centralized system configuration interface
- **Category Management**: View and manage transaction categories
- **Payment Method Configuration**: Manage available payment methods
- **Default Settings**: Configure default categories and payment methods
- **Configuration Display**: Grid/list view of all system configurations
- **Configuration Details**: View detailed information for each configuration item

#### Category Management
- **Default Categories**: Pre-configured transaction categories
- **Custom Categories**: Create user-defined categories
- **Category Icons**: Visual category identification
- **Category Organization**: Organize categories by type
- **Category Display**: View all categories in organized layout

#### Payment Method Configuration
- **Payment Methods**: Manage available payment methods
- **Default Method**: Set default payment method for quick entry
- **Custom Methods**: Add custom payment methods
- **Payment Method Display**: View all payment methods in organized layout

#### Configuration Management
- **Configuration Seeding**: Automatic setup of default configurations
- **Configuration Validation**: Ensure configuration integrity
- **System Preferences**: App-wide preference management
- **Configuration Adapter**: Efficient RecyclerView adapter for configuration display

### Data Management

#### Backup & Restore
- **Manual Backup**: Create backup of all user data
- **Automatic Backups**: Scheduled backup creation (future feature)
- **Backup Encryption**: Backups encrypted for security
- **Restore from Backup**: Restore data from backup file

#### Data Export
- **PDF Export**: Generate PDF financial reports
- **CSV Export**: Export transaction data for external analysis
- **JSON Export**: Complete data export in JSON format
- **Export Filters**: Export data for specific date ranges

#### Data Cleanup
- **Delete All Data**: Remove all transactions and budgets
- **Clear Cache**: Clear app cache and temporary files
- **Database Maintenance**: Optimize database performance

---

## 💾 Data Export & Backup

### Manual Backup & Restore

#### Manual Backup Operations
- **Backup Now**: One-click manual backup creation
- **CSV Format**: Backups stored as CSV files for easy access
- **Transaction Data**: Complete transaction history included
- **Metadata**: Backup includes timestamp, type (manual/auto), and file size
- **File Naming**: Automatic naming with format `manual_YYYY-MM-DD_HHmmss.csv` or `auto_YYYY-MM-DD_HHmmss.csv`
- **Backup Location**: Default storage in `Documents/Expenso/Backups` folder (user-accessible)
- **Custom Location**: Option to select custom backup folder via Storage Access Framework (SAF)
- **Location Reset**: Ability to reset backup location to default

#### Backup File Management
- **Recent Backups Display**: View 10 most recent backups in UI
- **All Backups Preserved**: All backup files preserved on disk (no automatic deletion)
- **Backup Information**: Display backup filename, date, size, and type (Manual/Auto)
- **Backup Restore**: Select and restore from any available backup file
- **Backup Deletion**: Delete individual backup files with confirmation
- **Storage Permissions**: Automatic permission handling for Android 6+ devices
- **SAF Integration**: Support for user-selected directories via Storage Access Framework

#### Restore Operations
- **Backup Selection Dialog**: Material Design dialog with RecyclerView for backup selection
- **Backup Validation**: Automatic validation of backup file format and integrity
- **CSV Import**: Parse and import transaction data from CSV backups
- **Error Handling**: Comprehensive error messages for failed restore operations
- **Transaction Import**: Restore transactions with validation and error reporting
- **Empty State**: Clear messaging when no backups are available

### Automatic Backup

#### Scheduled Backups
- **WorkManager Integration**: Background task scheduling for automatic backups
- **Frequency Options**: 
  - None (disabled)
  - Daily (every 24 hours)
  - Weekly (every 7 days)
  - Monthly (every 30 days)
- **Flexible Scheduling**: Uses `PeriodicWorkRequest` with `flexInterval` to prevent immediate execution
- **Last Backup Tracking**: Remembers last backup time to prevent duplicate backups
- **Minimum Interval Enforcement**: Skips backup if called too soon after last backup
- **Persistent Preferences**: Backup frequency and schedule state saved in SharedPreferences
- **Schedule Restoration**: WorkManager schedule restored on app restart if auto-backup enabled

#### Automatic Backup Features
- **Background Execution**: Runs in background without user interaction
- **User Context**: Accesses user session and transaction repository automatically
- **Status Updates**: UI displays last backup time and next scheduled backup
- **Error Recovery**: Automatic retry on transient failures
- **Battery Optimized**: Uses WorkManager for battery-efficient scheduling

### Export Formats

#### CSV Export
- **Transaction Data**: Export all transactions to CSV format
- **Complete Fields**: Includes all transaction fields (amount, description, category, date, etc.)
- **CSV Escaping**: Proper handling of special characters (commas, quotes, newlines)
- **Excel Compatible**: Format compatible with Microsoft Excel and Google Sheets
- **Header Row**: Includes column headers for easy identification

#### CSV Import
- **Format Validation**: Validates CSV structure and headers
- **Data Type Validation**: Ensures correct data types (amounts, dates, etc.)
- **Error Reporting**: Reports specific errors for invalid rows
- **Transaction Creation**: Imports transactions with full field mapping
- **Import Result**: Returns imported transactions and error list

### Backup Storage

#### Storage Locations
- **Default Location**: `Documents/Expenso/Backups` (user-accessible)
- **Fallback**: App-specific external storage if Documents folder unavailable
- **Custom Location**: User-selected folder via Storage Access Framework
- **SAF Persistence**: Persists custom backup URI with persistable permissions
- **Storage Access**: Automatic permission handling and validation

#### File Operations
- **File-based Storage**: Traditional File I/O for default location
- **DocumentFile Storage**: SAF-based storage for custom locations
- **Unified Interface**: Single interface handles both storage types
- **File Metadata**: File size, modification date, and type tracking
- **Backup Verification**: Checks backup file existence before operations

### Export Features
- **Date Range Selection**: Export data for specific time periods (future feature)
- **Category Filtering**: Export specific categories only (future feature)
- **Transaction Type Filtering**: Export income or expenses only (future feature)
- **Share Capability**: Share backup files via other apps (future feature)

### Security Considerations
- **Local Storage Only**: Backups stored locally on device
- **Database Encryption**: Source database encrypted by SQLCipher
- **Permission Protection**: Requires storage permissions for backup operations
- **User Control**: User has full control over backup location and deletion

---

## 🎨 User Interface

### Material Design 3
- **Modern Components**: Latest Material Design components
- **Material Cards**: Card-based layout for content organization
- **Material Dialogs**: Consistent dialog styling
- **Material Buttons**: Filled, outlined, and text button variants
- **Material Text Fields**: Enhanced text input with floating labels

### Theming

#### Theme Modes
- **Light Mode**: Light color scheme for daytime use
- **Dark Mode**: Dark color scheme for low-light environments
- **System Default**: Automatically follow system theme
- **Theme Persistence**: User preference saved across app restarts

#### Adaptive Colors
- **Dynamic Color**: Integration with system color palette (Android 12+)
- **Material You**: Support for Material You dynamic theming
- **Color Customization**: User-customizable accent colors (future feature)
- **High Contrast**: Accessibility support for high contrast mode

### Navigation

#### Activity-Based Navigation
- **Intent-Based Routing**: Standard Android Intent navigation
- **Deep Linking**: Support for deep link navigation
- **Parent Activity**: Proper activity hierarchy for back navigation
- **Navigation State**: Maintain navigation state across configuration changes

#### Bottom Navigation
- **Quick Access**: Bottom navigation bar for main activities
- **Icon Indicators**: Visual indicators for active activity
- **Badge Support**: Notification badges on navigation items

### User Experience

#### Responsive Design
- **Screen Size Adaptation**: Optimized for phones and tablets
- **Orientation Support**: Portrait and landscape orientations
- **Multi-Window**: Support for Android multi-window mode

#### Accessibility
- **WCAG 2.1 AA Compliance**: Accessibility standards compliance
- **Screen Reader Support**: Full TalkBack support
- **High Contrast Mode**: Enhanced visibility options
- **Text Scaling**: Support for system text scaling
- **Accessibility Labels**: Proper content descriptions for UI elements

#### Performance
- **Smooth Animations**: 60 FPS animations and transitions
- **Efficient Rendering**: Optimized RecyclerView with DiffUtil
- **Lazy Loading**: Efficient data loading with pagination
- **Memory Management**: Lifecycle-aware components prevent leaks

### UI Components

#### Dialogs
- **Add Transaction Dialog**: Comprehensive transaction entry form
- **Edit Transaction Dialog**: Transaction modification dialog
- **Create Budget Dialog**: Budget creation form
- **Confirmation Dialogs**: User confirmation for destructive actions
- **Currency Selection Dialog**: Enhanced currency picker

#### Lists & Adapters
- **Transaction Adapter**: Efficient RecyclerView adapter for transactions
- **Budget Adapter**: Budget list display adapter
- **Category Adapter**: Category selection adapter
- **Spending Trend Adapter**: Analytics trend display adapter
- **Category Analysis Adapter**: Category breakdown display

#### Charts & Visualizations
- **Line Charts**: Spending trends over time
- **Bar Charts**: Category-wise comparison
- **Pie Charts**: Category distribution
- **Interactive Charts**: Tap to view detailed data

---

## 🔄 Application Lifecycle

### Initialization Flow

#### Critical Phase (Synchronous)
- **SessionManager**: Initialize session management
- **ThemeManager**: Apply user theme preferences
- **Main Thread**: Remains responsive during critical init

#### Heavy Phase (Asynchronous)
- **EncryptionManager**: Setup Android Keystore access
- **Database**: Initialize SQLCipher encrypted database
- **Repositories**: Instantiate all repository implementations
- **Services**: Configure domain services
- **Background Thread**: Runs on dedicated executor service

### Initialization Safety
- **Initialization Listener Pattern**: Activities wait for initialization
- **Timeout Protection**: 15-second timeout for initialization
- **Error Recovery**: Automatic recovery from initialization failures
- **Thread Safety**: AtomicBoolean and CountDownLatch for synchronization

### Memory Management
- **Lifecycle Awareness**: Proper component lifecycle handling
- **Resource Cleanup**: onLowMemory() and onTrimMemory() support
- **Executor Shutdown**: Graceful executor service shutdown
- **Database Closing**: Proper database connection closure

---

## 🚀 Future Features

### Planned Enhancements
1. **Cloud Sync**: Optional cloud backup and sync (encrypted)
2. **Recurring Transactions**: Automatic transaction creation
3. **Bill Reminders**: Payment due date reminders
4. **Receipt Scanning**: OCR-based receipt import
5. **Multi-Account Support**: Multiple financial accounts
6. **Investment Tracking**: Investment portfolio management
7. **Goal Setting**: Financial goal tracking and progress
8. **Collaborative Budgets**: Shared budgets with family members
9. **Expense Splitting**: Split expenses among multiple users
10. **Advanced Reports**: Custom report builder

---

## 📱 Platform Support

### Android Version Support
- **Minimum SDK**: API 24 (Android 7.0 Nougat)
- **Target SDK**: API 35 (Android 15)
- **Recommended**: API 26+ (Android 8.0) for optimal experience

### Device Requirements
- **RAM**: Minimum 2GB (4GB recommended)
- **Storage**: 50MB for app, additional space for data
- **Permissions**: 
  - Biometric authentication (optional)
  - Storage (for backups and exports)

### Compatibility
- **Screen Sizes**: Phones and tablets
- **Orientations**: Portrait and landscape
- **Architectures**: ARMv7, ARM64, x86, x86_64

---

## 📊 Performance Metrics

### App Performance
- **App Launch**: < 2 seconds (after initial setup)
- **Database Queries**: < 100ms average
- **UI Animations**: 60 FPS target
- **Memory Usage**: < 100MB peak
- **Battery Impact**: Minimal background processing

### Data Handling
- **Transaction Loading**: Efficient pagination (20 items per page)
- **Analytics Calculation**: Background computation (< 500ms)
- **Database Operations**: Optimized with proper indexing
- **Encryption Overhead**: < 50ms per transaction encryption

---

## 🔧 Technical Details

### Architecture Patterns
- **Clean Architecture**: Separation of concerns with clear layer boundaries
- **MVVM**: Model-View-ViewModel for reactive UI updates
- **Repository Pattern**: Data source abstraction
- **Dependency Inversion**: Interfaces define contracts

### Data Flow
- **User Action** → **Activity/Fragment** → **ViewModel** → **Repository** → **Database**
- **Database** → **Repository** → **ViewModel** → **LiveData** → **UI Update**

### Threading
- **Main Thread**: UI operations only
- **Background Threads**: Database and computation operations
- **AppExecutorService**: Centralized thread pool management
- **Async Operations**: CompletableFuture for asynchronous operations

### Security Implementation
- **Encryption**: AES-256-GCM for field-level encryption
- **Database**: SQLCipher for full database encryption
- **Key Storage**: Android Keystore for secure key management
- **Session**: EncryptedSharedPreferences for session data

---

**Expenso** - Comprehensive, Secure, Offline-First Personal Finance Management

For detailed architecture information, see [ARCHITECTURE.md](./ARCHITECTURE.md)  
For security implementation details, see [SECURITY.md](./SECURITY.md)

