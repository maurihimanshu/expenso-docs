# Expenso - User Guide

## 📋 Table of Contents

1. [Getting Started](#-getting-started)
2. [Authentication & Security](#-authentication--security)
3. [Dashboard](#-dashboard)
4. [Managing Transactions](#-managing-transactions)
5. [Budget Tracking](#-budget-tracking)
6. [Financial Analytics](#-financial-analytics)
7. [Settings & Profile](#️-settings--profile)
8. [Data Backup & Restore](#-data-backup--restore)
9. [Configuration](#-configuration)
10. [Advanced Features](#-advanced-features)
11. [Tips & Best Practices](#-tips--best-practices)
12. [Troubleshooting](#-troubleshooting)
13. [Quick Reference](#-quick-reference)
14. [Additional Resources](#-additional-resources)

---

## 🚀 Getting Started

### First Launch

When you first open Expenso, you'll need to create an account:

1. **Create Account**
   - Enter your email address
   - Choose a strong password (8+ characters recommended)
   - Confirm your password
   - Tap "Register" to create your account

2. **Set Up Authentication**
   - Choose your preferred authentication method:
     - **Biometric**: Use fingerprint or face recognition (recommended)
     - **PIN**: Enter a 6-digit PIN as fallback
   - Follow the on-screen instructions to complete setup

3. **Welcome to Dashboard**
   - After successful registration, you'll land on the Dashboard
   - This is your main screen showing financial overview

### App Overview

Expenso is a **100% offline** personal finance management app. This means:
- ✅ All your data is stored locally on your device
- ✅ No internet connection required
- ✅ Your financial information never leaves your phone
- ✅ Complete privacy and security

---

## 🔐 Authentication & Security

### Login Methods

#### Biometric Authentication (Recommended)
- **Fingerprint**: Use your device's fingerprint sensor
- **Face Recognition**: Use facial recognition if available
- **Fast & Secure**: Quick access with enterprise-grade security
- **Automatic Fallback**: If biometric fails, you can use your PIN

#### PIN Authentication
- **6-Digit PIN**: Create a secure 6-digit personal identification number
- **Pattern Detection**: App detects weak patterns (like 123456 or repeated digits)
- **Security Tips**:
  - Avoid sequential numbers (123456)
  - Avoid repeated digits (111111)
  - Don't use easily guessable PINs (birthday, phone number)
  - Use a unique PIN not used elsewhere

### Security Features

**Automatic Logout**
- Session timeout protects your data if you leave the app
- Re-authenticate when returning after timeout period

**Data Encryption**
- All your financial data is encrypted at multiple levels
- Database is encrypted using industry-standard SQLCipher
- Your data remains secure even if your device is lost or stolen

**Privacy Protection**
- Root detection prevents execution on compromised devices
- No data transmission - everything stays on your device
- No tracking or analytics that share your information

---

## 🏠 Dashboard

The Dashboard is your financial command center. It provides a quick overview of your financial health.

### Dashboard Components

#### Financial Overview Cards
- **Current Balance**: Your net balance (Income - Expenses)
- **Total Income**: Sum of all your income transactions
- **Total Expenses**: Sum of all your expense transactions
- All amounts are displayed in your selected currency

#### Recent Transactions
- Shows your last 5-10 transactions
- Quick preview with amount, description, category, and date
- Tap any transaction to view full details
- "View All" button takes you to complete transaction list

#### Budget Overview
- Displays your active budgets with status indicators
- Color-coded status:
  - 🟢 Green: On Track (spending is healthy)
  - 🟠 Orange: Near Limit (approaching budget limit)
  - 🔴 Red: Over Budget (exceeded budget)
- Tap a budget to see details or manage it

#### Quick Actions
- **Floating Action Button (+)** : Quick access to add a new transaction
- **Activity Cards**: Navigate to:
  - Transactions
  - Budgets
  - Analytics
  - Settings
  - Profile
  - All Activities

### Using the Dashboard

**Pull to Refresh**: Swipe down on the dashboard to refresh all data and see latest updates.

**Navigation**: 
- Use bottom navigation bar to quickly switch between main sections
- Tap activity cards to open specific features
- Use the back button or swipe gestures to navigate back

---

## 💰 Managing Transactions

Transactions are the core of Expenso. Every financial activity (income, expense, transfer) is recorded as a transaction.

### Adding a Transaction

1. **Open Transactions**
   - From Dashboard, tap "Transactions" or use the floating action button (+)

2. **Fill Transaction Details**
   - **Type**: Select Income, Expense, or Transfer
   - **Amount**: Enter the transaction amount
   - **Currency**: Choose currency (supports multiple currencies)
   - **Description**: Brief description of the transaction
   - **Category**: Select or create a category
   - **Payment Method**: How you paid (Cash, Credit Card, UPI, Bank Transfer, etc.)
   - **Date & Time**: Transaction date and time
   - **Optional Fields**:
     - Notes: Additional information
     - Location: Where the transaction occurred
     - Tags: Add tags for better organization
     - Account Name: Link to specific account
     - Reference Number: Transaction reference/tracking number
     - Tax Deductible: Mark if this is a tax-deductible expense
     - Recurring: Mark if this is a recurring transaction

3. **Save Transaction**
   - Tap "Save" to add the transaction
   - Transaction appears immediately in your transaction list

### Transaction Types

**Income**
- Money you receive (salary, freelance, gifts, refunds, etc.)
- Displayed in green color
- Adds to your total balance

**Expense**
- Money you spend (bills, groceries, entertainment, etc.)
- Displayed in red color
- Subtracts from your total balance

**Transfer**
- Money moved between accounts or categories
- Neutral transaction type
- Useful for tracking internal transfers

### Viewing Transactions

**Transaction List**
- Scrollable list of all your transactions
- Shows: Amount, Description, Category, Date
- Tap any transaction to view full details

**Filtering Transactions**
- **By Type**: Show only Income, Expense, or Transfer
- **By Category**: Filter by specific category
- **By Payment Method**: Filter by payment method
- **By Date Range**: Select start and end dates
- **Search**: Type keywords to search descriptions and categories

**Sorting Transactions**
- **By Date**: Newest or oldest first
- **By Amount**: Highest or lowest amount first
- **By Category**: Alphabetical category order

### Editing Transactions

1. Open the transaction from the list
2. Tap "Edit" or the edit icon
3. Modify any fields as needed
4. Tap "Save" to update

### Deleting Transactions

1. Open the transaction details
2. Tap "Delete" or the delete icon
3. Confirm deletion in the dialog
4. Transaction is marked as deleted (soft delete - can be recovered if needed)

**Note**: Deleted transactions are hidden from normal views but can be restored. Permanent deletion may be available in future updates.

---

## 📊 Budget Tracking

Budgets help you control your spending by setting limits and tracking your progress.

### Creating a Budget

1. **Open Budgets**
   - From Dashboard, tap "Budgets"
   - Tap the "+" button or "Create Budget"

2. **Set Budget Details**
   - **Budget Name**: Give your budget a name (e.g., "Groceries", "Entertainment")
   - **Budget Amount**: Set your spending limit
   - **Category**: Link to a specific transaction category (optional)
   - **Period**: Choose Monthly, Weekly, or Yearly
   - **Start Date**: When the budget period begins
   - **End Date**: When the budget period ends

3. **Save Budget**
   - Tap "Create" to save your budget
   - Budget appears in your budgets list
   - Tracking begins immediately

### Budget Types

**Recurring Budgets** (Weekly/Monthly/Yearly):
- Automatically reset when the period ends
- Current period saved to history
- Spending resets to $0 for new period
- Budget continues indefinitely
- View past performance in budget history

**Custom Budgets** (One-Time):
- Fixed start and end dates
- Expire when period ends
- No automatic reset
- Good for special projects or one-time goals

### Budget Status

Budgets automatically track your spending and show status:

- **🟢 On Track**: Spending is under 80% of budget - you're doing well!
- **🟠 Near Limit**: Spending is 80-100% of budget - watch your spending
- **🔴 Over Budget**: You've exceeded the budget limit
- **📅 Expired**: Budget period has ended (custom budgets only)

### Budget Features

**Real-Time Tracking**
- Budgets update automatically as you add transactions
- See spending progress in real-time
- View percentage spent and remaining amount

**Budget Details View**
- See all transactions contributing to the budget
- View spending breakdown by date
- Monitor progress toward budget limit
- Days remaining until period ends

**Multiple Budgets**
- Create unlimited budgets for different categories
- Each budget tracks independently
- View all budgets on the budgets screen

### Managing Budgets

**Search Budgets**
- Use the search bar at the top to find budgets quickly
- Search filters by budget name, description, or category
- Results update in real-time as you type

**Filter by Status**
- Use filter chips to show budgets by status:
  - **All**: Show all budgets (default)
  - **On Track**: Only budgets under 80% spent
  - **Near Limit**: Budgets at 80-100% spent
  - **Over Budget**: Budgets that exceeded limits
- Tap a chip to activate filter, tap again to deselect

**Edit Budget**
- Tap the three-dot menu (⋮) on a budget card
- Select "Edit Budget"
- Modify budget amount, period, or dates
- Changes apply immediately

**View History** (Recurring Budgets Only)
- Tap the three-dot menu (⋮) on a budget card
- Select "View History"
- See all past periods with:
  - Period dates
  - Budgeted vs spent amounts
  - Final status and percentage
  - Visual progress indicators
- Great for tracking improvement over time!

**Delete Budget**
- Tap the three-dot menu (⋮) on a budget card
- Select "Delete Budget"
- Confirm deletion in the dialog
- Budget and its history will be removed

### Budget Tips

- **Start Small**: Begin with one or two important categories
- **Be Realistic**: Set achievable budget amounts based on past spending
- **Use Recurring Budgets**: Choose Weekly/Monthly/Yearly for ongoing spending categories
- **Use Custom for Projects**: Choose Custom period for one-time goals or projects
- **Check History**: Review past periods to identify spending patterns
- **Review Regularly**: Check budget status weekly to stay on track
- **Use Search & Filters**: Find budgets quickly using search and status filters
- **Adjust as Needed**: Don't hesitate to edit budgets as your needs change
- **Track Trends**: Use budget history to see if you're improving over time

---

## 📈 Financial Analytics

Analytics provides deep insights into your financial habits and patterns.

### Accessing Analytics

From Dashboard, tap "Analytics" to open the Analytics screen.

### Time Period Selection

Choose how you want to view your data:

- **Week**: Daily breakdown for the past 7 days
- **Month**: Monthly summary (shows current month in YYYY-MM format)
- **Year**: Monthly trends for the past 12 months

Select a period using the chip buttons at the top of the screen.

### Financial Overview

The overview section shows key metrics:

- **Total Income**: Sum of all income for the selected period
- **Total Expenses**: Sum of all expenses for the selected period
- **Net Balance**: Income minus expenses
- **Transaction Count**: Total number of transactions
- **Average Transaction**: Average amount per transaction
- **Largest Income**: Highest single income transaction
- **Largest Expense**: Highest single expense transaction

### Spending Trends

Visual charts showing how your spending changes over time:

**Week View**
- Daily spending line chart
- Day-by-day comparison
- Transaction counts per day
- Identify spending patterns throughout the week

**Month View**
- Monthly spending summary
- Income vs expense comparison
- Total transactions for the month
- Month displayed as YYYY-MM format

**Year View**
- 12-month spending trends
- Monthly income trends
- Comparative analysis across months
- Identify seasonal spending patterns

### Category Breakdown

See where your money is going:

- **Top Categories**: Categories with highest spending
- **Percentage Distribution**: How much each category contributes
- **Spending Amounts**: Total spent per category
- **Transaction Counts**: Number of transactions per category

**Category Charts**
- Pie charts: Visual distribution of spending
- Bar charts: Compare spending across categories
- Interactive: Tap charts to see detailed information

### Using Analytics

**Identify Patterns**
- Spot recurring expenses
- Find areas where you spend the most
- Track income sources

**Make Informed Decisions**
- See which categories need budget attention
- Understand spending trends
- Plan for future expenses

**Track Progress**
- Compare current period to previous periods
- Monitor improvements in spending habits
- Celebrate financial milestones

---

## ⚙️ Settings & Profile

### Profile Management

Access your profile from Dashboard → "Profile" or Settings → "Profile".

**View Profile Information**
- Email address (encrypted and secure)
- Account creation date
- Last login information

**Update Profile**
- Change email address (requires re-authentication)
- Update password (with current password verification)
- Account management options

**Account Security**
- Change authentication method (Biometric/PIN)
- View security settings
- Manage session timeout preferences

### App Settings

Access Settings from Dashboard → "Settings".

**Theme Settings**
- **Light Theme**: Light background with dark text
- **Dark Theme**: Dark background with light text (reduces eye strain)
- **System Default**: Follows device theme settings
- Theme changes apply immediately

**Currency Settings**
- **Primary Currency**: Set your default currency
- **Multi-Currency Support**: Add multiple currencies
- **Exchange Rates**: Configure exchange rate preferences
- All amounts display in selected currency

**Display Preferences**
- Date format preferences
- Number format preferences
- Decimal places for amounts
- Currency symbol display

**Notification Settings**
- Budget alerts (when approaching limit)
- Transaction reminders
- Weekly/monthly summaries (future feature)

**Data Management**
- View app data usage
- Clear cache (if available)
- Manage stored data

**About**
- App version information
- Build details
- License information
- Credits

### Security Settings

**Authentication**
- Enable/disable biometric authentication
- Change PIN
- Set PIN complexity requirements
- Session timeout duration

**Privacy**
- View encryption status
- Data privacy information
- Security certificate details

---

## 💾 Data Backup & Restore

Expenso provides robust backup and restore capabilities to protect your financial data.

### Manual Backup

**Creating a Backup**

1. **Open Data Backup**
   - From Dashboard → "All Activities" → "Data Backup"
   - Or from Settings → "Data Backup"

2. **Create Backup**
   - Tap "Backup Now" button
   - App creates a CSV file with all your transactions
   - Backup is saved to your device's storage
   - You'll see a success message when backup completes

**Backup Location**
- Default: `Documents/Expenso/Backups/` folder
- Backup files named with timestamp (e.g., `manual_2024-01-15_143022.csv`)
- You can choose a custom backup location using "Choose Backup Location"

**Custom Backup Location**
- Tap "Choose Backup Location" to select any folder on your device
- Navigate to your preferred folder using the file picker
- Grant permission to the selected folder
- All future backups will be saved to this location
- Tap "Reset Backup Location" to return to default location

### Automatic Backup

**Setting Up Auto Backup**

1. Open Data Backup screen
2. Select backup frequency from the dropdown:
   - **Daily**: Backs up once per day
   - **Weekly**: Backs up once per week
   - **Monthly**: Backs up once per month
   - **None**: Disable automatic backup
3. Tap "Save Frequency"
4. App will automatically create backups according to schedule

**How Auto Backup Works**
- Runs in the background automatically
- Uses WorkManager for reliable scheduling
- Only backs up if enough time has passed since last backup
- No manual intervention required
- Check "Recent Backups" to see auto-created backups

### Restoring from Backup

**Restore Process**

1. **Open Data Backup**
   - Navigate to Data Backup screen

2. **Select Backup to Restore**
   - Tap "Restore Backup" button
   - Dialog shows list of available backups
   - Backups are sorted by date (newest first)
   - Shows backup type (Manual/Auto), date, and file size

3. **Choose Backup File**
   - Tap the backup file you want to restore
   - Confirm restoration in the dialog
   - App validates the backup file format

4. **Restore Complete**
   - Transactions are imported from backup
   - Existing transactions are merged (duplicates handled)
   - You'll see a success message
   - Check Transactions screen to verify restored data

**Important Notes**
- Restoring does NOT delete existing transactions
- Duplicate transactions may be created if same transaction exists
- Backup files are CSV format (comma-separated values)
- Backup includes all transaction fields

### Managing Backups

**Recent Backups List**
- Shows the 10 most recent backups
- Displays: Filename, Date, Size, Type (Manual/Auto)
- All backups are preserved on disk (only 10 latest shown in UI)

**Viewing Backup Details**
- Backup filename shows creation date and time
- File size indicates number of transactions
- Type indicates if backup was manual or automatic

**Deleting Backups**
1. Find the backup in "Recent Backups" list
2. Tap "Delete" button on the backup item
3. Confirm deletion in the dialog
4. Backup file is permanently deleted from storage

**Backup File Format**
- CSV (Comma-Separated Values) format
- Compatible with Excel, Google Sheets, and other spreadsheet applications
- First row contains column headers
- Each subsequent row contains one transaction
- Can be opened and edited in spreadsheet applications

### Backup Best Practices

**Regular Backups**
- Create manual backups before major changes
- Enable automatic backup for peace of mind
- Weekly or monthly backups are usually sufficient

**Backup Storage**
- Store backups in cloud storage (Google Drive, Dropbox) for safety
- Keep multiple backup copies in different locations
- Regularly verify backup files can be opened

**Before Restoring**
- Create a new backup of current data before restoring old backup
- Verify backup file is from the correct date
- Understand that restoring may create duplicate transactions

**File Management**
- Backup files are named with timestamps for easy identification
- Keep backups organized by date
- Delete old backups periodically to save storage space

---

## 🔧 Configuration

The Configuration screen allows you to manage system-wide settings and categories.

### Accessing Configuration

From Dashboard → "All Activities" → "Configuration"

### Category Management

**View Categories**
- See all available transaction categories
- Categories are organized alphabetically
- Each category shows associated information

**Category Details**
- Category name and description
- Number of transactions in category
- Category usage statistics

**Note**: Detailed category management features may be available in future updates.

### System Configuration

**App Settings**
- Database configuration (advanced)
- System preferences
- Feature toggles

**Maintenance**
- Database integrity checks
- Performance optimization
- Cache management

**Important**: Configuration screen is primarily for system management. Most users won't need to modify these settings.

---

## 🚀 Advanced Features

### Query Runner

Query Runner is an advanced feature for users with SQL knowledge who want to perform custom data analysis.

**Access Query Runner**
- From Dashboard → "All Activities" → "Query Runner"

**What is Query Runner?**
- Execute custom SQL SELECT queries on your transactions
- Advanced filtering and analysis beyond standard UI
- Export query results to CSV
- Read-only access (cannot modify data)

**Who Should Use It?**
- Users familiar with SQL
- Power users needing custom reports
- Developers debugging data issues
- Users wanting advanced data analysis

**For detailed Query Runner documentation, see [QUERY_RUNNER.md](QUERY_RUNNER.md)**

**Key Features**
- Write SQL queries directly in the app
- Automatic user filtering (you only see your data)
- Security restrictions (read-only, transactions table only)
- CSV export of query results
- 500-row default limit (can specify custom LIMIT)

**Sample Uses**
- Find all transactions over a specific amount
- Calculate custom statistics
- Search transactions with complex criteria
- Generate custom reports
- Analyze spending patterns with SQL

**Security**
- Only SELECT queries allowed
- Automatically filters by your user ID
- Read-only database access
- Blocks dangerous SQL operations

---

## 💡 Tips & Best Practices

### Getting the Most Out of Expenso

**Regular Transaction Entry**
- Enter transactions immediately after they occur
- Set reminders to log transactions daily
- Use recurring transactions for bills and subscriptions
- The more data you enter, the better your insights

**Categorize Consistently**
- Use the same categories for similar expenses
- Create meaningful category names
- Don't create too many categories (20-30 is ideal)
- Consistent categorization improves analytics accuracy

**Budget Realistically**
- Base budgets on past spending patterns
- Start with categories where you overspend
- Adjust budgets monthly based on actual spending
- Set achievable goals to stay motivated

**Review Analytics Weekly**
- Check spending trends weekly
- Identify areas where you can save
- Track progress toward financial goals
- Use insights to make better financial decisions

**Backup Regularly**
- Enable automatic backup for peace of mind
- Create manual backups before major changes
- Store backups in multiple locations (device + cloud)
- Test restoring from backup occasionally

**Use Tags Effectively**
- Add tags to transactions for additional organization
- Use consistent tag names
- Examples: "business", "personal", "urgent", "reimbursable"
- Search by tags to find related transactions

**Take Advantage of Filters**
- Use date range filters for monthly reviews
- Filter by category to analyze specific spending
- Search function helps find specific transactions quickly
- Combine filters for precise data views

**Monitor Budgets Actively**
- Check budget status weekly
- Adjust spending when approaching limits
- Create budgets for categories you want to control
- Don't ignore "Near Limit" warnings

**Secure Your Account**
- Use biometric authentication for convenience
- Choose a strong, unique PIN
- Don't share your login credentials
- Keep your device secure with screen lock

**Privacy First**
- Remember: All data stays on your device
- No internet connection required
- Your financial data is never transmitted
- Enjoy complete privacy and security

### Financial Management Best Practices

**Set Financial Goals**
- Define short-term and long-term goals
- Track progress using Analytics
- Adjust spending to meet goals
- Celebrate milestones

**Track All Income**
- Record all income sources (salary, freelance, gifts)
- Don't forget small income amounts
- Accurate income tracking improves analytics

**Record Expenses Promptly**
- Enter expenses as soon as possible
- Don't wait days or weeks to log transactions
- Use recurring transactions for bills
- Set reminders for regular expenses

**Review Regularly**
- Weekly: Review transactions and budgets
- Monthly: Analyze spending trends
- Quarterly: Adjust budgets and goals
- Yearly: Annual financial review

**Use Multiple Currencies**
- Track expenses in different currencies
- Set primary currency for reporting
- Exchange rates handled automatically
- Useful for travelers or international users

---

## 🔍 Troubleshooting

### Common Issues and Solutions

#### Authentication Problems

**Cannot Login with Biometric**
- **Solution**: Try using your PIN instead
- **Solution**: Re-register biometric in device settings
- **Solution**: Ensure biometric authentication is enabled in app settings
- **Solution**: Restart the app if biometric fails repeatedly

**Forgot PIN**
- **Solution**: If biometric is available, use biometric to access
- **Solution**: If only PIN is available, you may need to reset the app (data will be lost)
- **Prevention**: Enable biometric authentication as backup

**Session Expired**
- **Solution**: Simply re-authenticate (biometric or PIN)
- **Normal Behavior**: Session timeout is a security feature
- **Tip**: Increase session timeout in Settings if it's too frequent

#### Transaction Issues

**Transaction Not Appearing**
- **Solution**: Check if transaction is in the correct date range
- **Solution**: Verify filters aren't hiding the transaction
- **Solution**: Refresh the transaction list (pull down)
- **Solution**: Check if transaction was accidentally deleted

**Wrong Amount Displayed**
- **Solution**: Check currency settings match transaction currency
- **Solution**: Verify amount was entered correctly
- **Solution**: Check if exchange rate is affecting display

**Cannot Edit Transaction**
- **Solution**: Ensure you're opening the transaction details
- **Solution**: Check if transaction is soft-deleted (restore first)
- **Solution**: Restart app if edit button doesn't respond

#### Budget Issues

**Budget Not Tracking Spending**
- **Solution**: Verify transactions are in the budget's date range
- **Solution**: Check if transactions match the budget's category
- **Solution**: Ensure budget is active (not paused or expired)
- **Solution**: Refresh the budgets screen

**Budget Status Incorrect**
- **Solution**: Check transaction dates match budget period
- **Solution**: Verify transactions are not soft-deleted
- **Solution**: Calculate manually to verify (Total Spending / Budget Amount)

**Cannot Create Budget**
- **Solution**: Ensure all required fields are filled
- **Solution**: Check budget amount is a valid number
- **Solution**: Verify start date is before end date
- **Solution**: Restart app if dialog doesn't appear

#### Analytics Issues

**Analytics Not Loading**
- **Solution**: Wait for analytics calculation (may take a few seconds)
- **Solution**: Select a different time period (Week/Month/Year)
- **Solution**: Refresh the analytics screen (pull down)
- **Solution**: Ensure you have transactions in the selected period

**Charts Not Displaying**
- **Solution**: Check if you have enough data for the chart
- **Solution**: Try a different time period
- **Solution**: Restart the app
- **Solution**: Verify transactions exist in selected period

**Incorrect Calculations**
- **Solution**: Check transaction types (Income vs Expense)
- **Solution**: Verify date ranges match expected period
- **Solution**: Check for soft-deleted transactions affecting totals
- **Solution**: Calculate manually to verify accuracy

#### Backup & Restore Issues

**Backup Not Creating**
- **Solution**: Check storage permissions are granted
- **Solution**: Verify sufficient storage space available
- **Solution**: Try choosing a different backup location
- **Solution**: Restart app and try again

**Cannot Restore Backup**
- **Solution**: Verify backup file is valid CSV format
- **Solution**: Check backup file is not corrupted
- **Solution**: Ensure backup file is from Expenso app
- **Solution**: Try a different backup file

**Backup File Not Found**
- **Solution**: Check default backup location: `Documents/Expenso/Backups/`
- **Solution**: Use "Choose Backup Location" if using custom location
- **Solution**: Verify backup file wasn't deleted or moved
- **Solution**: Check file manager for backup files

**Auto Backup Not Working**
- **Solution**: Verify auto backup is enabled and frequency is set
- **Solution**: Check if sufficient time has passed since last backup
- **Solution**: Ensure app isn't being killed by battery optimization
- **Solution**: Manually trigger a backup to test functionality

#### Performance Issues

**App Running Slowly**
- **Solution**: Close other apps to free up memory
- **Solution**: Clear app cache (Settings → Data Management)
- **Solution**: Restart the app
- **Solution**: Check if device has sufficient storage space

**Transactions Loading Slowly**
- **Solution**: Use filters to reduce number of transactions shown
- **Solution**: This is normal if you have thousands of transactions
- **Solution**: App loads transactions in batches (pagination)
- **Solution**: Wait for initial load to complete

**Analytics Taking Long to Calculate**
- **Solution**: This is normal for large datasets
- **Solution**: Wait for calculation to complete (shows loading indicator)
- **Solution**: Select shorter time period for faster calculation
- **Solution**: Complex analytics may take 5-10 seconds

#### Display Issues

**Theme Not Changing**
- **Solution**: Restart app after changing theme
- **Solution**: Check theme setting in Settings → Theme
- **Solution**: Ensure device isn't overriding app theme
- **Solution**: Try switching to a different theme and back

**Currency Not Displaying Correctly**
- **Solution**: Check currency setting in Settings → Currency
- **Solution**: Verify currency code is correct
- **Solution**: Check if currency symbol is supported
- **Solution**: Restart app after changing currency

**Data Not Refreshing**
- **Solution**: Pull down to refresh (swipe down on screen)
- **Solution**: Navigate away and back to refresh
- **Solution**: Restart the app
- **Solution**: Check if new transactions were actually saved

### Getting Help

**Documentation**
- Check this User Guide for detailed instructions
- Review [QUERY_RUNNER.md](QUERY_RUNNER.md) for advanced features
- See [ARCHITECTURE.md](ARCHITECTURE.md) for technical details
- Read [FEATURES.md](FEATURES.md) for complete feature list

**Common Solutions**
- Most issues can be resolved by restarting the app
- Check if you have the latest app version
- Verify device has sufficient storage and memory
- Ensure app permissions are granted (storage, biometric)

**Data Safety**
- Always backup your data before troubleshooting
- Your data is safe - it's stored locally and encrypted
- Restoring from backup can fix many issues
- Never share your login credentials

---

## 📱 Quick Reference

### Navigation

- **Dashboard**: Main screen with financial overview
- **Transactions**: Manage all transactions
- **Budgets**: Create and track budgets
- **Analytics**: View financial insights
- **Settings**: Configure app preferences
- **Profile**: Manage user account
- **All Activities**: Access all app features
- **Data Backup**: Backup and restore data
- **Query Runner**: Advanced SQL queries (for power users)

### Keyboard Shortcuts (Future Feature)
- Currently, Expenso is optimized for touch interaction
- Keyboard shortcuts may be available in future updates

### Gestures

- **Swipe Down**: Pull to refresh data
- **Swipe Left/Right**: Navigate between screens (where supported)
- **Tap**: Select items, open details, perform actions
- **Long Press**: Context menus (where available)

---

## 📝 Additional Resources

### Related Documentation

For more detailed information, please refer to these documents:

#### User-Focused Documentation
- **[QUERY_RUNNER.md](QUERY_RUNNER.md)** - Advanced SQL query feature guide
  - For power users who want to execute custom SQL queries
  - Includes 30+ sample queries and security details
  - Transaction table schema reference

#### Technical Documentation
- **[FEATURES.md](FEATURES.md)** - Complete feature documentation
  - Detailed feature specifications and implementation
  - Technical capabilities and configurations
  - Platform support and performance metrics

- **[ARCHITECTURE.md](ARCHITECTURE.md)** - System architecture details
  - Clean Architecture implementation
  - Component relationships and data flow
  - Database schema and design patterns

- **[SECURITY.md](SECURITY.md)** - Security implementation information
  - Multi-layer security architecture
  - Encryption and authentication details
  - Privacy protection mechanisms

#### General
- **[README.md](../README.md)** - Project overview and setup
  - Getting started with development
  - Technology stack and dependencies
  - Build commands and prerequisites

- **[TERMS_AND_CONDITIONS.md](TERMS_AND_CONDITIONS.md)** - Legal terms
  - Application terms of use
  - User responsibilities and disclaimers
  - Privacy policy information

### Support

**Self-Service**
- This User Guide covers most common questions
- Check Troubleshooting section for solutions
- Review feature-specific documentation

**Tips for Getting Help**
- Describe the issue clearly
- Include steps to reproduce the problem
- Note which app version you're using
- Check if issue occurs after restarting app

---

## 🎉 You're All Set!

You now have a comprehensive understanding of how to use Expenso effectively. Remember:

- ✅ Enter transactions regularly for best insights
- ✅ Use budgets to control spending
- ✅ Review analytics weekly to track progress
- ✅ Backup your data regularly
- ✅ Enjoy complete privacy with 100% offline operation

**Expenso** - Your secure, private, offline financial companion.

---

**Last Updated**: Based on Expenso application version with package `com.offline.expenso`

