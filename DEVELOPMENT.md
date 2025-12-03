# Development Guide

## Getting Started with Development

This guide will help you set up your development environment and understand the development workflow for PublicPulse.

## Prerequisites

### Required Software

1. **Flutter SDK** (3.4.4 or higher)
   - Download from [flutter.dev](https://flutter.dev/docs/get-started/install)
   - Add Flutter to your PATH

2. **Dart SDK** (included with Flutter)
   - Verify: `dart --version`

3. **IDE** (choose one):
   - **VS Code** (Recommended)
     - Install Flutter extension
     - Install Dart extension
   - **Android Studio**
     - Install Flutter plugin
     - Install Dart plugin

4. **Platform-specific tools**:
   - **Android**: Android Studio + Android SDK
   - **iOS**: Xcode (macOS only)
   - **Web**: Chrome

5. **Git**
   - For version control

6. **Firebase CLI** (optional but recommended)
   ```bash
   npm install -g firebase-tools
   dart pub global activate flutterfire_cli
   ```

## Initial Setup

### 1. Clone the Repository

```bash
git clone https://github.com/JosephJonathanFernandes/PublicPulse.git
cd PublicPulse
```

### 2. Install Dependencies

```bash
flutter pub get
```

### 3. Configure Firebase

Follow the detailed instructions in [FIREBASE_SETUP.md](FIREBASE_SETUP.md).

Quick setup:
```bash
flutterfire configure
```

### 4. Verify Setup

```bash
flutter doctor
```

Fix any issues reported by Flutter Doctor.

### 5. Run the App

```bash
# On connected device/emulator
flutter run

# On specific device
flutter devices
flutter run -d <device_id>

# Debug mode (default)
flutter run

# Release mode
flutter run --release
```

## Development Workflow

### Branch Strategy

```
main (production)
  └── develop (development)
       ├── feature/feature-name
       ├── fix/bug-name
       └── refactor/refactor-name
```

### Creating a New Feature

1. **Create a branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make changes**
   - Write code
   - Follow coding standards
   - Add comments

3. **Test your changes**
   ```bash
   flutter test
   flutter analyze
   ```

4. **Commit changes**
   ```bash
   git add .
   git commit -m "feat: add your feature description"
   ```

5. **Push and create PR**
   ```bash
   git push origin feature/your-feature-name
   ```

## Code Style

### Dart Formatting

```bash
# Format all files
dart format .

# Check formatting without changing files
dart format --set-exit-if-changed .

# Format specific file
dart format lib/main.dart
```

### Linting

```bash
# Run analyzer
flutter analyze

# Fix auto-fixable issues
dart fix --apply
```

### Best Practices

1. **Use const constructors** when possible
   ```dart
   const Text('Hello') // Good
   Text('Hello')       // Avoid if value is constant
   ```

2. **Meaningful names**
   ```dart
   // Good
   final String userName = 'John';
   
   // Bad
   final String n = 'John';
   ```

3. **Extract widgets**
   ```dart
   // Good - Reusable widget
   class CustomButton extends StatelessWidget {
     // ...
   }
   
   // Bad - Inline widget building method
   Widget _buildButton() { ... }
   ```

4. **Add documentation**
   ```dart
   /// Validates user email format.
   /// 
   /// Returns `true` if email is valid, `false` otherwise.
   bool validateEmail(String email) {
     // ...
   }
   ```

## Testing

### Running Tests

```bash
# All tests
flutter test

# Specific test file
flutter test test/widget_test.dart

# With coverage
flutter test --coverage

# Watch mode (re-run on changes)
flutter test --watch
```

### Writing Tests

**Widget Test Example:**
```dart
testWidgets('Login button should be enabled when form is valid', (tester) async {
  await tester.pumpWidget(const MyApp());
  
  // Find widgets
  final emailField = find.byKey(const Key('email_field'));
  final passwordField = find.byKey(const Key('password_field'));
  final loginButton = find.byKey(const Key('login_button'));
  
  // Enter text
  await tester.enterText(emailField, 'test@example.com');
  await tester.enterText(passwordField, 'password123');
  await tester.pump();
  
  // Verify button is enabled
  expect(tester.widget<ElevatedButton>(loginButton).enabled, true);
});
```

## Debugging

### Debug Mode

```bash
flutter run
```

Features:
- Hot reload: Press `r`
- Hot restart: Press `R`
- Widget inspector: Press `w`
- Performance overlay: Press `P`

### VS Code Debugging

1. Set breakpoints in code
2. Press F5 or click "Run and Debug"
3. Use debug console to inspect variables

### Android Studio Debugging

1. Click bug icon
2. Set breakpoints
3. Use debugger panel

### Common Issues

**Hot reload not working:**
```bash
flutter clean
flutter pub get
flutter run
```

**Build errors:**
```bash
# Clear cache
flutter clean

# Update dependencies
flutter pub upgrade

# Rebuild
flutter run
```

## Firebase Development

### Local Testing

1. Use Firebase Emulator Suite
   ```bash
   firebase emulators:start
   ```

2. Connect app to emulators
   ```dart
   if (kDebugMode) {
     FirebaseFirestore.instance.useFirestoreEmulator('localhost', 8080);
     FirebaseAuth.instance.useAuthEmulator('localhost', 9099);
   }
   ```

### Firestore Queries

```dart
// Get user complaints
final snapshot = await FirebaseFirestore.instance
    .collection('complaints')
    .where('userId', isEqualTo: userId)
    .orderBy('createdAt', descending: true)
    .get();
```

### Storage Upload

```dart
final ref = FirebaseStorage.instance
    .ref()
    .child('complaints/$complaintId/$fileName');
await ref.putFile(file);
final downloadUrl = await ref.getDownloadURL();
```

## Building

### Android

```bash
# Debug APK
flutter build apk --debug

# Release APK
flutter build apk --release

# App Bundle (for Play Store)
flutter build appbundle --release
```

### iOS

```bash
# Debug
flutter build ios --debug

# Release
flutter build ios --release
```

### Web

```bash
flutter build web --release
```

## Performance

### Profile Mode

```bash
flutter run --profile
```

### Performance Overlay

Press `P` in debug mode or:
```bash
flutter run --profile --trace-skia
```

### Optimization Tips

1. Use `const` constructors
2. Avoid rebuilding large widget trees
3. Implement `shouldRebuild` in CustomPainter
4. Use `ListView.builder` for long lists
5. Cache network images

## Useful Commands

```bash
# Clean build
flutter clean

# Update dependencies
flutter pub upgrade

# Check outdated packages
flutter pub outdated

# Generate code (if using build_runner)
flutter pub run build_runner build

# Analyze code
flutter analyze

# Format code
dart format .

# Run tests
flutter test

# Check Flutter installation
flutter doctor -v

# List devices
flutter devices

# Create new widget
# (manually create file in lib/)
```

## Resources

- [Flutter Documentation](https://flutter.dev/docs)
- [Dart Documentation](https://dart.dev/guides)
- [Firebase for Flutter](https://firebase.flutter.dev/)
- [Effective Dart](https://dart.dev/guides/language/effective-dart)
- [Flutter Best Practices](https://flutter.dev/docs/development/best-practices)

## Getting Help

- Check [Documentation](https://github.com/JosephJonathanFernandes/PublicPulse#readme)
- Search [Issues](https://github.com/JosephJonathanFernandes/PublicPulse/issues)
- Ask in [Discussions](https://github.com/JosephJonathanFernandes/PublicPulse/discussions)

---

Happy Coding! 🚀
