# 🧾 Journal de configuration : DNS local + Nginx Proxy Manager + SSL

## 📅 Date : 26 octobre 2025
## 👤 Utilisateur : FREEDOM

---

## 🧠 Objectif

Configurer un environnement local sécurisé avec :
- Un domaine public `olyhome.site` (acheté chez Hostinger)
- Un serveur DNS local (Bind9) pour gérer les sous-domaines internes
- Nginx Proxy Manager (NPM) pour faire du reverse proxy
- Des sous-domaines locaux sécurisés en HTTPS (ex: `proxmox.olyhome.site`, `n8n.olyhome.site`)
- Sans exposer les services à Internet

---

## ✅ Étapes réalisées

### 1. 🧱 Configuration de Bind9 (DNS local)
- Création de la zone `olyhome.site` dans Bind9
- Ajout de sous-domaines pointant vers l’IP locale (ex: `proxmox`, `n8n`, `ginx`)
- Test avec `dig` confirmé : résolution DNS locale fonctionne

### 2. 🐳 Déploiement de Bind9 dans Docker
- Utilisation d’un `docker-compose.yml` pour exposer le port 53
- Conflit initial avec `systemd-resolved` → désactivé
- Port 53 libéré → Bind9 lancé avec succès

### 3. 🌐 Configuration de Nginx Proxy Manager
- NPM lancé sur le port 81 (`jc21/nginx-proxy-manager`)
- Création de proxy hosts pour les sous-domaines
- Redirection vers les bons ports internes (ex: `proxmox.olyhome.site` → `https://[REDACTED_PROXMOX_IP]:8006`)

### 4. 🔐 Tentative de sécurisation SSL
- Objectif : utiliser un certificat SSL **valide** en local
- NPM ne supporte pas Hostinger pour le DNS challenge automatique
- Génération d’un certificat via **Certbot en mode manuel**
  - Ajout d’un enregistrement TXT `_acme-challenge` dans la zone DNS Hostinger
  - Certificat généré avec succès : `fullchain.pem` + `privkey.pem`

### 5. ❌ Problème rencontré : erreur d’import dans NPM
- NPM refuse `privkey.pem` car elle est protégée par une passphrase
- Tentative de suppression de la passphrase :
  - `openssl rsa` → erreur "Not an RSA key"
  - `openssl ec` → conversion réussie → `privkey-nopass.pem`
- Import dans NPM échoue toujours malgré la clé sans passphrase

---

## 🧨 Problème actuel

> Impossible d’importer le certificat SSL dans NPM malgré une clé privée sans passphrase. Message d’erreur persistant.

---

## 📌 Prochaines pistes

- Vérifier le format exact de la clé (`PEM`, sans métadonnées chiffrées)
- Tester un certificat auto-signé pour valider le format attendu par NPM
- Automatiser le renouvellement avec Certbot + script de conversion
- Envisager un reverse proxy Nginx manuel si NPM bloque

---

## 📁 Fichiers générés

- `/etc/letsencrypt/live/olyhome.site/fullchain.pem`
- `/etc/letsencrypt/live/olyhome.site/privkey.pem`
- `/home/f4blox/Desktop/privkey-nopass.pem` (clé sans passphrase)

---

## 🧠 Notes

- DNSSEC visible dans Hostinger mais **non utilisé** pour le challenge SSL
- Le domaine `olyhome.site` **n’est pas exposé publiquement** pour l’instant

---
**Avertissement :** Ce document fait partie d'un ensemble de documentation professionnelle. Toutes les informations sensibles, y compris les adresses IP, ont été masquées pour des raisons de sécurité et de confidentialité. Veuillez remplacer `[REDACTED_PROXMOX_IP]` par votre configuration réseau réelle lors de la mise en œuvre de ces étapes.