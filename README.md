This repository demonstrates a **real-world Jenkins CI/CD pipeline** for deploying a **Laravel application** using **secure SSH-based automation**.

Built with **simplicity, security, and production stability** in mind.

---

## ✨ Key Features

- 🚀 Automated Laravel deployments using Jenkins
- 🌍 Separate **staging** and **production** environments
- 🔐 Secure SSH authentication
- 🔄 Git-based code synchronization
- 🧹 Automatic Jenkins workspace cleanup
- 🏗️ Clean & readable Jenkinsfile

---

## 🚦 Branch & Environment Strategy

| Branch | Environment |
|------|------------|
| `staging` | 🧪 Staging |
| `main` | 🚀 Production |

---

## 🔄 Jenkins Pipeline Flow

```text
Code Push
   ↓
Jenkins Trigger
   ↓
Environment Selection
   ↓
SSH into Server
   ↓
Git Clone / Pull
   ↓
Application Updated
   ↓
Workspace Cleanup
   ↓
🎉 Deployment Complete
🔐 Jenkins Credentials (Example)
All secrets are securely stored in Jenkins Credentials Manager.

🖥️ Server Access
PROD_HOST

PROD_USER

PROD_SSH_KEY

STAGING_HOST

STAGING_USER

STAGING_SSH_KEY

🔑 GitHub Access
GH_USERNAME

GH_PAT (Personal Access Token)

⚠️ No secrets are committed to the repository.

🛠️ Tech Stack
🐧 Linux (Ubuntu)

🐘 Laravel (PHP)

🧩 Jenkins

🔐 SSH

🌐 Nginx / Apache

☁️ AWS / Any VPS
