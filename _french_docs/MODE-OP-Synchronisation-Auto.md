---
title: "Mode Opératoire: Synchronisation Automatique Obsidian vers GitHub"
category: automation_ai
date: 2025-10-29
tags: [obsidian, git, automatisation, synchronisation, workflow, github-pages]
difficulty: intermediate
status: complete
---

# 🔄 Mode Opératoire: Système de Synchronisation Automatique

## 📋 Vue d'ensemble

**Objectif:** Synchroniser automatiquement les modifications Obsidian vers GitHub et déployer sur le site web.

**Flux:**
```
Édition Obsidian → Détection (5s) → Commit Git → Push GitHub → Build Jekyll → Site Web Live
```

**Temps Total:** ~2-3 minutes de l'édition au déploiement

---

## 🎯 Implémentation Actuelle: Option 1 (File Watcher + Git)

### Architecture Système

```
┌─────────────────┐
│ Édition Obsidian│
└────────┬────────┘
         │
         ▼
┌─────────────────────────┐
│ inotify File Watcher    │
│ (service systemd)       │
└────────┬────────────────┘
         │ (débounce 5 sec)
         ▼
┌─────────────────────────┐
│ auto-sync-to-github.sh  │
│ - Détecte changements   │
│ - Git add               │
│ - Git commit            │
│ - Git push              │
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────┐
│ Dépôt GitHub            │
│ OLYAHYAI/Portfolio      │
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────┐
│ GitHub Actions          │
│ (.github/workflows/)    │
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────┐
│ Build Jekyll            │
│ (Auto-déclenché)        │
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────┐
│ Déploiement GitHub Pages│
│ https://olyahyai.       │
│ github.io/Portfolio/    │
└─────────────────────────┘
```

---

## 🚀 Procédure d'Installation

### Prérequis

- [x] Système Ubuntu/Debian Linux
- [x] Git installé
- [x] Obsidian avec vaults ProjectEN et ProjectFR_new
- [x] Compte GitHub (OLYAHYAI)
- [x] Dépôt: OLYAHYAI/Portfolio

### Étape 1: Authentification Git (UNE SEULE FOIS)

**Choisir UNE méthode:**

#### Option A: GitHub CLI (Recommandée)
```bash
gh auth login
```

#### Option B: Clé SSH (Plus sécurisée)
```bash
ssh-keygen -t ed25519 -C "votre@email.com"
cat ~/.ssh/id_ed25519.pub
# Ajouter à: https://github.com/settings/keys
cd /home/f4blox/Desktop/Gemini/opencode/openproject/OpenProject
git remote set-url origin git@github.com:OLYAHYAI/Portfolio.git
```

#### Option C: Credential Helper
```bash
git config --global credential.helper store
cd /home/f4blox/Desktop/Gemini/opencode/openproject/OpenProject
git push -u origin main
```

### Étape 2: Installer la Synchronisation Auto

```bash
cd /home/f4blox/Desktop/Gemini/opencode/scripts
./setup-auto-sync.sh
sudo systemctl enable obsidian-github-sync
sudo systemctl start obsidian-github-sync
```

### Étape 3: Activer GitHub Pages

1. Visiter: https://github.com/OLYAHYAI/Portfolio/settings/pages
2. Source: main / (root)
3. Sauvegarder

---

## 🔧 Opérations Quotidiennes

**Utilisation Normale:**
1. Éditer dans Obsidian
2. Sauvegarder (Ctrl+S)
3. Synchronisation auto en 5 secondes
4. En ligne en ~2 minutes

**Synchronisation Manuelle:**
```bash
/home/f4blox/Desktop/Gemini/opencode/scripts/sync-now.sh
```

**Surveillance:**
```bash
sudo systemctl status obsidian-github-sync
sudo journalctl -u obsidian-github-sync -f
```

---

## 🔮 Options Futures (Documentées)

### Option 2: MCP + n8n (Workflows IA)
- Messages de commit générés par IA
- Validation de contenu
- Intégration multi-services
- Configuration: 30-60 min

### Option 6: Claude + Amélioration de Contenu
- Contrôle qualité IA
- Auto-correction grammaire/orthographe
- Commits intelligents
- Vérification confidentialité
- Optimisation SEO
- Traduction auto EN ↔ FR
- Configuration: 1-2 heures

**Documentation:** `/opencode/conscience/updates/action-35-sync-options-analysis.md`

---

## 📚 Ressources

- Scripts: `/opencode/scripts/`
- Conscience: `/opencode/conscience/`
- Site Web: https://olyahyai.github.io/Portfolio/
- Dépôt: https://github.com/OLYAHYAI/Portfolio

---

**Dernière Mise à Jour:** 2025-10-29  
**Version:** 1.0  
**Statut:** Prêt pour déploiement

---
**Avertissement :** Ce document fait partie d'un ensemble de documentation professionnelle. Toutes les informations sensibles, y compris les adresses IP, ont été masquées pour des raisons de sécurité et de confidentialité.