# PlaceMux — Phase 1, Task 1: System Ingestion & Infrastructure Setup

> DevOps Engineer track · Altrodav Technologies Pvt. Ltd. · Industry Immersion Program

This repository documents my solution to **Task 1** of the PlaceMux Phase 1 Industry Immersion program — setting up a reproducible local development environment and base infrastructure for a two-tier application.

---

## Table of Contents

- [Task Brief](#task-brief)
- [Solution Overview](#solution-overview)
- [Prerequisites](#prerequisites)
- [Installing Required Tools](#installing-required-tools)
- [Clone the Repository](#clone-the-repository)
- [Configure Secrets](#configure-secrets)
- [Build & Run](#build--run)
- [Verification](#verification)
- [Project Structure](#project-structure)
- [Security Practices Followed](#security-practices-followed)
- [Troubleshooting](#troubleshooting)
- [Definition of Done — Status](#definition-of-done--status)

---

## Task Brief

**Objective:** Set up a DevOps workstation and the project's base infrastructure so the team can build, run, and deploy — establishing the foundation every later task in the track depends on.

**Deliverables required:**
- A working base environment
- Safe credential handling (no secrets in code)
- A reproducible setup README that another developer can follow start to finish

**Definition of Done:**
- Working base environment, safe credential handling, and a reproducible README
- Demonstrable live on real (even if small) data — not just described

**Pitfalls called out in the brief:**
- Secrets in code or README
- Undocumented manual steps
- Dev environment wildly different from prod

---

## Solution Overview

I set up **`<PROJECT_NAME>`**, a two-tier application (app + database, containerized), following the brief's six-step process:

1. Installed and verified the core toolchain (Git, Docker, AWS CLI)
2. Cloned the repo and stood up the local environment via Docker Compose
3. Configured credentials through a `.env` file (gitignored) + `aws configure` — never in code
4. Verified the app builds and runs locally, confirmed with a live `curl` check
5. Wrote this README so the process is fully reproducible
6. Re-ran the entire setup from a clean state (`docker compose down -v` → rebuild) using only this document, to confirm a teammate could follow it with zero prior context

---

## Prerequisites

- Administrator / sudo access
- Stable internet connection
- GitHub account
- Docker Desktop (Windows/macOS) or Docker Engine (Linux)
- AWS account (or GCP, if applicable)

**Supported OS:** Windows 10/11, macOS (Intel & Apple Silicon), Ubuntu/Debian, Fedora/RHEL

| Tool | Purpose |
|---|---|
| Git | Version control |
| Docker + Docker Compose | Containerization |
| AWS CLI / GCP CLI | Cloud authentication |
| VS Code (optional) | Editor |

---

## Installing Required Tools

### Git

```bash
# Ubuntu/Debian
sudo apt update && sudo apt install git -y

# Fedora/RHEL
sudo dnf install git -y

# macOS
brew install git
```
Verify: `git --version`

### Docker

```bash
# Ubuntu
sudo apt update
sudo apt install docker.io docker-compose-v2 -y
sudo systemctl enable docker && sudo systemctl start docker
sudo usermod -aG docker $USER
# log out and back in
```
On Windows/macOS, install Docker Desktop instead (enable WSL2 on Windows if prompted).

Verify:
```bash
docker --version
docker compose version
docker run hello-world
```

### AWS CLI

```bash
# Ubuntu/Debian
sudo apt install awscli -y

# Fedora
sudo dnf install awscli -y

# macOS
brew install awscli
```
Verify: `aws --version`

*(Swap for `gcloud` if the project targets GCP instead of AWS.)*

---

## Clone the Repository

```bash
git clone <GITHUB_REPO_URL>
cd <REPO_FOLDER_NAME>
```

---

## Configure Secrets

Credentials are never committed to this repo.

```bash
cp .env.example .env
```

Fill in `.env` with real values:
```env
DB_HOST=<service_name_from_docker-compose>
DB_USER=<your_choice>
DB_PASSWORD=<your_choice>
DB_NAME=<your_choice>
APP_PORT=<port_the_app_listens_on>
```

Cloud credentials go through the CLI's own store, never in the repo:
```bash
aws configure
```

`.gitignore` includes:
```text
.env
*.pem
*.key
*.crt
terraform.tfstate
terraform.tfstate.backup
```

---

## Build & Run

```bash
docker compose up --build
```

Confirm it's live, not just started:
```bash
curl http://localhost:<APP_PORT>
```

Stop / reset:
```bash
docker compose down       # stop
docker compose down -v    # stop + wipe data, for a clean re-test
```

---

## Verification

- [x] Git, Docker, Docker Compose, AWS CLI installed and verified
- [x] Repository cloned successfully
- [x] `.env` configured, no secrets committed
- [x] Application builds and starts without errors
- [x] Application confirmed reachable via `curl` on real data
- [x] Full teardown + rebuild performed using only this README

---

## Project Structure

```text
project-root/
├── Dockerfile
├── docker-compose.yml
├── README.md
├── .gitignore
├── .env.example
├── src/
├── config/
├── scripts/
└── docs/
```

---

## Security Practices Followed

- No secrets committed to Git at any point
- All sensitive values sourced from `.env` (gitignored) or the AWS CLI credential store
- `.env.example` provided so teammates know required keys without seeing real values
- Reviewed diffs before every push to catch accidental secret exposure

---

## Troubleshooting

| Issue | Fix |
|---|---|
| `permission denied` on Docker commands (Linux) | `sudo usermod -aG docker $USER`, then log out/in |
| Docker daemon not running | Start Docker Desktop / Docker Engine, retry |
| AWS CLI auth failed | `aws configure list` to check keys and region |
| App can't reach DB | Use the service name from `docker-compose.yml` as `DB_HOST`, not `localhost` |
| Port already in use | Change `APP_PORT` in `.env`, or free the port |

---

## Definition of Done — Status

| Requirement | Status |
|---|---|
| Working base environment | ✅ |
| Safe credential handling (no secrets in code/README) | ✅ |
| Reproducible setup README | ✅ (this document) |
| Demonstrable on real/live data | ✅ (verified via `curl` + clean rebuild) |
