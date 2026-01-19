# Case 2: App Crash Due to Null Data on Profile Screen

## 🔴 Problem
The app crashed with a "Null check operator used on a null value" error when navigating to the profile screen.
This issue occurred when user data had not yet been loaded but the UI attempted to access it.

## 🔍 Root Cause
The profile screen used the null check operator (!) on a nullable user data object.
When the screen was built before the data was available, the forced null assertion caused the app to crash.

## 🛠 Fix
Added proper null handling by checking if the user data is available before building the UI.
A loading indicator is shown until the data is ready, and fallback values are used to prevent crashes.

## ✅ Result
The profile screen no longer crashes.
The app now safely handles delayed or missing data and provides a stable user experience.

## 📸 Before
![Crash Screenshot](screenshots/before.png)

## 📸 After
![Fixed Screenshot](screenshots/after.png)
