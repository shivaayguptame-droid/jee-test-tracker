# Contributing

Thanks for helping improve JEE Test Tracker. Issues and pull requests should be clear, respectful, and focused on one change at a time.

## Before opening an issue

- Search existing issues and discussions for the same problem or idea.
- Include the tracker version, exam track (JEE or NEET), browser/device, and steps to reproduce a bug.
- Remove names, email addresses, test data, screenshots with private information, access tokens, and API keys from anything you share.

## Pull requests

1. Make a focused change and explain the user problem it addresses.
2. Keep unrelated UI and behavior unchanged.
3. Describe how you checked the change. For UI changes, include before/after screenshots when useful, with private information hidden.
4. Never put Supabase service-role keys, provider API keys, Google OAuth secrets, refresh tokens, or personal test data in source files or commits.
5. Treat database migrations carefully. Do not run destructive SQL against production. Preserve user data and RLS protections.
6. Use a clear commit subject. Conventional prefixes such as `fix:`, `feat:`, and `docs:` are welcome and help automated release notes.

## Project notes

The tracker is primarily a static HTML application hosted with GitHub Pages, with Supabase functions and database migrations for backend features. Check the existing architecture before adding dependencies or changing deployment behavior. Do not commit generated secrets, local environment files, or unrelated formatting changes.

By submitting a contribution, you confirm that you have the right to submit it under the repository's license.
