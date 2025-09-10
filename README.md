<p align="center">
  <img title="LuminPay" src="https://res.cloudinary.com/dqjm8yyzd/image/upload/v1757546948/logo-home_kcmfse.svg" width="280px"/>
</p>

# 💳 LuminPay NodeJS SDK

The **LuminPay NodeJS SDK** is the official integration library for [LuminPay](https://luminpay.co) — a **merchant-first crypto payment gateway**.  

It helps businesses and developers:  
- Accept **crypto payments** (stablecoins like USDC/USDT and crypto)  
- Provide **inline checkout experiences** or hosted checkout links  
- Enable **onramp (fiat → crypto)** and **offramp (crypto → fiat)**  

---

## ✨ Features

- 🛒 **Checkout Integration** – Inline widget or hosted checkout links  
- 🪪 **Wallet-as-a-Service (WaaS)** – Dynamically create wallets per merchant/customer  
- 🌍 **Multi-chain Support** – Solana, Ethereum, Tron, BSC, Bitcoin, Waves  
- 💸 **Onramp / Offramp** – Convert fiat to crypto and withdraw to bank/crypto addresses  
- 🔄 **Stablecoin Settlement** – Accept USDC/USDT, Crypto and auto-settle into your wallet  
- 📡 **Webhooks** – Receive real-time notifications on payments and settlement  
- 🧾 **Reconciliation** – API for transaction history and settlement reporting  

---

## 🚀 Installation

Install with npm:

```bash
npm install luminpay-node-sdk
```

Or with Yarn:

```bash
yarn add luminpay-node-sdk
```

---

## 🔧 Initialization

```javascript
const LuminPay = require("luminpay-node-sdk");

const luminpay = new LuminPay({
  merchantId: process.env.LUMINPAY_MERCHANT_ID,
  apiKey: process.env.LUMINPAY_API_KEY,
  environment: "sandbox", // or "production"
});
```

> 💡 Use **sandbox keys** for testing and **production keys** when going live.  
> Keys can be found in the [LuminPay Dashboard](https://dashboard.luminpay.co).

---

## 🛒 Checkout

### Hosted Checkout Link

```javascript
const session = await luminpay.Checkout.createSession({
  amount: 100,
  currency: "USDC",
  chain: "solana",
  reference: "ORDER-1234",
  customerEmail: "customer@example.com",
  successUrl: "https://yourapp.com/success",
  cancelUrl: "https://yourapp.com/cancel",
});

console.log("Checkout URL:", session.checkoutUrl);
```

### Inline Checkout

```html
<div id="luminpay-checkout"></div>

<script src="https://cdn.luminpay.io/inline-checkout.js"></script>
<script>
  new LuminPayInlineCheckout({
    merchantId: "your-merchant-id",
    apiKey: "your-api-key",
    amount: 50,
    currency: "USDT",
    chain: "ethereum",
    containerId: "luminpay-checkout",
    onSuccess: (res) => console.log("Payment success:", res),
    onFailure: (err) => console.error("Payment failed:", err),
  }).render();
</script>
```

---

## 🪪 Wallet-as-a-Service (WaaS)

```javascript
const wallet = await luminpay.Wallets.create({
  customerId: "customer_001",
  chains: ["solana", "ethereum", "tron"],
});

console.log(wallet.addresses);
```

Retrieve a wallet:

```javascript
const wallet = await luminpay.Wallets.get("customer_001");
console.log(wallet);
```

---

## 💸 Onramp (Fiat → Crypto)

```javascript
const onramp = await luminpay.Onramp.initiate({
  amount: 5000,
  currency: "NGN",
  coin: "USDT",
  accountId: "bank-account-id",
  reference: "ONRAMP-001",
});

console.log(onramp.status); // "pending"
```

---

## 🏦 Offramp (Crypto → Fiat)

Withdraw crypto to a bank account:

```javascript
const payout = await luminpay.Payouts.initiate({
  amount: 100,
  coin: "USDC",
  chain: "solana",
  destination: {
    type: "bank",
    bankCode: "044",
    accountNumber: "0123456789",
  },
  reference: "PAYOUT-001",
});

console.log(payout.status); // "processing"
```

Or to a crypto wallet:

```javascript
const payout = await luminpay.Payouts.initiate({
  amount: 100,
  coin: "USDC",
  chain: "ethereum",
  destination: {
    type: "wallet",
    address: "0x1234abcd...",
  },
});
```

---

## 📡 Webhooks

Example payload:

```json
{
  "event": "payment.success",
  "data": {
    "reference": "ORDER-1234",
    "amount": 100,
    "currency": "USDC",
    "chain": "solana",
    "status": "confirmed"
  }
}
```

Express example:

```javascript
app.post("/webhook", (req, res) => {
  const event = req.body.event;

  switch (event) {
    case "payment.success":
      console.log("Payment confirmed:", req.body.data);
      break;
    case "payment.failed":
      console.log("Payment failed:", req.body.data);
      break;
  }

  res.sendStatus(200);
});
```

---

## 🧾 Reconciliation

```javascript
const txs = await luminpay.Transactions.list({
  status: "confirmed",
  from: "2025-01-01",
  to: "2025-01-31",
});

console.log(txs);
```

Get a single transaction:

```javascript
const tx = await luminpay.Transactions.get("txn_12345");
console.log(tx);
```

---

## 📂 Repository Structure

```
luminpay-node-sdk/
├── src/
│   ├── checkout.js
│   ├── wallets.js
│   ├── payouts.js
│   ├── onramp.js
│   ├── transactions.js
│   └── utils.js
├── examples/
├── tests/
├── package.json
└── README.md
```

---

## 🧪 Examples

- [Checkout Session](./examples/checkout.js)  
- [Inline Checkout](./examples/inline.js)  
- [Wallet-as-a-Service](./examples/wallets.js)  
- [Onramp](./examples/onramp.js)  
- [Payouts](./examples/payouts.js)  
- [Webhooks](./examples/webhooks.js)  

---

Run tests:

```bash
yarn test
```

---

## 📜 License

MIT © 2025 [LuminPay](https://luminpay.io)

---

## 🌍 Links

- [Website](https://luminpay.co)  
- [Docs](https://docs.luminpay.co)  
- [Dashboard](https://dashboard.luminpay.co)  
- [NPM](https://www.npmjs.com/package/luminpay-node-sdk)  
- [GitHub Issues](https://github.com/luminpay/luminpay-node-sdk/issues)  
