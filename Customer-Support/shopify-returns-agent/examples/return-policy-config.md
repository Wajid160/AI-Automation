# 📜 Return Policy Configuration Examples

This file contains copy-pasteable configuration objects for the **"Check Return Eligibility"** node in the workflow.

Copy the `const policy = { ... };` block that matches your business needs and paste it into the Code node.

---

## 1. Standard E-commerce (Default)
*Balanced policy suitable for most retail stores.*

```javascript
const policy = {
  // 30 days from delivery
  returnWindowDays: 30,
  
  // 30 days for exchanges
  exchangeWindowDays: 30,
  
  // Auto-approve low risk returns (under $50) to save time
  autoApproveUnder: 50,
  
  // Flag expensive returns (over $200) for human review
  requiresApprovalOver: 200,
  
  // Do not allow returning "Final Sale" tagged items
  allowFinalSaleReturns: false,
  
  // Customer pays for return shipping (no refund of original shipping)
  refundOriginalShipping: false
};
```

---

## 2. Fast Fashion / High Volume
*Generous window to encourage sales, but stricter on final sale and shipping.*

```javascript
const policy = {
  // Longer window for trying on clothes
  returnWindowDays: 45,
  
  exchangeWindowDays: 60,
  
  // Higher auto-approval to handle volume
  autoApproveUnder: 100,
  
  // Only review very large orders
  requiresApprovalOver: 500,
  
  // STRICT: No returns on clearance
  allowFinalSaleReturns: false,
  
  // Store keeps original shipping cost
  refundOriginalShipping: false
};
```

---

## 3. Luxury / Boutique
*Tight control, human review for most items, premium service.*

```javascript
const policy = {
  // Shorter window for high-value items
  returnWindowDays: 14,
  
  exchangeWindowDays: 14,
  
  // NO auto-approval - review everything
  autoApproveUnder: 0,
  
  // Review everything (redundant with above, but clear)
  requiresApprovalOver: 0,
  
  // Never allow final sale returns
  allowFinalSaleReturns: false,
  
  // Premium service: refund everything including shipping
  refundOriginalShipping: true
};
```

---

## 4. Electronics / Tech
*Strict windows and approval needed due to potential damage/fraud.*

```javascript
const policy = {
  // Standard strict window
  returnWindowDays: 30,
  
  exchangeWindowDays: 30,
  
  // Very low auto-approve (cables, cases only)
  autoApproveUnder: 20,
  
  // Review almost everything to check for serial swapping risk
  requiresApprovalOver: 50,
  
  allowFinalSaleReturns: false,
  
  refundOriginalShipping: false
};
```

---

## 5. Holiday Season (Temporary)
*Extended windows for Q4 gifting.*

```javascript
const policy = {
  // Extended window for gifts (Nov purchases valid till Jan 31)
  returnWindowDays: 90,
  
  exchangeWindowDays: 90,
  
  // Relaxed approval to handle post-Xmas rush
  autoApproveUnder: 75,
  
  requiresApprovalOver: 300,
  
  allowFinalSaleReturns: false, // Or true if "Gift"
  
  refundOriginalShipping: false
};
```

---

## How to Apply

1. Open n8n workflow
2. Double-click **"Check Return Eligibility"** node (Code node)
3. Locate the `const policy` section at the top
4. Replace it with one of the examples above
5. Save the node and **Activate** the workflow
