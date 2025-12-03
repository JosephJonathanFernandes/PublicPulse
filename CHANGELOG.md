# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Planned
- Push notifications for complaint status updates
- Multi-language support
- Dark mode theme
- Complaint categories and filtering
- Rating system for resolved complaints
- Analytics dashboard for citizens
- Export complaint history

## [1.0.0] - 2025-12-03

### Added
- Initial release of PublicPulse
- User authentication (citizen and admin roles)
- Citizen features:
  - Sign up and login
  - File complaints with text descriptions
  - Attach images and documents to complaints
  - View complaint history and status
  - Update account settings
  - Password recovery
- Admin features:
  - View all complaints
  - Update complaint status
  - Manage user complaints
- Firebase integration:
  - Authentication
  - Cloud Firestore for data storage
  - Cloud Storage for file uploads
- Material Design UI
- Real-time complaint status updates

### Security
- Firebase API keys properly configured
- Secure authentication flow
- Protected file uploads
- User data privacy

### Documentation
- Comprehensive README with setup instructions
- Contributing guidelines
- Code of conduct
- Security policy
- Architecture documentation
- License (MIT)

---

## Release Notes Format

### Types of Changes
- **Added** for new features
- **Changed** for changes in existing functionality
- **Deprecated** for soon-to-be removed features
- **Removed** for now removed features
- **Fixed** for any bug fixes
- **Security** for vulnerability fixes

### Version Numbers
- **MAJOR** version for incompatible API changes
- **MINOR** version for new functionality in a backwards compatible manner
- **PATCH** version for backwards compatible bug fixes

[Unreleased]: https://github.com/JosephJonathanFernandes/PublicPulse/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/JosephJonathanFernandes/PublicPulse/releases/tag/v1.0.0
