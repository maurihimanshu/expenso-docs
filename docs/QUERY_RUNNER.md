# Query Runner - User Guide

## 📋 Table of Contents

1. [Overview](#-overview)
2. [Features & Capabilities](#-features--capabilities)
3. [Security Architecture](#️-security-architecture)
4. [Pros & Cons](#-pros--cons)
5. [Transaction Table Schema](#-transaction-table-schema)
6. [Sample Queries](#-sample-queries)
7. [Usage Guidelines](#-usage-guidelines)
8. [Limitations & Restrictions](#️-limitations--restrictions)
9. [Best Practices](#-best-practices)
10. [Troubleshooting](#-troubleshooting)
11. [Additional Resources](#-additional-resources)
12. [Notes](#-notes)

---

## 🎯 Overview

The **Query Runner** is a powerful feature in Expenso that allows advanced users to execute custom SQL queries against the `transactions` table. It provides a secure, read-only interface for data analysis, custom reporting, and advanced filtering that goes beyond the standard UI capabilities.

### Key Characteristics

- **Read-Only Access**: Only SELECT queries are allowed; no data modification operations
- **User-Scoped**: All queries are automatically scoped to the current logged-in user's data
- **Secure Execution**: Multiple layers of security validation before query execution
- **Encrypted Database**: Uses SQLCipher to access the encrypted database securely
- **Export Capability**: Results can be exported to CSV format for external analysis

---

## ✨ Features & Capabilities

### Core Features

1. **Custom SQL Query Execution**
   - Write and execute SELECT queries directly in the app
   - Real-time query validation and security checks
   - Results displayed in a scrollable RecyclerView

2. **Automatic User Filtering**
   - Automatically appends `user_id` filter to all queries
   - Ensures users can only access their own transaction data
   - Prevents cross-user data access

3. **Query Result Display**
   - Dynamic column header detection
   - Scrollable result set with all transaction fields
   - Row-by-row display with column names and values

4. **CSV Export**
   - Export query results to CSV file
   - Uses Storage Access Framework (SAF) for file selection
   - Proper CSV escaping for special characters (commas, quotes, newlines)
   - Compatible with Excel, Google Sheets, and other tools

5. **Security Enforcement**
   - Keyword blocking for dangerous SQL operations
   - Table access restriction (only `transactions` table)
   - Automatic LIMIT enforcement (default: 500 rows)
   - Read-only database connection

### Advanced Capabilities

1. **Aggregate Queries**
   - SUM, AVG, COUNT, MAX, MIN functions
   - GROUP BY for data grouping
   - HAVING for aggregate filtering

2. **Complex Filtering**
   - Multiple WHERE conditions with AND/OR
   - Date range filtering using transaction_date
   - Pattern matching with LIKE
   - NULL value handling

3. **Sorting & Ordering**
   - ORDER BY with multiple columns
   - ASC/DESC sorting
   - Combined sorting on multiple fields

4. **Joins (Limited)**
   - Can reference `transactions` and `user_transactions` views
   - Cross-table joins are restricted for security

5. **Subqueries**
   - Correlated and non-correlated subqueries
   - IN, EXISTS, NOT EXISTS subquery patterns

---

## 🛡️ Security Architecture

### Multi-Layer Security Model

#### Layer 1: Query Type Validation
- **Requirement**: Query must start with `SELECT`
- **Rejection**: Any other query type (INSERT, UPDATE, DELETE, etc.)
- **Purpose**: Enforce read-only access

#### Layer 2: Keyword Blocking
- **Blocked Keywords**: `DROP`, `DELETE`, `ALTER`, `INSERT`, `UPDATE`, `ATTACH`, `PRAGMA`, `VACUUM`
- **Method**: Case-insensitive pattern matching in uppercase query
- **Purpose**: Prevent destructive or system-level operations

#### Layer 3: Table Access Restriction
- **Allowed Tables**: `transactions`, `user_transactions` (case-insensitive)
- **Validation**: Regex pattern matching for table references
- **Rejection**: Queries referencing any other tables or views
- **Purpose**: Limit data access scope

#### Layer 4: User Scope Enforcement
- **Automatic Filtering**: Appends `WHERE user_id = '<current_user_id>'` if not present
- **Where Clause Enhancement**: Adds `AND user_id = '<current_user_id>'` if WHERE exists but user_id is missing
- **SQL Injection Prevention**: Escapes single quotes in user_id using `''` replacement
- **Purpose**: Ensure data isolation between users

#### Layer 5: Result Set Limiting
- **Default Limit**: 500 rows if LIMIT not specified
- **User-Specified Limit**: Allows custom LIMIT in query
- **Purpose**: Prevent excessive memory usage and database load

#### Layer 6: Read-Only Database Connection
- **Connection Mode**: `OPEN_READONLY` flag set on SQLCipher database
- **Database Access**: Direct SQLCipher connection with encrypted passphrase
- **Purpose**: Prevent any write operations even if query passes other checks

### Security Flow Diagram

```
User Input Query
    ↓
[Layer 1] Validate SELECT-only
    ↓
[Layer 2] Block dangerous keywords
    ↓
[Layer 3] Restrict to transactions table
    ↓
[Layer 4] Enforce user_id filter
    ↓
[Layer 5] Apply LIMIT (if missing)
    ↓
[Layer 6] Open read-only database connection
    ↓
Execute Query → Return Cursor
```

---

## ✅ Pros & Cons

### Advantages (Pros)

#### 1. **Powerful Data Analysis**
- ✅ Execute complex queries not available in standard UI
- ✅ Aggregate functions for statistics and summaries
- ✅ Custom date range filtering with precision
- ✅ Pattern matching with LIKE for text searches

#### 2. **Export for External Analysis**
- ✅ Export results to CSV for Excel/Google Sheets
- ✅ Proper CSV formatting with escaping
- ✅ No data loss during export

#### 3. **Developer-Friendly**
- ✅ Direct SQL access for debugging
- ✅ Test queries without modifying code
- ✅ Validate data integrity
- ✅ Understand database schema

#### 4. **Secure by Design**
- ✅ Multiple security layers prevent data breaches
- ✅ Read-only access prevents accidental data loss
- ✅ Automatic user scoping prevents unauthorized access
- ✅ SQL injection protection through escape mechanisms

#### 5. **User Experience**
- ✅ Simple interface with text input and results display
- ✅ Real-time query execution
- ✅ Clear error messages for rejected queries

### Disadvantages (Cons)

#### 1. **Limited to Transactions Table**
- ❌ Cannot query other tables (users, categories, budgets)
- ❌ Cannot join with other tables for comprehensive analysis
- ❌ Restricted to `transactions` and `user_transactions` views only

#### 2. **Read-Only Operations**
- ❌ Cannot modify data through Query Runner
- ❌ Cannot create temporary tables or views
- ❌ Cannot execute stored procedures (not supported by SQLite)

#### 3. **Performance Considerations**
- ❌ Complex queries may be slow on large datasets
- ❌ Default 500-row limit may truncate results
- ❌ No query optimization hints

#### 4. **SQL Knowledge Required**
- ❌ Requires SQL knowledge to use effectively
- ❌ No query builder or visual interface
- ❌ Error messages may be technical for non-developers

#### 5. **No Query History**
- ❌ Previously executed queries are not saved
- ❌ No favorite queries storage
- ❌ Must re-type queries on each session

#### 6. **Limited SQLite Features**
- ❌ Cannot use CTEs (Common Table Expressions) - SQLite version dependent
- ❌ Window functions limited by SQLite version
- ❌ No support for full-text search extensions

---

## 📊 Transaction Table Schema

### Complete Field Reference

The `transactions` table contains the following fields (all accessible in queries):

| Field Name | Type | Description | Example Values |
|-----------|------|-------------|----------------|
| `id` | TEXT (PK) | Unique transaction identifier | "txn_123456" |
| `user_id` | TEXT | User identifier (automatically filtered) | "user_abc" |
| `amount` | TEXT | Transaction amount (plain text) | "100.50", "-50.00" |
| `currency_code` | TEXT | ISO currency code | "USD", "EUR", "INR" |
| `exchange_rate` | TEXT | Exchange rate if multi-currency | "1.0", "0.85" |
| `type` | TEXT | Transaction type | "INCOME", "EXPENSE", "TRANSFER" |
| `category_id` | TEXT | Category identifier | "cat_food", "cat_transport" |
| `subcategory` | TEXT | Subcategory name | "Restaurants", "Gas" |
| `description` | TEXT | Transaction description | "Lunch at restaurant" |
| `notes` | TEXT | Additional notes | "Client meeting" |
| `transaction_date` | INTEGER | Date timestamp (milliseconds) | 1640995200000 |
| `transaction_time` | INTEGER | Time timestamp (milliseconds) | 1641024000000 |
| `account_name` | TEXT | Account identifier | "Checking", "Savings" |
| `payment_method` | TEXT | Payment method | "Credit Card", "Cash", "Bank Transfer" |
| `reference_number` | TEXT | Reference/tracking number | "REF123", "INV-456" |
| `location` | TEXT | Transaction location | "New York, NY", "Online" |
| `tags` | TEXT | Comma-separated tags | "business,travel,urgent" |
| `is_recurring` | INTEGER | Recurring flag (0 or 1) | 0, 1 |
| `recurring_pattern` | TEXT | JSON recurring pattern | '{"frequency":"monthly"}' |
| `receipt_image_path` | TEXT | Path to receipt image | "/storage/receipts/img1.jpg" |
| `is_tax_deductible` | INTEGER | Tax deductible flag (0 or 1) | 0, 1 |
| `created_at` | INTEGER | Creation timestamp | 1640995200000 |
| `updated_at` | INTEGER | Last update timestamp | 1641024000000 |
| `is_deleted` | INTEGER | Soft delete flag (0 or 1) | 0, 1 |
| `version` | INTEGER | Optimistic locking version | 1, 2, 3 |

### Important Notes

1. **Timestamps**: `transaction_date`, `transaction_time`, `created_at`, `updated_at` are stored as **integers** (milliseconds since epoch). Use `datetime()` or date arithmetic functions for date comparisons.

2. **Amount Field**: Stored as **TEXT** (plain text, not encrypted). Can be converted to numeric using `CAST(amount AS REAL)` for calculations.

3. **Boolean Fields**: `is_recurring`, `is_tax_deductible`, `is_deleted` are stored as **INTEGER** (0 or 1), not true boolean.

4. **Soft Delete**: Queries automatically exclude soft-deleted records (`is_deleted = 0`) in standard operations, but Query Runner shows all records unless explicitly filtered.

5. **User ID Filtering**: The `user_id` filter is automatically added by SafeQueryRunner, so you don't need to include it in your queries.

---

## 💡 Sample Queries

### Basic Queries

#### 1. Get All Transactions (Limited to 500)
```sql
SELECT * FROM transactions
```
**Result**: All fields for up to 500 transactions, automatically filtered by current user.

#### 2. Get Specific Fields
```sql
SELECT id, description, amount, transaction_date, type FROM transactions
```
**Result**: Only selected fields for all transactions.

#### 3. Get Latest Transactions
```sql
SELECT * FROM transactions ORDER BY transaction_date DESC LIMIT 10
```
**Result**: 10 most recent transactions sorted by date.

### Filtering Queries

#### 4. Filter by Transaction Type
```sql
SELECT * FROM transactions WHERE type = 'EXPENSE'
```
**Result**: All expense transactions.

#### 5. Filter by Category
```sql
SELECT * FROM transactions WHERE category_id = 'cat_food'
```
**Result**: All transactions in the food category.

#### 6. Filter by Date Range
```sql
SELECT * FROM transactions 
WHERE transaction_date >= 1640995200000 
  AND transaction_date <= 1672531199000
```
**Result**: Transactions between specific timestamps (year 2022).

#### 7. Filter by Payment Method
```sql
SELECT * FROM transactions WHERE payment_method = 'Credit Card'
```
**Result**: All credit card transactions.

#### 8. Filter by Multiple Conditions
```sql
SELECT * FROM transactions 
WHERE type = 'EXPENSE' 
  AND amount LIKE '%-%' 
  AND transaction_date >= 1640995200000
```
**Result**: Expenses with negative amounts (expense indicator) after a specific date.

### Search Queries

#### 9. Search in Description
```sql
SELECT * FROM transactions WHERE description LIKE '%restaurant%'
```
**Result**: Transactions containing "restaurant" in description.

#### 10. Search with Case-Insensitive
```sql
SELECT * FROM transactions WHERE UPPER(description) LIKE '%LUNCH%'
```
**Result**: Transactions with "LUNCH" in description (case-insensitive).

#### 11. Search in Notes
```sql
SELECT * FROM transactions WHERE notes LIKE '%business%'
```
**Result**: Transactions with "business" in notes field.

### Aggregate Queries

#### 12. Total Expenses
```sql
SELECT SUM(CAST(amount AS REAL)) as total_expenses 
FROM transactions 
WHERE type = 'EXPENSE'
```
**Result**: Sum of all expense amounts.

#### 13. Average Transaction Amount
```sql
SELECT AVG(CAST(amount AS REAL)) as avg_amount 
FROM transactions
```
**Result**: Average transaction amount.

#### 14. Count Transactions by Type
```sql
SELECT type, COUNT(*) as count 
FROM transactions 
GROUP BY type
```
**Result**: Number of transactions grouped by type (INCOME, EXPENSE, TRANSFER).

#### 15. Total by Category
```sql
SELECT category_id, SUM(CAST(amount AS REAL)) as category_total 
FROM transactions 
WHERE type = 'EXPENSE'
GROUP BY category_id 
ORDER BY category_total DESC
```
**Result**: Total expenses grouped by category, sorted by highest total.

#### 16. Monthly Spending
```sql
SELECT 
  strftime('%Y-%m', datetime(transaction_date / 1000, 'unixepoch')) as month,
  SUM(CAST(amount AS REAL)) as monthly_total
FROM transactions
WHERE type = 'EXPENSE'
GROUP BY month
ORDER BY month DESC
```
**Result**: Monthly expense totals formatted as YYYY-MM.

#### 17. Transaction Statistics
```sql
SELECT 
  COUNT(*) as total_transactions,
  MIN(CAST(amount AS REAL)) as min_amount,
  MAX(CAST(amount AS REAL)) as max_amount,
  AVG(CAST(amount AS REAL)) as avg_amount
FROM transactions
WHERE type = 'EXPENSE'
```
**Result**: Statistical summary of expense transactions.

### Date & Time Queries

#### 18. Transactions This Month
```sql
SELECT * FROM transactions
WHERE strftime('%Y-%m', datetime(transaction_date / 1000, 'unixepoch')) = strftime('%Y-%m', 'now')
```
**Result**: All transactions from the current month.

#### 19. Transactions by Day
```sql
SELECT 
  date(datetime(transaction_date / 1000, 'unixepoch')) as day,
  COUNT(*) as transaction_count
FROM transactions
GROUP BY day
ORDER BY day DESC
LIMIT 30
```
**Result**: Transaction count per day for the last 30 days.

#### 20. Transactions on Specific Date
```sql
SELECT * FROM transactions
WHERE date(datetime(transaction_date / 1000, 'unixepoch')) = '2024-01-15'
```
**Result**: All transactions on January 15, 2024.

### Advanced Queries

#### 21. Recurring Transactions
```sql
SELECT * FROM transactions WHERE is_recurring = 1
```
**Result**: All recurring transactions.

#### 22. Tax Deductible Expenses
```sql
SELECT * FROM transactions 
WHERE type = 'EXPENSE' AND is_tax_deductible = 1
```
**Result**: All tax-deductible expense transactions.

#### 23. Transactions with Receipts
```sql
SELECT * FROM transactions 
WHERE receipt_image_path IS NOT NULL AND receipt_image_path != ''
```
**Result**: Transactions that have receipt images attached.

#### 24. Transactions by Location
```sql
SELECT location, COUNT(*) as count, SUM(CAST(amount AS REAL)) as total
FROM transactions
WHERE location IS NOT NULL
GROUP BY location
ORDER BY total DESC
```
**Result**: Transactions grouped by location with counts and totals.

#### 25. Tagged Transactions
```sql
SELECT * FROM transactions 
WHERE tags IS NOT NULL AND tags != ''
```
**Result**: All transactions that have tags assigned.

#### 26. Top Spending Categories
```sql
SELECT 
  category_id,
  COUNT(*) as transaction_count,
  SUM(CAST(amount AS REAL)) as total_spent
FROM transactions
WHERE type = 'EXPENSE'
GROUP BY category_id
ORDER BY total_spent DESC
LIMIT 10
```
**Result**: Top 10 expense categories by total spending.

#### 27. Payment Method Analysis
```sql
SELECT 
  payment_method,
  COUNT(*) as usage_count,
  SUM(CAST(amount AS REAL)) as total_amount,
  AVG(CAST(amount AS REAL)) as avg_amount
FROM transactions
WHERE payment_method IS NOT NULL
GROUP BY payment_method
ORDER BY total_amount DESC
```
**Result**: Analysis of payment methods with usage statistics.

#### 28. Year-over-Year Comparison
```sql
SELECT 
  strftime('%Y', datetime(transaction_date / 1000, 'unixepoch')) as year,
  strftime('%m', datetime(transaction_date / 1000, 'unixepoch')) as month,
  SUM(CAST(amount AS REAL)) as monthly_total
FROM transactions
WHERE type = 'EXPENSE'
GROUP BY year, month
ORDER BY year DESC, month DESC
```
**Result**: Monthly expense totals grouped by year and month.

#### 29. Large Transactions
```sql
SELECT * FROM transactions
WHERE ABS(CAST(amount AS REAL)) > 1000
ORDER BY ABS(CAST(amount AS REAL)) DESC
```
**Result**: All transactions with absolute amount greater than 1000.

#### 30. Transaction Frequency Analysis
```sql
SELECT 
  account_name,
  COUNT(*) as transaction_count,
  MIN(transaction_date) as first_transaction,
  MAX(transaction_date) as last_transaction
FROM transactions
WHERE account_name IS NOT NULL
GROUP BY account_name
```
**Result**: Transaction frequency per account with date ranges.

### Subquery Examples

#### 31. Transactions Above Average
```sql
SELECT * FROM transactions
WHERE CAST(amount AS REAL) > (
  SELECT AVG(CAST(amount AS REAL)) 
  FROM transactions 
  WHERE type = 'EXPENSE'
)
```
**Result**: Transactions with amounts above the average expense.

#### 32. Categories with Multiple Transactions
```sql
SELECT * FROM transactions
WHERE category_id IN (
  SELECT category_id 
  FROM transactions 
  GROUP BY category_id 
  HAVING COUNT(*) > 5
)
```
**Result**: Transactions from categories that have more than 5 transactions.

---

## 📖 Usage Guidelines

### How to Use Query Runner

1. **Access Query Runner**
   - Navigate to "All Activities" → "Query Runner"
   - Or launch directly if you have the activity shortcut

2. **Enter Your Query**
   - Type or paste your SQL SELECT query in the text input field
   - Query should reference `transactions` or `user_transactions` table
   - No need to include `user_id` filter (automatically added)

3. **Execute Query**
   - Click the "Run" button to execute
   - Results will appear in the scrollable RecyclerView below
   - Column names are shown at the top of each row

4. **Export Results**
   - After running a query, click "Export CSV"
   - Choose a location to save the file
   - File will be saved as `query_results.csv`

### Query Writing Tips

1. **Always Start with SELECT**
   - Only SELECT queries are allowed
   - Other query types will be rejected

2. **Reference Transactions Table**
   - Must mention `transactions` or `user_transactions` in query
   - Table name matching is case-insensitive

3. **User ID is Automatic**
   - Don't include `user_id` in WHERE clause unless you want to be explicit
   - SafeQueryRunner automatically adds user filtering

4. **Include LIMIT for Large Results**
   - Default limit is 500 rows
   - Add `LIMIT 100` or higher if you expect more results
   - Remove LIMIT if you want the default 500 limit

5. **Date Handling**
   - Timestamps are in milliseconds (integers)
   - Use `datetime(transaction_date / 1000, 'unixepoch')` for date operations
   - Use `strftime()` for date formatting

6. **Amount Calculations**
   - Convert TEXT amount to REAL: `CAST(amount AS REAL)`
   - Use `ABS()` for absolute values
   - Handle negative amounts for expenses

---

## ⚠️ Limitations & Restrictions

### Enforced Limitations

1. **Query Type Restriction**
   - ❌ Only SELECT queries allowed
   - ❌ INSERT, UPDATE, DELETE, DROP, ALTER blocked

2. **Table Access Restriction**
   - ❌ Only `transactions` and `user_transactions` tables accessible
   - ❌ Cannot query `users`, `categories`, `budgets`, or other tables
   - ❌ Cannot create or access views

3. **Dangerous Keywords Blocked**
   - ❌ DROP, DELETE, ALTER, INSERT, UPDATE
   - ❌ ATTACH, PRAGMA, VACUUM
   - ❌ Any query containing these keywords (even in comments) is rejected

4. **Read-Only Connection**
   - ❌ Database opened in read-only mode
   - ❌ Cannot modify data even if query passes validation

5. **Result Set Limiting**
   - ❌ Default limit of 500 rows if not specified
   - ❌ Large result sets may be truncated

6. **User Scope Enforcement**
   - ❌ Cannot query other users' data
   - ❌ `user_id` filter automatically enforced
   - ❌ Cannot bypass user filtering

### SQLite Limitations

1. **SQLite Version Features**
   - Limited window functions support
   - CTEs (Common Table Expressions) may not be available
   - Some advanced SQL features not supported

2. **Performance**
   - Complex queries on large datasets may be slow
   - No query optimization hints
   - Index usage depends on query structure

3. **Data Type Handling**
   - Amount stored as TEXT requires casting for calculations
   - Date/timestamp arithmetic requires conversion functions
   - Boolean values stored as INTEGER (0 or 1)

---

## 🎓 Best Practices

### Query Optimization

1. **Use Indexed Columns**
   - Filter on indexed columns: `user_id`, `transaction_date`, `type`, `category_id`, `payment_method`
   - Composite indexes available on (`user_id`, `transaction_date`), (`user_id`, `type`), (`user_id`, `category_id`)

2. **Limit Result Sets**
   - Always specify LIMIT when you know the expected result size
   - Use OFFSET for pagination
   - Example: `LIMIT 100 OFFSET 0` for first 100, `LIMIT 100 OFFSET 100` for next 100

3. **Select Only Needed Columns**
   - Use `SELECT col1, col2, ...` instead of `SELECT *`
   - Reduces memory usage and improves performance

4. **Efficient Date Filtering**
   - Use `transaction_date` (integer) for range queries
   - Convert to date format only when displaying
   - Example: `WHERE transaction_date BETWEEN 1640995200000 AND 1672531199000`

### Security Best Practices

1. **Never Share User IDs**
   - Don't include hardcoded user IDs in queries
   - Let SafeQueryRunner handle user filtering automatically

2. **Sanitize Input (if building queries programmatically)**
   - Escape single quotes if constructing queries dynamically
   - Use parameterized queries when possible (though Query Runner doesn't support this)

3. **Validate Query Results**
   - Check result counts match expectations
   - Verify data integrity after queries

### Query Writing Best Practices

1. **Use Descriptive Column Names in SELECT**
   - Use aliases for clarity: `SELECT amount AS transaction_amount`
   - Makes exported CSV files more readable

2. **Format Dates in Queries**
   - Use `strftime()` for readable date output
   - Example: `strftime('%Y-%m-%d', datetime(transaction_date / 1000, 'unixepoch'))`

3. **Handle NULL Values**
   - Use `IS NULL` or `IS NOT NULL` for NULL checks
   - Use `COALESCE()` for default values: `COALESCE(description, 'N/A')`

4. **Use Appropriate Sorting**
   - Always use `ORDER BY` for predictable results
   - Sort by date descending for recent-first: `ORDER BY transaction_date DESC`

5. **Group Related Conditions**
   - Use parentheses for complex WHERE clauses
   - Example: `WHERE (type = 'EXPENSE' OR type = 'TRANSFER') AND amount > 100`

### Export Best Practices

1. **Export Before Complex Analysis**
   - Export raw data to CSV for Excel/Google Sheets analysis
   - Perform aggregations in spreadsheet tools if needed

2. **Include Relevant Columns**
   - Select only columns needed for analysis
   - Use aliases for clarity in exported CSV

3. **Name Export Files Clearly**
   - The app suggests `query_results.csv`
   - Rename to descriptive names like `monthly_expenses_2024.csv`

---

## 🔍 Troubleshooting

### Common Error Messages

1. **"Only SELECT queries allowed"**
   - **Cause**: Query doesn't start with SELECT
   - **Fix**: Ensure query begins with `SELECT`

2. **"Query contains restricted keyword: [keyword]"**
   - **Cause**: Query contains blocked keyword (DROP, DELETE, etc.)
   - **Fix**: Remove the blocked keyword or rewrite query

3. **"Access restricted: only transactions data can be queried"**
   - **Cause**: Query doesn't reference `transactions` or `user_transactions`
   - **Fix**: Add `FROM transactions` or `FROM user_transactions` to query

4. **"Failed to run query: [error]"**
   - **Cause**: SQL syntax error or invalid query structure
   - **Fix**: Check SQL syntax, verify column names, ensure proper WHERE/ORDER BY clauses

5. **"User not logged in"**
   - **Cause**: Session expired or user logged out
   - **Fix**: Re-login to the application

### Performance Issues

1. **Slow Query Execution**
   - **Solution**: Add WHERE conditions on indexed columns
   - **Solution**: Limit result set with LIMIT
   - **Solution**: Select only needed columns instead of `SELECT *`

2. **Large Result Sets**
   - **Solution**: Use LIMIT to reduce result size
   - **Solution**: Add more specific WHERE conditions
   - **Solution**: Export to CSV for external analysis

### Data Issues

1. **Unexpected Results**
   - **Check**: Verify date range filters are correct
   - **Check**: Ensure amount casting is correct: `CAST(amount AS REAL)`
   - **Check**: Check for NULL values with `IS NULL` or `IS NOT NULL`

2. **Missing Data**
   - **Check**: Verify `is_deleted = 0` if filtering out deleted records
   - **Check**: Ensure date ranges are inclusive (use `>=` and `<=`)
   - **Check**: Check if LIMIT is truncating results

---

## 📚 Additional Resources

### SQLite Documentation
- SQLite SQL Syntax: https://www.sqlite.org/lang.html
- SQLite Date/Time Functions: https://www.sqlite.org/lang_datefunc.html
- SQLite Aggregate Functions: https://www.sqlite.org/lang_aggfunc.html

### Related Expenso Documentation
- **[ARCHITECTURE.md](ARCHITECTURE.md)**: System architecture and database design
- **[SECURITY.md](SECURITY.md)**: Security implementation details
- **[FEATURES.md](FEATURES.md)**: Complete feature list
- **[USER_GUIDE.md](USER_GUIDE.md)**: Complete user manual and usage instructions
- **[README.md](../README.md)**: Project overview and getting started guide

### Transaction Data Reference
- See `TransactionEntity.java` for complete field definitions
- See `TransactionDao.java` for available query patterns
- Database schema defined in `AppDatabase.java`

---

## 📝 Notes

- All queries are executed against the encrypted SQLCipher database
- Results are displayed in memory and can be exported to CSV
- Query history is not persisted between sessions
- Complex queries may take several seconds to execute on large datasets
- The Query Runner is designed for advanced users with SQL knowledge
- For simple queries, consider using the standard UI features (Transactions Activity, Analytics Activity)

---

**Last Updated**: Based on Expenso application version with package `com.offline.expenso`

