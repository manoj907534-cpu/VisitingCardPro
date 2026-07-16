# Visiting Card Scanner — Setup Guide

## 1. Create the Apps Script backend
1. Go to [script.google.com](https://script.google.com) → **New project**.
2. Delete the default code, paste in `Code.gs`.
3. Edit the top of the file:
   ```js
   const ALLOWED_USERNAME = "your_username";
   const ALLOWED_PASSWORD = "your_password";
   ```
   Use a private, unique passphrase — not a password you use elsewhere.

## 2. Enable the Drive API (required for OCR)
1. In the Apps Script editor, click **Services** (+ icon) in the left sidebar.
2. Find **Drive API** and click **Add**. This is the *advanced* Drive service, separate from `DriveApp` — OCR only works through it.

## 3. Deploy as a Web App
1. Click **Deploy → New deployment**.
2. Select type **Web app**.
3. Set:
   - **Execute as:** Me
   - **Who has access:** Anyone
4. Click **Deploy**, authorize the requested permissions, and copy the **Web app URL** it gives you (ends in `/exec`).

## 4. Connect the frontend
1. Open `index.html`.
2. Replace this line near the top:
   ```js
   const GAS_WEB_APP_URL = "PASTE_YOUR_DEPLOYED_WEB_APP_URL_HERE";
   ```
   with your copied `/exec` URL.

## 5. Publish on GitHub Pages
1. Push `index.html` to a GitHub repo.
2. Go to repo **Settings → Pages**.
3. Set source to your main branch (root), save.
4. Your scanner will be live at `https://<username>.github.io/<repo>/`.

## 6. First run
- On first visit, you'll see the login modal — enter the username/password you set in `Code.gs`. These are cached in your browser's `localStorage` so you won't need to re-enter them each time (use **Logout** to clear).
- A Google Drive folder `Visiting_Cards_Images` and a Sheet `Visiting_Cards_Database` will be created automatically the first time you use it.

## 7. Using the Scan tab (batch mode)
- Optionally type an **Event** tag (e.g. "TechConf 2026") — it's applied to every card in that batch.
- Drag & drop **multiple** images at once, or tap **Use Camera** and capture several cards in a row before hitting **Done**.
- Cards process one at a time with a "Card X of Y" progress indicator. Review/edit the auto-filled fields for each, then **Save & Continue** (or **Skip**).
- If a scanned email or phone matches an existing contact, you'll be asked to **Save Anyway** or **Skip** — this catches duplicate scans at multi-day events.

## 8. Using the Contacts tab
- Shows every saved card as a row with its actual scanned image thumbnail.
- **Search** by name, company, email, or event; **filter** by event using the dropdown.
- Tap **Edit** to open the full card image alongside editable fields, or **Delete** to remove a contact (with confirmation).
- **Export CSV** or **Export vCard** downloads whatever is currently visible in your search/filter — useful for importing just one event's contacts into your phone or CRM.

## Notes & limitations
- **This is not real authentication** — it's a shared secret check on a public endpoint. Fine for a personal tool where you keep the URL and password private; don't use it for anything sensitive.
- Google Apps Script's free daily quotas apply (URL fetch calls, Drive OCR conversions, etc.) — comfortable for personal/light team use, but can throttle under heavy volume.
- If OCR results come back empty, double check the Drive API advanced service is enabled and that you re-authorized permissions after enabling it.
