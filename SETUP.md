# Outlook AI Translator — Setup Guide

Translate emails to the recipient's language with one click, powered by Claude (Anthropic).

---

## Prerequisites

- A free GitHub account
- An Anthropic API key → [console.anthropic.com](https://console.anthropic.com)
- Access to Outlook on the Web (OWA) or Outlook Desktop (Microsoft 365)

---

## Step 1 — Create the GitHub repo

1. Go to [github.com/new](https://github.com/new)
2. Repository name: **`outlook-translator`**
3. Set visibility to **Public**
4. Click **Create repository**

---

## Step 2 — Upload the files

Upload these two files to the repo root:

- `taskpane.html`
- `manifest.xml`

You can drag-and-drop them on the GitHub repo page, or use **Add file → Upload files**.

Also create an `assets/` folder and add three placeholder PNG icons (16×16, 32×32, 80×80 pixels).
Any simple icon works — Office only uses them in the ribbon. You can use the same image for all three.

---

## Step 3 — Enable GitHub Pages

1. In your repo, go to **Settings → Pages**
2. Under **Branch**, select `main` and folder `/root`
3. Click **Save**
4. Wait ~2 minutes. Your site will be live at:
   `https://YOUR-GITHUB-USERNAME.github.io/outlook-translator/`

---

## Step 4 — Update manifest.xml

Open `manifest.xml` and replace every occurrence of `YOUR-GITHUB-USERNAME` (6 places) with your actual GitHub username.

Then commit/upload the updated file.

---

## Step 5 — Sideload the add-in into Outlook

### Outlook on the Web (OWA)

1. Open [outlook.office.com](https://outlook.office.com)
2. Click **New Mail**
3. In the compose window, click the `...` (More options) menu
4. Choose **Get Add-ins**
5. Go to **My add-ins** tab
6. Scroll to **Custom add-ins** → click **+ Add a custom add-in → Add from URL**
7. Paste:
   ```
   https://YOUR-GITHUB-USERNAME.github.io/outlook-translator/manifest.xml
   ```
8. Click **OK** and confirm the warning

### Outlook Desktop (Windows, Microsoft 365)

1. Open Outlook Desktop
2. Go to **Home** ribbon → **Get Add-ins** (or **Manage Add-ins**)
3. In the store dialog, click **My add-ins**
4. Under **Custom add-ins** → **Add from file...**
5. If that option isn't available, use the URL method via OWA above (it syncs to desktop automatically)

---

## Step 6 — First-time API key setup

1. Open a **New Email** in Outlook
2. You should see a **🌐 Translate** button in the compose toolbar
3. Click it — the side panel opens
4. Scroll to **Anthropic API Key**, paste your `sk-ant-…` key, click **Save**
5. The key is stored in your browser's `localStorage` — it never leaves your machine (except when calling Anthropic's API directly)

---

## Usage

1. Compose an email and add recipients
2. Click **🌐 Translate** in the toolbar
3. The panel auto-detects the target language from recipient domains
4. Override manually if needed by clicking a language button
5. Click **Translate Email**
6. The translation + bilingual AI disclaimer is appended below your original text

---

## Adding Languages / Recipients

Open `taskpane.html` and find `DOMAIN_LANG_MAP` in the `<script>` block:

```javascript
const DOMAIN_LANG_MAP = [
  { fragments: ['.it', 'italy', 'italia', 'fosber'], lang: 'it', name: 'Italian', flag: '🇮🇹' },
  ...
];
```

**Add a specific person:**
```javascript
{ fragments: ['carlos@acme.com'], lang: 'es', name: 'Spanish', flag: '🇪🇸' },
```

**Add a domain:**
```javascript
{ fragments: ['.jp', 'toyota'], lang: 'ja', name: 'Japanese', flag: '🇯🇵' },
```

Commit the change — GitHub Pages auto-deploys in ~60 seconds. No reinstall needed.

---

## Troubleshooting

| Issue | Fix |
|---|---|
| "Could not read email body" | Make sure you're in a **compose** window, not reading |
| "API error 401" | API key is wrong or expired — re-enter it |
| Translate button missing | Reload Outlook; check manifest URL is accessible |
| Language not detected | Domain not in `DOMAIN_LANG_MAP` — use manual override or add an entry |
| Pages site returns 404 | Wait a few more minutes; check Pages is enabled in Settings |

---

## Security Notes

- Your API key is stored only in **browser localStorage** on your own machine
- The key is sent directly to `api.anthropic.com` — no intermediate server
- Do **not** use this on a shared or public computer
- The manifest and taskpane code are public on GitHub — do not commit your API key

---

*Built with Claude Sonnet · Anthropic · May 2026*
