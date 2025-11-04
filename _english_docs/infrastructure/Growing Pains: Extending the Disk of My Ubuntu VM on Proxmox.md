---
title: "[Mini Guide] Étendre la Partition Ubuntu après Extension du Disque Proxmox"
date: 2025-10-24
tags: [homelab, proxmox, ubuntu, partition, disque, admin]
---

# Mini Processus : Étendre une Partition après Resize Proxmox

## 1. Situation de départ
- Vous avez augmenté la taille du disque dans Proxmox (exemple : de 32 Go à 100 Go).
- Mais Ubuntu affiche encore 32 Go sur la partition `/` avec `df -h`.

## 2. Vérifiez le nouveau disque et les partitions
- Affichez le layout avec :
  ```bash
  lsblk
  ```
- Vous verrez par exemple :
  - `sda` : 100G
  - `sda1` : 1M (EFI, peu utile ici)
  - `sda2` : 32G (partition principale non étendue)

## 3. Étendre la partition sur tout le disque
- Installez l’utilitaire :
  ```bash
  sudo apt install cloud-guest-utils
  ```
- Étendez la partition :
  ```bash
  sudo growpart /dev/sda 2
  ```

## 4. Étendre le système de fichiers
- Pour la plupart des Ubuntu en ext4 (partition racine `/`):
  ```bash
  sudo resize2fs /dev/sda2
  ```
- Pour XFS :
  ```bash
  sudo xfs_growfs /
  ```

## 5. Validez
- Vérifiez que l’espace disque a augmenté :
  ```bash
  df -h
  ```

---
**Notes** :
- Ce processus n’efface aucune donnée.
- Testé sur Ubuntu VM sous Proxmox.
- Si blocage, voir les logs de la VM ou demandez assistance.

