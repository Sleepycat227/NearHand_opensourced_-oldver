# NearHand 🤝
### "Help is always nearby 💛"

A community app connecting elderly people with helpers and volunteers for daily needs, errands, and companionship — with built-in medication reminders.

---

## Setup Instructions

### 1. Firebase Setup (Required)

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new project named **NearHand**
3. Add an Android app with package name: `com.nearhand.app`
4. Download `google-services.json` and place it in `/app/` folder
5. Enable the following in Firebase Console:
   - **Authentication** → Email/Password sign-in
   - **Firestore Database** → Start in test mode (for development)
   - **Cloud Messaging** (auto-enabled)

### 2. Firestore Security Rules (for production)

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{uid} {
      allow read, write: if request.auth.uid == uid;
    }
    match /requests/{requestId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null;
      allow update: if request.auth != null;
    }
    match /medications/{medId} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### 3. Build in Android Studio

1. Open the `NearHand` folder in Android Studio
2. Let Gradle sync
3. Run on device or emulator (API 24+)

---

## App Structure

```
NearHand/
├── auth/
│   ├── SplashActivity      — Auto-routes by login state & role
│   ├── LoginActivity       — Email/password login
│   └── RegisterActivity    — Register with role selection (Elder / Helper)
│
├── elder/
│   ├── ElderHomeActivity   — Dashboard with 3 action cards
│   ├── PostRequestActivity — Post a help request (type + description)
│   └── MyRequestsActivity  — View all past/active requests
│
├── helper/
│   ├── HelperHomeActivity  — Browse open requests + My accepted tab
│   ├── RequestDetailActivity — Accept a request
│   └── RequestsAdapter     — Shared RecyclerView adapter
│
├── medication/
│   ├── MedicationListActivity  — View all med reminders
│   ├── AddMedicationActivity   — Set a new reminder (with TimePicker)
│   ├── MedicationAdapter       — RecyclerView for meds
│   ├── MedicationAlarmReceiver — Fires the notification
│   └── MedicationAlarmScheduler— Schedules/cancels AlarmManager alarms
│
├── model/
│   └── Models.kt           — User, HelpRequest, Medication, enums
│
└── utils/
    ├── FirebaseRepository  — All Firestore/Auth operations
    └── NearHandMessagingService — FCM token refresh & push
```

## Request Types
- 🛒 Run an Errand
- 🦵 Leg Massage
- 🤗 Quick Hug
- 💬 Just Chat
- 💊 Help with Medicine
- 🙏 Other

## Color Palette
- Primary: `#F5A623` (Warm Amber)
- Accent:  `#4CAF87` (Calm Green)
- Background: `#FAFAFA`

---

## Next Steps / Future Features
- [ ] Push notification to helpers when new request is posted
- [ ] Elder ↔ Helper linking system (family accounts)
- [ ] In-app chat between elder and helper
- [ ] Request location / address field
- [ ] Rating & feedback after completed requests
- [ ] Caregiver dashboard to monitor multiple elders
