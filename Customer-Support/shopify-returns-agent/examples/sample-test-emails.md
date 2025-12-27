# 📧 Sample Test Emails

Use these to test your Returns Agent before going live.

---

## Test Scenario 1: Valid Return ✅

**Objective:** Test approval flow for eligible return

**Prerequisites:**
- Create test order in Shopify
- Mark as "Fulfilled" and "Paid"
- Note order number (e.g., #1007)

**Test Email:**
```
From: your.test.email@gmail.com
To: your.support.email@gmail.com
Subject: I want to return my order #1007

Hi,

I received my order yesterday but the product doesn't fit properly. 
I'd like to return it for a full refund.

Thank you,
Test Customer
```

**Expected Response:**
- ✅ Receives email within 10 seconds
- ✅ AI confirms return is approved
- ✅ Explains next steps (refund timeline)
- ✅ Professional, friendly tone
- ✅ References correct order number

**Sample Expected Response:**
```
Hello,

Thank you for reaching out to us. I've reviewed your request for 
order #1007, and I'm happy to confirm that your return has been approved.

Since you mentioned the product doesn't fit properly, I've noted that 
as the return reason. Our team will process your refund within 24 hours, 
and you'll receive confirmation via email.

Please allow 5-7 business days for the refund to appear in your account 
after processing.

If you have any questions, please let me know!

Best regards,
Customer Service Team
```

---

## Test Scenario 2: Ineligible Return (Outside Window) ❌

**Objective:** Test rejection flow for expired return window

**Prerequisites:**
- Use test order from >30 days ago
- Or manually edit date in "Check Return Eligibility" for testing

**Test Email:**
```
From: your.test.email@gmail.com
To: your.support.email@gmail.com
Subject: Return order #1001

Hello,

I'd like to return the item I ordered. It's been about 6 weeks 
but I haven't used it yet.

Thanks
```

**Expected Response:**
- ✅ Polite rejection
- ✅ Explains why (outside 30-day window)
- ✅ Offers alternatives (discount code, store credit)
- ✅ Empathetic tone

**Sample Expected Response:**
```
Dear Customer,

Thank you for contacting us about order #1001.

Unfortunately, I'm unable to process a return for this order as it 
was placed 42 days ago, which is outside our 30-day return window. 
Our return policy allows returns within 30 days of the order date.

However, as a gesture of goodwill, I'd like to offer you a 15% discount 
code for your next purchase: THANKYOU15

We truly appreciate your business and hope to serve you again soon.

Best regards,
Customer Service Team
```

---

## Test Scenario 3: Unfulfilled Order ❌

**Objective:** Test rejection with alternative suggestion

**Prerequisites:**
- Create test order but DON'T fulfill it
- Leave status as "Unfulfilled"

**Test Email:**
```
From: your.test.email@gmail.com
To: your.support.email@gmail.com
Subject: I need to return order #1005

Hi,

I want to return my order. I changed my mind.

Thank you
```

**Expected Response:**
- ✅ Explains order not yet shipped
- ✅ Suggests cancellation instead
- ✅ Provides instructions

**Sample Expected Response:**
```
Hello,

Thank you for reaching out about order #1005.

I've checked your order and I see it hasn't shipped yet! Since the 
order is still being prepared, you can cancel it instead of going 
through the return process.

Would you like me to cancel this order for you? If so, please 
confirm and I'll process the cancellation immediately. You'll receive 
a full refund to your original payment method.

Please let me know!

Best regards,
Customer Service Team
```

---

## Test Scenario 4: No Order Number 🔍

**Objective:** Test AI request for order information

**Test Email:**
```
From: your.test.email@gmail.com
To: your.support.email@gmail.com
Subject: Return request

Hello,

I bought a sweater last week and it doesn't fit. I want to return it.

Thanks
```

**Expected Response:**
- ✅ Friendly request for order number
- ✅ Explains where to find it
- ✅ Assures help once provided

**Sample Expected Response:**
```
Hello,

Thank you for reaching out! I'd be happy to help you with your return.

To process your return, I'll need your order number. You can find it in:
- The order confirmation email (subject: "Order #1234 confirmed")
- Your account order history on our website
- The format is usually #1234

Once you reply with your order number, I'll check your order details 
and assist you right away!

Best regards,
Customer Service Team
```

---

## Test Scenario 5: Invalid Order Number ❓

**Objective:** Test handling of non-existent orders

**Test Email:**
```
From: your.test.email@gmail.com
To: your.support.email@gmail.com
Subject: Return my order #9999

Hi,

I need to return order #9999. Please help.

Thanks
```

**Expected Response:**
- ✅ Politely says order not found
- ✅ Suggests checking order number
- ✅ Offers troubleshooting help

**Sample Expected Response:**
```
Hello,

Thank you for contacting us about order #9999.

I've searched our system but I'm unable to locate an order with that 
number. This could mean:
- The order number might be slightly different
- The order might be from a different store
- The email address used for the order is different

Could you please:
1. Double-check the order number in your confirmation email
2. Verify you're contacting the correct store
3. Let me know the email address used to place the order

I'm here to help once we locate your order!

Best regards,
Customer Service Team
```

---

## Test Scenario 6: High-Value Order 💎

**Objective:** Test handling of expensive items (needs approval)

**Prerequisites:**
- Create order over $200 (or your threshold)
- Fulfill it

**Test Email:**
```
From: your.test.email@gmail.com
To: your.support.email@gmail.com
Subject: Return #1008

I need to return the camera from order #1008. It's not what I expected.
```

**Expected Response:**
- ✅ Acknowledges return request
- ✅ Mentions will review (high value)
- ✅ Sets expectation (24-hour response)

**Sample Expected Response:**
```
Hello,

Thank you for reaching out about order #1008.

I understand you'd like to return the camera. Since this is a 
high-value item ($899.95), I've escalated your request to our 
returns specialist who will review it and contact you within 24 hours.

They'll provide you with detailed return instructions and arrange 
for a prepaid return label to ensure safe transit.

We appreciate your patience!

Best regards,
Customer Service Team
```

---

## Test Scenario 7: Multiple Items 📦

**Objective:** Test partial return handling

**Prerequisites:**
- Create order with 2-3 different products
- Fulfill it

**Test Email:**
```
From: your.test.email@gmail.com
To: your.support.email@gmail.com
Subject: Partial return for #1009

Hi,
 
I want to return just the blue shirt from order #1009, 
but keep the other items.

Thanks
```

**Expected Response:**
- ✅ Acknowledges partial return request
- ✅ Asks for clarification on specific items
- ✅ Explains process

---

## Test Scenario 8: Defective Product 🔧

**Objective:** Test priority handling for defects

**Test Email:**
```
From: your.test.email@gmail.com
To: your.support.email@gmail.com
Subject: Defective item in order #1010

Hello,

The product I received in order #1010 is defective. It doesn't 
work properly. I need a replacement or refund.

Please help urgently.
```

**Expected Response:**
- ✅ Prioritizes as defect (not just return)
- ✅ Apologizes for issue
- ✅ Offers replacement OR refund
- ✅ Fast-track processing

---

## Running All Tests

### Test Checklist

Run these tests in order:

- [ ] Test 1: Valid Return ✅
- [ ] Test 2: Outside Window ❌
- [ ] Test 3: Unfulfilled Order ❌
- [ ] Test 4: No Order Number 🔍
- [ ] Test 5: Invalid Order ❓
- [ ] Test 6: High-Value Order 💎
- [ ] Test 7: Multiple Items 📦
- [ ] Test 8: Defective Product 🔧

### Success Criteria

All tests should:
- ✅ Execute within 10 seconds
- ✅ Send appropriate response
- ✅ Show green in n8n Executions
- ✅ No errors in logs

### Failed Tests

If any test fails:
1. Check Executions tab in n8n
2. Identify failed node
3. Check Troubleshooting guide
4. Fix issue
5. Rerun test

---

## Custom Test Cases

Add your own based on your business:

**Example for Fashion Store:**
```
Subject: Wrong size for order #[NUMBER]
Body: I ordered a size M but need size L instead
Expected: Offer exchange process
```

**Example for Electronics:**
```
Subject: DOA - order #[NUMBER]
Body: Product arrived dead on arrival, won't turn on
Expected: Priority handling, replacement offered
```

---

## Production Testing

Before going live:

1. **Test with real team members**
   - Have 3-5 colleagues send test emails
   - Use different email clients (Gmail, Outlook, etc.)
   - Try mobile and desktop

2. **Test edge cases**
   - Very old orders
   - Very new orders (today)
   - Orders with special characters
   - International customers

3. **Monitor first week closely**
   - Check every execution
   - Read every AI response
   - Adjust prompts as needed

---

## Need More Test Cases?

Contact: wajidjaved160@gmail.com