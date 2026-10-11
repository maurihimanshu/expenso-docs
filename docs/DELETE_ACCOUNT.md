# 🗑️ Delete Account and Data — Expenso

## About Expenso
**Expenso** is a privacy-first, completely **offline personal finance manager** that enables users to track income, expenses, budgets, reminders, statements, and analytics securely on their own device.  
All user data, including account credentials and financial records, are stored **strictly locally** inside an encrypted **SQLCipher database** (`expenso_secure.db`).  
We **do not collect**, **store**, or **transmit** any user data to external servers or cloud providers.

---

## 🔐 How to Delete Your Account and Data

Because all data resides exclusively on your local device, deleting your account is accomplished by **erasing your local encrypted data**.  
You can do this through the following options:

### Option 1 — In-App Reset (Recommended)
1. Open **Expenso** and log in.
2. Navigate to **Settings → Profile** (or **Settings → Data Management**).
3. Tap **Clear All Data / Delete Account**.
4. Confirm your master password or biometric authentication to permanently erase the local database and cryptographic keys.

### Option 2 — Clear App Storage via Android Settings
1. Open Android **Settings → Apps → Expenso**.
2. Tap **Storage & cache**.
3. Tap **Clear storage** (or **Clear data**).
4. Confirm — this immediately deletes the encrypted database file, all Keystore-backed shared preferences, and cached session tokens.

### Option 3 — Uninstall the App
1. Long-press the **Expenso** app icon on your home screen or app drawer.
2. Tap **Uninstall**.
3. Confirm — Android will permanently remove the application package along with its private encrypted storage sandbox (`/data/data/com.offline.expenso`).

---

## 🧾 What Data Is Permanently Erased

When your local data is cleared or the app is uninstalled, all of the following records are **permanently and irreversibly destroyed**:
- **User Credentials**: Email, hashed passwords (PBKDF2-HMAC-SHA256), encrypted phone number, recovery phrases, and profile settings.
- **Transactions**: All itemized income, expense, and transfer records.
- **Budgets & History**: Active budgets, spending limits, thresholds, and archived historical period performance.
- **Payment Reminders**: Scheduled reminders, recurring schedules, and reminder history logs.
- **Saved Queries**: Custom developer/power-user SQL queries stored in Safe Query Runner.
- **Configurations & Categories**: Custom categories, payment methods, and user ordering preferences.
- **Cryptographic Keys**: Android Hardware Keystore keys and isolated SQLCipher database passphrases.

---

## ⚠️ What Data Might Remain on Your Device

If you previously exported manual backups or generated PDF financial statements via the Storage Access Framework (SAF), those exported files remain in your device's storage until you delete them manually:
1. Open your device's **Files / File Manager** app.
2. Navigate to your selected export directory (e.g. **Documents/Expenso** or **Downloads**).
3. Search for:
   - Backup archives: `Expenso_Backup_*.zip` or `*.csv`
   - Statements: `Expenso_Statement_*.pdf`
4. Select and delete these files permanently.

---

## 🕒 Data Retention Policy

Expenso maintains **zero servers, zero cloud synchronization, and zero remote backups**.  
Once data is erased from your device, it is **permanently gone** and cannot be recovered by anyone, including the developer.

---

## 💬 Support & Inquiries

If you need guidance on erasing app data or understanding Expenso's privacy model, contact:

📧 **Email:** [himanshumauri@gmail.com](mailto:himanshumauri@gmail.com)

---

## ✅ Developer & Privacy Summary
- **Developer:** Himanshu Kumar
- **Package Name:** `com.offline.expenso`
- **Data Storage:** 100% Local (SQLCipher AES-256 encrypted database)
- **Data Transmission:** None (zero internet permissions)

---

*Last updated: 2026*
