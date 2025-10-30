tw```
Tu es un expert en réseaux, DNS et reverse proxy. Explique pas à pas, puis propose un schéma ASCII clair, l’architecture suivante :

- Pare-feu/routeur : pfSense sur un sous-réseau distinct (LAN).
- DNS :
  - Option A (public) : zone DNS hébergée chez Cloudflare ; Let’s Encrypt en DNS‑01 via Nginx Proxy Manager (NPM) pour émettre un certificat valide sans ouvrir 80/443 pour la validation ; proxy host “n8n.mon-domaine.tld” pointant vers le service n8n (Docker).
  - Option B (local) : Split‑DNS avec pfSense (Host Overrides) pour “n8n.local.home” vers l’IP LAN du host Docker ; certificat local (mkcert ou auto‑signé) importé en “Custom SSL” dans NPM.
- Reverse proxy : Nginx Proxy Manager en Docker, WebSocket activé, Force SSL, HTTP/2.
- Application : n8n en Docker (port interne 5678), cookie sécurisé compatible HTTPS.
- Contraintes : aucune exposition WAN nécessaire pour DNS‑01 (Option A) ; en Option B, tout reste local et chiffré mais nécessite l’installation du CA sur les postes clients.

Livrables attendus :
1) Un plan de mise en œuvre détaillé pour A et B.
2) Les vérifications (dig/nslookup, curl -I) et commandes clés.
3) Un schéma ASCII de l’architecture.
4) Les risques et mesures de sécurité (rotation token Cloudflare, limites de scope, installation du CA, isolement réseaux Docker).
5) Conseils pour la maintenance (renouvellement certs, sauvegarde NPM, backup n8n).
```