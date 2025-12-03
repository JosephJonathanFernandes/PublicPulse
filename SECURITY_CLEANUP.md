# 🚨 CRITICAL SECURITY ACTION REQUIRED

## ⚠️ Sensitive Files Currently Exposed

Your repository currently has **sensitive Firebase configuration files** tracked in Git that contain API keys. These files need to be removed from Git history immediately.

### Files That Need Attention:
- ✅ `lib/firebase_options.dart` - Contains Firebase API keys
- ✅ `android/app/google-services.json` - Contains Google Services configuration

## 🔒 Immediate Actions Required

### Option 1: Remove from Git (Keep Local Files) - RECOMMENDED

This removes the files from Git tracking but keeps them on your local machine:

```powershell
# Remove files from Git tracking (but keep them locally)
git rm --cached lib/firebase_options.dart
git rm --cached android/app/google-services.json

# Commit the changes
git add .gitignore
git commit -m "security: remove sensitive Firebase config files from tracking"

# Push to remote
git push origin main
```

### Option 2: Complete History Cleanup (Advanced)

If these files have been in your repository for a while, the API keys are already exposed in Git history. You should:

1. **Rotate your Firebase API keys:**
   - Go to [Firebase Console](https://console.firebase.google.com/)
   - Navigate to Project Settings
   - Under "Your apps", regenerate API keys for each platform
   - Download new configuration files

2. **Remove files from entire Git history:**

   **Using git filter-repo (Recommended):**
   ```powershell
   # Install git-filter-repo first
   # https://github.com/newren/git-filter-repo

   # Create a backup first!
   cd ..
   git clone public_pulse public_pulse_backup
   cd public_pulse

   # Remove sensitive files from history
   git filter-repo --path lib/firebase_options.dart --invert-paths
   git filter-repo --path android/app/google-services.json --invert-paths

   # Force push (this rewrites history!)
   git push origin --force --all
   ```

   **Using BFG Repo-Cleaner (Alternative):**
   ```powershell
   # Download BFG from https://rtyley.github.io/bfg-repo-cleaner/
   
   # Create a list of files to remove
   echo "firebase_options.dart" > files-to-remove.txt
   echo "google-services.json" >> files-to-remove.txt
   
   # Run BFG
   java -jar bfg.jar --delete-files files-to-remove.txt
   
   # Clean up
   git reflog expire --expire=now --all
   git gc --prune=now --aggressive
   
   # Force push
   git push origin --force --all
   ```

### After Cleanup:

1. **Verify files are not tracked:**
   ```powershell
   git ls-files | Select-String -Pattern "firebase_options|google-services"
   ```
   Should return nothing.

2. **Reconfigure Firebase:**
   - Follow instructions in `FIREBASE_SETUP.md`
   - Generate new configuration files
   - DO NOT commit them (they're in .gitignore now)

3. **Verify .gitignore is working:**
   ```powershell
   git status
   ```
   Firebase config files should NOT appear in the list.

## 🛡️ Security Best Practices Going Forward

### 1. Keep Config Files Private
- ✅ `.gitignore` has been updated to exclude all Firebase config files
- ✅ Never commit files containing API keys, tokens, or credentials
- ✅ Use environment variables for sensitive data when possible

### 2. Firebase Security Rules
Make sure your Firebase project has proper security rules:

**Firestore Rules:**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

**Storage Rules:**
```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### 3. Restrict API Keys
In Firebase Console:
1. Go to Project Settings > General
2. Click on your app
3. Under "API keys", restrict them by:
   - Android app package name
   - iOS bundle ID
   - Web domain

### 4. Enable App Check
Firebase App Check helps protect your backend resources from abuse:
1. Go to Firebase Console > App Check
2. Enable for your apps
3. Follow setup instructions

### 5. Monitor API Usage
- Enable Firebase Usage alerts
- Monitor for unusual activity
- Set up budget alerts

## 📋 Quick Checklist

- [ ] Remove sensitive files from Git tracking
- [ ] Update .gitignore (already done)
- [ ] Generate new Firebase configuration files
- [ ] Configure Firebase Security Rules
- [ ] Restrict API keys in Firebase Console
- [ ] Enable Firebase App Check
- [ ] Set up monitoring and alerts
- [ ] Verify files are not in `git status`
- [ ] Push changes to remote repository

## ❓ Need Help?

If you're unsure about any of these steps:
1. Review [SECURITY.md](SECURITY.md)
2. Check [FIREBASE_SETUP.md](FIREBASE_SETUP.md)
3. Open an issue (without sharing sensitive info)

## 📚 Additional Resources

- [Git Guardian - Remove Secrets](https://docs.gitguardian.com/secrets-detection/secrets-detection-engine/detectors)
- [Firebase Security Rules](https://firebase.google.com/docs/rules)
- [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/)
- [git-filter-repo](https://github.com/newren/git-filter-repo)

---

**Remember:** Security is critical. Take the time to do this right! 🔒
