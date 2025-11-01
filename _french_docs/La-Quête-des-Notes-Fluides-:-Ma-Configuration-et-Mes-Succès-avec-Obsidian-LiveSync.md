## Overview
This document summarizes the setup, issues encountered, and resolutions implemented during the deployment of Obsidian Self-hosted LiveSync using Docker and CouchDB.

---

## Setup Goals
- Enable secure, efficient syncing of Obsidian vaults across devices.
- Use Dockerized CouchDB as the backend database.
- Implement End-to-End Encryption (E2EE) version 2 for data security.
- Handle multi-OS file system case sensitivity gracefully.

---

## Key Issues & Resolutions

### 1. Missing Docker Compose
**Issue:** `docker-compose` command not found when managing Docker containers.  
**Resolution:** Installed Docker Compose on Ubuntu via official GitHub releases and set executable permissions.

### 2. CORS Configuration for CouchDB
**Issue:** Native fetch API failed with CORS error during LiveSync.  
**Resolution:** Edited `local.ini` inside Docker container to enable CORS with appropriate origins and headers. Restarted CouchDB container.

### 3. Configuration Mismatch: Chunk Size
**Issue:** Local LiveSync client reported `customChunkSize` mismatch (local 60 vs remote 0).  
**Resolution:** Unified chunk size to 60 via LiveSync plugin settings on all clients; accepted remote update prompt.

### 4. Filename Case Sensitivity
**Issue:** Potential sync conflicts due to file name case sensitivity across Linux, Windows, and Android devices.  
**Resolution:** Set LiveSync setting `handleFilenameCaseSensitive` to false when syncing with case-insensitive devices; set true only if all clients use case-sensitive file systems.

### 5. End-to-End Encryption Algorithm Update
**Issue:** Older E2EE version detected with known vulnerabilities.  
**Resolution:** Upgraded to E2EE version `v2` in LiveSync plugin; ensured all clients run version 0.25.0+ for compatibility.

### 6. Vault Rebuild Confirmation
**Issue:** Sync metadata rebuild required due to remote database corruption or config changes.  
**Resolution:** Confirmed rebuild within LiveSync plugin interface; backed up data prior to rebuild.

### 7. Connectivity Issues with CouchDB
**Issue:** `Could not connect to server` messages during sync attempts.  
**Resolution:**  
- Verified container running and accessible via network.  
- Checked firewall settings and CouchDB URL in LiveSync plugin.  
- Restarted Docker containers as needed.

---

## Step-by-Step Workflow

1. Install Docker Compose on Ubuntu.
2. Configure CouchDB's `local.ini` for CORS support inside Docker container.
3. Restart the CouchDB Docker container.
4. Configure LiveSync plugin settings:  
   - `customChunkSize` = 60  
   - Case sensitivity (`handleFilenameCaseSensitive`) according to OS mix  
   - Encryption algorithm = v2
5. Confirm the vault rebuild when prompted to reset sync metadata.
6. Verify network connectivity and access to CouchDB from all client devices.
7. Monitor LiveSync logs and resolve any configuration mismatch prompts.
8. Backup regularly for data safety.

---

## References
- [Obsidian Self-hosted LiveSync Setup Guide](https://dev.to/lightningdev123/how-to-set-up-a-self-hosted-obsidian-sync-server-hcn)  
- [CouchDB Configuration Docs](https://docs.couchdb.org/en/stable/config/index.html)  
- [Official Obsidian LiveSync GitHub](https://github.com/vrtmrz/obsidian-livesync)  

---

## Notes
Maintain configuration consistency across devices and apply updates promptly to ensure secure, reliable synchronization of your Obsidian vault.

---

*Document created on 25-Oct-2025*

---
**Avertissement :** Ce document fait partie d'un ensemble de documentation professionnelle. Toutes les informations sensibles, y compris les adresses IP, ont été masquées pour des raisons de sécurité et de confidentialité.