# 🔍 Debug Checklist for Google Sheets + Telegram

## ✅ Verify These Settings

### 1. Apps Script Deployment
Go to your Google Sheet → **Extensions** → **Apps Script**

Check:
- [ ] Click **Deploy** → **Manage deployments**
- [ ] Your deployment shows: **Web app**
- [ ] **Who has access:** `Anyone` (NOT "Anyone with Google account")
- [ ] URL matches: `https://script.google.com/macros/s/AKfycbz1dVbjSp5vMIM8Pt-9mUF2B7581BmlqBAQtJwGP1PbQ2j_U76dtfyUM9TLPURZZxxJJQ/exec`

If not set to "Anyone":
1. Click the gear icon ⚙️ next to your deployment
2. Change **Who has access** to `Anyone`
3. Click **Deploy**

---

### 2. Test the Webhook Directly

Open this URL in a new tab to test if it's accessible:
```
https://script.google.com/macros/s/AKfycbz1dVbjSp5vMIM8Pt-9mUF2B7581BmlqBAQtJwGP1PbQ2j_U76dtfyUM9TLPURZZxxJJQ/exec
```

Expected: You should see a JSON response like:
```json
{"success":false,"error":"Missing required fields"}
```

If you see:
- **404 Error** → Wrong URL, re-deploy the script
- **403 Error** → Permissions issue, change "Who has access" to "Anyone"
- **Login prompt** → Not deployed as public

---

### 3. Check Apps Script Logs

In Apps Script editor:
1. Click **Executions** (left sidebar)
2. Look for recent executions
3. Check if there are errors

Common errors:
- `TypeError: Cannot read property 'postData' of undefined` → Request format issue
- `Exception: Service invoked too many times` → Rate limiting (wait a minute)
- `Telegram error: {"ok":false,"error_code":400,"description":"..."}` → Bot token/chat ID issue

---

### 4. Verify Bot Token & Chat ID

In your Apps Script code, confirm:
```javascript
const TELEGRAM_BOT_TOKEN = 'YOUR_ACTUAL_TOKEN'; // NOT 'YOUR_BOT_TOKEN_HERE'
const TELEGRAM_CHAT_ID = 'YOUR_ACTUAL_ID';      // NOT 'YOUR_CHAT_ID_HERE'
```

Test manually:
1. Replace YOUR_BOT_TOKEN and YOUR_CHAT_ID with real values
2. In Apps Script, click **Run** on any function
3. Check if you get a Telegram message

---

### 5. Browser Console Errors

When testing the form:
1. Open browser DevTools (F12)
2. Go to **Console** tab
3. Submit the form
4. Copy any error messages

Share the error with me and I'll help fix it!

---

## 🚀 Quick Fix Commands

After making changes in Apps Script:
1. **Save** the script (Ctrl+S)
2. **Deploy** → **Manage deployments** → Edit existing deployment
3. Click **Deploy** again (even if no code changes, this refreshes it)
4. Wait 1-2 minutes for propagation

---

## Still Not Working?

Try this test:
1. Create a simple test page with just the fetch call
2. Or use curl/postman to test the webhook directly

Example curl command:
```bash
curl -X POST \
  https://script.google.com/macros/s/AKfycbz1dVbjSp5vMIM8Pt-9mUF2B7581BmlqBAQtJwGP1PbQ2j_U76dtfyUM9TLPURZZxxJJQ/exec \
  -H "Content-Type: text/plain;charset=utf-8" \
  -d '{"email":"test@test.com","jobLink":"https://example.com"}'
```

Expected response:
```json
{"success":true,"message":"Submission recorded"}
```
