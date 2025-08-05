# ☁️ NextCloud AIO Docker Compose Stack

[![MIT License](https://img.shields.io/github/license/Vantasin/NextCloudAIO?style=flat-square)](LICENSE)
[![Docker Compose](https://img.shields.io/badge/Docker-Compose-blue?logo=docker)](https://www.docker.com/)
[![ZFS](https://img.shields.io/badge/ZFS-OpenZFS-blue?style=flat-square)](https://openzfs.org/)

[![Nextcloud AIO](https://img.shields.io/badge/Nextcloud-AIO-blue?logo=nextcloud)](https://github.com/nextcloud/all-in-one)

A self-hosted, all-in-one Nextcloud solution using Docker. Includes Nextcloud, PostgreSQL, Redis, Collabora, OnlyOffice, Backup, and more — all managed through a single container interface.

---

## 📁 Directory Structure

```bash
tank/
├── docker/
│   ├── compose/
│   │   └── nextcloud/              # Git repo lives here
│   │       ├── docker-compose.yml  # Main Docker Compose config
│   │       ├── .env                # Runtime environment variables and secrets (gitignored!)
│   │       ├── env.example         # Example .env file for reference
│   │       └── README.md           # This file
│   └── data/
│       └── nextcloud/              # Volume mounts and persistent data
```

---

## 🧰 Prerequisites

* Docker Engine
* Docker Compose V2
* Git
* (Optional) ZFS on Linux for dataset management

> ⚠️ **Note:** These instructions assume your ZFS pool is named `tank`. If your pool has a different name (e.g., `rpool`, `zdata`, etc.), replace `tank` in all paths and commands with your actual pool name.

---

## ⚙️ Setup Instructions

1. **Create the stack directory and clone the repository**

   If using ZFS:
   ```bash
   sudo zfs create -p tank/docker/compose/nextcloud
   cd /tank/docker/compose/nextcloud
   sudo git clone https://github.com/Vantasin/NextCloudAIO.git .
   ```

   If using standard directories:
   ```bash
   mkdir -p ~/docker/compose/nextcloud
   cd ~/docker/compose/nextcloud
   git clone https://github.com/Vantasin/NextCloudAIO.git .
   ```

2. **Create the runtime data directory** (optional)

   If using ZFS:
   ```bash
   sudo zfs create -p tank/docker/data/nextcloud
   ```

   If using standard directories:
   ```bash
   mkdir -p ~/docker/data/nextcloud
   ```

3. **Configure environment variables**

   Copy and modify the `.env` file as necessary:

   ```bash
   sudo cp env.example .env
   sudo nano .env
   sudo chmod 600 .env
   ```

   > **Notes:** You must create two Nextloud URLs eg. `nextcloud.example.com` & `setup.example.com` using [Nginx Proxy Manager](https://github.com/Vantasin/Nginx-Proxy-Manager.git) as a reverse proxy for HTTPS certificates via Let's Encrypt.

   > **Tip:** Nextcloud AIO uses two ports/proxies, one is `port 11000` for access to the Nextcloud GUI, the other is `port 8080` to setup Nextcloud AIO.
   >
   > **GUI Proxy Host:**
   >  - **Domain Name:** `https://nextcloud.example.com`
   >  - **Scheme:** http
   >  - **Forward Hostname/IP:** `nextcloud-aio-apache`
   >  - **Forward Port:** 11000
   >
   > **Setup Proxy Host:**
   >  - **Domain Name:** `https://setup.example.com`
   >  - **Scheme:** https
   >  - **Forward Hostname/IP:** `nextcloud-aio-mastercontainer`
   >  - **Forward Port:** 8080

4. **Start nextcloud**

   ```bash
   sudo docker compose up -d
   ```

---

## 🌐 Accessing Nextcloud Web UI

Once deployed, access **Nextcloud** using:

- **Initialize Nextcloud AIO:** Navigate to the external proxy: `https://setup.example.com`.

	- **IMPORTANT:** Take a screenshot of the All-In-One setup passphrase!

	- Use the passphrase to login.

	- Use the internal proxy: `https://nextcloud.example.com` to register your domain with Nextcloud AIO and for access to the Nextcloud GUI.

	- Use the initial Nextcloud username & passowrd provided during setup to login for the first time.

	- **IMPORTANT:** Setup `Backup and restore` and take a screenshot of the `Backup information`.

- **Optional:** Download the desktop and mobile apps to easily access and sync with your Nextcloud.

---

## 🙏 Acknowledgments

- [ChatGPT](https://openai.com/chatgpt) — for assistance in generating setup scripts and templates.
- [Docker](https://www.docker.com/) — for container orchestration and runtime.
- [OpenZFS](https://openzfs.org/) — for advanced local filesystem features, dataset organization, and snapshotting.
- [Nextcloud AIO](https://github.com/nextcloud/all-in-one) — official Docker stack for Nextcloud. Learn more at [nextcloud.com/aio](https://nextcloud.com/aio/).