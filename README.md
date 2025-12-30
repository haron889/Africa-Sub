# Africa
**Developer-first payments infrastructure for Africa**

Paycraft is a Stripe-inspired payments API designed for African developers.  
It makes it easy to accept **M-Pesa and card payments** using clean APIs, predictable webhooks, and a powerful sandbox environment.

> Built for developers. Designed for reliability. Kenya-first, Africa-ready.

---

## ✨ Features

- 🇰🇪 **M-Pesa STK Push** (Paybill & Till ready)
- 💳 Card payments (Visa & Mastercard – roadmap)
- 🧪 **Full sandbox mode** (no real money)
- 🔁 Webhooks with retries & signature verification
- 🔐 Secure API key authentication
- ♻️ Idempotent requests (no double charges)
- 📚 Stripe-style developer documentation

---

## 🧠 Philosophy (Why Paycraft?)

Most payment gateways in Africa are built for **merchants and sales teams**.

Paycraft is built for:
- Developers
- Startups
- SaaS products
- Marketplaces
- Fintech MVPs

**Docs before dashboards. APIs before UI. Reliability before features.**

---

## 🏗️ Architecture Overview
Client App ↓ Paycraft API ↓ Sandbox Engine (test mode) OR PSP / M-Pesa (live mode) ↓ Webhook System ↓ Merchant Server
Copy code

- Test mode never touches real M-Pesa
- Live mode integrates via licensed PSPs
- Webhooks are the source of truth

---

## 🚀 Quickstart

### 1. Get API Keys

Create an account and get your keys:

- `sk_test_xxx` → Sandbox
- `sk_live_xxx` → Production

⚠️ Never expose secret keys in frontend code.

---

### 2. Install SDK (Node.js)

```bash
npm install paycraft
3. Initialize Client
Copy code
Js
import Paycraft from "paycraft";

const paycraft = new Paycraft({
  secretKey: process.env.PAYCRAFT_SECRET_KEY
});
4. Create an M-Pesa Payment
Copy code
Js
const payment = await paycraft.payments.create({
  amount: 1500,
  currency: "KES",
  method: "mpesa",
  phone: "254712345678",
  reference: "ORDER_1021",
  callback_url: "https://example.com/webhooks/paycraft"
});
Customer receives an STK push to confirm the payment.
🔄 Payment Lifecycle
Copy code

created → pending → successful
                    failed
                    expired
successful is final and irreversible
All state changes emit webhook events
🧪 Sandbox Mode
Sandbox mode simulates real payments without charging users.
Test Phone Numbers
Phone
Result
254700000001
Successful payment
254700000002
Insufficient funds
254700000003
User cancelled
254700000004
Timeout / expired
Sandbox behaves exactly like live mode, including webhooks.
🔔 Webhooks
Paycraft sends webhooks for all important events:
payment.pending
payment.successful
payment.failed
payment.expired
Example Webhook Payload
Copy code
Json
{
  "event": "payment.successful",
  "livemode": false,
  "data": {
    "id": "pay_test_123",
    "amount": 1500,
    "currency": "KES"
  }
}
⚠️ Always verify webhook signatures before trusting data.
🔐 Authentication
All requests use Bearer authentication:
Copy code
Http
Authorization: Bearer sk_test_xxx
Requests without valid keys will fail with authentication_failed.
❌ Error Handling
Errors are human-readable and actionable.
Copy code
Json
{
  "error": {
    "code": "invalid_phone",
    "message": "Phone number must be in international format: 2547XXXXXXXX"
  }
}
Common error codes:
invalid_request
authentication_failed
insufficient_funds
provider_error
📦 Project Structure
Copy code
Txt
.
├── api/
│   ├── payments/
│   ├── webhooks/
│   ├── subscriptions/
│   └── sandbox/
├── sdk/
│   ├── node/
│   └── python/
├── docs/
├── database/
└── README.md
🛠️ Tech Stack
Backend: Node.js (Fastify)
Database: PostgreSQL
Queue: Redis / Bull
Webhooks: HMAC signatures
Hosting: Render / Fly.io
Docs: Docusaurus / ReadMe
🧭 Roadmap
[ ] Card payments
[ ] Subscriptions & recurring billing
[ ] Marketplace payouts
[ ] Fraud & risk signals
[ ] Multi-country expansion (NG, GH, ZA)
🤝 Contributing
We welcome contributions from developers who care about:
Clean APIs
Reliability
Developer experience
Steps:
Fork the repo
Create a feature branch
Write tests
Open a PR
📄 License
MIT License © Paycraft
🌍 Vision
To become the financial infrastructure layer for African software,
starting with Kenya, built the right way — developer-first.
📬 Contact
Issues & discussions: GitHub Issues
Early adopters: Open an issue with [early-access]
If Stripe were built in Africa, it would look like this.
