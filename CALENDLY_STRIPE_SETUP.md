# Calendly & Stripe Integration Setup Guide

## ✅ Integration Complete

All pages have been connected to the booking system with Calendly and Stripe integration.

---

## 🔧 **Configuration Required**

### 1. **Calendly Setup**

The booking page is already configured with:
- Calendly URL: `https://calendly.com/dranju-mehta1/new-meeting`
- Event listener for booking confirmations
- Automatic data capture from Calendly bookings

**To Update Calendly URL:**
1. Edit `book-Appointment/index.html`
2. Find line ~76 in book-Appointment/index.html: `data-url="https://calendly.com/dranju-mehta1/new-meeting"`
3. Replace with your Calendly event URL

**Calendly Webhook Setup (Optional but Recommended):**
1. Go to Calendly → Settings → Integrations → Webhooks
2. Add webhook URL: `https://yourdomain.com/api/calendly-webhook`
3. Select events: `invitee.created`, `invitee.canceled`
4. This allows server-side booking confirmation

---

### 2. **Stripe Setup**

**Current Status:**
- Stripe.js is loaded
- Payment form is ready
- Demo mode available for testing

**To Enable Real Payments:**

1. **Get Your Stripe Keys:**
   - Sign up at https://stripe.com
   - Go to Developers → API keys
   - Copy your **Publishable Key** (starts with `pk_`)

2. **Update Publishable Key:**
   - Edit `book-Appointment/index.html`
   - Find line ~776: `const stripe = Stripe('pk_test_51P...');`
   - Replace with your actual publishable key

3. **Create Backend Endpoint:**
   You need a server endpoint to create payment intents securely.

   **Example (Node.js/Express):**
   ```javascript
   const stripe = require('stripe')('sk_test_YOUR_SECRET_KEY');
   
   app.post('/api/create-payment-intent', async (req, res) => {
     const { amount, currency, metadata } = req.body;
     
     const paymentIntent = await stripe.paymentIntents.create({
       amount: amount, // in pence (5000 = £50.00)
       currency: currency || 'gbp',
       metadata: metadata,
       automatic_payment_methods: {
         enabled: true,
       },
     });
     
     res.json({ clientSecret: paymentIntent.client_secret });
   });
   ```

4. **Update API Endpoint:**
   - Edit `book-Appointment/index.html`
   - Find line ~1018: `const response = await fetch('/api/create-payment-intent', ...)`
   - Replace `/api/create-payment-intent` with your actual backend URL

---

## 🔄 **Booking Flow**

### Current Flow:
1. **User selects treatment** → Step 1
2. **User clicks "View Calendar"** → Opens Calendly modal
3. **User books time in Calendly** → Event captured automatically
4. **User fills personal details** → Step 3
5. **User pays deposit via Stripe** → Step 4
6. **Confirmation shown** → Success page

### Calendly Event Capture:
The system automatically captures:
- Appointment date & time
- Patient email
- Patient name
- Phone number (if provided in Calendly)

---

## 📝 **All Pages Connected**

✅ **Main Pages Updated:**
- `index.html` - All booking links point to booking page
- `Anti-Wrinkle-Injections/index.html` - All booking links updated
- `Dermal-Fillers/index.html` - All booking links updated
- `Skin-Boosters/index.html` - All booking links updated
- `Real-Stories/index.html` - All booking links updated

**Note:** Contact section links (`#booking`) remain as anchor links to the contact section on the same page.

---

## 🚀 **Testing**

### Test Calendly Integration:
1. Go to booking page
2. Select a treatment
3. Click "View Calendar"
4. Book an appointment in Calendly
5. Verify data is captured automatically

### Test Stripe Integration (Demo Mode):
1. Complete booking flow
2. Enter test card: `4242 4242 4242 4242`
3. Any future expiry date
4. Any CVC
5. Payment will simulate successfully

### Test Stripe Integration (Live Mode):
1. Use real Stripe test keys
2. Use Stripe test cards: https://stripe.com/docs/testing
3. Verify payment intent creation
4. Check Stripe dashboard for payments

---

## 🔐 **Security Notes**

1. **Never expose your Stripe Secret Key** in frontend code
2. **Always use HTTPS** in production
3. **Validate all data** on the server side
4. **Store booking data** securely in a database
5. **Send confirmation emails** after successful payment

---

## 📧 **Email Confirmations**

After payment, you should:
1. Send confirmation email to patient
2. Send notification to clinic
3. Add booking to calendar system
4. Store in database

**Recommended Services:**
- SendGrid
- Mailgun
- AWS SES
- Nodemailer (Node.js)

---

## 🗄️ **Database Storage**

Store booking data including:
- Patient information
- Treatment details
- Appointment date/time
- Payment status
- Booking reference
- Stripe payment intent ID

**Recommended Databases:**
- PostgreSQL
- MongoDB
- Firebase
- Supabase

---

## ✅ **Next Steps**

1. ✅ Replace Calendly URL with your actual event URL
2. ✅ Get Stripe publishable key and update in code
3. ✅ Create backend server for payment intents
4. ✅ Set up database for booking storage
5. ✅ Configure email service for confirmations
6. ✅ Test complete booking flow
7. ✅ Deploy to production

---

## 📞 **Support**

For issues or questions:
- Calendly Docs: https://developer.calendly.com/
- Stripe Docs: https://stripe.com/docs
- Stripe Testing: https://stripe.com/docs/testing

---

*Last Updated: 2024*
