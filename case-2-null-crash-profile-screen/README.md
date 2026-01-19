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

## Problematic Code

```dart
import 'package:flutter/material.dart';

class ProfileScreen extends StatelessWidget {
  final Map<String, dynamic>? userData;

  const ProfileScreen({super.key, this.userData});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Profile")),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              userData!['name'], // ❌ CRASH HERE
              style: const TextStyle(fontSize: 20),
            ),
            const SizedBox(height: 10),
            Text(userData!['email']), // ❌ CRASH HERE
          ],
        ),
      ),
    );
  }
}

```


### Fixed Code Snippet
```dart
import 'package:flutter/material.dart';

class ProfileScreen extends StatelessWidget {
  final Map<String, dynamic>? userData;

  const ProfileScreen({super.key, this.userData});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Profile")),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: userData == null
            ? const Center(
                child: CircularProgressIndicator(),
              )
            : Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    userData!['name'] ?? 'Unknown User',
                    style: const TextStyle(fontSize: 20),
                  ),
                  const SizedBox(height: 10),
                  Text(userData!['email'] ?? 'No email'),
                ],
              ),
      ),
    );
  }
}

```

## 📸 Before
![Crash Screenshot](screenshots/before.png)

## 📸 After
![Fixed Screenshot](screenshots/after.png)
