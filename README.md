# PublicPulse 📱

> Your Concerns, Our Commitment.

[![Flutter](https://img.shields.io/badge/Flutter-3.4.4+-02569B?style=flat&logo=flutter)](https://flutter.dev)
[![Firebase](https://img.shields.io/badge/Firebase-Latest-FFCA28?style=flat&logo=firebase)](https://firebase.google.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

PublicPulse is a modern mobile application that bridges the gap between citizens and government authorities, enabling efficient complaint management and resolution tracking. Built with Flutter and Firebase, it provides a seamless experience for citizens to report issues and administrators to manage them effectively.

## ✨ Features

### For Citizens
- 🔐 **Secure Authentication** - Sign up and login with email/password
- 📝 **File Complaints** - Submit detailed complaints with attachments
- 📸 **Media Support** - Attach images and documents to complaints
- 📊 **Track Status** - Monitor complaint progress in real-time
- 🔔 **Notifications** - Get updates on complaint status changes
- 👤 **Account Management** - Update profile and manage account settings
- 🔒 **Password Recovery** - Reset forgotten passwords easily

### For Administrators
- 🎯 **Complaint Dashboard** - View and manage all complaints
- ✅ **Status Management** - Update complaint status (pending, in-progress, resolved)
- 📈 **Analytics** - Track complaint trends and resolution metrics
- 👥 **User Management** - Manage citizen accounts
- 🔍 **Search & Filter** - Find specific complaints quickly

## 🚀 Getting Started

### Prerequisites

Before you begin, ensure you have the following installed:
- Flutter SDK (3.4.4 or higher)
- Dart SDK (included with Flutter)
- Android Studio / Xcode (for mobile development)
- Firebase account and project
- Git

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/JosephJonathanFernandes/PublicPulse.git
   cd PublicPulse
   ```

2. **Install dependencies**
   ```bash
   flutter pub get
   ```

3. **Configure Firebase**
   
   ⚠️ **Important**: You need to set up your own Firebase project and configuration files.
   
   - Create a Firebase project at [Firebase Console](https://console.firebase.google.com/)
   - Enable Authentication (Email/Password)
   - Enable Cloud Firestore
   - Enable Firebase Storage
   
   Then, add your configuration files:
   - `lib/firebase_options.dart` (use FlutterFire CLI)
   - `android/app/google-services.json`
   - `ios/Runner/GoogleService-Info.plist`
   
   ```bash
   # Install FlutterFire CLI
   dart pub global activate flutterfire_cli
   
   # Configure Firebase
   flutterfire configure
   ```

4. **Run the app**
   ```bash
   flutter run
   ```

## 🏗️ Project Structure

```
lib/
├── main.dart              # Application entry point and main screens
├── firebase_options.dart  # Firebase configuration (not in repo)
└── ...

assets/
└── logo.jpg              # Application logo

android/                  # Android-specific configuration
ios/                      # iOS-specific configuration
web/                      # Web-specific configuration
```

## 🛠️ Technologies Used

- **Frontend**: Flutter & Dart
- **Backend**: Firebase
  - Authentication
  - Cloud Firestore
  - Cloud Storage
- **State Management**: StatefulWidget
- **UI Components**: Material Design

## 📦 Dependencies

```yaml
dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^1.0.6
  firebase_core: ^3.5.0
  firebase_auth: ^5.2.1
  cloud_firestore: ^5.4.2
  intl: ^0.19.0
  file_picker: ^8.1.2
  firebase_storage: ^12.3.2
```

## 🔐 Security

- API keys and sensitive configuration files are excluded from version control
- Firebase security rules should be configured to restrict unauthorized access
- See [SECURITY.md](SECURITY.md) for reporting vulnerabilities

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details on:
- Code of conduct
- Development process
- How to submit pull requests
- Coding standards

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👥 Authors

- **Joseph Jonathan Fernandes** - [@JosephJonathanFernandes](https://github.com/JosephJonathanFernandes)

## 🙏 Acknowledgments

- Flutter team for the amazing framework
- Firebase for backend infrastructure
- All contributors who help improve PublicPulse

## 📞 Support

For support, please:
- Open an issue on GitHub
- Check existing documentation
- Review closed issues for solutions

## 🗺️ Roadmap

- [ ] Push notifications
- [ ] Multi-language support
- [ ] Dark mode
- [ ] Complaint categories
- [ ] Rating system for resolved complaints
- [ ] Analytics dashboard for citizens
- [ ] Export complaint history

## 📱 Screenshots

_Coming soon - Screenshots will be added in future updates_

---

**Made with ❤️ for better citizen-government communication**
