# Security Policy

## Supported Versions

Currently supported versions of PublicPulse:

| Version | Supported          |
| ------- | ------------------ |
| 1.0.x   | :white_check_mark: |
| < 1.0   | :x:                |

## Reporting a Vulnerability

We take the security of PublicPulse seriously. If you believe you have found a security vulnerability, please report it to us responsibly.

### How to Report

**DO NOT** open a public GitHub issue for security vulnerabilities.

Instead, please report security vulnerabilities by:

1. **Email**: Send details to the project maintainers through a private channel
2. **GitHub Security Advisory**: Use GitHub's "Security" tab to privately report a vulnerability
3. **Direct Message**: Contact @JosephJonathanFernandes directly on GitHub

### What to Include

When reporting a vulnerability, please include:

- **Type of vulnerability** (e.g., SQL injection, XSS, authentication bypass)
- **Full path of source file(s)** related to the vulnerability
- **Location of the affected source code** (tag/branch/commit or direct URL)
- **Step-by-step instructions** to reproduce the issue
- **Proof-of-concept or exploit code** (if possible)
- **Impact of the vulnerability** - what can an attacker do?
- **Suggested fix** (if you have one)

### What to Expect

After you submit a vulnerability report:

1. **Acknowledgment**: We'll acknowledge receipt within 48 hours
2. **Assessment**: We'll investigate and assess the severity within 7 days
3. **Updates**: We'll keep you informed of our progress
4. **Resolution**: We'll work on a fix and coordinate disclosure timing with you
5. **Credit**: We'll credit you in the security advisory (unless you prefer to remain anonymous)

## Security Best Practices for Users

### Firebase Configuration

1. **Never commit Firebase configuration files** to public repositories
   - `lib/firebase_options.dart`
   - `android/app/google-services.json`
   - `ios/Runner/GoogleService-Info.plist`

2. **Use Firebase Security Rules** to protect your data:
   ```javascript
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       // Example: Only authenticated users can read/write their own data
       match /users/{userId} {
         allow read, write: if request.auth != null && request.auth.uid == userId;
       }
     }
   }
   ```

3. **Enable Firebase App Check** to prevent abuse

4. **Use environment-specific Firebase projects**
   - Development
   - Staging
   - Production

### Authentication

1. **Enable two-factor authentication** on your Firebase account
2. **Use strong, unique passwords**
3. **Regularly rotate API keys** and credentials
4. **Implement rate limiting** on authentication endpoints
5. **Monitor authentication logs** for suspicious activity

### Data Protection

1. **Encrypt sensitive data** before storing in Firestore
2. **Minimize data collection** - only collect what's necessary
3. **Implement proper access controls** using Firebase Security Rules
4. **Regular security audits** of your database rules
5. **Use HTTPS** for all network communications

### Code Security

1. **Keep dependencies updated**
   ```bash
   flutter pub upgrade
   ```

2. **Run security audits**
   ```bash
   flutter analyze
   ```

3. **Review third-party packages** before adding them
4. **Use code signing** for release builds
5. **Obfuscate your code** in production builds

### API Keys and Secrets

1. **Use environment variables** for sensitive data
2. **Never hardcode credentials** in source code
3. **Implement key rotation policies**
4. **Use Firebase Remote Config** for dynamic configuration
5. **Monitor API usage** for anomalies

## Known Security Considerations

### Firebase API Keys

Firebase API keys in `firebase_options.dart` are designed to be included in client apps. However:

- They should be restricted in the Firebase Console
- Configure allowed apps/domains
- Implement Firebase Security Rules
- Use Firebase App Check

### File Uploads

The app allows file uploads. Ensure:

- File size limits are enforced
- File types are validated
- Files are scanned for malware
- Storage rules restrict access appropriately

## Security Updates

We will publish security advisories for:

- Critical vulnerabilities
- High-severity issues
- Security patches and updates

Subscribe to repository notifications to stay informed.

## Compliance

This project aims to comply with:

- GDPR (General Data Protection Regulation)
- Data minimization principles
- User privacy best practices

## Additional Resources

- [Firebase Security Documentation](https://firebase.google.com/docs/rules)
- [OWASP Mobile Security](https://owasp.org/www-project-mobile-security/)
- [Flutter Security Best Practices](https://flutter.dev/docs/deployment/security)

---

**Remember**: Security is a shared responsibility. Stay vigilant and report issues promptly.
