# JEE Test Tracker

A browser-based workspace for logging JEE mock tests, reviewing mistakes, spotting performance trends, and planning what to study next.

**Live app:** [shivaayguptame-droid.github.io/jee-test-tracker](https://shivaayguptame-droid.github.io/jee-test-tracker/)

## What it does

- Records test scores, subject-wise results, percentile, and notes.
- Organizes mistakes and question images by test. Images can be saved first and sent for AI analysis later.
- Shows performance trends, weak areas, recurring mistakes, and study priorities.
- Provides a study planner and can connect to Google Calendar.
- Supports user accounts and cloud sync through Supabase, with each user's data separated by database policies.
- Includes optional AI analysis and a Claude MCP integration when their separately deployed services are configured.

## Use the hosted tracker

1. Open the [live app](https://shivaayguptame-droid.github.io/jee-test-tracker/).
2. Create an account or sign in. Use the same account on each device where you want your cloud data.
3. Add a test from **Add test**. You can enter results manually or use the available import options.
4. Add mistake notes or images. In the test form, choose whether to analyze images now or save them for later.
5. Connect Google Calendar from the Schedule section if you want calendar events in the tracker.

## Run locally

This is a static app. Open `index.html` in a browser to preview the interface. For sign-in, Google OAuth, and other integrations, use the hosted HTTPS URL or serve the files from an HTTPS origin configured in the corresponding provider dashboards. Opening the page as a `file://` URL will not satisfy most OAuth redirect settings.

## Deploy with GitHub Pages

The repository is set up to publish the site from the `main` branch. In GitHub, open **Settings → Pages** and confirm the publishing source is `main` and the repository root (`/`). After changes are committed and pushed to `main`, wait for the Pages deployment to finish, then refresh the site. A hard refresh may be needed to clear an older cached page.

## Connected services

### Supabase

The frontend uses the Supabase project URL and a **publishable** key configured in `index.html`. The publishable key is designed to be used by browser apps; database access must still be protected by correctly configured Row Level Security (RLS) policies. The database schema, auth settings, private `mistake-images` Storage bucket, and Edge Functions must be deployed in Supabase separately.

For a fork or a new deployment:

1. Create/configure a Supabase project and apply the matching schema and RLS policies.
2. Set the project URL and publishable key in the frontend configuration in `index.html`.
3. Add the deployed site URL to Supabase Auth's site URL and redirect URL allowlist. For this repository, the app URL is `https://shivaayguptame-droid.github.io/jee-test-tracker/`.
4. Create the private `mistake-images` Storage bucket and apply user-scoped storage policies before enabling image uploads.
5. Deploy and configure any required Edge Functions separately. The browser calls the `analyze-test` function for AI analysis; Claude MCP server code and secrets are not included in this static frontend repository.

**Never place a Supabase `service_role` key, Google client secret, Gemini/API key, or refresh token in `index.html` or any other public file.** Keep privileged credentials in the appropriate server-side secrets manager. RLS must be enabled and verified for every user-data table and storage object.

### Google Calendar

Google Calendar uses a browser OAuth client ID configured in `index.html`. In Google Cloud, enable the Calendar API, configure the OAuth consent screen, and allow the hosted app's origin. Calendar access also depends on the user's Google account granting permission. Do not put an OAuth client secret in the frontend.

### Claude MCP

The Claude connector is a separately deployed service, not part of GitHub Pages. Configure its server URL, OAuth flow, and server-side secrets in its hosting environment. The MCP backend must validate the signed-in user and enforce user-scoped access for every operation; it must not expose Google refresh tokens or privileged Supabase keys to Claude or the browser.

## Releases

GitHub Actions runs Release Please on pushes to `main`. It reads Conventional Commit messages, then opens or updates a release pull request with the version, changelog, and manifest changes. Merge that release pull request to publish the GitHub release and tag. The manifest normally records the last released version and is maintained by Release Please; do not set it to a planned version by hand.

Examples of commit messages:

- `feat: add deferred mistake image analysis`
- `fix: show enlarged mistake images above the test dialog`
- `docs: add project setup guide`
- `chore: update release workflow`

## Project files

- `index.html` — tracker UI and browser-side application code.
- `oauth/consent.html` — OAuth consent page used by the configured authorization flow.
- `release-please-config.json` — Release Please configuration.
- `.release-please-manifest.json` — last released version, maintained by Release Please.
- `.github/workflows/release-please.yml` — release pull request automation.
- `CHANGELOG.md` — release history generated from commit messages.

## Privacy and security

Test records and mistake images may contain personal study information. Keep RLS enabled and use private storage policies. The frontend publishable key is not a substitute for access policies. Do not commit database secrets, service-role keys, provider API keys, refresh tokens, or private student data.
