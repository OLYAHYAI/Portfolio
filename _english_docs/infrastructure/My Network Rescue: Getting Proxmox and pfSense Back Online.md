
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
- Proxmox utilise `vmbr0` avec l'IP **192.168.11.254**.
- pfSense doit avoir :
  - **WAN** : 192.168.11.253 (même réseau que Proxmox)
  - **LAN** : réseau séparé (ex. 192.168.1.1)

## 🧱 Configuration réseau recommandée

### `/etc/network/interfaces`
```bash
auto vmbr0
iface vmbr0 inet static
    address 192.168.11.254
    netmask 255.255.255.0
    gateway 192.168.11.1
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
- **pfSense WAN** → `vmbr0` → IP : 192.168.11.253
- **pfSense LAN** → `vmbr1` → IP : 192.168.1.1
- **Autres VMs** → connectées à `vmbr1` pour être derrière pfSense

---

## 🔐 Séparation des réseaux
- `vmbr0` : accès Internet (WAN)
- `vmbr1` : réseau interne (LAN)

## 📌 Remarques
- Ne jamais utiliser la même interface pour WAN et LAN.
- pfSense doit gérer le DHCP sur le LAN.

