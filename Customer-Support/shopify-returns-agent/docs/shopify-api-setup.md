# 🛍️ Shopify API Setup - Detailed Guide

Complete walkthrough for setting up Shopify Admin API access.

---

## Overview

The Returns Agent needs to:
- Read order information
- Check order status
- Verify fulfillment
- (Future: Create refunds)

This requires Shopify Admin API access.

---

## Step-by-Step Setup

### 1. Access Shopify Admin

1. **Log in to your Shopify store**
```
   https://YOUR-STORE.myshopify.com/admin
```

2. **Navigate to Settings**
   - Click ⚙️ Settings (bottom left)

### 2. Access Apps Section

3. **Go to Apps and sales channels**
   - Settings → Apps and sales channels

4. **Click "Develop apps"**
   - Located in top right corner
   - If you don't see it, you might need store owner permissions

### 3. Create Custom App

5. **Click "Create an app"**

6. **App details:**
```
   App name: Returns Automation
   App developer: Your Name/Company
```

7. **Click "Create app"**

### 4. Configure API Scopes

8. **Click "Configure Admin API scopes"**

9. **Scroll through and select these scopes:**

   **Required scopes:**
   - ✅ **read_orders** - Read order information
   - ✅ **write_orders** - Update order notes
   - ✅ **read_products** - Read product details
   - ✅ **read_customers** - Read customer info
   - ✅ **read_fulfillments** - Check shipping status

   **Optional (for future features):**
   - ⬜ **write_fulfillments** - Update shipping
   - ⬜ **write_returns** - Create returns (Shopify Plus only)
   - ⬜ **read_inventory** - Check stock levels

10. **Click "Save"**

### 5. Install the App

11. **Click "Install app"** (top right)

12. **Review permissions**
    - Confirm you're okay with the scopes
    - Click "Install app"

### 6. Get API Credentials

13. **Click "API credentials" tab**

14. **Reveal Admin API access token**
    - Click "Reveal token once"
    - ⚠️ **IMPORTANT:** You can only see this ONCE!
    - Copy the token immediately
    - Store in password manager

15. **Token format:**
```
    shpat_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

16. **Note your API details:**
```
    API key: (optional, not needed for this project)
    Admin API access token: shpat_xxx... ← YOU NEED THIS
    API secret key: (optional)
```

### 7. Note Store Details

17. **Your store subdomain:**
    - From URL: `https://YOUR-STORE.myshopify.com`
    - Store name: `YOUR-STORE`
    - Example: `my-clothing-store`

---

## Testing API Access

### Test 1: Get Shop Info

**Using cURL:**
```bash
curl -X GET \
  'https://YOUR-STORE.myshopify.com/admin/api/2024-01/shop.json' \
  -H 'X-Shopify-Access-Token: shpat_YOUR_TOKEN'
```

**Expected response:**
```json
{
  "shop": {
    "id": 123456789,
    "name": "Your Store",
    "email": "your@email.com",
    ...
  }
}
```

### Test 2: Get Recent Orders
```bash
curl -X GET \
  'https://YOUR-STORE.myshopify.com/admin/api/2024-01/orders.json?limit=5' \
  -H 'X-Shopify-Access-Token: shpat_YOUR_TOKEN'
```

**Expected response:**
```json
{
  "orders": [
    {
      "id": 5678901234,
      "name": "#1001",
      "email": "customer@example.com",
      ...
    }
  ]
}
```

### Test 3: Get Specific Order
```bash
curl -X GET \
  'https://YOUR-STORE.myshopify.com/admin/api/2024-01/orders.json?name=%231001' \
  -H 'X-Shopify-Access-Token: shpat_YOUR_TOKEN'
```

**If all 3 tests work: ✅ API setup complete!**

---

## Troubleshooting

### Error: "API credentials are missing or invalid"

**Solution:**
1. Verify token copied correctly (no spaces)
2. Check app is installed
3. Regenerate token if needed

### Error: "403 Forbidden"

**Solution:**
1. Check API scopes
2. Ensure `read_orders` is enabled
3. Reinstall app after adding scopes

### Error: "404 Not Found"

**Solution:**
1. Check store subdomain is correct
2. Verify API version (use 2024-01 or later)
3. Check URL format

---

## Security Best Practices

### Protect Your API Token

✅ **DO:**
- Store in password manager (1Password, LastPass)
- Use environment variables in production
- Rotate token every 90 days
- Use minimal required scopes

❌ **DON'T:**
- Share via email or Slack
- Commit to Git/GitHub
- Use in client-side code
- Give to untrusted apps

### Token Rotation

**Rotate your token quarterly:**
1. Generate new token in Shopify
2. Update in n8n credentials
3. Test workflow still works
4. Delete old token from Shopify

---

## API Limits

**Shopify API rate limits:**

| Plan | Requests/Second | Burst |
|------|----------------|-------|
| Basic | 2 | 40 |
| Shopify | 4 | 80 |
+| Advanced | 6 | 120 |
+| Plus | 8+ | Custom |

**For Returns Agent:**
- Typical usage: 1-2 requests per email
- Well under limits for most stores
- Monitor if processing 100+ returns/day

---

## API Versions

**Current version:** 2024-01

**Update API version:**
1. Check Shopify docs for latest stable version
2. Update URLs in n8n HTTP Request nodes
3. Test thoroughly before deploying

**Version format:**
```
/admin/api/YYYY-MM/orders.json
```

Example:
```
/admin/api/2024-01/orders.json  ← Use this format
```

---

## Webhooks (Optional)

**For real-time notifications:**

Instead of polling Gmail, you can use Shopify webhooks:

1. Create webhook in Shopify
2. Topic: `orders/create`
3. Point to n8n webhook URL
4. Trigger workflow on new orders

**Benefits:**
- Instant notifications
- No polling delay
- More efficient

**Note:** Requires webhook node in n8n (not covered in v1.0)

---

## Resources

- **Shopify API Docs:** https://shopify.dev/docs/api/admin-rest
- **API Reference:** https://shopify.dev/docs/api/admin-rest/2024-01/resources/order
- **n8n Shopify Nodes:** https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.shopify/

---

## Need Help?

API setup questions:
- 📖 Check official Shopify docs
- 💬 Ask in Shopify community forums
- 📧 Contact: wajidjaved160@gmail.com