# 📚 PW Vidyapeeth Maharashtra — Sheet Directory

A private, internal web app for **PW Vidyapeeth Maharashtra** team members to browse, search, pin, and access Google Sheets — with a full authentication system (login, signup, forgot password via OTP).

---

## 🖥️ Live Preview

> Deploy via GitLab Pages or any static hosting (Netlify, Vercel, GitHub Pages).  
> Access is restricted to `@pw.live` email accounts only.

---

## 📁 Project Structure

```
maharastra-sheet-directory/
│
├── index.html       → Main app (Sheet Directory dashboard)
├── login.html       → Login page
├── signup.html      → Sign up / Register page
├── forgot.html      → Forgot password (OTP-based reset)
│
├── app.js           → Core app logic (fetch sheets, render, filter, pin, search)
├── auth.js          → Sign-in logic (validates against Google Sheet user DB)
│
├── style.css        → All styles — themes, layout, components
└── README.md        → This file
```

---

## ✨ Features

### 🔐 Authentication
| Feature | Details |
|---|---|
| Sign Up | Register with `@pw.live` email + password (min 6 chars) |
| Login | Credentials verified against a private Google Sheet user database |
| Forgot Password | 3-step OTP flow — enter email → verify OTP → set new password |
| Session | Stored in `localStorage` (`loggedUser` key) |
| Route Guard | `index.html` redirects to `login.html` if not logged in |

> ⚠️ Only `@pw.live` email addresses are allowed to register or log in.

---

### 📋 Sheet Directory
- Fetches sheet data live from a **published Google Sheet (CSV)**
- Each sheet card shows: **name, owner, last modified date, category badge**
- Click any card or **Open ↗** button to open the sheet in a new tab

### 🔍 Search
- Real-time search by sheet name
- Works in combination with the active sidebar filter

### 📌 Pinning
- Pin any sheet for quick access — pinned sheets appear in a dedicated **Pinned** section at the top
- Pins are saved per-user in `localStorage`
- Unpin from the pinned card directly

### 🗂️ Sidebar Filters
| Filter | Description |
|---|---|
| All Sheets | Shows every sheet |
| Pinned | Shows only your pinned sheets |
| Analysis | Sheets with "analysis" in the name |
| B2B | B2B related sheets |
| Acad | Academic sheets |
| BWS | BWS sheets |
| Comms → Gen / PTM / Orientation / Batch Start | Communication sub-categories |
| MH → Maharashtra | Maharashtra region sheets |
| 🏢 ERP PW | Opens `https://erp.pw.live/home` in a new tab |
| 📋 PW Web Report | Opens `https://ambikeshishere.github.io/PW-Web/` in a new tab |

### 🎨 Themes
Three built-in themes, toggled from the sidebar bottom:
| Theme | Description |
|---|---|
| 🌙 Dark | Default dark mode (deep navy + orange accent) |
| ☀️ Light | Clean light mode |
| 🌿 Eve | Warm sepia tone — easy on eyes |

Theme preference is saved in `localStorage` and persists across sessions.

---

## ⚙️ Configuration

### 1. Google Sheet — User Database (`auth.js`)
```js
const USER_DB_URL = "https://docs.google.com/spreadsheets/d/e/YOUR_SHEET_ID/pub?gid=0&single=true&output=csv";
```
This sheet should have the following columns:
| Column A | Column B |
|---|---|
| email | password |
| user@pw.live | yourpassword |

> **Important:** Publish the sheet as CSV (`File → Share → Publish to web`).

---

### 2. Google Sheet — Sheet Directory (`app.js`)
```js
const CSV_URL = "https://docs.google.com/spreadsheets/d/e/YOUR_SHEET_ID/pub?gid=0&single=true&output=csv";
```
This sheet should have the following columns:
| Col A | Col B | Col C | Col D | Col E | Col F |
|---|---|---|---|---|---|
| Sheet Name | Open Link | Owner | Last Modified | Last Modified Date | Web Link |

---

### 3. Google Apps Script — Signup & OTP (`signup.html`, `forgot.html`)
```js
const SCRIPT_URL = "https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec";
```
The Apps Script handles:
- **Signup** — writes new user to the user database sheet
- **Send OTP** — generates and emails a 6-digit OTP
- **Verify OTP** — validates the OTP
- **Reset Password** — updates the password in the sheet

---

## 🚀 Getting Started

### Option 1 — Run Locally
Just open `login.html` in your browser. No build step required — it's pure HTML/CSS/JS.

```bash
# Clone the repo
git clone https://gitlab.com/Ambikesh/maharastra-sheet-directory.git
cd maharastra-sheet-directory

# Open in browser
open login.html
```

### Option 2 — Deploy to GitLab Pages
1. Push to your GitLab repo
2. Add a `.gitlab-ci.yml` file:

```yaml
pages:
  stage: deploy
  script:
    - mkdir .public
    - cp -r * .public
    - mv .public public
  artifacts:
    paths:
      - public
  only:
    - main
```

3. Your app will be live at `https://your-username.gitlab.io/maharastra-sheet-directory/login.html`

---

## 🔒 Security Notes

> This project is designed for **internal team use only**. A few things to keep in mind:

- Passwords are currently stored as plain text in Google Sheets — consider hashing them in the Apps Script before storing
- The Google Sheet user DB URL is public (by design, for CSV access) — avoid storing sensitive data beyond email + password
- Session management uses `localStorage` — suitable for internal tools, not for high-security applications

---

## 🧩 Tech Stack

| Technology | Usage |
|---|---|
| HTML5 / CSS3 | Structure & styling |
| Vanilla JavaScript | App logic, no frameworks |
| Google Sheets (CSV) | Sheet data source + user database |
| Google Apps Script | Backend for signup, OTP, password reset |
| Google Fonts (Syne + DM Sans) | Typography |
| localStorage | Session + theme + pin persistence |

---

## 📸 Pages Overview

| Page | File | Description |
|---|---|---|
| Login | `login.html` | Email + password sign in |
| Sign Up | `signup.html` | New account registration with password strength meter |
| Forgot Password | `forgot.html` | 3-step OTP-based password reset |
| Dashboard | `index.html` | Sheet browser with sidebar, search, filters, pinning |

---

## 👤 Author

**Ambikesh Srivastava**  
GitLab: [@Ambikesh](https://gitlab.com/Ambikesh)

---

## 📄 License

This project is for internal use at **PW Vidyapeeth Maharashtra**.  
Not intended for public distribution.