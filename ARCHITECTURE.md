# PublicPulse Architecture

## Overview

PublicPulse is a Flutter-based mobile application that connects citizens with government authorities for complaint management. This document outlines the architecture, design decisions, and technical implementation details.

## Technology Stack

### Frontend
- **Framework**: Flutter 3.4.4+
- **Language**: Dart
- **UI**: Material Design
- **State Management**: StatefulWidget (Future: Consider Provider/Riverpod)

### Backend
- **Authentication**: Firebase Authentication
- **Database**: Cloud Firestore
- **Storage**: Firebase Storage
- **Platform**: Firebase

## System Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Flutter Application                      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   UI Layer   │  │ State Mgmt   │  │   Services   │      │
│  │  (Widgets)   │─▶│ (StatefulW)  │─▶│  (Firebase)  │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└────────────────────────────┬────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────┐
│                      Firebase Services                       │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │     Auth     │  │  Firestore   │  │   Storage    │      │
│  │              │  │  (Database)  │  │   (Files)    │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────────────────────────────────────────┘
```

## Application Flow

### 1. Authentication Flow

```
Splash Screen
    │
    ▼
User Type Selection
    │
    ├─▶ Citizen Login ──▶ Citizen Signup
    │       │                  │
    │       └──────────────────┘
    │              │
    │              ▼
    │      Firebase Auth
    │              │
    │              ▼
    │       Citizen Home
    │
    └─▶ Admin Login
            │
            ▼
        Admin Home
```

### 2. Complaint Management Flow

```
Citizen Home
    │
    ▼
Citizen Options
    │
    ├─▶ File Complaint
    │       │
    │       ├─▶ Enter Details
    │       ├─▶ Attach Files (Optional)
    │       ├─▶ Submit to Firestore
    │       └─▶ Upload to Storage
    │
    ├─▶ View My Complaints
    │       │
    │       └─▶ Real-time Updates
    │
    └─▶ Account Settings
            │
            └─▶ Update Profile
```

### 3. Admin Flow

```
Admin Home
    │
    ├─▶ View All Complaints
    │       │
    │       ├─▶ Filter by Status
    │       ├─▶ Search Complaints
    │       └─▶ View Details
    │
    └─▶ Manage Complaint
            │
            ├─▶ Update Status
            ├─▶ Add Comments
            └─▶ Mark as Resolved
```

## Data Models

### User Model
```dart
{
  "uid": String,
  "email": String,
  "name": String,
  "phone": String,
  "address": String,
  "userType": "citizen" | "admin",
  "createdAt": Timestamp
}
```

### Complaint Model
```dart
{
  "complaintId": String,
  "userId": String,
  "userName": String,
  "title": String,
  "description": String,
  "category": String,
  "location": String,
  "status": "pending" | "in-progress" | "resolved",
  "attachments": [
    {
      "fileName": String,
      "fileUrl": String,
      "uploadedAt": Timestamp
    }
  ],
  "createdAt": Timestamp,
  "updatedAt": Timestamp,
  "resolvedAt": Timestamp?,
  "adminNotes": String?
}
```

## Firebase Structure

### Firestore Collections

```
/users
  /{userId}
    - uid
    - email
    - name
    - phone
    - address
    - userType

/complaints
  /{complaintId}
    - complaintId
    - userId
    - userName
    - title
    - description
    - category
    - location
    - status
    - attachments[]
    - createdAt
    - updatedAt
    - resolvedAt
    - adminNotes

/admin
  /{adminId}
    - uid
    - email
    - name
    - role
```

### Storage Structure

```
/complaints
  /{complaintId}
    /{fileName}
```

## Security Rules

### Firestore Rules (Example)

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Users can only read/write their own data
    match /users/{userId} {
      allow read, write: if request.auth != null && 
                         request.auth.uid == userId;
    }
    
    // Citizens can create and read their own complaints
    match /complaints/{complaintId} {
      allow create: if request.auth != null;
      allow read: if request.auth != null;
      allow update, delete: if request.auth != null && 
                             (resource.data.userId == request.auth.uid ||
                              isAdmin());
    }
    
    // Admin collection
    match /admin/{adminId} {
      allow read, write: if request.auth != null && isAdmin();
    }
    
    function isAdmin() {
      return get(/databases/$(database)/documents/users/$(request.auth.uid))
             .data.userType == 'admin';
    }
  }
}
```

### Storage Rules (Example)

```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /complaints/{complaintId}/{fileName} {
      // Allow upload if authenticated
      allow create: if request.auth != null;
      
      // Allow read if authenticated
      allow read: if request.auth != null;
      
      // Allow delete only by admins or complaint owner
      allow delete: if request.auth != null;
    }
  }
}
```

## Design Patterns

### Current Implementation

1. **StatefulWidget Pattern**: Used for managing local state
2. **Callback Pattern**: For handling user interactions
3. **Builder Pattern**: For constructing complex UI

### Future Improvements

1. **Repository Pattern**: Separate data access logic
2. **BLoC/Provider**: Better state management
3. **Dependency Injection**: Improved testability
4. **MVVM Architecture**: Clear separation of concerns

## Code Organization

```
lib/
├── main.dart                    # Entry point & main screens
├── firebase_options.dart        # Firebase configuration
│
├── models/                      # (Future) Data models
│   ├── user_model.dart
│   ├── complaint_model.dart
│   └── admin_model.dart
│
├── services/                    # (Future) Business logic
│   ├── auth_service.dart
│   ├── complaint_service.dart
│   └── storage_service.dart
│
├── screens/                     # (Future) UI screens
│   ├── splash/
│   ├── auth/
│   ├── citizen/
│   └── admin/
│
├── widgets/                     # (Future) Reusable widgets
│   ├── common/
│   └── custom/
│
└── utils/                       # (Future) Utilities
    ├── constants.dart
    ├── validators.dart
    └── helpers.dart
```

## Performance Considerations

### Current
- Direct Firestore queries in widgets
- Real-time listeners for data updates
- Image compression before upload

### Optimization Opportunities
1. **Pagination**: Load complaints in batches
2. **Caching**: Cache frequently accessed data
3. **Lazy Loading**: Load images on demand
4. **Query Optimization**: Use compound indexes
5. **Connection Management**: Handle offline scenarios

## Testing Strategy

### Unit Tests
- Service layer logic
- Data model validation
- Utility functions

### Widget Tests
- Individual widget behavior
- User interaction flows
- Form validation

### Integration Tests
- Complete user flows
- Firebase integration
- End-to-end scenarios

## Deployment

### Development
```bash
flutter run
```

### Production Build

**Android**:
```bash
flutter build apk --release
flutter build appbundle --release
```

**iOS**:
```bash
flutter build ios --release
```

## Monitoring & Analytics

### Recommended Tools
- Firebase Analytics
- Firebase Crashlytics
- Firebase Performance Monitoring
- Cloud Logging

## Future Enhancements

### Short-term
1. Push notifications
2. Complaint categories
3. Location-based services
4. Image optimization

### Long-term
1. AI-powered complaint routing
2. Multi-language support
3. Analytics dashboard
4. Offline mode
5. Progressive Web App
6. Admin analytics
7. Complaint priority levels
8. Email notifications

## Contributing

When contributing to the architecture:

1. Follow existing patterns
2. Document new patterns
3. Update this document
4. Discuss major changes in issues

## Resources

- [Flutter Architecture](https://flutter.dev/docs/development/data-and-backend/state-mgmt/options)
- [Firebase Best Practices](https://firebase.google.com/docs/firestore/best-practices)
- [Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)

---

**Last Updated**: December 2025
