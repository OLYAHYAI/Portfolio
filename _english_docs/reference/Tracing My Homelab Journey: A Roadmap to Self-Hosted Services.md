---
title: "Conversation Roadmap and Setup Guide"
date: 2025-10-24
tags: [homelab, obsidian, n8n, automation, roadmap, workflow]
---

# Homelab Overview
You are managing your homelab using Proxmox with VMs for Ubuntu, Kali, and pfSense.
Your main goals include hosting your own services, automating workflows, and connecting everything securely with Twingate.

# Obsidian Setup
- Obsidian is being self-hosted using CouchDB.
- You’ll run it on the Ubuntu VM and connect to it using **Obsidian LiveSync**.
- Secure remote access is managed via **Twingate**.

# Step-by-Step Setup Plan

## Phase 1: Server Preparation
1. Update and secure the Ubuntu VM:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. Install Docker and Docker Compose:
   ```bash
   sudo apt install docker.io docker-compose -y
   ```
3. Create directories for your services:
   ```bash
   mkdir ~/obsidian-sync ~/n8n && cd ~/obsidian-sync
   ```

## Phase 2: Deploy CouchDB for Obsidian Sync
1. Create docker-compose.yml file:
   ```yaml
   version: '3.8'
   services:
     couchdb:
       image: couchdb:latest
       container_name: couchdb-for-obsidian
       environment:
         - COUCHDB_USER=admin
         - COUCHDB_PASSWORD=your_secure_password
       volumes:
         - ./data:/opt/couchdb/data
         - ./config:/opt/couchdb/etc/local.d
       ports:
         - 5984:5984
       restart: unless-stopped
   ```
2. Launch CouchDB:
   ```bash
   docker compose up -d
   ```
3. Open the CouchDB dashboard: `http://<ip>:5984/_utils`
4. Login using your credentials and create database **obsidian**.
5. In Obsidian, install the *Self-Hosted LiveSync* plugin and connect to CouchDB.

## Phase 3: Install and Configure n8n
1. Move to **n8n** directory:
   ```bash
   cd ~/n8n
   ```
2. Create docker-compose.yml:
   ```yaml
   version: '3'
   services:
     n8n:
       image: n8nio/n8n:latest
       environment:
         - GENERIC_TIMEZONE=Europe/Paris
       ports:
         - 5678:5678
       volumes:
         - ./data:/home/node/.n8n
       restart: unless-stopped
   ```
3. Run n8n:
   ```bash
   docker compose up -d
   ```
4. Open **n8n** in your browser: `http://<server-ip>:5678`

## Phase 4: Automate Backups with n8n
1. Create a backup directory: `/backups/obsidian`
2. In n8n, create a new workflow:
   - **Trigger:** Cron node (Weekly on Sunday)
   - **Command Node:**
     ```bash
     tar czf /backups/obsidian_$(date +%F).tar.gz /path/to/obsidian-vault
     ```
   - **Notification Node (Optional):** Send message via Telegram or Discord API.

## Phase 5: Remote Access and Security
1. Install and link your server to your Twingate account.
2. Access your n8n and CouchDB dashboards remotely in a secure way.
3. Set up HTTPS tunnels if needed (optional step using Pinggy or Cloudflare Tunnel).

# Weekend Action Plan

### Saturday
- Proxmox maintenance and Ubuntu updates.
- Install Docker & deploy CouchDB.
- Test CouchDB connection with Obsidian desktop.

### Sunday
- Deploy n8n and create your first backup automation.
- Configure Twingate for external secure access.
- Document your setup directly in Obsidian.

# End Goal
By Monday, you'll have:
- A self-hosted Obsidian sync system (CouchDB)
- n8n automation for weekly backups
- Secure remote workflow using Twingate
---
