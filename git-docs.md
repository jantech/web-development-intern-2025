## ✅ Step 1: Install Git on Windows

Same as before:

1. Download: [https://git-scm.com/download/win](https://git-scm.com/download/win)
2. Run installer with defaults (especially “Git from command line and 3rd-party software”).

---

## ✅ Step 2: Configure Git (Username & Email)

Open **Git Bash** and run:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

---

## ✅ Step 3: Enable Git Credential Manager (Recommended)

Git for Windows includes **Git Credential Manager (GCM)** to store your GitHub login securely.

Verify it's enabled (usually enabled by default in modern Git versions):

```bash
git config --global credential.helper manager-core
```

This stores your GitHub credentials securely in the Windows Credential Manager.

---

## ✅ Step 4: Clone GitHub Repo Using HTTPS

Go to your GitHub repo → click **Code** → choose **HTTPS** (e.g., `https://github.com/your-username/your-repo.git`).

Then run:

```bash
git clone https://github.com/your-username/your-repo.git
```

You'll be prompted for your **GitHub username and a personal access token** (not your password — GitHub no longer supports account passwords for Git over HTTPS).

---


### ✅ **Fix for HTTPS Clone Issue on Windows**

#### 🔧 Step 1: Tell Git to Use Windows SSL (Avoid OpenSSL)

In **Command Prompt** (or Git Bash), run:

```cmd
git config --global http.sslBackend schannel
```

This tells Git to use **Windows' native SSL** (schannel) instead of OpenSSL, which is what's causing your "bad record mac" error.

#### 🔁 Step 2: Retry the HTTPS Clone

```cmd
git clone https://github.com/your-username/your-repo.git
```


---

## ✅ Step 5: Generate and Use a GitHub Personal Access Token (PAT)

1. Go to: [https://github.com/settings/tokens](https://github.com/settings/tokens)
2. Click **"Generate new token (classic)"**

   - Scope: Give it at least `repo` access
   - Expiration: Choose what makes sense for you

3. Copy the token when shown (you won’t see it again)

When Git asks for your **password**, paste this **token** instead.

✅ Git will cache it via GCM, so you won’t need to re-enter it.

---

## ✅ Step 6: Push and Pull with HTTPS

Now that you’re authenticated, you can:

```bash
git add .
git commit -m "Initial commit"
git push origin main
```

You’ll only need to do authentication once (thanks to the credential manager).

---

### 🔄 Switching from SSH to HTTPS (optional)

If your repo is already using SSH and you want to switch to HTTPS:

```bash
git remote set-url origin https://github.com/your-username/your-repo.git
```

---
