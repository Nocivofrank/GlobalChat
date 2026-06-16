# GlobalChat

GlobalChat is a Chrome and Firefox extension that opens a public conversation for a webpage only after the user clicks the toolbar button. It does not monitor browsing or maintain visit history.

## Architecture

- React and TypeScript panel shared by both browsers.
- Chrome 116+ uses `chrome.sidePanel` and an MV3 service worker.
- Firefox 142+ uses `sidebar_action`, an MV3 event page, and Firefox's built-in data-consent declaration.
- Supabase provides PostgreSQL, Google OAuth, Realtime, and the authenticated write API.
- Browser clients can read public rooms but cannot mutate database tables directly.

## Local Setup

1. Install dependencies with `npm install`.
2. Create a Supabase project and install the Supabase CLI.
3. Apply `supabase/migrations/202606150001_initial_schema.sql`.
4. Deploy the function with `supabase functions deploy api`.
5. Enable Google in Supabase Authentication providers.
6. Copy `.env.example` to `.env.chrome` and `.env.firefox`, then add the public Supabase URL and publishable key.
7. Build with `npm run build`.

The publishable key is expected to appear in extension bundles. Never place the service-role key or Google client secret in an environment file used by Vite.

## OAuth Redirects

Load each unpacked extension once, open its background console, and evaluate:

```js
chrome.identity.getRedirectURL("oauth")
```

For Firefox, use:

```js
browser.identity.getRedirectURL("oauth")
```

Add the exact development and production values to Supabase Authentication's redirect allowlist. Google redirects to Supabase; Supabase then redirects to the browser-generated URL.

Chrome store IDs affect redirect URLs. Register the final Chrome redirect after the store assigns the production extension ID. Firefox uses the configured Gecko extension ID.

## Commands

```text
npm run dev:chrome
npm run dev:firefox
npm test
npm run typecheck
npm run build
npm run security:check
npm run package
```

Load `dist/chrome` through `chrome://extensions` with Developer mode enabled. Load `dist/firefox/manifest.json` through `about:debugging`.

## First Administrator

Users cannot assign roles. After the intended administrator has signed in once, promote that profile using the Supabase SQL editor while authenticated as a project owner:

```sql
update public.profiles
set role = 'admin'
where id = 'THE_AUTH_USER_UUID';
```

Record and tightly limit every use of project-owner access.

## Release Checklist

- Run `npm run package`.
- Confirm the package check reports no secrets, source maps, remote scripts, or broad permissions.
- Test OAuth using production extension IDs.
- Verify RLS using anonymous, normal-user, suspended-user, and administrator accounts.
- Publish the privacy policy and account-deletion instructions.
- Submit the separate ZIP files to Mozilla Add-ons and the Chrome Web Store.

## Security Notes

- All messages render as text. Only detected HTTP(S) URLs become links.
- The database stores a SHA-256 room key and hostname/path label, not raw query strings.
- Empty page rooms are not created.
- Direct table writes are revoked; the Edge Function revalidates identity and account status.
- Posting is limited to five messages per minute and 100 per day per account.
- Private browsing is disabled and local/private-network URLs are rejected.

See [PRIVACY.md](PRIVACY.md), [SECURITY.md](SECURITY.md), and [COMMUNITY_GUIDELINES.md](COMMUNITY_GUIDELINES.md).
