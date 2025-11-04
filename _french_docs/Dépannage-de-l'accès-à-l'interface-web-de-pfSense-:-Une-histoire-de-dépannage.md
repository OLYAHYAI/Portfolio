# Dépannage accès interface Web pfSense via WAN

**Auteur :** Omar LYAHYAI  
**Date :** 2025-10-25

---

## 🎯 Objectif
Accéder à l'interface web de pfSense via l'interface WAN (ex. `[REDACTED_WAN_IP]`) depuis un poste client (ex. `[REDACTED_CLIENT_IP]`).

---

## 🔍 Symptômes
- Impossible d'accéder à `https://[REDACTED_WAN_IP]`
- Logs indiquent : `block in on em0 from [REDACTED_CLIENT_IP] to [REDACTED_WAN_IP] port 443`

---

## ✅ Étapes de dépannage

### 1. Vérifier que le service GUI est actif
```bash
Option 11) Restart GUI
```

### 2. Vérifier l'interface WAN
```bash
ip a
```
→ Confirmer que l'IP WAN est bien `[REDACTED_WAN_IP]`

### 3. Désactiver temporairement le pare-feu (test uniquement)
```bash
pfctl -d
```
→ Tester l'accès à `https://[REDACTED_WAN_IP]`

### 4. Ajouter une règle temporaire via le shell
```bash
echo "pass in quick on em0 proto tcp from [REDACTED_CLIENT_IP] to [REDACTED_WAN_IP] port 443 keep state" | pfctl -f -
```
→ Remplacer `em0` si l'interface WAN a un autre nom

### 5. Créer une règle persistante via l'interface web
```text
Firewall > Rules > WAN > Add
```
- Action : Pass
- Interface : WAN
- Protocol : TCP
- Source : Single host → `[REDACTED_CLIENT_IP]`
- Destination : WAN address
- Port : HTTPS (443)
- Description : Autoriser accès GUI depuis PC portable

### 6. Vérifier le service web
```bash
sockstat -4 | grep :443
```
→ Confirmer que le service écoute bien sur le port 443

---

## 🔐 Identifiants par défaut pfSense
- **Nom d'utilisateur** : `admin`
- **Mot de passe** : `pfsense`

Si oublié :
```bash
Option 3) Reset admin account and password
```

---

## 🧠 Astuce sécurité
Limiter l'accès à l'interface web uniquement à ton IP : `[REDACTED_CLIENT_IP]`

---

## 📎 Notes complémentaires
- pfSense bloque par défaut l'accès à l'interface web via WAN
- Toujours tester les règles avec prudence
- Utiliser des alias pour simplifier les règles de pare-feu

---
**Avertissement :** Ce document fait partie d'un ensemble de documentation professionnelle. Toutes les informations sensibles, y compris les adresses IP, ont été masquées pour des raisons de sécurité et de confidentialité. Veuillez remplacer `[REDACTED_WAN_IP]` et `[REDACTED_CLIENT_IP]` par votre configuration réseau réelle lors de la mise en œuvre de ces étapes.