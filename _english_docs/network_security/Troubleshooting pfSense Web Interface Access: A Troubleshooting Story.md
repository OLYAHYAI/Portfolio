
# [1mDépannage accès interface Web pfSense via WAN[0m

**Auteur :** Omar LYAHYAI  
**Date :** 2025-10-25

---

## [1m🎯 Objectif[0m
Accéder à l'interface web de pfSense via l'interface WAN (ex. 192.168.11.253) depuis un poste client (ex. 192.168.11.250).

---

## [1m🔍 Symptômes[0m
- Impossible d'accéder à `https://192.168.11.253`
- Logs indiquent : `block in on em0 from 192.168.11.250 to 192.168.11.253 port 443`

---

## [1m✅ Étapes de dépannage[0m

### 1. Vérifier que le service GUI est actif
```bash
Option 11) Restart GUI
```

### 2. Vérifier l'interface WAN
```bash
ip a
```
→ Confirmer que l'IP WAN est bien `192.168.11.253`

### 3. Désactiver temporairement le pare-feu (test uniquement)
```bash
pfctl -d
```
→ Tester l'accès à `https://192.168.11.253`

### 4. Ajouter une règle temporaire via le shell
```bash
echo "pass in quick on em0 proto tcp from 192.168.11.250 to 192.168.11.253 port 443 keep state" | pfctl -f -
```
→ Remplacer `em0` si l'interface WAN a un autre nom

### 5. Créer une règle persistante via l'interface web
```text
Firewall > Rules > WAN > Add
```
- Action : Pass
- Interface : WAN
- Protocol : TCP
- Source : Single host → `192.168.11.250`
- Destination : WAN address
- Port : HTTPS (443)
- Description : Autoriser accès GUI depuis PC portable

### 6. Vérifier le service web
```bash
sockstat -4 | grep :443
```
→ Confirmer que le service écoute bien sur le port 443

---

## [1m🔐 Identifiants par défaut pfSense[0m
- **Nom d'utilisateur** : `admin`
- **Mot de passe** : `pfsense`

Si oublié :
```bash
Option 3) Reset admin account and password
```

---

## [1m🧠 Astuce sécurité[0m
Limiter l'accès à l'interface web uniquement à ton IP : `192.168.11.250`

---

## [1m📎 Notes complémentaires[0m
- pfSense bloque par défaut l'accès à l'interface web via WAN
- Toujours tester les règles avec prudence
- Utiliser des alias pour simplifier les règles de pare-feu

