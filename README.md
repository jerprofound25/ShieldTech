# ShieldTech — GitHub Releases + Cloudflare Pages (no billing setup)

This version does **not** use Firebase Storage. APK files live as public assets attached to GitHub Releases. Firebase Authentication and Firestore are used for the owner-only publisher session, catalogue metadata, and basic analytics. Check the current Firebase Spark-plan quotas before launch.

## Files
- `index.html`: complete responsive site, catalogue, publisher form, install instructions, privacy section, and optional consent-based location analytics.
- `firestore.rules`: starter Firestore rules. Replace `webjer25@gmail.com` with your verified publisher email before publishing the rules.

## 1. Create Firebase project without enabling billing
1. Open https://console.firebase.google.com/ and create a new project.
2. Select the no-cost Spark plan; do not upgrade to Blaze or attach billing.
3. Register a Web app and copy its Firebase web config into `index.html`'s `firebaseConfig` object.
4. Replace `webjer25@gmail.com` in `index.html` with your publisher email.
5. In Authentication, enable Email/Password, create your user, and verify the email address.
6. Create Firestore Database, open Rules, paste `firestore.rules`, replace the email there, and publish the rules.
7. Do not enable Firebase Storage; this project doesn't need it.

## 2. Upload an APK to a GitHub Release
1. Open your GitHub repository and choose Releases → Draft a new release.
2. Create a tag such as `v1.0.0` and add release notes.
3. Attach the `.apk` as a release asset and publish the release.
4. Open the published release asset and copy its direct URL. It should look like `https://github.com/OWNER/REPO/releases/download/v1.0.0/app-name.apk`.
5. Sign in to the ShieldTech Publisher area and paste that URL into the form with the app details. Only your verified admin account can create listings under the Firestore rules.

GitHub release assets must be public for visitors to download without a GitHub login. GitHub has upload limits and policies; check the current limit for your account/release.

## 3. Deploy on Cloudflare Pages
1. Push `index.html` and `firestore.rules` to a GitHub repository. Keep `index.html` in the repository root.
2. In Cloudflare, open Workers & Pages → Create → Pages → Connect to Git.
3. Select the repository. Framework preset: None. Build command: leave blank. Build output directory: `.`
4. Deploy. Cloudflare will redeploy on commits.

## Important security notes
- The Firebase web config is client-side config, not a private key. Never commit service-account keys or passwords.
- Firebase's client-side admin-email check only affects the UI; Firestore rules enforce owner-only publishing.
- Analytics writes are public and may be spammed. This starter is suitable for initial testing, not high-volume production analytics. Add App Check / abuse protection and retention controls before broad promotion.
- Location is only requested after a visitor actively taps the location-sharing button. Do not enable covert location collection.
- Website JavaScript cannot grant Android permissions. Android shows the actual install and permission prompts.
- A GitHub-hosted APK link is not proof the APK is safe. Scan and test files before publishing.
