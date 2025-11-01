# 🧾 Compte Rendu : Configuration Proxmox + Cloudflare Tunnel

## 📌 Contexte
- **Objectif** : Accéder à l’interface Proxmox via un sous-domaine public sécurisé (Cloudflare Tunnel).
- **Domaine utilisé** : `proxmox.olyhome.site`
- **Infrastructure** :
  - Proxmox installé sur IP locale `[YOUR_PROXMOX_IP]`
  - Tunnel Cloudflare configuré
  - Certificat Let's Encrypt généré via ACME DNS (plugin Cloudflare)

## ✅ Étapes réalisées
1. Création du tunnel Cloudflare et association du hostname
2. Configuration ACME dans Proxmox avec plugin DNS Cloudflare
3. Obtention du certificat SSL Let's Encrypt pour `proxmox.olyhome.site`
4. Redémarrage du service `pveproxy` pour appliquer le certificat
5. Vérification de la configuration réseau (`ip a`, `/etc/network/interfaces`)
6. Redémarrage du service `cloudflared`
7. Ajout du hostname dans Cloudflare Zero Trust > Tunnel > Public Hostnames
8. Activation de `cloudflared` au démarrage (`systemctl enable cloudflared`)

## ❌ Problèmes rencontrés
- Perte de connectivité réseau après reboot (ping impossible)
- Tunnel Cloudflare marqué comme **DOWN** (Erreur 1033)
- Erreur **502 Bad Gateway** lors de l'accès externe
- `cloudflared` ne redémarrait pas automatiquement
- TLS strict bloquait l'accès à cause du certificat auto-signé

## 🔍 Diagnostic
- La passerelle (`gateway`) n'était pas appliquée au démarrage malgré sa présence dans `/etc/network/interfaces`
- Le service `cloudflared` était lancé avec `--token`, ce qui ignorait le fichier `config.yml`
- Le tunnel n'était pas associé à un hostname dans Cloudflare Zero Trust
- Le mode **Full (Strict)** de Cloudflare exigeait un certificat valide

## 🛠️ Résolution
- Ajout manuel de la route par défaut : `ip route add default via [YOUR_GATEWAY_IP] dev vmbr0`
- Création du fichier `/etc/cloudflared/config.yml` avec :
```yaml
tunnel: <TUNNEL_ID>
credentials-file: /etc/cloudflared/<TUNNEL_ID>.json

ingress:
  - hostname: proxmox.olyhome.site
    service: https://localhost:8006
    originRequest:
      noTLSVerify: true
  - service: http_status:404
```
- Modification du service systemd pour utiliser `--config /etc/cloudflared/config.yml`
- Ajout du hostname dans Cloudflare Tunnel > Public Hostnames
- Passage du mode TLS à **Full** (au lieu de Full Strict)

## ✅ Résultat final
- Accès fonctionnel à `https://proxmox.olyhome.site`
- Tunnel Cloudflare stable et automatique au démarrage
- Interface Proxmox sécurisée et accessible à distance

## 📸 Visuels à intégrer
- Capture du `curl -vk https://proxmox.olyhome.site`
- Capture du tunnel Cloudflare marqué comme Healthy
- Capture du DNS `CNAME` vers `cfargotunnel.com`
- Capture du fichier `config.yml`

## 🔮 Prochaines étapes
### 🔧 DNS locaux avec certificats SSL pour services internes
- Créer des entrées DNS internes (via pfSense ou dnsmasq) pour :
  - `n8n.olyhome.lan`
  - `nginx.olyhome.lan`
  - `pfsense.olyhome.lan`
- Générer des certificats Let's Encrypt pour ces services
- Ajouter des tunnels Cloudflare ou reverse proxy NGINX pour les exposer
- Centraliser les logs et la supervision (Grafana, Prometheus...)

---
🕓 Rapport généré le : 2025-10-27 14:35

---
**Avertissement :** Ce document fait partie d'un ensemble de documentation professionnelle. Toutes les informations sensibles, y compris les adresses IP, ont été masquées pour des raisons de sécurité et de confidentialité. Veuillez remplacer `[YOUR_PROXMOX_IP]` et `[YOUR_GATEWAY_IP]` par votre configuration réseau réelle lors de la mise en œuvre de ces étapes.