# Firebase Configuration Instructions

## ⚠️ Important Security Notice

**NEVER commit actual Firebase configuration files to version control!**

The following files contain sensitive API keys and should be kept private:
- `lib/firebase_options.dart`
- `android/app/google-services.json`
- `ios/Runner/GoogleService-Info.plist`
- `macos/Runner/GoogleService-Info.plist`

These files are already added to `.gitignore` to prevent accidental commits.

## Setup Instructions

### Step 1: Create a Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Add project" or select an existing project
3. Follow the setup wizard
4. Enable Google Analytics (optional but recommended)

### Step 2: Enable Required Services

In your Firebase project, enable:

1. **Authentication**
   - Go to Authentication → Sign-in method
   - Enable "Email/Password"
   - Configure email templates (optional)

2. **Cloud Firestore**
   - Go to Firestore Database
   - Create database
   - Choose "Start in production mode"
   - Select a location (choose closest to your users)

3. **Cloud Storage**
   - Go to Storage
   - Get started
   - Choose "Start in production mode"
   - Use default storage location

### Step 3: Configure Firebase for Flutter

#### Option A: Using FlutterFire CLI (Recommended)

```bash
# Install FlutterFire CLI
dart pub global activate flutterfire_cli

# Login to Firebase
firebase login

# Configure Firebase for your Flutter project
flutterfire configure

# Follow the prompts:
# 1. Select your Firebase project
# 2. Select platforms (Android, iOS, Web, macOS, Windows)
# 3. Enter your package names/bundle IDs
```

This will automatically:
- Generate `lib/firebase_options.dart`
- Download platform-specific config files
- Set up your project correctly

#### Option B: Manual Configuration

##### For Android

1. In Firebase Console, add an Android app
2. Enter your package name: `com.example.publicpulse_2`
   - Find this in `android/app/build.gradle` under `applicationId`
3. Download `google-services.json`
4. Place it in `android/app/`
5. Ensure `android/build.gradle` has:
   ```gradle
   classpath 'com.google.gms:google-services:4.3.15'
   ```
6. Ensure `android/app/build.gradle` has:
   ```gradle
   apply plugin: 'com.google.gms.google-services'
   ```

##### For iOS

1. In Firebase Console, add an iOS app
2. Enter your bundle ID: `com.example.publicpulse2`
   - Find this in Xcode or `ios/Runner/Info.plist`
3. Download `GoogleService-Info.plist`
4. Open `ios/Runner.xcworkspace` in Xcode
5. Drag `GoogleService-Info.plist` into the project (Runner folder)
6. Ensure "Copy items if needed" is checked

##### For Web

1. In Firebase Console, add a Web app
2. Copy the configuration
3. Update `web/index.html` with Firebase SDK scripts
4. Or use FlutterFire CLI to generate `lib/firebase_options.dart`

##### For macOS

1. In Firebase Console, add a macOS app
2. Enter bundle ID
3. Download `GoogleService-Info.plist`
4. Place in `macos/Runner/`

### Step 4: Configure Security Rules

#### Firestore Rules

Go to Firestore → Rules and update:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Helper function to check if user is authenticated
    function isAuthenticated() {
      return request.auth != null;
    }
    
    // Helper function to check if user is admin
    function isAdmin() {
      return isAuthenticated() && 
             get(/databases/$(database)/documents/users/$(request.auth.uid)).data.userType == 'admin';
    }
    
    // Users collection
    match /users/{userId} {
      allow read: if isAuthenticated();
      allow write: if isAuthenticated() && request.auth.uid == userId;
    }
    
    // Complaints collection
    match /complaints/{complaintId} {
      allow create: if isAuthenticated();
      allow read: if isAuthenticated();
      allow update: if isAuthenticated() && 
                     (resource.data.userId == request.auth.uid || isAdmin());
      allow delete: if isAdmin();
    }
    
    // Admin collection
    match /admin/{adminId} {
      allow read, write: if isAdmin();
    }
  }
}
```

#### Storage Rules

Go to Storage → Rules and update:

```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    
    // Complaints attachments
    match /complaints/{complaintId}/{fileName} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && 
                    request.resource.size < 10 * 1024 * 1024 && // 10MB max
                    request.resource.contentType.matches('image/.*|application/pdf');
      allow delete: if request.auth != null;
    }
  }
}
```

### Step 5: Set Up Firestore Indexes

Some queries may require composite indexes. Firebase will provide a link to create them when needed.

Common indexes for PublicPulse:

1. **Complaints by user and date**
   - Collection: `complaints`
   - Fields: `userId` (Ascending), `createdAt` (Descending)

2. **Complaints by status and date**
   - Collection: `complaints`
   - Fields: `status` (Ascending), `createdAt` (Descending)

### Step 6: Verify Configuration

Run your app and test:

```bash
flutter run
```

Check for:
- Successful Firebase initialization
- Authentication working
- Firestore read/write operations
- File uploads to Storage

### Troubleshooting

#### Common Issues

1. **"Default FirebaseApp is not initialized"**
   - Ensure `Firebase.initializeApp()` is called in `main()`
   - Check that config files are in correct locations

2. **"Permission denied" errors**
   - Review Firestore/Storage security rules
   - Ensure user is authenticated before accessing data

3. **Build errors on Android**
   - Check Google Services plugin is applied
   - Ensure `google-services.json` is in `android/app/`
   - Run `flutter clean` and rebuild

4. **Build errors on iOS**
   - Ensure `GoogleService-Info.plist` is added to Xcode project
   - Clean build folder in Xcode
   - Run `pod install` in `ios/` directory

### Environment-Specific Setup

For different environments (dev, staging, prod):

1. Create separate Firebase projects
2. Use different configuration files
3. Switch configurations based on build flavor

```bash
# Example: Different configs for different environments
lib/
  firebase_options_dev.dart
  firebase_options_staging.dart
  firebase_options_prod.dart
```

## Security Checklist

- [ ] API keys restricted in Firebase Console
- [ ] Configured allowed apps/domains
- [ ] Security rules properly configured
- [ ] Storage rules limit file sizes and types
- [ ] Firebase App Check enabled (recommended)
- [ ] Authentication email templates customized
- [ ] Sensitive data encrypted
- [ ] Regular security audits scheduled

## Resources

- [FlutterFire Documentation](https://firebase.flutter.dev/)
- [Firebase Console](https://console.firebase.google.com/)
- [Firebase Security Rules](https://firebase.google.com/docs/rules)
- [FlutterFire CLI](https://firebase.flutter.dev/docs/cli/)

---

**Need Help?** Open an issue on GitHub or check existing issues for solutions.
