# My Legal Minder

> AI-powered calendar and task management for legal professionals

My Legal Minder is an intelligent assistant that syncs with your existing calendar ecosystem (Outlook → Apple Calendar), provides natural language interaction for managing your schedule, and exports calendar data in universal ICS format compatible with both iOS and Outlook.

Built specifically for busy attorneys who need seamless calendar management across multiple platforms without the friction of manual data entry.

---

## 🎯 Overview

My Legal Minder solves the friction of managing legal calendars across multiple platforms. It:

- **Syncs** your calendar and reminders from Apple's native apps (which pull from Outlook)
- **Understands** natural language commands like "Schedule a client meeting with Sarah Johnson next Thursday at 2pm"
- **Exports** calendar data as ICS files compatible with both iOS and Outlook
- **Remembers** your scheduling patterns, categories, and preferences
- **Scales** from a small law firm to thousands of users without architectural changes

---

## ✨ Key Features

### Phase 1 (Current MVP)
- ✅ EventKit/RemindersKit integration for native Apple calendar access
- ✅ Conversational AI chatbot powered by Claude 3.5 Sonnet
- ✅ Automatic sync of calendar events and reminders to cloud backend
- ✅ Natural language event creation and modification
- ✅ ICS file generation for cross-platform compatibility
- ✅ Secure Firebase authentication
- ✅ Context-aware responses based on your existing schedule

### Phase 2 (Coming Soon)
- 🔄 Background sync with automatic updates
- 📱 iOS push notifications for reminders
- 🎨 Enhanced UI/UX
- ⏰ Automated daily sync checks
- 📊 Basic analytics dashboard

### Phase 3 (Future)
- 🔌 Real-time WebSocket sync
- 🤝 Multi-user collaborative scheduling
- 📈 AI-powered scheduling suggestions
- 🗣️ Voice input support
- 📧 Email-based event creation

---

## 🏗️ Architecture

### High-Level Design

```
┌─────────────────┐
│   iOS App       │
│  (Swift/SwiftUI)│
│                 │
│  EventKit       │◄──── Syncs from Apple Calendar ◄──── Outlook
│  RemindersKit   │
└────────┬────────┘
         │ JSON over HTTPS
         │ (Firebase Auth)
         ▼
┌─────────────────┐
│  Google Cloud   │
│   Cloud Run     │
│  (FastAPI)      │
├─────────────────┤
│  • Sync API     │
│  • Chat API     │
│  • Export API   │
└────────┬────────┘
         │
    ┌────┴─────┬──────────┐
    ▼          ▼          ▼
┌────────┐ ┌────────┐ ┌──────────┐
│Firebase│ │Cloud   │ │ Claude   │
│Auth    │ │Firestore│ │ API      │
└────────┘ └────────┘ └──────────┘
```

### Technology Stack

**iOS Frontend:**
- Swift 5.9+
- SwiftUI for declarative UI
- EventKit for calendar access
- Combine for reactive programming
- Firebase SDK for authentication

**Backend:**
- Python 3.11
- FastAPI web framework
- Google Cloud Run (serverless containers)
- Cloud Firestore (NoSQL database)
- Anthropic Claude API for conversational AI
- Firebase Authentication

**Infrastructure:**
- Terraform for infrastructure as code
- GitHub Actions for CI/CD
- Docker for containerization
- Google Cloud Platform

---

## 📁 Project Structure

```
mylegalminder/
├── backend/                    # Python FastAPI backend
│   ├── app/
│   │   ├── api/               # API route handlers
│   │   │   ├── sync.py        # Calendar sync endpoints
│   │   │   ├── chat.py        # Chatbot endpoints
│   │   │   └── export.py      # ICS export endpoints
│   │   ├── models/            # Pydantic data models
│   │   ├── services/          # Business logic
│   │   │   ├── firestore.py   # Database operations
│   │   │   ├── claude.py      # Claude API integration
│   │   │   ├── ics_generator.py
│   │   │   └── auth.py        # Firebase auth
│   │   └── utils/             # Helper functions
│   ├── tests/                 # pytest test suite
│   ├── Dockerfile
│   └── requirements.txt
│
├── ios/                       # iOS Swift app
│   └── MyLegalMinder/
│       ├── App/              # App entry point
│       ├── Models/           # Swift data models
│       ├── Services/         # EventKit, API, Auth
│       ├── Views/            # SwiftUI views
│       ├── ViewModels/       # MVVM view models
│       └── Utils/            # Extensions & helpers
│
├── infrastructure/            # Terraform & deployment
│   ├── terraform/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── environments/
│   └── scripts/
│       ├── deploy.sh
│       └── setup-gcloud.sh
│
├── .github/
│   └── workflows/            # CI/CD pipelines
│       ├── backend-ci.yml
│       └── ios-build.yml
│
└── docs/
    ├── API.md               # API documentation
    ├── SETUP.md             # Setup instructions
    └── ARCHITECTURE.md      # Technical architecture
```

---

## 🚀 Quick Start

### Prerequisites

- **iOS Development:**
  - macOS with Xcode 15+
  - iOS 17+ device or simulator
  - Apple Developer account

- **Backend Development:**
  - Python 3.11+
  - Google Cloud account
  - Firebase project
  - Anthropic API key

### Installation

#### 1. Clone the Repository

```bash
git clone https://github.com/jasontbarnesesq/mylegalminder.git
cd mylegalminder
```

