# GlobalChat Website

Static website for Google OAuth branding and browser-store policy links.

## Public URLs

After deploying this `site` folder to `globalchats.net`, use these links:

- Homepage: `https://globalchats.net/`
- Privacy Policy: `https://globalchats.net/privacy/`
- Terms of Service: `https://globalchats.net/terms/`
- Security Policy: `https://globalchats.net/security/`

## Google Auth Platform

In Google Auth Platform > Branding:

- App name: `GlobalChat`
- Authorized domain: `globalchats.net`
- Homepage URL: `https://globalchats.net/`
- Privacy Policy URL: `https://globalchats.net/privacy/`
- Terms of Service URL: `https://globalchats.net/terms/`
- Support email: `support@globalchats.net` or another email you actively monitor

## Render Static Site Settings

Create a new Static Site on Render and use:

- Root Directory: `site`
- Build Command: leave blank, or use `echo static site`
- Publish Directory: `.`

Then point the `globalchats.net` DNS records to Render using the values Render gives you.
