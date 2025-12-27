# 🔧 Troubleshooting Guide

Common issues and solutions for the Shopify Returns Agent.

---

## 🚨 Quick Diagnostics

**If workflow isn't working, check these first:**

1. ✅ Is workflow **Active**? (Toggle in top right of n8n)
2. ✅ Are all credentials **connected**? (No red error icons on nodes)
3. ✅ Did you create a **test order** in Shopify that's **fulfilled**?
4. ✅ Are you sending test emails to the **correct email** address?

---

## Common Issues

### 1. Gmail Trigger Not Firing

**Symptom:** No workflow execution when email is sent

**Causes & Solutions:**

**A. Workflow not active**
- ✅ Toggle workflow to "Active" (top right)
- Green dot should appear next to workflow name

**B. Gmail not connected**
- ✅ Click Gmail Trigger node
- ✅ Check "Authentication" - should show your email
- ✅ If "Reconnect required", click to re-authenticate

**C. Gmail search filter too restrictive**
- ✅ Click Gmail Trigger node
- ✅ Check "Search" field
- ✅ Try simplified filter: `subject:return`

**D. Polling delay**
- ✅ Gmail checks every 2 minutes by default
- ✅ Wait up to 5 minutes for first trigger
- ✅ Reduce polling interval in node settings if needed

**Test:**
```
Send email with subject: "return order #1001"
Wait 2-3 minutes
Check n8n Executions tab
```

---

### 2. Order Not Found in Shopify

**Symptom:** Error message "Order not found in Shopify"

**Causes & Solutions:**

**A. Order number format mismatch**
- ✅ Shopify orders have format: #1001, #1002, etc.
- ✅ Customer must include # in email
- ✅ Regex extracts numbers after #

**Fix:** Update "Extract Order Number" regex to handle both:
```javascript
const patterns = [
  /#(\d{3,10})/i,           // Matches #1234
  /order[:\s#]*(\d{3,10})/i // Matches "order 1234" or "order: 1234"
];
```

**B. Wrong store subdomain**
- ✅ Check HTTP Request URL
- ✅ Should be: `https://YOUR-STORE.myshopify.com/...`
- ✅ Replace YOUR-STORE with actual store name

**C. API credentials expired**
- ✅ Check Shopify Admin → Apps → Your App
- ✅ Verify app still installed
- ✅ Regenerate API token if needed

**Test:**
```
In Shopify Admin:
- Note order number: #1007
- In n8n, click "Search Order" node
- Execute manually with order number
- Should return order data
```

---

### 3. Shopify API 401 Unauthorized

**Symptom:** Error "401 Unauthorized" on HTTP Request nodes

**Causes & Solutions:**

**A. Invalid API token**
- ✅ Go to Shopify Admin → Apps → Develop apps
- ✅ Click your app → API credentials
- ✅ Regenerate "Admin API access token"
- ✅ Update in n8n Header Auth credential

**B. Missing API scopes**
- ✅ Check app has these scopes:
  - `read_orders` ✅
  - `write_orders` ✅
  - `read_products` ✅
- ✅ If missing, add scopes and reinstall app

**C. Wrong header format**
- ✅ Header name must be EXACTLY: `X-Shopify-Access-Token`
- ✅ Value format: `shpat_xxxxxxxxxxxxx`
- ✅ No extra spaces or quotes

**Test:**
```
Create simple HTTP Request:
Method: GET
URL: https://YOUR-STORE.myshopify.com/admin/api/2024-01/shop.json
Headers: X-Shopify-Access-Token: your-token
Execute - should return shop info
```

---

### 4. AI Responses Are Generic or Wrong

**Symptom:** AI gives irrelevant responses or incorrect information

**Causes & Solutions:**

**A. Data not flowing to AI node**
- ✅ Click AI Agent node
- ✅ Check "Previous Node" data preview
- ✅ Should show order data, eligibility info

**B. System prompt needs improvement**
- ✅ Review system prompt in AI Agent node
- ✅ Make sure it references: `{{ $json.order.name }}`
- ✅ Add more specific instructions

**C. Gemini API quota exceeded**
- ✅ Check Google AI Studio quota
- ✅ Free tier: 60 requests/minute
- ✅ Upgrade if hitting limits

**Test:**
```
Click AI Agent node
Execute manually
Read output - should be relevant to order data
If not, check system prompt variables
```

---

### 5. Email Not Sending

**Symptom:** Workflow executes successfully but customer doesn't receive email

**Causes & Solutions:**

**A. Gmail node not configured**
- ✅ Click Gmail Send/Reply node
- ✅ Verify "To:" field has correct email
- ✅ Format: `{{ $('Extract Order Number').item.json.customerEmail }}`

**B. Gmail quota exceeded**
- ✅ Gmail free: 500 emails/day
- ✅ Google Workspace: 2000 emails/day
- ✅ Check quota in Gmail settings

**C. Email in spam**
- ✅ Ask customer to check spam folder
- ✅ Add your domain to SPF/DKIM if using custom domain

**D. Reply operation failing**
- ✅ If using "Reply" operation, needs Message ID
- ✅ Check: `{{ $('Receive Return Request').item.json.id }}`
- ✅ If missing, use "Send" operation instead

**Test:**
```
Click Gmail Send node
Change "To:" to your personal email
Execute manually
Check if you receive email
```

---

### 6. Workflow Execution Slow

**Symptom:** Takes 30+ seconds to respond

**Causes & Solutions:**

**A. AI model timeout**
- ✅ Gemini can take 5-10 seconds
- ✅ Normal for first response
- ✅ Consider using faster model if available

**B. Too many HTTP requests**
- ✅ Check if making redundant Shopify API calls
- ✅ Combine data fetching where possible

**C. n8n cloud limitations**
- ✅ Free tier has slower execution
- ✅ Upgrade to Pro for faster processing

**Optimization tips:**
- Use "Run Once for All Items" on Code nodes
- Cache frequently accessed data
- Reduce number of nodes in workflow