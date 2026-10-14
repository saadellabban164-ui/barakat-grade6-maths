# Grade 6 Maths — Barakat Private Language School

This is the English Grade 6 Maths quiz platform for the language classes **6A** and **6B** at **Barakat Private Language School**. It is branded as Grade 6 Maths and built by VSOS — Saad El Labban.

## Current product behavior

The site provides Username + Password student accounts, class selection, per-class leaderboard, teacher-published quizzes, per-quiz timer settings, PWA installation, and a protected Admin Portal. There are no seeded student names, fake scores, demo credentials, or placeholder content.

## Student accounts

Students use two buttons: `Create account` and `Sign in`.

- Create account requires full name, username, password, and class.
- Sign in requires username and password.
- Usernames are normalized to lowercase and converted internally to a Firebase Auth login ID ending in `@students.grade6.local`; students never need to enter or manage an email address.
- Usernames must contain only letters, numbers, `.`, `_`, or `-`, and be 3–30 characters long.
- Passwords are handled by Firebase Authentication and are never stored in Firestore or localStorage.
- Student profile data is stored in `students/{uid}`.

## Production Firebase setup

1. Create the Firebase Web App for project `grade-6-maths-b64f4`.
2. Enable **Authentication → Sign-in method → Email/Password**. Keep Anonymous disabled for this version.
3. Create Firestore Database in Production mode. The selected `me-central1` Doha region is suitable.
4. Publish the rules in `firebase.rules`.
5. Put only the Firebase Web App configuration in `firebase-config.js`. Never put a service-account JSON or Admin SDK secret in the repository.
6. Deploy the repository through the existing GitHub Pages workflow.

The web configuration values are expected to be visible in browser JavaScript. Access control comes from Firebase Authentication and Firestore Rules.

## Admin Portal setup

1. In **Authentication → Users**, create the teacher account with an email and a strong password.
2. Copy the teacher user's **UID**.
3. In **Firestore Database → Data**, create a collection named `admins`.
4. Create a document whose document ID is exactly the teacher UID.
5. Add a field such as:

```text
role: admin
name: Ms. Shaimaa Saad
```

6. Do not try to create the `admins` document from the website. The rules intentionally block browser writes to this collection.
7. Open the website, choose **Admin portal**, and sign in with the teacher email and password.

The portal accepts either this protected `admins/{uid}` document or a trusted Firebase custom claim named `admin: true`. No admin password is hard-coded in the repository.

## Firestore collections

- `students/{uid}` stores the authenticated student's name, username, and class.
- `scores/{scoreId}` stores a submitted score with class and authenticated UID.
- `quizzes/{quizId}` stores teacher-published quiz metadata.
- `admins/{uid}` grants Admin Portal access when created manually by the project owner.

Students can read published quizzes, create their own profile, and submit a validated score. Only authenticated users can read the leaderboard. Only an admin can create, update, or delete quizzes and manage scores.

## Security notes

The browser quiz cannot prove that a client did not tamper with answers. The rules validate score shape, class, score range, name length, and UID ownership, but a tamper-resistant exam requires a trusted Cloud Function to calculate results from submitted answers. Do not place service-account keys, admin passwords, or Firebase Admin SDK credentials in GitHub.

## PWA and notifications

`manifest.webmanifest` provides the installable app name **Grade 6 Maths**. `sw.js` caches the app shell. Browser notifications can be extended later with Firebase Cloud Messaging and a trusted Cloud Function; server keys must never be placed in the client repository.
