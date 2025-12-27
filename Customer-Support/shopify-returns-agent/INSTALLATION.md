# 📦 Installation Guide

Complete step-by-step guide to set up the Shopify Returns & Exchanges Agent.

**Estimated setup time:** 45-60 minutes

---

## Prerequisites Checklist

Before starting, ensure you have:

- [ ] n8n Cloud account (or self-hosted n8n instance)
- [ ] Shopify store with Admin access
- [ ] Gmail account for customer support
- [ ] Google Cloud account (for Gemini API access)
- [ ] 60 minutes of uninterrupted time

---

## Step 1: n8n Setup (10 minutes)

### Option A: n8n Cloud (Recommended)

1. **Sign up for n8n Cloud**
   - Go to [n8n.cloud](https://n8n.cloud)
   - Create account
   - Choose plan ($20/month minimum for production use)

2. **Create new workflow**
   - Click "New Workflow"
   - Name it: "Shopify Returns Agent"

### Option B: Self-Hosted n8n

1. **Install n8n**
```bash
   npm install n8n -g
   n8n start
```

2. **Access n8n**
   - Open browser: `http://localhost:5678`
   - Create account

---

## Step 2: Shopify API Setup (15 minutes)

### Create Custom App

1. **Go to Shopify Admin**
```
   https://YOUR-STORE.myshopify.com/admin
```

2. **Navigate to Apps**
   - Settings → Apps and sales channels
   - Click "Develop apps" (top right)

3. **Create App**
   - Click "Create an app"
   - App name: "Returns Automation"
   - Click "Create app"

### Configure API Scopes

4. **Click "Configure Admin API scopes"**

5. **Select these scopes:**
   - ✅ `read_orders`
   - ✅ `write_orders`
   - ✅ `read_products`
   - ✅ `read_fulfillments`
   - ✅ `read_customers`

6. **Save**

### Install App

7. **Click "Install app"** (top right)
   - Confirm installation

8. **Reveal API Token**
   - Click "Reveal token once"
   - **⚠️ COPY THIS TOKEN** - you can only see it once!
   - Format: `shpat_xxxxxxxxxxxxxxxxxxxxx`
   - Store securely (password manager)

### Get Store Details

9. **Note your store subdomain:**
   - From URL: `https://YOUR-STORE.myshopify.com/admin`
   - Store name: `YOUR-STORE`

---

## Step 3: Gmail API Setup (15 minutes)

### Enable Gmail API

1. **Go to Gmail**
   - Use the email that will receive return requests
   - Example: `returns@yourstore.com` or your support email

2. **In n8n, add Gmail Trigger node**

3. **Click "Connect my account"**
   - Follow OAuth2 flow
   - Grant permissions to n8n

### Configure Gmail Filter (Optional but Recommended)

4. **Create Gmail label** (optional)
   - In Gmail: Settings → Labels
   - Create label: "Returns"

5. **Create filter** (optional)
   - Settings → Filters
   - Match: `subject:(return OR refund OR exchange)`
   - Apply label: "Returns"

---

## Step 4: Google Gemini API Setup (10 minutes)

### Get API Key

1. **Go to Google AI Studio**
```
   https://makersuite.google.com/app/apikey
```

2. **Create API Key**
   - Click "Create API Key"
   - Copy the key
   - Format: `AIza...`

### Add to n8n

3. **In n8n, add AI Agent node**

4. **Create credential:**
   - Select "Google Gemini"
   - Paste API key
   - Save

---

## Step 5: Import Workflow (5 minutes)

### Download Workflow

1. **Download** `workflow.json` from this repo

### Import to n8n

2. **In n8n:**
   - Click "..." menu (top right)
   - Select "Import from File"
   - Choose `workflow.json`
   - Click "Import"

### Connect Credentials

3. **Click on each node that needs credentials:**
   - Gmail Trigger → Select your Gmail account
   - HTTP Request nodes → Create "Header Auth" credential
   - AI Agent nodes → Select your Gemini credential

---

## Step 6: Configure Shopify Credentials (10 minutes)

### Create Header Auth Credential

1. **Click any HTTP Request node** (e.g., "Search Order")

2. **Authentication:**
   - Select: "Generic Credential Type"
   - Credential Type: "Header Auth"

3. **Click "Create New Credential"**

4. **Header Auth Settings:**
```
   Name: X-Shopify-Access-Token
   Value: shpat_xxxxxxxxxxxxx  (your token from Step 2)
```

5. **Save credential**

6. **Reuse this credential** for all HTTP Request nodes:
   - "Search Order"
   - Any future Shopify API calls

---

## Step 7: Configure Return Policy Rules (5 minutes)

### Update Check Return Eligibility Node

1. **Click "Check Return Eligibility" node**

2. **Edit the JavaScript code** - find these lines:
```javascript
const policy = {
  returnWindowDays: 30,  // ← Change to your policy
  exchangeWindowDays: 30,
  autoApproveUnder: 50,
  requiresApprovalOver: 200,
  allowFinalSaleReturns: false,
  refundOriginalShipping: false
};
```

3. **Customize values:**
   - `returnWindowDays`: How many days after order placement
   - `autoApproveUnder`: Orders under this $ amount auto-approved
   - `requiresApprovalOver`: Orders over this $ need human review
   - `allowFinalSaleReturns`: true/false for final sale items

4. **Save node**

---

## Step 8: Test the Workflow (10 minutes)

### Create Test Order

1. **In Shopify Admin:**
   - Orders → Create order
   - Add a product
   - Add customer email (your test email)
   - Mark as "Paid"
   - **Fulfill the order** (important!)
   - Note the order number (e.g., #1001)

### Send Test Email

2. **From your personal email, send to your support email:**
```
Subject: I want to return my order #1001

Hi,

I received my order but the product doesn't fit. I'd like to return it for a refund.

Thank you!
```

### Verify Response

3. **Check your email:**
   - Should receive AI response within 10 seconds
   - Response should confirm return eligibility
   - Should explain next steps

4. **Check n8n execution:**
   - Go to n8n → Executions tab
   - Should show successful run
   - All nodes green ✅

---

## Step 9: Test All Scenarios (15 minutes)

Test these 4 scenarios:

### Test 1: Eligible Return ✅
```
Subject: Return order #1001
Expected: Approval email
```

### Test 2: Ineligible Return ❌
Create an old test order (or unfulfilled), then:
```
Subject: Return order #1002
Expected: Polite rejection email
```

### Test 3: Missing Order Number 🔍
```
Subject: I want to return my snowboard
(No order number in email)
Expected: Request for order number
```

### Test 4: Invalid Order ❓
```
Subject: Return order #9999
(Non-existent order)
Expected: Order not found message
```

---

## Step 10: Go Live (5 minutes)

### Final Checks

- [ ] All 4 test scenarios passed
- [ ] AI responses are professional
- [ ] Workflow executes without errors
- [ ] Gmail filter configured (if used)

### Activate Workflow

1. **In n8n:**
   - Toggle workflow to "Active"
   - Monitor first few real emails closely

### Monitor Performance

2. **Check daily for first week:**
   - Executions tab in n8n
   - Verify responses are appropriate
   - Adjust AI prompts if needed

---

## Troubleshooting Installation

### Issue: Gmail not triggering
**Solution:** 
- Check Gmail OAuth2 connection
- Verify Gmail filter is not too restrictive
- Wait up to 5 minutes for first trigger

### Issue: Shopify API 401 error
**Solution:**
- Verify API token is correct
- Check API scopes include `read_orders` and `write_orders`
- Ensure app is installed in Shopify

### Issue: AI responses are generic
**Solution:**
- Check AI Agent nodes have correct prompts
- Verify Gemini API key is valid
- Ensure data is flowing from previous nodes

---

## Next Steps

✅ **Installation Complete!**

Now:
1. Read [CONFIGURATION.md](CONFIGURATION.md) to customize
2. Check [TROUBLESHOOTING.md](TROUBLESHOOTING.md) for common issues
3. Monitor performance for first week

---

## Need Help?

- 📖 Check [TROUBLESHOOTING.md](TROUBLESHOOTING.md)
- 🐛 Open an issue on GitHub
- 📧 Email: wajidjaved160@gmail.com