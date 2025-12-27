# 📧 Gmail Setup Guide

How to connect your Gmail account to n8n for monitoring return requests.

---

## Method 1: Easy Setup (n8n Cloud)

If you are using n8n Cloud, the setup is automatic.

1. **Add "Gmail Trigger" node** to canvas
2. **Credentials** → "Create New"
3. **Select "OAuth2"**
4. **Click "Sign in with Google"**
5. **Accept permissions**
   - Read email
   - Send email
   - Modify email labels (optional)

✅ **Done!**

---

## Method 2: Custom Credentials (Self-Hosted)

If you are self-hosting n8n, you need to create a Google Cloud Project.

### 1. Create Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create new project: "Shopify Returns Agent"
3. Enable **Gmail API**:
   - APIs & Services → Enable APIs
   - Search "Gmail API"
   - Enable it

### 2. Configure OAuth Consent Screen

1. **APIs & Services → OAuth consent screen**
2. **User Type:** External
3. **App Info:**
   - Name: "Returns Agent"
   - Email: Your email
4. **Scopes:**
   - `.../auth/gmail.readonly`
   - `.../auth/gmail.send`
   - `.../auth/gmail.labels`
5. **Test Users:**
   - Add your own email address

### 3. Create Credentials

1. **APIs & Services → Credentials**
2. **Create Credentials → OAuth Client ID**
3. **App Type:** Web Application
4. **Redirect URI:**
   - Copy this FROM your n8n credential window
   - usually: `https://your-n8n-instance.com/rest/oauth2-credential/callback`
5. **Create**
6. **Copy Client ID and Client Secret**

### 4. Connect in n8n

1. **Open n8n**
2. **Create new Gmail credential**
3. **Paste Client ID**
4. **Paste Client Secret**
5. **Click "Sign in with Google"**

---

## 🔍 Testing Connectivity

1. **In n8n:**
   - Open Gmail Trigger node
   - Click "Test Step" (or "Listen for events")
   
2. **Send email:**
   - Send a test email to the connected account
   - Subject: "Test return"

3. **Verify:**
   - n8n should show "Event received"
   - Data should contain email subject, body, sender

---

## 🔒 Security Tips

- Create a dedicated email like `returns@yourstore.com`
- Do NOT use your personal primary email if possible
- Use "Least Privilege":
   - If you only need to read, don't grant send keys (though this agent needs both)
- Periodically check Google Cloud Console for active sessions
