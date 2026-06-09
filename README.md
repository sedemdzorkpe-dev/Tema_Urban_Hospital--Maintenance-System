# 🏥 Tema Urban Hospital — Maintenance Fault Reporting System
## Complete Setup & Deployment Guide

---

## 📋 OVERVIEW

This system provides:
- **Fault Reporting** — All 40 hospital units can report maintenance faults
- **Memo Generator** — Official inter-departmental memos with signatures
- **Admin Dashboard** — Live charts, monthly/half-year/annual summaries
- **Excel Export** — For management analysis and procurement planning
- **Firebase Backend** — Real-time data synced across all devices
- **GitHub Pages Hosting** — Free, reliable, accessible from any device

---

## STEP 1 — SET UP FIREBASE (Free)

### 1.1 Create Firebase Project
1. Go to **https://console.firebase.google.com**
2. Click **"Add project"**
3. Name it: `tema-urban-hospital-maintenance`
4. Disable Google Analytics (not needed) → Click **"Create project"**

### 1.2 Enable Firestore Database
1. In the left menu click **"Firestore Database"**
2. Click **"Create database"**
3. Choose **"Start in test mode"** (you can tighten later)
4. Select region: **europe-west1** (closest to Ghana) → Click **"Enable"**

### 1.3 Get Your Firebase Config
1. Click the ⚙️ gear icon → **"Project settings"**
2. Scroll down to **"Your apps"** → Click the **`</>`** (Web) icon
3. App nickname: `TUH Maintenance App` → Click **"Register app"**
4. Copy the `firebaseConfig` object — it looks like:
```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "tema-urban-hospital-maintenance.firebaseapp.com",
  projectId: "tema-urban-hospital-maintenance",
  storageBucket: "tema-urban-hospital-maintenance.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

### 1.4 Update index.html with Your Config
Open `index.html` and find this section (around line 510):
```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  ...
```
Replace ALL the placeholder values with your actual Firebase config values.

### 1.5 Deploy Firestore Security Rules
Install Firebase CLI and deploy rules:
```bash
npm install -g firebase-tools
firebase login
firebase init firestore
firebase deploy --only firestore:rules
```

---

## STEP 2 — SET UP GITHUB REPOSITORY

### 2.1 Create Repository
1. Go to **https://github.com** — log in or create account
2. Click **"New repository"** (green button)
3. Name: `tema-urban-hospital-maintenance`
4. Set to **Public** (required for free GitHub Pages)
5. Click **"Create repository"**

### 2.2 Upload Your Files
**Option A — Using GitHub website (easiest):**
1. On your new repo page, click **"uploading an existing file"**
2. Drag and drop ALL files including the `.github` folder
3. Commit message: `Initial deployment — TUH Maintenance System`
4. Click **"Commit changes"**

**Option B — Using Git (if installed):**
```bash
cd tema-maintenance
git init
git add .
git commit -m "Initial deployment — TUH Maintenance System"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/tema-urban-hospital-maintenance.git
git push -u origin main
```

### 2.3 Enable GitHub Pages
1. Go to your repo → **Settings** tab
2. Left menu → **"Pages"**
3. Under **"Source"**, select **"GitHub Actions"**
4. The deploy workflow will run automatically on every push

### 2.4 Your Live URL
After deployment (takes ~2 minutes), your app will be live at:
```
https://YOUR-GITHUB-USERNAME.github.io/tema-urban-hospital-maintenance/
```

---

## STEP 3 — SHARE WITH UNITS

### Login Credentials

| Role | Unit Selection | Password |
|------|---------------|----------|
| **Admin (Maintenance Officer)** | Maintenance | `admin2024` |
| **All other staff** | Their unit | `tema2024` |

### Recommended: Change Passwords
Open `index.html` and find:
```javascript
const ADMIN_PASS = "admin2024";
const STAFF_PASS = "tema2024";
```
Change to strong passwords before deploying to production.

### Sharing the App
Send this URL to all unit heads:
```
https://YOUR-USERNAME.github.io/tema-urban-hospital-maintenance/
```
The app works on:
- ✅ Desktop computers
- ✅ Laptops
- ✅ Tablets
- ✅ Mobile phones
- ✅ Any modern web browser (Chrome, Firefox, Edge, Safari)

---

## STEP 4 — USING THE SYSTEM

### For Unit Staff:
1. Open the app URL in any browser
2. Select your unit from the dropdown
3. Enter password: **tema2024**
4. Click **"Report Fault"** tab
5. Fill in your name, phone, select fault category and specific item
6. Set priority (Low / Medium / High)
7. Click **"Submit Fault Report"**

### For Generating Memos:
1. Click the **"Memo Pad"** tab
2. Fill in From/To units, date, reference number, subject
3. Write the memo body
4. Click **"Generate Memo Preview"**
5. Fill in requester and approver signatures (name, role, phone)
6. Click **"Print Memo"**

### For Maintenance Officer (Admin):
1. Log in with unit = **Maintenance**, password = **admin2024**
2. Extra tabs appear: **Dashboard** and **All Reports**
3. **Dashboard** — View live charts filtered by month / 6 months / year / custom date
4. **All Reports** — See every submission, update status, search/filter
5. **Export Excel** — Download full data for further analysis

---

## STEP 5 — OPTIONAL ENHANCEMENTS

### Add Hospital Logo Image
Replace the SVG logo in `index.html` by adding an `<img>` tag:
```html
<img src="logo.png" style="width:60px;height:60px;object-fit:contain">
```
Upload your `logo.png` to the repository alongside `index.html`.

### Custom Domain (Optional)
1. Buy a domain (e.g., `maintenance.temaurban.gov.gh`)
2. In GitHub Pages settings → Add custom domain
3. Follow DNS configuration instructions

### Email Notifications (Optional)
Use Firebase Functions + SendGrid or EmailJS to send email alerts to the maintenance officer when new faults are submitted.

### Per-Unit Passwords (Optional)
Replace the single `STAFF_PASS` with a lookup object:
```javascript
const UNIT_PASSWORDS = {
  "Laboratory": "lab2024",
  "OPD Unit": "opd2024",
  // ... etc
};
```

---

## 📁 FILE STRUCTURE

```
tema-maintenance/
├── index.html              ← Main application (single file)
├── firebase.json           ← Firebase hosting config
├── firestore.rules         ← Firestore security rules
├── firestore.indexes.json  ← Firestore query indexes
├── README.md               ← This guide
└── .github/
    └── workflows/
        └── deploy.yml      ← Auto-deploy to GitHub Pages
```

---

## 🔧 TROUBLESHOOTING

**App shows "Firebase not configured":**
→ You haven't replaced the placeholder Firebase config in `index.html`. See Step 1.4.

**Data not saving across devices:**
→ Firebase config is wrong. Check Project ID and API key.

**GitHub Pages not deploying:**
→ Go to repo → Actions tab → Check if workflow ran. If failed, check error message.

**Can't log in:**
→ Make sure you selected a unit AND entered the correct password.

**App looks broken on phone:**
→ Clear browser cache. The app is fully responsive on all screen sizes.

---

## 📞 SUPPORT

For technical support with this system, contact your IT department or the system developer.

System built for: **Tema Urban Hospital, Maintenance Department**
Ghana Health Service · Greater Accra Region

---
*Last updated: 2024*