#### 2. Backend Setup

```bash
cd backend

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Set up environment variables
cp .env.example .env
# Edit .env with your credentials:
# - PROJECT_ID (Google Cloud project)
# - ANTHROPIC_API_KEY
# - FIREBASE_CREDENTIALS_PATH
```

#### 3. Google Cloud Setup

```bash
# Install Google Cloud CLI
# https://cloud.google.com/sdk/docs/install

# Authenticate
gcloud auth login
gcloud config set project YOUR_PROJECT_ID

# Enable required APIs
gcloud services enable run.googleapis.com
gcloud services enable firestore.googleapis.com
gcloud services enable secretmanager.googleapis.com

# Create Firestore database
gcloud firestore databases create --region=us-central1
```

#### 4. Firebase Setup

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new project (or use existing)
3. Enable Authentication → Email/Password
4. Download `GoogleService-Info.plist` for iOS
5. Download service account JSON for backend
6. Add Firebase configuration to your `.env`

#### 5. Deploy Backend

```bash
# Build and deploy to Cloud Run
cd backend
gcloud run deploy legal-minder-api \
  --source . \
  --region us-central1 \
  --allow-unauthenticated \
  --set-env-vars PROJECT_ID=YOUR_PROJECT_ID

# Note the service URL - you'll need it for iOS app
```

#### 6. iOS Setup

```bash
cd ios/MyLegalMinder

# Install dependencies (if using CocoaPods)
pod install

# Open project
open MyLegalMinder.xcworkspace

# Add GoogleService-Info.plist to Xcode project
# Update API base URL in Config.swift with your Cloud Run URL
```

#### 7. Run the App

1. Select your target device/simulator in Xcode
2. Build and run (⌘+R)
3. Grant calendar and reminders permissions when prompted
4. Sign in with email/password
5. Tap "Sync" to upload your calendar data
6. Start chatting!

---

## 💬 Usage Examples

### Natural Language Commands

**Create Events:**
```
"Schedule a client meeting with John Smith next Thursday at 2pm"
"Add a court appearance on March 5th at 10am in Courtroom 3B"
"Block out tomorrow afternoon for document review"
```

**Create Reminders:**
```
"Remind me to file the motion by Friday"
"Set a high-priority reminder to review the contract"
"Add a task to call opposing counsel"
```

**Query Schedule:**
```
"What do I have scheduled for tomorrow?"
"Am I free on Thursday afternoon?"
"Show me all client meetings this week"
```

**Modify Events:**
```
"Move my 3pm meeting to 4pm"
"Cancel the deposition on Tuesday"
"Add Sarah Johnson to the client meeting attendees"
```

---

## 💰 Cost Estimates

### For 4 Users (Law Firm)

**Google Cloud Infrastructure:**
- Cloud Run: ~$0-2/month (within free tier)
- Firestore: $0/month (within free tier)
- Cloud Storage: $0/month (within free tier)
- Firebase Auth: $0/month (within free tier)
- Cloud Scheduler: $0.30/month
- **Total GCP: ~$0.30-2/month**

**Anthropic Claude API:**
- ~50-200 conversations/month
- Estimated: $2-10/month
- **Total Claude: ~$2-10/month**

**Grand Total: $5-15/month** for entire law firm

### Scalability (1,000 Users)

**Google Cloud:**
- Cloud Run: $50-100/month
- Firestore: $25-50/month
- Other services: $20-30/month
- **Total GCP: ~$100-200/month**

**Claude API:**
- ~20,000-50,000 conversations/month
- Estimated: $200-500/month
- **Total Claude: ~$200-500/month**

**Grand Total: $300-700/month** for 1,000 active users  
**Per-user cost: $0.30-0.70/month**

---

## 🛣️ Roadmap

### ✅ Phase 1: MVP (Weeks 1-4)
- [x] Architecture design
- [x] Backend API structure
- [ ] EventKit/RemindersKit integration
- [ ] Basic chat interface
- [ ] ICS export functionality
- [ ] Firebase authentication
- [ ] Initial deployment

### 🔄 Phase 2: Polish (Weeks 5-7)
- [ ] Background sync
- [ ] Push notifications
- [ ] Enhanced UI/UX
- [ ] Cloud Scheduler integration
- [ ] Monitoring dashboards

### 🚀 Phase 3: Scale (Months 3-6)
- [ ] Microservices split
- [ ] API Gateway integration
- [ ] Advanced caching
- [ ] Load testing
- [ ] Multi-tenant support

---

## 📄 License

Copyright © 2026 Jason T. Barnes, Esq.  
All rights reserved.

This is proprietary software developed for Barnes Law Firm.

---

## 👤 Author

**Jason T. Barnes, Esq.**  
Securities Attorney & Developer  
[jasontbarnes.com](https://jasontbarnes.com)

---

## 📈 Status

![Build Status](https://img.shields.io/badge/build-in%20development-yellow)
![Version](https://img.shields.io/badge/version-1.0.0--alpha-blue)
![License](https://img.shields.io/badge/license-Proprietary-red)

**Current Phase:** Phase 1 (MVP Development)  
**Target Launch:** March 2026  
**Active Users:** 4 (Barnes Law Firm)

---

*Built with ❤️ for legal professionals who value their time*

---

I've created a comprehensive README that covers everything we've discussed. Would you like me to save this to a file you can download, or would you like me to make any adjustments first?
