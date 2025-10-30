
# 🛠️ Session Home Lab – Proxmox & Réseau – 22 Octobre 2025

## Informations générales
- **Utilisateur** : F4BLOX
- **Machine Server** : Lenovo ThinkCentre M920q
  - CPU : Intel i5-8500T
  - RAM : 16 Go
  - Disque principal : SSD 512 Go
  - Disque secondaire : HDD 250 Go
- **Adresse IP du serveur Proxmox** : `192.168.11.254`

---

## 1. ✅ Connexion à l’interface web Proxmox
- Accès via navigateur à `https://192.168.11.254:8006`
- Identifiants : root
  - Utilisateur : `root`
  - Domaine : `pam`
  - Mot de passe : défini à l’installation

---

## 2. 🔌 Connexion Ethernet entre routeur et serveur
- Connexion directe via câble Ethernet
- Configuration IP manuelle :
  - PC1 : `192.168.11.1`
  - PC2 : `192.168.11.254`
  - Masque : `255.255.255.0`
- Objectif : communication directe entre les deux machines

---

## 3. 📡 Vérification du Wi-Fi en ligne de commande
- Commandes utilisées :
  - `ip a` → liste des interfaces réseau
  - `lspci | grep -i network` → détecter les cartes réseau
  - `iw dev` → interfaces Wi-Fi
  - `lsusb` → périphériques USB (clé Wi-Fi)

---

## 4. 🌐 Changement d’adresse IP du serveur Proxmox
- Fichier modifié : `/etc/network/interfaces`
- Exemple de configuration :
  ```
  auto vmbr0
  iface vmbr0 inet static
      address 192.168.11.254
      netmask 255.255.255.0
      gateway 192.168.11.1
      bridge_ports enp3s0
      bridge_stp off
      bridge_fd 0
  ```
- Redémarrage du réseau : `systemctl restart networking`

---

## 5. 📦 Utilisation d’une image ISO depuis le laptop
- Méthodes :
  - Upload via interface web : `Datacenter > Node > local > Content > Upload`
  - SCP : `scp fichier.iso root@192.168.11.254:/var/lib/vz/template/iso/`
- Objectif : créer une VM avec une image ISO personnalisée

---

## 6. 🧠 Choix de LLM adaptés à la machine
- Modèles recommandés :
  - Mistral 7B Q4
  - Phi-2
  - Gemma 2B
- Outils compatibles :
  - `Ollama`
  - `llama.cpp`
  - `MCP` via Docker

---

## 7. 🐳 Installation de Docker + MCP + LLM
- Docker :
  ```bash
  sudo apt update && sudo apt install docker.io
  ```
- MCP :
  ```bash
  docker run -d -p 3000:3000 ghcr.io/jmorganca/mcp:latest
  ```
- Ollama :
  ```bash
  curl -fsSL https://ollama.com/install.sh | sh
  ```

---

## 8. 📝 Outils de prise de notes recommandés
- **Obsidian** : local, markdown, liens entre notes
- **Joplin** : open-source, synchronisable
- **Logseq / Trilium** : Docker possible
- **Notion** : cloud, gestion de projet
- **Markdown + Git** : versionnage des notes techniques

---

## 9. 🔐 Choix d’un OS pour gérer le réseau
- **pfSense** : firewall, VPN, DHCP, DNS, VLAN
- **OPNsense** : alternative moderne
- **VyOS** : CLI avancée
- **Ubuntu Server** : personnalisable avec outils réseau

---

## 10. 📊 Surveillance et limitation du trafic avec pfSense
- **Surveillance** :
  - Traffic Graphs
  - Packet Capture
  - IDS/IPS (Snort, Suricata)
- **Limitation** :
  - Traffic Shaping (pipes, queues)
  - Règles de pare-feu
  - Proxy Squid + SquidGuard

---

## 11. 💾 Utilisation d’un disque dur HDD (250 GB)
- **Usage recommandé** :
  - Sauvegardes (backups)
  - Logs
  - ISO / modèles LLM
- **Formatage** :
  ```bash
  mkfs.ext4 /dev/sdX1
  ```
- **Montage** :
  ```bash
  mkdir /mnt/hdd250
  mount /dev/sdX1 /mnt/hdd250
  ```
- **Ajout dans Proxmox** :
  - Interface : `Datacenter > Storage > Add > Directory`
  - Chemin : `/mnt/hdd250`
  - Contenu : backups, ISO, templates

---

## 🔚 Fin de session
- Toutes les étapes sont prêtes à être documentées dans Obsidian.
- Adresse IP du serveur Proxmox : `192.168.11.254`
