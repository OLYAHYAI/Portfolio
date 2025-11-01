# 🧾 Rapport de session - 2025-10-23

## 👤 Utilisateur : Omar LYAHYAI

## 🔧 Activités réalisées

### ✅ Installation de Kali Linux
- Création d'une VM Kali sur Proxmox
- BIOS utilisé : SeaBIOS (après échec avec OVMF)
- Disque : 80 Go
- CPU : 4 cores
- RAM : 8 Go
- Contrôleur SCSI : VirtIO SCSI Single
- Installation réussie avec GRUB

### 📦 Mise à jour complète (full-upgrade)
- **Kali Linux** :
  - Commande exécutée : `sudo apt update && sudo apt full-upgrade`
  - Système à jour
- **Ubuntu** :
  - Commande exécutée : `sudo apt update && sudo apt full-upgrade`
  - Système à jour

### 🔐 Installation de pfSense
- Téléchargement de l’ISO depuis le site officiel
- Création d’une VM avec :
  - BIOS : SeaBIOS
  - CPU : 2 cores
  - RAM : 2 à 4 Go
  - Disque : 20 Go
  - Réseau : 2 interfaces (WAN sur vmbr0, LAN sur vmbr1)
- Bridge `vmbr1` créé manuellement
- Installation en cours

### 📸 Snapshot prévu
- Un snapshot sera effectué une fois l’installation de pfSense terminée
- Objectif : sauvegarder l’état stable du lab

## 🧠 Notes complémentaires
- Modèle Qwen3 4B téléchargé pour tests LLM
- Format GGUF utilisé pour compatibilité CPU
- Test de plusieurs modèles dans LM Studio

---
**Fin de session.**

---
**Avertissement :** Ce document fait partie d'un ensemble de documentation professionnelle. Toutes les informations sensibles, y compris les adresses IP, ont été masquées pour des raisons de sécurité et de confidentialité.