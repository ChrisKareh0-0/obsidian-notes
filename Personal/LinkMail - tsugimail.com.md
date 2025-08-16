## Initial flow
1- the user must sign in to there gmail account
2- when signed in an inbox of the gmail account is shown
3- the main feature of the mail app is to detect the content of the mail, if there are dates it suggests creating an event in the calendar the title is the subject of the email
4- the app should have keyboard shortcuts to navigate like a window manager
5- there's a button in the taskbar called hunting mode, which it creates a table of all the (universities, jobs ... similar applications) so it shows the status of each one of them
6- if the user is subscribed to newsletters, there should be a page that auto summarizes everything based on the user's interest
7- markdown notes
8 - Custom Labels + auto assign
9- every sender - recipient have there contacts saved
10 - Freeform calendar view
11 - High value email check (Auto Delete)
12 - Custom folders with custom automated scripts

## **Frontend - Cross-Platform Strategy**

**Option 1: React Ecosystem (Recommended)**

- **Web**: React + TypeScript + Vite
- **Desktop**: Tauri (Rust + React) - lighter than Electron
- **Mobile**: React Native + Expo

**Option 2: Flutter Hybrid**

- **Mobile + Desktop**: Flutter
- **Web**: React/Vue (Flutter web still has limitations)

## **Backend Architecture**

**Core Services**:

- **API**: Node.js + TypeScript + Fastify/Express
- **Email Engine**: Rust or Go (for performance with IMAP/SMTP)
- **Database**: PostgreSQL + Redis (caching)
- **Real-time**: WebSockets + Server-Sent Events

**Email Infrastructure**:

```
Email Protocols: IMAP, SMTP, POP3
Security: OAuth2, JWT, end-to-end encryption
Search: Elasticsearch or Typesense
File Storage: S3-compatible (attachments)
Push Notifications: FCM (Android), APNs (iOS), WebPush
```

## **Recommended Full Stack**

**Frontend**:

- **Language**: TypeScript
- **Web**: React + Tailwind CSS + Zustand (state)
- **Desktop**: Tauri + React (90% smaller than Electron)
- **Mobile**: React Native + Expo Router
- **Shared**: Shared component library + business logic

**Backend**:

- **API Layer**: Node.js + TypeScript + Prisma ORM
- **Email Service**: Rust (for IMAP/SMTP performance)
- **Database**: PostgreSQL + Redis
- **Queue**: BullMQ (email processing)
- **Search**: Typesense (easier than Elasticsearch)

**Infrastructure**:

- **Hosting**: Railway/Vercel (dev) → AWS/GCP (production)
- **CI/CD**: GitHub Actions
- **Monitoring**: Sentry + LogRocket

## **Why This Stack?**

**Advantages**:

- ~80% code reuse between platforms
- TypeScript everywhere for consistency
- Tauri = native performance, smaller bundle than Electron
- React Native = mature ecosystem, good performance
- Rust email engine = blazing fast IMAP/SMTP handling

**Development Flow**:

1. Start with web app (fastest iteration)
2. Desktop with Tauri (mostly same code)
3. Mobile with React Native (shared logic)

Would you like me to elaborate on any part of this stack, or do you have preferences for specific technologies?