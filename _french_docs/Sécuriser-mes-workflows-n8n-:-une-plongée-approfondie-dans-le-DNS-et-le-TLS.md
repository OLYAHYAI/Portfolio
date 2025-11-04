# 🌐 DNS & TLS pour n8n — Portfolio

## 🎯 Objectif
Disposer d’un accès **fiable et chiffré** à n8n (et services associés) soit **en public** (Let’s Encrypt DNS‑01), soit **en LAN** (Split‑DNS + certificat local).

---

## 🔷 Option A — Domaine public + Cloudflare + DNS Challenge (Let’s Encrypt)

### Étapes
1. **Domaine** : Freenom → domaine gratuit (ex. `mon-portfolio.tk`).
2. **Cloudflare** : add site, remplacer **nameservers** chez le registrar, statut **Active**.
3. **Token API** : Cloudflare → *API Tokens* → modèle **DNS Edit** limité à la zone.
4. **Nginx Proxy Manager** :
   - Proxy Host → `n8n.mon-portfolio.tk` → `n8n-n8n-1:5678` (Websocket ✅).
   - SSL → **Request a new certificate** → **DNS Challenge** (Cloudflare) + API Token.
   - Force SSL + HTTP/2.
5. Accès : `https://n8n.mon-portfolio.tk`.

### Avantages / Limites
- ✅ Certificat public valide, renouvellement auto, pas d’ouverture 80/443 pour la validation.
- ⚠️ Nécessite un **domaine public** + propagation DNS.

---

## 🔷 Option B — DNS local pfSense + Certificat local

### Étapes
1. **Split‑DNS** : pfSense → DNS Resolver → *Host Overrides* → `n8n.local.home` → IP LAN du host.
2. **Cert local** :
   - OpenSSL auto‑signé **ou** `mkcert` (recommandé pour confiance navigateur).
   - Import **Custom SSL** dans NPM.
3. **NPM Proxy Host** :
   - Domaine : `n8n.local.home` → `n8n-n8n-1:5678` (Websocket ✅), SSL **Custom**, Force SSL.
4. Accès LAN : `https://n8n.local.home`.

### Avantages / Limites
- ✅ Aucune dépendance externe, fonctionne hors ligne.
- ⚠️ Certificat à **faire approuver** sur les postes (installer le CA).

---

## 🧰 Dépannage rapide
- **n8n “secure cookie”** : utiliser **HTTPS** ou `N8N_SECURE_COOKIE=false` (test seulement).
- **NPM ne résout pas le conteneur** : utiliser l’IP du host au lieu du nom du conteneur.
- **DNS interne ne répond pas** : vérifier *Host Overrides* et que les clients utilisent pfSense comme DNS.
- **Let’s Encrypt échoue en DNS‑01** : vérifier **API Token** Cloudflare (scopes), propagation TXT.

---

## ✅ Checklist
- [ ] Domaine public OU suffixe local choisi.
- [ ] Cloudflare actif (option A) OU Host Override pfSense (option B).
- [ ] NPM Proxy Host créé (Websocket ✅).
- [ ] Certificat : Let’s Encrypt (A) OU Custom (B).
- [ ] Accès via **HTTPS** vérifié.

---

## 🗺️ Architecture (résumé)
- **pfSense** : firewall + DNS (Split‑DNS).
- **Docker host** : Nginx Proxy Manager (reverse proxy TLS), n8n.
- **Cloudflare** (Option A) : DNS public + DNS‑01 challenge.
- **Clients LAN** : naviguent vers `https://n8n.local.home` ou `https://n8n.mon-portfolio.tk`.

---
**Avertissement :** Ce document fait partie d'un ensemble de documentation professionnelle. Toutes les informations sensibles, y compris les adresses IP, ont été masquées pour des raisons de sécurité et de confidentialité.