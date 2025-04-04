# 🔐 Secure Shared Development Environment Setup

This project sets up a **secure, encrypted development environment** using Docker, Ollama, and Open WebUI. It detects whether you're running a GPU or CPU machine, installs dependencies, creates an encrypted disk container, and launches an AI-ready interface.

---

## 🚀 Features

- ✅ Detects GPU or defaults to CPU
- 🔐 Creates and mounts encrypted storage (`/securedata`)
- 🐳 Installs Docker and Docker Compose
- 🧠 Installs [Ollama](https://ollama.com) (no models pulled by default)
- 🌐 Launches [Open WebUI](https://github.com/open-webui/open-webui) on **port 3000**
- 🔄 Runs Ollama as a system service using systemd
- 💡 Supports both CPU-only and GPU-accelerated setups

---

## 🧰 Requirements

- 🐧 Ubuntu Server 20.04 or later (fresh install recommended)
- 🔑 Sudo or root privileges
- 🌐 Internet connection
- 💾 At least 10–20 GB free disk space for encrypted container

---

## 📦 Installation Guide

### 1. Download the script

```bash
curl -O https://raw.githubusercontent.com/<your-username>/<your-repo>/main/setup.sh
```

### 2. Make it executable

```bash
chmod +x setup.sh
```

### 3. Run the script

```bash
sudo ./setup.sh
```

📝 During setup, you’ll be prompted to enter the size of your encrypted container in **GB** (e.g., `10` for 10 GB).

---

## 💾 How Much Disk Space Should I Allocate?

When prompted:

> `Enter encrypted container size in GB (e.g., 5, 10, 50):`

Here’s a quick reference:

| Container Size | What You Can Fit                                 |
|----------------|--------------------------------------------------|
| **5 GB**       | Minimum: Open WebUI + 1 small model              |
| **10 GB**      | Room for WebUI + 1–2 medium LLMs                 |
| **20–30 GB**   | Several models + embeddings, chat history, etc.  |
| **50+ GB**     | Ideal for experimentation and multiple models    |

**✅ Recommended Minimum:**  
**10–20 GB** for a usable setup with room to grow.

🧠 LLMs are large. For example:
- `llama3` ~4.5 GB
- `llama2:13b` ~7–8 GB
- WebUI state, logs, and cache can grow over time

---

## 🔐 Encryption Key Details

This setup uses a **randomly generated encryption key**, stored at:

```
/root/.securekey
```

- 🔒 This key is used to encrypt and decrypt your data volume.
- 🧠 If this key is lost or deleted, you will **not** be able to recover your data.
- 🔄 You can regenerate the key (not recommended unless reinitializing) with:

```bash
sudo head -c 64 /dev/urandom > /root/.securekey
sudo chmod 600 /root/.securekey
```

> ⚠️ **Reminder:** Back up your key file securely if this setup is important. Without it, your encrypted volume is unrecoverable.

---

## 🧠 Installed Services

| Service        | URL/Path                 | Description                                         |
|----------------|--------------------------|-----------------------------------------------------|
| **Ollama**     | http://localhost:11434   | AI model backend (no model installed by default)    |
| **Open WebUI** | http://localhost:3000    | Frontend interface for chatting with LLMs           |
| **Encrypted Volume** | `/securedata`     | Secure, persistent data storage                     |

---

## 🔒 Managing the Encrypted Volume

To manually **unmount and close** the encrypted volume:

```bash
sudo umount /securedata
sudo cryptsetup luksClose securedata
```

To **reopen and mount** later:

```bash
sudo cryptsetup luksOpen /securedata/container.img securedata --key-file /root/.securekey
sudo mount /dev/mapper/securedata /securedata
```

---

## 🛠️ Next Steps

- 🧠 Pull a model using: `ollama pull <model>` (e.g., `mistral`, `llama2`, etc.)
- 💬 Use Open WebUI to interact with your local models
- ⚙️ Add custom dev containers or tools into `/securedata`

---

## 📬 Need Help?

- 📂 [Open an Issue](https://github.com/<your-username>/<your-repo>/issues)
- 💬 Reach out via GitHub if you run into problems

---

> ✅ Perfect for AI devs, teams, or tinkerers needing a **secure, self-hosted, GPU/CPU-compatible AI environment**.
