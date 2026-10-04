# Grade 6 Maths — Barakat Private Language School

This is the English Grade 6 Maths quiz platform for the language classes **6A** and **6B** at **Barakat Private Language School**. It is branded as Grade 6 Maths and built by VSOS — Saad El Labban.

## Current product behavior

The site contains a 20-question Maths practice quiz, a 20-minute timer, student name and class selection, per-class leaderboard, PWA installation, and an admin portal entry point. There are no seeded student names, fake scores, demo credentials, or placeholder content. The leaderboard stays empty until real results are recorded.

Without Firebase configuration, student profiles and results are saved only in the browser used to take the quiz. This is intentionally shown as local device storage and is not presented as a shared school database.

## Production Firebase setup

1. Create a Firebase Web App for the school.
2. Enable Firebase Authentication with Email/Password and create the teacher admin account.
3. Create Firestore and deploy `firebase.rules`.
4. Put only the Firebase Web App configuration in `firebase-config.js` as `window.FIREBASE_CONFIG`.
5. Give the teacher account the `admin: true` custom claim from a trusted server environment. Never put a service-account key in this repository.
6. Enable GitHub Pages for `main` and the repository root.

The web configuration values are not service-account secrets. Access control comes from Firebase Authentication and Firestore Rules. The Admin portal refuses access when Firebase is not configured; there is no hard-coded fallback password.

## Firebase collections

- `students/{uid}` stores the student profile for the authenticated anonymous user.
- `scores/{scoreId}` stores a submitted score with the class and student UID.
- `quizzes/{quizId}` stores quizzes published by an authenticated admin.

The current front end keeps a local copy for offline use. Connect a school Firebase project before treating the leaderboard as a shared online record.

## PWA and notifications

`manifest.webmanifest` provides the installable app name **Grade 6 Maths**. `sw.js` caches the app shell. Browser notifications can be extended with Firebase Cloud Messaging and a trusted Cloud Function; server keys must never be placed in the client repository.
