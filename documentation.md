---
layout: page
title: Homelab Documentation
permalink: /documentation/
---

# 📚 Homelab Documentation Portal

Welcome to the complete homelab documentation. Choose your language and explore the knowledge base.

---

## 🇬🇧 English Documentation

### Categories

{% assign docs_dir = 'docs/english' %}

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin: 20px 0;">

<div>
#### 🔧 Infrastructure (5 guides)
Building and maintaining core infrastructure
- Proxmox VE setup
- pfSense configuration  
- VM management
- Storage optimization
- Network recovery

**[View →](/docs/english/infrastructure/)**
</div>

<div>
#### 🔒 Network Security (4 guides)
DNS, TLS, SSL, and security architecture
- SSL certificates
- Reverse proxies
- DNS configuration
- pfSense troubleshooting

**[View →](/docs/english/network_security/)**
</div>

<div>
#### 🤖 Automation & AI (4 guides)
Automation, LLM integration, and note management
- MCP deployment
- n8n workflows
- Obsidian sync
- AI integration

**[View →](/docs/english/automation_ai/)**
</div>

<div>
#### 🐳 Docker Services (3 guides)
Container orchestration and Docker Compose
- Docker Compose setup
- Volume configuration
- Image management
- Service orchestration

**[View →](/docs/english/docker_services/)**
</div>

<div>
#### 📍 Remote Access (2 guides)
Secure external access solutions
- Cloudflare Tunnel
- Twingate setup
- VPN configuration

**[View →](/docs/english/remote_access/)**
</div>

<div>
#### 📖 Reference (4 guides)
URLs, roadmaps, and architecture guides
- Access points
- Service URLs
- Architecture diagrams
- Prompts & templates

**[View →](/docs/english/reference/)**
</div>

</div>

---

## 🇫🇷 Documentation Française

### Catégories

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin: 20px 0;">

<div>
#### 🔧 Infrastructure (5 guides)
Construction et maintenance de l'infrastructure de base
- Configuration Proxmox VE
- Configuration pfSense
- Gestion des VM
- Optimisation du stockage
- Récupération réseau

**[Voir →](/docs/french/infrastructure/)**
</div>

<div>
#### 🔒 Sécurité Réseau (4 guides)
DNS, TLS, SSL et architecture de sécurité
- Certificats SSL
- Proxies inverses
- Configuration DNS
- Dépannage pfSense

**[Voir →](/docs/french/network_security/)**
</div>

<div>
#### 🤖 Automatisation & IA (4 guides)
Automatisation, intégration LLM et gestion des notes
- Déploiement MCP
- Workflows n8n
- Synchronisation Obsidian
- Intégration IA

**[Voir →](/docs/french/automation_ai/)**
</div>

<div>
#### 🐳 Services Docker (3 guides)
Orchestration de conteneurs et Docker Compose
- Configuration Docker Compose
- Configuration de volume
- Gestion d'images
- Orchestration des services

**[Voir →](/docs/french/docker_services/)**
</div>

<div>
#### 📍 Accès à Distance (2 guides)
Solutions d'accès externe sécurisé
- Tunnel Cloudflare
- Configuration Twingate
- Configuration VPN

**[Voir →](/docs/french/remote_access/)**
</div>

<div>
#### 📖 Référence (4 guides)
URLs, feuilles de route et guides d'architecture
- Points d'accès
- URLs de services
- Diagrammes d'architecture
- Modèles & invites

**[Voir →](/docs/french/reference/)**
</div>

</div>

---

## 📊 Documentation Statistics

- **Total Files**: 48 (24 EN + 24 FR)
- **Total Categories**: 8
- **Last Updated**: {{ site.time | date: "%B %d, %Y" }}
- **Languages**: English, Français

---

## 🔍 Quick Links

| Resource | Link |
|----------|------|
| **Project Home** | [Home](/) |
| **All Categories** | [Browse Docs](/documentation/) |
| **GitHub Repository** | [GitHub](https://github.com/yourusername/opencode) |
| **Contact** | [Email](mailto:{{ site.email }}) |

---

## 💡 How to Use

1. **Select a Language**: Choose English or Français above
2. **Pick a Category**: Browse the 8 knowledge categories
3. **Read & Learn**: Deep dive into any topic
4. **Reference**: Use for troubleshooting and setup guides
5. **Contribute**: Help us improve the documentation!

---

**Last Updated**: {{ site.time | date: "%Y-%m-%d %H:%M:%S" }}
