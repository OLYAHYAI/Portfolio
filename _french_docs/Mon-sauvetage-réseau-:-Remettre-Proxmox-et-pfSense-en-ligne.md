# Configuration Proxmox et pfSense

**Auteur :** Omar LYAHYAI  
**Date :** 2025-10-25

---

## 🛠️ Problème initial
- Changement d'adresse IP sur le serveur Proxmox.
- L'interface web (UI) est devenue inaccessible.

## ✅ Résolution
1. Vérification de l'interface réseau avec `ip a`.
2. Redémarrage des services réseau :
   ```bash
   systemctl restart networking
   systemctl restart pveproxy
   ```
3. Activation manuelle de l'interface `enp3s0` :
   ```bash
   ip link set emo0 up
   ```

---

## 🔧 Objectif réseau avec pfSense
- Proxmox utilise `vmbr0` avec l'IP **[YOUR_PROXMOX_IP]**.
- pfSense doit avoir :
  - **WAN** : `[YOUR_PFSENSE_WAN_IP]` (même réseau que Proxmox)
  - **LAN** : réseau séparé (ex. `[YOUR_PFSENSE_LAN_IP]`)

## 🧱 Configuration réseau recommandée

### `/etc/network/interfaces`
```bash
auto vmbr0
iface vmbr0 inet static
    address [YOUR_PROXMOX_IP]
    netmask 255.255.255.0
    gateway [YOUR_GATEWAY_IP]
    bridge_ports enp3s0
    bridge_stp off
    bridge_fd 0

auto vmbr1
iface vmbr1 inet manual
    bridge_ports none
    bridge_stp off
    bridge_fd 0
```

## 🧠 Affectation des interfaces dans Proxmox
- **pfSense WAN** → `vmbr0` → IP : `[YOUR_PFSENSE_WAN_IP]`
- **pfSense LAN** → `vmbr1` → IP : `[YOUR_PFSENSE_LAN_IP]`
- **Autres VMs** → connectées à `vmbr1` pour être derrière pfSense

---

## 🔐 Séparation des réseaux
- `vmbr0` : accès Internet (WAN)
- `vmbr1` : réseau interne (LAN)

## 📌 Remarques
- Ne jamais utiliser la même interface pour WAN et LAN.
- pfSense doit gérer le DHCP sur le LAN.

---
**Avertissement :** Ce document fait partie d'un ensemble de documentation professionnelle. Toutes les informations sensibles, y compris les adresses IP, ont été masquées pour des raisons de sécurité et de confidentialité. Veuillez remplacer `[YOUR_PROXMOX_IP]`, `[YOUR_PFSENSE_WAN_IP]`, `[YOUR_PFSENSE_LAN_IP]` et `[YOUR_GATEWAY_IP]` par votre configuration réseau réelle lors de la mise en œuvre de ces étapes.