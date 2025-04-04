# 🔐 Secure Shared Development Environment Setup

This project provides a **secure, encrypted development environment** using Docker, Ollama, and Open WebUI. It automatically detects GPU/CPU, installs necessary tools, creates an encrypted volume, and spins up a multi-modal AI environment with a modern frontend.

---

## 🚀 Features

- ✅ Automatically detects GPU or CPU
- 🔐 Creates and mounts an encrypted data container at `/securedata`
- 🐳 Installs Docker and Docker Compose
- 🧠 Installs [Ollama](https://ollama.com) with the **Llama 3** base model
- 🌐 Deploys [Open WebUI](https://github.com/open-webui/open-webui) on **port 3000**
- 🔄 Runs Ollama as a system service via systemd
- 📦 Supports both CPU-only and GPU-accelerated environments

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

📝 During setup, you’ll be prompted to enter the size of your encrypted container in **GB**.

---

## 💾 How Much Disk Space Should I Allocate?

When prompted:

> `Enter encrypted container size in GB (e.g., 5, 10, 50):`

Here’s a quick reference:

| Container Size | What You Can Fit                                 |
|----------------|--------------------------------------------------|
| **5 GB**       | Minimum: Open WebUI + 1 small model              |
| **10 GB**      | Room for WebUI + 1–2 medium LLMs (e.g., Llama3)  |
| **20–30 GB**   | Several models + embeddings, chat history, etc.  |
| **50+ GB**     | Ideal for experimentation and multiple models    |

📦 **Recommended Minimum:**  
**10–20 GB** for a usable setup with room to grow.

🧠 LLMs are large. For example:
- `llama3` ~4.5 GB
- `llama2:13b` ~7–8 GB
- WebUI state, logs, and cache can grow over time

---

## 🔐 Important: Change the Encryption Key

By default, the script generates a random key stored at:

```
/root/.securekey
```

⚠️ You must **back up or rotate this key** if you care about the data. If it's deleted, the data cannot be recovered.

> 🔥 If you modify the script to use a passphrase instead, avoid defaults like `"changeme"` and **set a secure passphrase**.

### 🔄 Regenerate the encryption key (optional):

```bash
sudo head -c 64 /dev/urandom > /root/.securekey
sudo chmod 600 /root/.securekey
```

---

## 🧠 Installed Services

| Service       | Address                  | Description                     |
|---------------|--------------------------|---------------------------------|
| **Ollama**     | http://localhost:11434   | AI model backend                |
| **Open WebUI** | http://localhost:3000    | Friendly frontend interface     |
| **Encrypted Data**| `/securedata`         | Secure volume for persistent data |

---

## 🔒 Managing the Encrypted Volume

To **close and unmount**:

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

- Use `ollama pull <model>` to add more LLMs
- Run your own dev containers or agents inside `/securedata`
- Configure Open WebUI for multi-user access or secure reverse proxies

---

## 📬 Need Help?

- 📂 [Issues](https://github.com/<your-username>/<your-repo>/issues)
- 📧 Feel free to reach out if you're stuck!

---

> ✅ This setup is great for AI engineers, teams, and solo tinkerers looking for **secure, self-hosted, GPU/CPU-compatible AI environments**.
