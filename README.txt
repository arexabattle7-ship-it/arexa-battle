AREXA BATTLE — FIREBASE MIGRATION BUILD (NOT YET PRODUCTION-READY)

Included panels:
- AREXA_BATTLE_ADMIN.html
- AREXA_BATTLE_PLAYER.html
- Organizer panel file removed; Organizer login link removed from Player header.

Firebase project selected from the supplied HTML configuration:
- Project ID: arexabattle-75a47
- Realtime Database: https://arexabattle-75a47-default-rtdb.firebaseio.com
- Hosting: https://arexabattle-75a47.web.app

IMPORTANT STATUS / HONEST LIMITATIONS
The supplied HTML files are legacy demo panels. Their existing login, sharedData sync, wallet, player signup and coin logic still contain localStorage/shared-database demo behavior. The new rules below deny legacy sharedData access, so these HTML panels must NOT be deployed as a working production app yet. The backend functions are a secure starting layer, not yet wired into every existing UI button. Do not accept real deposits, withdrawals, or payments using this build.

What is included:
- Firebase Hosting configuration.
- Realtime Database rules default-deny direct client writes; private user and wallet reads are owner/admin only.
- Cloud Functions callable endpoints for username reservation, tournament creation/join, withdrawal holds/reviews, admin coin adjustments and room details.
- Admin custom-claim bootstrap script.
- Organizer HTML omitted.

What remains before production:
1. Replace legacy HTML password-hash login with Firebase Authentication (Email/Password or another verified identity flow) and wire username registration/login to the callable username reservation function.
2. Replace legacy arexaBattle/sharedData reads/writes with per-user reads and callable server mutations. The current UI still calls old localStorage/sharedData functions in multiple places.
3. Add the deposit request/review callable flow and a verified payment reconciliation process. A UPI UTR text entry alone does not prove payment.
4. Gate room credentials so only joined players can retrieve them; the included public tournament node is not sufficient for publishing secrets. Move room secrets to a server-controlled private path and add a callable/authorized read.
5. Add App Check, rate limits, password reset UX, account blocking, audit retention, tests and error handling.
6. Deploy Functions and Rules, create the first Firebase Auth user, grant its admin custom claim using the trusted script, then sign out/in to refresh the token.

Setup
1. Install Node.js 20+ and Firebase CLI: npm install -g firebase-tools
2. Sign in: firebase login
3. From this folder run: firebase use arexabattle-75a47
4. Install backend dependencies: cd functions && npm install && cd ..
5. Deploy rules/functions: firebase deploy --only database,functions
6. Deploy Hosting only AFTER the frontend migration checklist above is completed: firebase deploy --only hosting

Do not put service-account JSON files in Hosting or commit them to a public repository. google-services.json is an Android config file and is not used by these web HTML panels.

UI note: Existing HTML/CSS was retained as a reference and only the Organizer login link and Organizer management UI were removed. The UI has not been visually redesigned.


LOGIN SESSION PERSISTENCE UPDATE (2026-11-15)
- Admin panel session is now stored in browser localStorage under its own key.
- Player panel session is stored under a separate key.
- Closing/reopening the app or browser should keep the user logged in on the same device/browser.
- Logout clears only that panel's saved session.
- This is a persistence fix for the existing legacy username/password login. It is NOT Firebase Authentication.
- Sessions are device/browser-specific; clearing app/site data or using another device will require login again.
- Do not treat this as production-grade authentication. The existing legacy demo login and shared-data architecture still need migration to Firebase Auth and secure backend authorization before public launch.
