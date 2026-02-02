# 🔐 SSH Keys for Students (Mac & Windows)

This guide explains **what SSH keys are**, **why you need them**, and **how to create and share them safely** on **macOS** and **Windows**.

You’ll use SSH keys to:
- Access servers and HPC systems
- Connect to GitHub / GitLab
- Work with cloud or container-based tools
- Avoid typing passwords repeatedly

---

## 🧠 What is an SSH key?

An SSH key is a **secure pair of files**:

- **Private key** → stays on *your computer* (**never share this**)
- **Public key** → safe to share with instructors, servers, and services

Think of it like:
- 🔑 Private key = your personal house key  
- 🏠 Public key = the lock installed on the door

---

## ⚠️ Critical rules (read this first)

❌ **Never share your private key**  
❌ **Never upload private keys to GitHub or Google Drive**  
✅ **Only share the public key** (`.pub` file)

🚨 In this class, **public keys will be shared via the class Google Sheet or Slack channel** — *never via email attachments*.

---

# macOS Instructions

### 1️⃣ Open Terminal
- Press `Cmd + Space`
- Type **Terminal**
- Press **Enter**

---

### 2️⃣ Generate an SSH key

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

When prompted:
- **File location** → press **Enter** (use default)
- **Passphrase** → optional but recommended

This creates:
- `~/.ssh/id_ed25519` → **private key**
- `~/.ssh/id_ed25519.pub` → **public key**

---

### 3️⃣ View your public key

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy **the entire line** (starts with `ssh-ed25519`).

---

### 4️⃣ Share your public key (Class Instructions)

Paste **only the public key** into **ONE** of the following (as instructed):

- ✅ **Class Google Sheet** (designated column)
- ✅ **Class Slack channel** (plain text, not a file)

📌 Include:
- Your **full name**
- Your **username** (if required)

Example Slack post:
```
Jane Doe – SSH Key
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA...
```

---

# Windows Instructions

## ✅ Option A: Windows 10 / 11 (PowerShell – Recommended)

### 1️⃣ Open PowerShell
- Right-click **Start**
- Select **Windows Terminal** or **PowerShell**

---

### 2️⃣ Generate SSH key

```powershell
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Press **Enter** for defaults.

Keys are created in:
```
C:\Users\<your-username>\.ssh\
```

---

### 3️⃣ View your public key

```powershell
type $env:USERPROFILE\.ssh\id_ed25519.pub
```

Copy the output.

---

## ✅ Option B: Git Bash (If Git for Windows is installed)

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
cat ~/.ssh/id_ed25519.pub
```

---

### 4️⃣ Share your public key (Class Instructions)

Paste **only the public key** into:
- ✅ **Class Google Sheet**
- ✅ **Class Slack channel**

❌ Do **not** upload key files  
❌ Do **not** email keys

---

# 🔍 How to verify your key later

For GitHub:

```bash
ssh -T git@github.com
```

You should see:
```
You've successfully authenticated
```

---

# 🛑 Common mistakes to avoid

| Mistake | Why it’s a problem |
|------|------------------|
| Sharing `id_ed25519` | That is your private key |
| Posting screenshots of keys | Hard to revoke, unsafe |
| Uploading keys to GitHub repos | Security risk |
| Reusing keys from shared machines | Not secure |

---

# 📌 Summary

✔ Generate SSH key on **your own machine**  
✔ Keep **private key private**  
✔ Share **public key only** via **Google Sheet or Slack**  
✔ One-time setup for the entire semester

---

If you lose your private key or switch computers:
- Generate a **new key**
- Re-post your **new public key**
- Inform the instructor/TA
