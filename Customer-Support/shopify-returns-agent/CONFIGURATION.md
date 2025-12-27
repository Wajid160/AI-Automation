# ⚙️ Configuration Guide

Customize the Returns Agent to match your store's policies and brand voice.

---

## Return Policy Configuration

### Location
File: **"Check Return Eligibility" node** (Code node)

### Settings
```javascript
const policy = {
  // How many days after order date customer can return
  returnWindowDays: 30,  // ← CUSTOMIZE THIS
  
  // How many days for exchanges (usually same as returns)
  exchangeWindowDays: 30,
  
  // Orders under this amount: auto-approve
  autoApproveUnder: 50,
  
  // Orders over this amount: require manual review
  requiresApprovalOver: 200,
  
  // Can customers return final sale items?
  allowFinalSaleReturns: false,
  
  // Refund original shipping cost?
  refundOriginalShipping: false
};
```

### Common Configurations

**Generous Policy (Fashion/Apparel):**
```javascript
returnWindowDays: 60,
autoApproveUnder: 100,
requiresApprovalOver: 500,
allowFinalSaleReturns: false,
refundOriginalShipping: true
```

**Standard Policy (Electronics):**
```javascript
returnWindowDays: 30,
autoApproveUnder: 50,
requiresApprovalOver: 200,
allowFinalSaleReturns: false,
refundOriginalShipping: false
```

**Strict Policy (Clearance Store):**
```javascript
returnWindowDays: 14,
autoApproveUnder: 25,
requiresApprovalOver: 100,
allowFinalSaleReturns: false,
refundOriginalShipping: false
```

---

## AI Response Customization

### Tone & Style

Each AI Agent node has a system prompt you can customize.

### AI Return Assistant (Approved)

**Location:** "AI Return Assistant..." node (TRUE path from eligibility check)

**Current tone:** Professional, friendly, helpful

**To make more casual:**
```
Change: "We're happy to confirm your return has been approved"
To: "Great news! Your return is all set"
```

**To make more formal:**
```
Change: "You'll receive a confirmation"
To: "You will receive formal confirmation via email"
```

### AI Return Assistant (Rejected)

**Location:** "AI Return Assistant..." node (FALSE path from eligibility check)

**Current tone:** Empathetic but firm

**To add compensation:**
Add to system prompt:
```
If outside return window, offer 15% discount code for next purchase
```

---

## Email Configuration

### Gmail Search Filter

**Location:** Gmail Trigger node → "Search" field

**Current:**
```
subject:(return OR refund OR exchange OR "send back" OR damaged OR "wrong item" OR defective)
```

**To add keywords:**
```
subject:(return OR refund OR exchange OR "send back" OR damaged OR "wrong item" OR defective OR "not happy" OR "change mind")
```

**To restrict to specific label:**
```
subject:(return OR refund) label:customer-service
```

---

## Advanced Customization

### Product-Specific Rules

**To block returns on specific product types:**

In "Check Return Eligibility" node, add:
```javascript
// Check if any items are non-returnable
const hasNonReturnableItems = orderData.line_items.some(item => {
  const productType = (item.product_type || '').toLowerCase();
  const nonReturnableTypes = ['gift card', 'digital download', 'custom'];
  return nonReturnableTypes.includes(productType);
});

if (hasNonReturnableItems) {
  eligible = false;
  ineligibleReasons.push('Order contains non-returnable items (gift cards, digital products)');
}
```

### Time-Based Rules

**To block returns on weekends/holidays:**
```javascript
const today = new Date();
const dayOfWeek = today.getDay(); // 0 = Sunday, 6 = Saturday

if (dayOfWeek === 0 || dayOfWeek === 6) {
  // Still accept request, but flag for Monday processing
  needsApproval = true;
}
```

### Customer Tier Rules

**VIP customers get extended window:**
```javascript
// Check if customer is VIP (has tag in Shopify)
const customerTags = orderData.customer.tags || '';
const isVIP = customerTags.includes('VIP');

const returnWindow = isVIP ? 60 : 30; // VIPs get 60 days
```

---

## Workflow Behavior

### Execution Frequency

**Gmail Trigger** checks every 2 minutes by default.

**To change:**
1. Click Gmail Trigger node
2. In trigger settings, adjust polling interval

**Recommendations:**
- High volume: Every 1 minute
- Medium volume: Every 2-5 minutes
- Low volume: Every 10 minutes

### Error Handling

**Current:** Errors stop execution

**To add retry logic:**
1. Click HTTP Request node
2. Options → Add Option → "Retry On Fail"
3. Set retries: 3
4. Set retry delay: 5000ms

---

## Notification Preferences

### Add Slack Notifications

**For high-value returns:**

1. Add Slack node after "Check Return Eligibility"
2. Add IF node: `{{ $json.order.total_price > 500 }}`
3. Send Slack message to merchant

### Add Email Notifications

**For manual review needed:**

1. Add Gmail Send node
2. To: your-email@store.com
3. Subject: "Return needs review: Order #{{ $json.order.name }}"

---

## Multi-Language Support

### Adding Spanish Responses

**In AI Agent nodes**, add to system prompt:
```
If customer email is in Spanish, respond in Spanish.
Otherwise respond in English.
```

Or create duplicate workflows per language.

---

## Seasonal Adjustments

### Holiday Extended Returns

**During holidays, extend return window:**
```javascript
// Check if current date is in holiday season
const today = new Date();
const month = today.getMonth(); // 0 = Jan, 11 = Dec

const isHolidaySeason = month === 10 || month === 11; // Nov-Dec
const returnWindow = isHolidaySeason ? 60 : 30;
```

---

## Testing Configuration Changes

**After any change:**

1. Save workflow
2. Create test order
3. Send test email
4. Verify response matches expectation
5. Check execution in n8n

**Best practice:** Test in development store first!

---

## Configuration Backup

**Save your settings:**

1. Export workflow: Menu → Export
2. Save to GitHub
3. Note custom settings in comments
4. Version control your changes

---

## Need Help?

Configuration questions:
- 📖 Check [TROUBLESHOOTING.md](TROUBLESHOOTING.md)
- 📧 Email: wajidjaved160@gmail.com