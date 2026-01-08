---
title: "Fixing Keystatic GitHub OAuth on Cloudflare Workers"
date: 2026-01-08
tags: ["dev", "cloudflare"]
description: "How to fix Keystatic CMS OAuth authorization failures on Cloudflare Workers caused by missing redirect_uri in token exchange"
---

## The Problem

Keystatic CMS fails GitHub OAuth authentication on Cloudflare Workers with 401 errors on `/api/keystatic/github/refresh-token`. The OAuth login flow appears to work, but token exchange fails silently.

This happens because Keystatic sends `redirect_uri` during the initial authorization request to GitHub, but omits it during token exchange. GitHub's OAuth requires that if you send `redirect_uri` in the authorization step, you must also send the identical value during token exchange.

## Why This Happens

GitHub's OAuth flow has two steps:
1. **Authorization**: User authorizes your app → GitHub redirects back with a code
2. **Token exchange**: Your app exchanges the code for an access token

If you include `redirect_uri` in step 1, GitHub requires the exact same `redirect_uri` in step 2. Keystatic includes it in step 1 but forgets it in step 2, causing GitHub to reject the request with a 401.

## Solution: Patch Keystatic's OAuth Handler

Create a postinstall script that patches `@keystatic/core` to include `redirect_uri` in token exchange requests.

### Step 1: Create the Patch Script

Save this to `scripts/patch-keystatic.cjs`:

```javascript
#!/usr/bin/env node
const fs = require('fs');
const path = require('path');

const filePath = path.join(
  __dirname,
  '../node_modules/@keystatic/core/dist/keystatic-core-api-generic.worker.js'
);

console.log('Patching Keystatic OAuth handler...');

try {
  let content = fs.readFileSync(filePath, 'utf8');

  if (content.includes('PATCH: Add redirect_uri')) {
    console.log('Keystatic already patched, skipping.');
    process.exit(0);
  }

  const oldCallbackCode = `const url = new URL('https://github.com/login/oauth/access_token');
  url.searchParams.set('client_id', config.clientId);
  url.searchParams.set('client_secret', config.clientSecret);
  url.searchParams.set('code', code);
  const tokenRes = await fetch(url, {`;

  // Set your production URL here
  const PRODUCTION_SITE_URL = 'https://your-site.com';

  const newCallbackCode = `const url = new URL('https://github.com/login/oauth/access_token');
  url.searchParams.set('client_id', config.clientId);
  url.searchParams.set('client_secret', config.clientSecret);
  url.searchParams.set('code', code);
  // PATCH: Add redirect_uri to token exchange
  const reqUrlForRedirect = new URL(req.url);
  const isProduction = reqUrlForRedirect.hostname !== 'localhost' && !reqUrlForRedirect.hostname.includes('127.0.0.1');
  const siteOrigin = isProduction ? '${PRODUCTION_SITE_URL}' : reqUrlForRedirect.origin;
  url.searchParams.set('redirect_uri', \`\${siteOrigin}/api/keystatic/github/oauth/callback\`);
  const tokenRes = await fetch(url, {`;

  if (!content.includes(oldCallbackCode)) {
    console.error('Could not find the callback code to patch. Keystatic may have been updated.');
    process.exit(1);
  }

  content = content.replace(oldCallbackCode, newCallbackCode);

  // Also patch the login handler for consistency
  const oldLoginCode = `url.searchParams.set('redirect_uri', \`\${reqUrl.origin}/api/keystatic/github/oauth/callback\`);`;
  const newLoginCode = `// PATCH: Use production URL in production
  const isLoginProduction = reqUrl.hostname !== 'localhost' && !reqUrl.hostname.includes('127.0.0.1');
  const loginSiteOrigin = isLoginProduction ? '${PRODUCTION_SITE_URL}' : reqUrl.origin;
  url.searchParams.set('redirect_uri', \`\${loginSiteOrigin}/api/keystatic/github/oauth/callback\`);`;

  if (content.includes(oldLoginCode)) {
    content = content.replace(oldLoginCode, newLoginCode);
  }

  fs.writeFileSync(filePath, content);
  console.log('Keystatic patched successfully!');
} catch (error) {
  console.error('Error patching Keystatic:', error.message);
  process.exit(1);
}
```

**Important**: Replace `https://your-site.com` with your actual production URL.

### Step 2: Configure Package Scripts

Add to your `package.json`:

```json
{
  "scripts": {
    "build": "node scripts/patch-keystatic.cjs && astro build",
    "postinstall": "node scripts/patch-keystatic.cjs"
  }
}
```

This ensures the patch runs after every `npm install` and before each build.

### Step 3: Set Environment Variables

Don't put secrets in `wrangler.jsonc` or `wrangler.toml`. Set these in the Cloudflare dashboard (Settings → Environment Variables):

- `KEYSTATIC_GITHUB_CLIENT_ID` - Your GitHub App client ID
- `KEYSTATIC_GITHUB_CLIENT_SECRET` - Your GitHub App client secret
- `PUBLIC_KEYSTATIC_GITHUB_APP_SLUG` - Your GitHub App slug
- `KEYSTATIC_SECRET` - Random secret for session encryption

### Step 4: Configure GitHub App

In your GitHub App settings:
- **Homepage URL**: `https://your-site.com/`
- **Callback URL**: `https://your-site.com/api/keystatic/github/oauth/callback`
- **Repository permissions**: Contents (Read & Write)
- Install the app on your repository

### Step 5: Deploy

```bash
rm -rf node_modules
npm install  # Applies the patch
npm run build
git push  # Trigger Cloudflare deployment
```

## What About Just Recreating the GitHub App?

In my case, recreating the GitHub App from scratch fixed the issue even without the patch. This suggests the original app had some corrupted OAuth configuration. However, the patch fixes a real bug in Keystatic and ensures consistent `redirect_uri` handling, so it's worth applying regardless.

## Debugging Tips

If OAuth still fails after applying the fix:

1. **Check environment variables**: Verify all four variables are set in Cloudflare dashboard, not in config files
2. **Verify callback URL**: Must match exactly between GitHub App settings and your production URL
3. **Check GitHub App installation**: Make sure the app is installed on the correct repository
4. **Clear cache**: `rm -rf node_modules` and reinstall to ensure patch applies
5. **Inspect network logs**: Look for 401 errors on `/api/keystatic/github/refresh-token` or `/oauth/callback`

## Why This Matters

Keystatic is a solid CMS for Astro/Next.js projects, but its OAuth implementation has this quirk that breaks on serverless platforms like Cloudflare Workers. This patch ensures reliable authentication without modifying your application code or switching deployment platforms.
