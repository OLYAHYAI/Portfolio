
# 🚀 Intégration IA avec MCP, Docker et N8N

**Auteur :** Omar LYAHYAI  
**Date :** 2025-10-25

---

## 🎯 Objectif
Automatiser l'organisation des notes Obsidian à l'aide d'une IA accessible via API, orchestrée avec Docker, MCP et N8N.

---

## 🧑‍💻 Prérequis
- Docker installé
- MCP (Modular Copilot Platform) déployé
- N8N installé (via Docker ou local)
- Vault Obsidian accessible localement

---

## 🧪 Étapes détaillées

### 1. 📦 Déployer MCP avec Docker
```bash
docker run -d   --name mcp   -p 5000:5000   -v $(pwd)/mcp_data:/app/data   ghcr.io/modularcopilot/mcp:latest
```
- MCP expose une API sur `http://localhost:5000`

### 2. 🧠 Préparer un prompt pour l'IA
Exemple de prompt :
```json
{
  "prompt": "Unifie toutes mes notes Obsidian en anglais. Traduis les titres français, regroupe les contenus similaires, et crée un index thématique."
}
```

### 3. 🤖 Créer un workflow N8N
#### Étapes du workflow :
1. **Trigger** : manuel ou planifié
2. **Read Files** : lire les fichiers `.md` du vault Obsidian
3. **HTTP Request** : envoyer le contenu à MCP via POST
   - URL : `http://localhost:5000/api/prompt`
   - Body : JSON avec le prompt et le contenu des notes
4. **Write File** : sauvegarder la réponse dans un nouveau fichier `.md`

### 4. ⚙️ Exemple de configuration HTTP Request dans N8N
- **Method** : POST
- **URL** : `http://localhost:5000/api/prompt`
- **Headers** : `Content-Type: application/json`
- **Body** :
```
{
  "prompt": "Organise mes notes Obsidian par thème et langue.",
  "notes": "{{ $json["content"] }}"
}
```

### 5. 📁 Sauvegarde dans Obsidian
- Le fichier généré est enregistré dans le dossier `Vault/Index/organisation.md`
- Tu peux lier ce fichier dans ta note d’accueil Obsidian

---

## 🧠 Astuces
- Utilise des tags `#fr` et `#en` pour identifier les langues
- Crée une note d’index multilingue avec des liens vers chaque thème
- Tu peux enrichir le prompt pour inclure des traductions, des regroupements ou des résumés

---

## 🔗 Ressources
- [MCP GitHub](https://github.com/modularcopilot/mcp)
- [N8N Docs](https://docs.n8n.io)
- [Obsidian](https://obsidian.md)

