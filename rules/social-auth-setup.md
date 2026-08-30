# Social Auth Setup — Step-by-Step

> **Time needed:** ~30 min for read access on all four platforms. ~1-3 days for full write access (TikTok review).
>
> **Security:** Tokens stored in macOS Keychain only. Never logged to memory or this repo. Revoke = delete Keychain entry.

## Before you start

You will need:
- A web browser logged into the accounts you want to auth
- Admin access to the NagaVision Facebook Page (for IG)
- ~30 minutes uninterrupted

## 1. Instagram (Meta Graph API)

**Why:** Official read + write-draft. Needs Business/Creator account (personal accounts lost API access Dec 2024).

### Steps

1. **Convert to professional account**
   - Open IG app → Settings → Account → Switch to professional account
   - Choose **Business** (preferred for analytics) or **Creator**
   - Category: "Creative services" or "Product/Service"

2. **Link to Facebook Page**
   - Settings → Account → Linked accounts → Facebook
   - Create page "NagaVision" if you don't have one
   - Confirm link

3. **Create Meta App**
   - Go to https://developers.facebook.com/apps/
   - Click "Create App" → type **Business** → Next
   - App name: **NagaVision Social**
   - App contact email: your email
   - Click "Create App"

4. **Add Instagram product**
   - In the app dashboard, click "Add Product"
   - Find **Instagram Graph API** → Set Up

5. **Generate access token**
   - Go to Tools → Graph API Explorer
   - Select your app
   - Add permissions: `instagram_basic`, `instagram_content_publish`, `pages_show_list`, `pages_read_engagement`
   - Click "Generate Access Token"
   - Authorise when prompted
   - **Copy the token** (starts with `IGAA...`)

6. **Exchange for long-lived token (60 days)**
   - Use this URL in browser (replace placeholders):
   ```
   https://graph.facebook.com/v19.0/oauth/access_token?grant_type=fb_exchange_token&client_id=YOUR_APP_ID&client_secret=YOUR_APP_SECRET&fb_exchange_token=SHORT_LIVED_TOKEN
   ```
   - The response includes a long-lived token (60 days). Use this one.

### Paste to Rose in this format

```
IG token: IGAAxxxxxxxxxxxx
Account: @nagavisionltd
FB Page ID: 123456789
FB Page name: NagaVision
```

## 2. TikTok (Content Posting API)

**Why:** Official read + post. Posting requires app review (1-3 days); read works immediately via Login Kit.

### Steps

1. **Register as developer**
   - Go to https://developers.tiktok.com/
   - Sign in with the NagaVision TikTok account
   - Complete developer profile

2. **Create app**
   - Developer Portal → Apps → Create App
   - App name: **NagaVision Social**
   - Category: **Content & Social**
   - Submit

3. **Add products**
   - In your app, click "Add Product"
   - Add **Login Kit** (immediate read access)
   - Add **Content Posting API** (requires review)

4. **Get Login Kit token (read access)**
   - In Login Kit config, copy your Client Key
   - Implement OAuth flow (Rose will handle the callback URL once Client Key is set)
   - For first-time read access, use the "Try it now" button to get a sample token

5. **Submit Content Posting for review (optional, for write access)**
   - In Content Posting API settings, fill in:
     - Use case: "Promote NagaVision brand content"
     - Screenshots of your intended posts
     - Privacy policy URL (use https://nagavision.uk/privacy)
   - Submit. Review takes 1-3 business days.

### Paste to Rose

```
TT client_key: abcd1234
TT access_token: tt-xxxxx
TT open_id: your-open-id-here
Status: posting approved / read-only (pending review)
```

## 3. X / Twitter (API v2)

**Why:** Read mentions, draft replies, post on your behalf. Write needs paid tier.

### Steps

1. **Apply for developer account** (if you don't have one)
   - Go to https://developer.twitter.com/portal
   - Sign in with the @nagavisionltd account
   - Apply → "Hobbyist" or "Building business tools" → describe NagaVision
   - Approval usually instant for Hobbyist

2. **Create project + app**
   - Developer Portal → Projects → Create Project
   - Name: **NagaVision Social**
   - Use case: "Brand management for creative agency"
   - Create app inside project → name: **NagaVision Social**

3. **Set app permissions**
   - App settings → User authentication settings → Set up
   - App permissions: **Read and Write**
   - Type of app: **Web App** (or Native if preferred)
   - Callback URL: `http://localhost:8080/callback` (Rose will set up a local listener)
   - Save

4. **Generate keys & tokens**
   - Keys and tokens tab
   - **API Key and Secret** → Regenerate, copy both
   - **Bearer Token** → copy
   - **Access Token and Secret** → Generate with Read+Write scopes, copy both

5. **Pricing tier**
   - Free tier: read-only, 100 posts/month
   - **Basic (£100/mo):** needed for read+write. Rose will handle the upgrade.

### Paste to Rose

```
X bearer_token: AAAAAAAAAAAAAAAAAAAAxxxx
X api_key: xxxxx
X api_secret: xxxxx
X access_token: xxxxx
X access_secret: xxxxx
Handle: @nagavisionltd
```

## 4. YouTube (Google Cloud)

**Why:** Read channel stats, draft video descriptions/comments.

### Steps

1. **Create Google Cloud project**
   - Go to https://console.cloud.google.com/
   - Project dropdown → New Project → "NagaVision Social"
   - Create

2. **Enable YouTube Data API v3**
   - APIs & Services → Library
   - Search "YouTube Data API v3" → Enable

3. **Configure OAuth consent screen**
   - APIs & Services → OAuth consent screen
   - User type: **External**
   - App name: **NagaVision Social**
   - Support email: your email
   - Scopes: add `youtube.readonly`, `youtube.force-ssl`
   - Test users: add your @nagavisionltd email
   - Save and continue

4. **Create OAuth 2.0 Client ID**
   - APIs & Services → Credentials → Create Credentials → OAuth client ID
   - Application type: **Web application**
   - Name: **NagaVision Social**
   - Authorised redirect URIs: `http://localhost:8080/callback`
   - Create
   - Copy **Client ID** and **Client Secret**

### Paste to Rose

```
YT client_id: xxxxx.apps.googleusercontent.com
YT client_secret: GOCSPX-xxxxx
```

## After all tokens are pasted

Rose will:
1. Store each token in macOS Keychain (encrypted)
2. Test each with a live API call
3. Wire the Social Radar agent (daily 11:00 BST)
4. Auto-discover accounts NagaVision follows (Cro, Empire Alpha, Ellipsis, @curtsoul, @nagaxmusic artists)
5. Confirm when monitoring is live

## Revoking access

If you ever need to revoke:
- **IG:** Meta Business Suite → Settings → Business integrations → remove NagaVision Social
- **TikTok:** TikTok → Settings → Manage apps → remove NagaVision Social
- **X:** Developer Portal → your app → Revoke
- **YouTube:** Google Account → Security → Third-party apps → remove NagaVision Social

Then tell Rose to delete the Keychain entries.

---

**Last updated:** 2026-08-29
**Status:** pending — auth not yet started
