# Contributing to PublicPulse 🤝

First off, thank you for considering contributing to PublicPulse! It's people like you that make PublicPulse such a great tool for connecting citizens with government services.

## Code of Conduct

By participating in this project, you are expected to uphold our [Code of Conduct](CODE_OF_CONDUCT.md).

## How Can I Contribute?

### Reporting Bugs 🐛

Before creating bug reports, please check the existing issues to avoid duplicates. When you create a bug report, include as many details as possible:

**Bug Report Template:**
- **Description**: Clear and concise description of the bug
- **Steps to Reproduce**: Step-by-step instructions
- **Expected Behavior**: What you expected to happen
- **Actual Behavior**: What actually happened
- **Screenshots**: If applicable
- **Environment**:
  - Flutter version
  - Dart version
  - Device/Emulator details
  - OS (Android/iOS version)

### Suggesting Enhancements 💡

Enhancement suggestions are tracked as GitHub issues. When creating an enhancement suggestion, include:

- **Use case**: Why would this enhancement be useful?
- **Proposed solution**: How should it work?
- **Alternatives**: Other solutions you've considered
- **Additional context**: Screenshots, mockups, etc.

### Pull Requests 📝

1. **Fork the Repository**
   ```bash
   git clone https://github.com/YOUR_USERNAME/PublicPulse.git
   cd PublicPulse
   ```

2. **Create a Branch**
   ```bash
   git checkout -b feature/your-feature-name
   # or
   git checkout -b fix/your-bug-fix
   ```

3. **Make Your Changes**
   - Write clean, readable code
   - Follow the existing code style
   - Add comments for complex logic
   - Update documentation if needed

4. **Test Your Changes**
   ```bash
   flutter test
   flutter analyze
   ```

5. **Commit Your Changes**
   ```bash
   git add .
   git commit -m "feat: add amazing feature"
   ```

   **Commit Message Convention:**
   - `feat:` - New feature
   - `fix:` - Bug fix
   - `docs:` - Documentation changes
   - `style:` - Code style changes (formatting, etc.)
   - `refactor:` - Code refactoring
   - `test:` - Adding or updating tests
   - `chore:` - Maintenance tasks

6. **Push to Your Fork**
   ```bash
   git push origin feature/your-feature-name
   ```

7. **Open a Pull Request**
   - Go to the original repository
   - Click "New Pull Request"
   - Select your branch
   - Fill out the PR template

## Development Setup

### Prerequisites
- Flutter SDK (3.4.4+)
- Dart SDK
- Firebase account
- IDE (VS Code or Android Studio)

### Getting Started
```bash
# Clone your fork
git clone https://github.com/YOUR_USERNAME/PublicPulse.git

# Install dependencies
flutter pub get

# Run the app
flutter run
```

## Coding Standards

### Dart Style Guide
- Follow the [Effective Dart](https://dart.dev/guides/language/effective-dart) guidelines
- Use meaningful variable and function names
- Keep functions small and focused
- Add documentation comments for public APIs

### Flutter Best Practices
- Use `const` constructors when possible
- Avoid deeply nested widgets
- Extract reusable widgets into separate files
- Use meaningful widget names

### Example:
```dart
// Good ✅
class UserProfileCard extends StatelessWidget {
  const UserProfileCard({
    super.key,
    required this.userName,
    required this.userEmail,
  });

  final String userName;
  final String userEmail;

  @override
  Widget build(BuildContext context) {
    return Card(
      child: ListTile(
        title: Text(userName),
        subtitle: Text(userEmail),
      ),
    );
  }
}

// Bad ❌
class widget1 extends StatelessWidget {
  String n = "";
  String e = "";
  
  Widget build(c) {
    return Card(child: ListTile(title: Text(n), subtitle: Text(e)));
  }
}
```

## Testing

- Write unit tests for utility functions
- Write widget tests for UI components
- Ensure all tests pass before submitting PR

```bash
# Run all tests
flutter test

# Run specific test
flutter test test/widget_test.dart
```

## Documentation

- Update README.md if you change functionality
- Add inline comments for complex logic
- Update API documentation for public methods
- Include examples for new features

## Firebase Changes

If your contribution involves Firebase changes:
- Document required Firebase configuration
- Update setup instructions in README
- Don't commit Firebase configuration files
- Provide example configuration templates

## Questions?

Feel free to:
- Open an issue for discussion
- Reach out to maintainers
- Check existing issues and PRs

## Recognition

Contributors will be recognized in:
- README.md contributors section
- Release notes
- Project documentation

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

Thank you for contributing to PublicPulse! 🎉
