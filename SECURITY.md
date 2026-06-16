# Security Policy

## Reporting

Do not disclose vulnerabilities in public chat rooms or issue trackers. Use the private security contact listed in the extension-store publication and include reproduction steps, affected version, and impact.

## Deployment Requirements

- Store `SUPABASE_SERVICE_ROLE_KEY` and Google OAuth secrets only in managed provider secrets.
- Never prefix private values with `VITE_`.
- Require MFA for Supabase, Google Cloud, Mozilla Add-ons, Chrome Web Store, and source-hosting owners.
- Restrict production project membership and review audit logs.
- Run `npm run package` before every release.
- Keep dependencies and browser minimum versions current.

## Threat Controls

- Strict extension CSP and no remote executable code.
- Minimal permissions without `<all_urls>`, history, cookies, or content scripts.
- Server-side authorization, validation, account-state checks, and rate limiting.
- RLS plus revoked direct write privileges.
- Text-only rendering and safe external-link attributes.
- Soft deletion for user-hidden messages, retained moderator access to original records, and administrator audit records.
