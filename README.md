# 📚 IT Documentation System

> **A fully on-premises, version-controlled documentation platform for IT teams** — built with GitLab CE, MkDocs Material, and Nginx. Every document is tracked, searchable, and auto-published within 60 seconds of a `git push`.

<br>

**Built at:** [Viettel Timor, Unipessoal, Lda.](https://www.telemor.tl/) — CBD10, Timor Plaza, Dili, Timor-Leste 🇹🇱  
**Author:** Julião Martins · IT Collaborator & Junior Developer

---

## 🖥️ What Is This?

This system provides a centralized, internal documentation hub for IT teams. All documentation is written in **Markdown**, stored in a self-hosted **GitLab** repository, automatically compiled into a beautiful website by **MkDocs**, and served to the entire internal network via **Nginx** — with zero data ever leaving the company network.

**No cloud. No subscriptions. No external dependencies.** 100% on-premises.

---

## ✨ Key Features

| Feature | Description |
|---|---|
| 🔄 **Auto-publish** | Push to GitLab → docs site updates automatically in ~60 seconds via CI/CD |
| 🕓 **Full version history** | Every change is tracked — who edited, what changed, when |
| ↩️ **Instant rollback** | Revert any document to any previous version with one command |
| 🔍 **Full-text search** | Search across all documents instantly from the browser |
| 🌐 **Browser-based reading** | No special software needed — open any browser on the internal network |
| 🔒 **100% on-premises** | GitLab, MkDocs, and Nginx all run on a single internal server |
| 🆓 **Completely free** | All components are open-source with MIT/BSD/GPL licenses |

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        INTERNAL NETWORK                         │
│                                                                 │
│  ┌─────────────────┐   git push   ┌──────────────────────────┐  │
│  │  Writer's PC    │ ───────────→ │      GitLab CE Server    │  │
│  │  (VS Code +     │              │  - Stores all .md files  │  │
│  │   Git client)   │              │  - Tracks change history │  │
│  └─────────────────┘              │  - Triggers CI/CD        │  │
│                                   └────────────┬─────────────┘  │
│                                                │                │
│                                   CI/CD auto-triggered          │
│                                                ↓                │
│                                   ┌────────────────────────┐    │
│                                   │   MkDocs Build Engine  │    │
│                                   │  - Reads .md files     │    │
│                                   │  - Generates HTML site │    │
│                                   └────────────┬───────────┘    │
│                                                │ copies site/   │
│                                                ↓                │
│  ┌─────────────────┐   http://   ┌─────────────────────────┐   │
│  │   All IT Staff  │ ──────────→ │      Nginx Web Server   │   │
│  │   (read docs)   │             │  http://docs.internal   │   │
│  └─────────────────┘             │  Port: 8080             │   │
│                                  └─────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

> GitLab CE, MkDocs, and Nginx all run on **one single server** to minimize infrastructure cost.

---

## 🧩 Technology Stack

| Component | Role | License |
|---|---|---|
| **GitLab CE** | Git repository, version history, CI/CD pipeline trigger | MIT (Free) |
| **MkDocs** | Converts `.md` files into a static documentation website | BSD (Free) |
| **MkDocs Material** | Professional UI theme — sidebar, search, dark mode | MIT (Free) |
| **Nginx** | Serves the built site to all internal network users | BSD (Free) |
| **GitLab Runner** | Executes the automated build and deploy pipeline | MIT (Free) |
| **VS Code** *(optional)* | Markdown editor with Git integration for writers | MIT (Free) |

---

## 📁 Documentation Structure

```
it-docs/
├── mkdocs.yml                        ← Site configuration
├── .gitlab-ci.yml                    ← CI/CD pipeline
└── docs/
    ├── index.md                      ← Homepage
    ├── infrastructure/               ← Server, Network, Storage, Cloud/VM
    ├── applications/                 ← Core Systems, Billing, Portal
    ├── database/                     ← Oracle, MySQL
    ├── security/                     ← Security policies & audits
    ├── operations/                   ← Runbooks, Incidents, Change Management
    └── designs/                      ← PDF, Visio, Draw.io, Excel files
```

### What this system stores

- ✅ Operations Runbooks
- ✅ Incident Reports
- ✅ Change Management Records
- ✅ System Design Documents (PDF, Visio, Draw.io, Excel)
- ✅ Infrastructure, Database, and Security Documentation

---

## 🖥️ Server Requirements

| Component | Specification |
|---|---|
| **OS** | Ubuntu 22.04 LTS (64-bit) |
| **CPU** | 4 cores minimum |
| **RAM** | 16 GB |
| **Disk** | 100 GB (SSD recommended) |
| **Network** | Static internal IP address |

---

## 🚀 Quick Install Overview

> Full step-by-step instructions are in [`docs/deployment-guide.md`](docs/)

```bash
# 1. Prepare the server
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl openssh-server ca-certificates

# 2. Install GitLab CE
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
sudo EXTERNAL_URL="http://<SERVER_IP>" apt install -y gitlab-ce

# 3. Install MkDocs + Material theme
sudo apt install -y python3 python3-pip
pip3 install mkdocs mkdocs-material

# 4. Install and configure Nginx
sudo apt install -y nginx
sudo systemctl enable nginx

# 5. Install and register GitLab Runner
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
sudo apt install -y gitlab-runner
sudo gitlab-runner register   # follow prompts
```

**Total estimated setup time: ~5–6 hours** (including all verification steps).

---

## ⚙️ CI/CD Pipeline

Every `git push` to the `main` branch triggers an automatic two-stage pipeline:

```yaml
stages:
  - build
  - deploy

build-docs:
  stage: build
  script:
    - pip3 install mkdocs mkdocs-material --quiet
    - mkdocs build

deploy-docs:
  stage: deploy
  script:
    - cp -r site/* /var/www/it-docs/
    - chown -R www-data:www-data /var/www/it-docs/
```

Push → Build → Deploy → **Live in ~60 seconds.**

---

## 📋 Daily Workflow

**For writers:**
```bash
git pull                          # Get latest changes
# Edit .md files in VS Code
git add .
git commit -m "update: <description>"
git push                          # Site auto-updates in 60s
```

**For readers:** Open any browser on the internal network → `http://<SERVER_IP>:8080`  
No login. No installation. Just read.

---

## 🛡️ Backup & Recovery

Automated daily backup via cron at 2:00 AM:

```bash
# /etc/crontab
0 2 * * * /opt/gitlab/bin/gitlab-backup create CRON=1 >> /var/log/gitlab-backup.log 2>&1
```

Restore is a single command:
```bash
sudo gitlab-backup restore BACKUP=<TIMESTAMP>
```

---

## ✅ Post-Deployment Checklist

| # | Test | Expected Result |
|---|---|---|
| 1 | Open `http://<SERVER_IP>` | GitLab login page appears |
| 2 | Open `http://<SERVER_IP>:8080` | Documentation site appears |
| 3 | Edit a `.md` file and push | CI/CD pipeline triggered |
| 4 | Wait 60s, refresh docs site | Updated content visible |
| 5 | Use search box | Search results appear |
| 6 | Check pipeline in GitLab | Both jobs show ✅ Passed |

---

## 📦 References

| Component | Download | Docs |
|---|---|---|
| GitLab CE | [packages.gitlab.com](https://packages.gitlab.com/gitlab/gitlab-ce) | [docs.gitlab.com](https://docs.gitlab.com/ee/install) |
| MkDocs | [mkdocs.org](https://www.mkdocs.org/#installation) | [user guide](https://www.mkdocs.org/user-guide) |
| MkDocs Material | [squidfunk.github.io](https://squidfunk.github.io/mkdocs-material/getting-started) | [reference](https://squidfunk.github.io/mkdocs-material/reference) |
| Nginx | [nginx.org](https://nginx.org/en/download.html) | [nginx.org/docs](https://nginx.org/en/docs) |
| GitLab Runner | [docs.gitlab.com/runner](https://docs.gitlab.com/runner/install) | [runner docs](https://docs.gitlab.com/runner) |
| Ubuntu 22.04 LTS | [ubuntu.com](https://ubuntu.com/download/server) | [server docs](https://ubuntu.com/server/docs) |

---

## 👤 About the Author

**Julião Martins** — IT Collaborator & Junior Developer  
📍 Dili, Timor-Leste 🇹🇱  
🏢 Built during tenure at **Viettel Timor, Unipessoal, Lda.** — CBD10, Timor Plaza, Dili

> *This project was designed and deployed to solve a real problem: keeping IT documentation organized, versioned, and accessible across a team — using only free, open-source tools running entirely on-premises.*

---

<div align="center">

**⭐ If you found this useful, give it a star!**

</div>