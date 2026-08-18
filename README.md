# AMISH — Fresh & Pure — Ordering Website

## ১. প্রথমে Firestore নিরাপত্তা নিয়ম ঠিক করুন (জরুরি!)

Firebase কনসোলে (console.firebase.google.com → আপনার প্রজেক্ট → Firestore Database → Rules) গিয়ে নিচের rules বসান:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /products/{document=**} {
      allow read: if true;
      allow write: if true;
    }
    match /orders/{document=**} {
      allow read: if true;
      allow write: if true;
    }
  }
}
```

⚠️ **"Test mode"-এ তৈরি করলে এই rules ৩০ দিন পর নিজে থেকে বন্ধ হয়ে যায়** — উপরের rules বসিয়ে Publish করলে সাইট চলতেই থাকবে। এই rules যে কাউকে ডাটা পড়তে/লিখতে দেয় (ছোট বিজনেসের জন্য প্র্যাক্টিক্যাল, কিন্তু সংবেদনশীল ডাটা রাখলে ভবিষ্যতে Firebase Authentication যোগ করা ভালো)।

## ২. লোকালি টেস্ট করুন (ঐচ্ছিক, কম্পিউটারে Node.js থাকলে)

```bash
npm install
npm run dev
```

ব্রাউজারে http://localhost:5173 খুলে চেক করুন।

## ৩. Vercel-এ ডিপ্লয় করুন (সবচেয়ে সহজ রাস্তা)

1. এই পুরো ফোল্ডারটা GitHub-এ একটা নতুন repository হিসেবে আপলোড করুন (GitHub Desktop অ্যাপ দিয়ে সহজে করা যায়, কোনো কমান্ড লাইন লাগবে না)
2. https://vercel.com এ গিয়ে GitHub দিয়ে সাইন আপ করুন (ফ্রি)
3. "Add New Project" → আপনার GitHub repo সিলেক্ট করুন → "Deploy" চাপুন
4. Vercel নিজে থেকে বুঝে নেবে এটা Vite প্রজেক্ট, বিল্ড করে একটা লিংক দেবে (যেমন amish-site.vercel.app) — এখানেই সাইট লাইভ হয়ে যাবে

## ৪. নিজের ডোমেইন (amish.com.bd) কানেক্ট করুন

1. Vercel প্রজেক্টে → Settings → Domains → "Add" → লিখুন `amish.com.bd`
2. Vercel কিছু DNS রেকর্ড দেখাবে (A record বা CNAME)
3. যেখান থেকে ডোমেইন কিনেছেন (Exonhost/DIANAHosting/BTCL ইত্যাদি) সেখানকার DNS সেটিংসে গিয়ে এই রেকর্ডগুলো বসান
4. ২৪-৪৮ ঘণ্টার মধ্যে ডোমেইন প্রোপাগেট হয়ে যাবে, SSL (https) অটোমেটিক লেগে যাবে

## ৫. Admin প্যানেল
সাইটের নিচে "Admin" বাটনে পাসকোড: `amish2026` — এটা `src/App.jsx` ফাইলে `ADMIN_PASS` কনস্ট্যান্ট খুঁজে বদলে ফেলতে পারেন।

## জানা প্রয়োজন
- এই সাইট Firebase Firestore-এ ডাটা সেভ করে (REST API দিয়ে, কোনো npm প্যাকেজ ছাড়াই) — তাই ডিপ্লয় করার আগে ধাপ ১ (Firestore rules) অবশ্যই করতে হবে, নাহলে অর্ডার/স্টক সেভ হবে না
- Firebase-এর ফ্রি টিয়ার (Spark Plan) ছোট বিজনেসের জন্য যথেষ্ট — দিনে হাজার হাজার রিকোয়েস্ট পর্যন্ত ফ্রি
