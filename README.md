# 🔐 SecureVault

### Your files, encrypted before they ever leave your browser.

---

## 🌟 The Idea

Most "secure" file sharing services ask you to trust them — trust that they won't peek at your files, trust that their servers are never breached, trust that "we encrypt everything" actually means what you think it means.

**SecureVault flips that model.**

Every file is encrypted **inside your browser**, using your device's own cryptographic engine (the Web Crypto API), *before* a single byte is uploaded. The server stores only ciphertext. Even if the database were compromised, your files would be unreadable noise without the right key.

This isn't "encryption as a checkbox" — it's encryption as the foundation.

---

## ✨ What It Does

### 🔑 Authentication
Sign in with email/password (bcrypt-hashed) or Google Sign-In, restricted to verified organizational accounts. Includes a full forgot-password flow with email OTP verification.

### 🛡️ Client-Side Encryption Engine
Powered entirely by the **Web Crypto API**:
- **AES-256-GCM** for fast, authenticated file encryption
- **RSA-OAEP (2048-bit)** for secure key exchange between sender and recipient
- **SHA-256** for password-derived keys

No server-side encryption libraries. No plaintext ever transmitted.

### 📤 Send Encrypted Files to Anyone
Upload a file, enter a recipient's email, and SecureVault:
1. Generates a fresh RSA key pair for that transfer
2. Encrypts your file with AES, then wraps the AES key with RSA
3. Emails the recipient a one-time OTP
4. Auto-deletes the encrypted file and keys from storage after retrieval

### 💾 Personal Encrypted Vault
Upload files to your own private storage — encrypted with AES-256, retrievable only via OTP.

### 🔓 Standalone Encrypt/Decrypt Tool
No account needed. Drop any file in, set a password, and get back an encrypted file you can decrypt later — entirely offline-capable, all in-browser.

### 🎨 Polished Multi-Page Experience
A complete site experience: landing page, about, services, pricing, reviews, contact, and a dashboard-style profile page — all responsive and themed with smooth gradients and modern UI patterns.

---

## 🏗️ Architecture

┌─────────────────────────────────────────────┐
│                  BROWSER                     │
│  ┌─────────────────────────────────────┐    │
│  │   Web Crypto API                     │    │
│  │   AES-256-GCM  +  RSA-OAEP-2048      │    │
│  │   File encrypted/decrypted HERE      │    │
│  └─────────────────────────────────────┘    │
│              ↓ ciphertext only ↓             │
└─────────────────────────────────────────────┘
                    │
                    ▼
        ┌───────────────────────┐
        │       Supabase         │
        │  Auth · Storage · DB   │
        │   (stores ciphertext)  │
        └───────────────────────┘
                    │
                    ▼
        ┌───────────────────────┐
        │       EmailJS          │
        │  OTP & notifications   │
        └───────────────────────┘

---

## 🧰 Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | HTML5, CSS3, Vanilla JavaScript (ES Modules) |
| **Cryptography** | Web Crypto API — AES-GCM, RSA-OAEP, SHA-256 |
| **Backend-as-a-Service** | Supabase (Auth, Postgres, Storage buckets) |
| **Email/OTP** | EmailJS |
| **Auth Providers** | Google Identity Services + bcrypt.js |

---

## 📂 Project Structure

SecureVault/
├── index.html              # Landing page
├── login.html / login.js   # Login, Google SSO, password reset
├── register.html / register.js
├── profile.html             # User dashboard
│   ├── profile.js           # RSA/AES file send + retrieve logic
│   └── profile2.js          # Personal vault upload/retrieve
├── encrypt.html             # Standalone encrypt/decrypt utility
├── about.html / service.html / price.html / review.html / contact.html
└── *.css                    # Themed stylesheets per page

---

## 🚀 Getting Started

1. **Clone the repository**
   git clone https://github.com/your-username/SecureVault.git
   cd SecureVault

2. **Set up Supabase**
   - Create a project
   - Create storage buckets: `uploads` and `saved`
   - Create a `users` table (username, password)

3. **Set up EmailJS**
   - Create a service + templates for OTP delivery

4. **Configure credentials**
   - Replace Supabase URL/keys and EmailJS IDs
   - ⚠️ Use environment variables or a backend proxy — never commit service-role keys to source control

5. **Run locally**
   - Serve with any static server, e.g. VS Code Live Server

---

## 🔒 Security Philosophy

- **Encrypt first, upload second** — plaintext never crosses the network
- **Ephemeral by design** — shared files and keys are auto-purged after retrieval
- **Hashed credentials** — passwords stored only as bcrypt hashes
- **Hybrid cryptography** — AES for speed, RSA for secure key exchange

---

## 🛣️ Roadmap

- [ ] Move secrets to a secure backend / serverless functions
- [ ] Add two-factor authentication
- [ ] File size limits & progress indicators
- [ ] Drag-and-drop upload UI
- [ ] End-to-end encrypted chat for file context/notes

---

## 📜 License

Built as an academic/portfolio project. Feel free to fork, learn from, and extend it.

---

**Made with 🔐 and a healthy distrust of plaintext.**
