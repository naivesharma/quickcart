# 🛒 QuickCart

> **Shop local. Skip the queue.**
> A click-and-collect app for small-town grocery stores — customers order ahead, stores pack it, customers walk in and pick up.

---

## 🗂 Project Structure

```
quickcart/
├── public/
│   └── index.html
├── src/
│   ├── firebase/
│   │   └── config.js          ← 🔑 Put your Firebase config here
│   ├── context/
│   │   ├── AuthContext.js     ← Login / register / user state
│   │   └── CartContext.js     ← Cart state management
│   ├── components/
│   │   └── Navbar.js          ← Shared top navigation
│   ├── pages/
│   │   ├── Splash.js          ← App loading screen
│   │   ├── Login.js
│   │   ├── Register.js
│   │   ├── customer/
│   │   │   ├── Home.js        ← Browse nearby stores
│   │   │   ├── StoreCatalog.js← Browse items, add to cart
│   │   │   ├── Checkout.js    ← Review & place order
│   │   │   ├── OrderConfirm.js← Real-time order tracking
│   │   │   └── MyOrders.js    ← Order history
│   │   └── merchant/
│   │       ├── Dashboard.js   ← Store overview & metrics
│   │       ├── Orders.js      ← Manage incoming orders
│   │       ├── ManageItems.js ← Add/edit catalog items
│   │       └── StoreSetup.js  ← Store profile settings
│   ├── App.js                 ← Router & protected routes
│   ├── index.js
│   └── index.css              ← Global styles & design tokens
├── firestore.rules            ← Security rules for Firebase
└── package.json
```

---

## 🚀 Setup Guide

### Step 1 — Clone & install

```bash
git clone <your-repo>
cd quickcart
npm install
```

### Step 2 — Create a Firebase project

1. Go to [firebase.google.com](https://firebase.google.com) → **Add project**
2. Name it `quickcart` (or anything you like)
3. Enable **Google Analytics** (optional)

### Step 3 — Enable Firebase services

In the Firebase Console:

| Service | How to enable |
|---------|--------------|
| **Authentication** | Build → Authentication → Get started → Email/Password → Enable |
| **Firestore** | Build → Firestore Database → Create database → Start in **test mode** |

### Step 4 — Add your Firebase config

1. Firebase Console → Project Settings (⚙️) → Your apps → **Web app** → Register app
2. Copy the `firebaseConfig` object
3. Paste it into `src/firebase/config.js`:

```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123...",
};
```

### Step 5 — Deploy Firestore security rules

```bash
npm install -g firebase-tools
firebase login
firebase init firestore    # select your project, use existing firestore.rules
firebase deploy --only firestore:rules
```

### Step 6 — Run the app

```bash
npm start
```

Open [http://localhost:3000](http://localhost:3000)

---

## 👤 How to use

### As a Customer
1. Register → select **Customer** → enter your city (e.g. `Sikar`)
2. Browse nearby stores in your city
3. Pick a store → add items to cart
4. Go to checkout → place order (pay at store)
5. Watch real-time order status on the confirmation screen
6. Walk to the store and pick up your packed order — no queue!

### As a Store Owner
1. Register → select **Store Owner** → enter your city
2. Complete store setup (name, area, emoji, packing time)
3. Add your catalog items (name, price, unit, category, emoji)
4. Incoming orders appear on your dashboard in real-time
5. Tap **Start packing** → **Mark ready** → customer sees status update live
6. Customer walks in, picks up, pay done!

---

## 🗄 Firestore Data Model

```
users/{uid}
  name, email, role (customer|merchant), city

stores/{uid}           ← same UID as merchant user
  name, area, city, emoji, readyTime, ownerId, phone, description

stores/{uid}/items/{itemId}
  name, price, unit, category, emoji

orders/{orderId}
  customerId, customerName, storeId, items[], total, note
  status: new → packing → ready → picked_up
  createdAt
```

---

## 🛣 Roadmap (next features)

- [ ] Push notifications when order is ready (Firebase Cloud Messaging)
- [ ] SMS alert via Twilio
- [ ] UPI payment integration (Razorpay)
- [ ] Store search by item name across all stores
- [ ] Customer ratings for stores
- [ ] Multi-city admin panel
- [ ] PWA / installable mobile app

---

## 🛠 Tech Stack

| Layer | Tech |
|-------|------|
| Frontend | React 18, React Router v6 |
| Styling | CSS Modules + custom design tokens |
| Backend | Firebase (Auth + Firestore) |
| Real-time | Firestore `onSnapshot` listeners |
| Fonts | Syne (display) + Outfit (body) |
