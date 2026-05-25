# differentworld-web

Public-facing landing page + universal-link verification files for the
[Different World](https://differentworld.app) mobile app.

Served via GitHub Pages at `https://differentworld.app`.

## What's here

```
.
├── .well-known/
│   ├── apple-app-site-association  ← iOS Universal Link verification
│   └── assetlinks.json             ← Android App Link verification
├── CNAME                            ← differentworld.app
├── .nojekyll                        ← keeps GitHub Pages from filtering .well-known/
├── index.html                       ← landing page
└── README.md
```

## Universal Links / App Links

These files tell iOS and Android which native app handles
`https://differentworld.app/v/*` and `/invite/*` URLs.

### Updating `apple-app-site-association`

You'll see `TODO_APPLE_TEAM_ID` in the file — replace with the
10-character Team ID from your Apple Developer Membership page.

Validate the file once the domain is live:

```bash
curl https://differentworld.app/.well-known/apple-app-site-association | jq
```

Apple's validator:
https://branch.io/resources/aasa-validator/

### Updating `assetlinks.json`

The current file has the **debug** keystore fingerprint
(`EC:2E:...:D7`). When you generate a release keystore, add its
fingerprint to the array:

```bash
keytool -list -v -keystore <release-keystore> -alias <alias> | grep SHA256
```

Then verify:

```bash
curl https://differentworld.app/.well-known/assetlinks.json | jq
adb shell pm verify-app-links --re-verify com.jardine.differentworld
adb shell pm get-app-links com.jardine.differentworld
```

## DNS

Apex `differentworld.app` → A records pointing to GitHub Pages IPs:
- 185.199.108.153
- 185.199.109.153
- 185.199.110.153
- 185.199.111.153

`www.differentworld.app` → CNAME `jardinefaner.github.io`

## Pages settings

Settings → Pages → Source: **Deploy from a branch**, branch `main`,
folder `/` (root). Enforce HTTPS once DNS verifies (~10-30 minutes).
