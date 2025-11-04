
# Journal Technique – 2025-10-22

## Auteur : Omar LYAHYAI

---

## 🔧 Tests de modèles LLM

### GPT-OSS-20B
- Tentative de téléchargement et d'exécution du modèle `gpt-oss-20b` (21B paramètres, quantifié en 4-bit, ~12 Go).
- Licence : Apache 2.0 (open source, usage commercial autorisé).
- Incompatibilité détectée avec le matériel actuel (ThinkPad E14 Gen 2 – i5-1135G7, 16 Go RAM, 256 Go SSD) :
  - Pas de GPU dédié (Iris Xe intégré, supporte Vulkan mais avec performances limitées).
  - RAM insuffisante pour charger le modèle complet.
  - Risques : lenteurs extrêmes, plantages, ou échec de chargement.

### Mistral 7B (Q4)
- Recherche de modèles Mistral 7B quantifiés (Q4) via LM Studio.
- Modèles identifiés :
  - `mistral-7b-ielts-evaluator-q4` (évaluation de textes IELTS).
  - `Mistral-7B-Instruct-v0.1-Q4_K_M` (usage général, recommandé).
- Taille : ~4.3 Go, compatible avec la configuration actuelle.
- Recommandation : utiliser Mistral 7B Q4 ou TinyLlama/Phi-2 pour de meilleures performances locales.

---

## ⚙️ Proxmox – Débogage VM Kali Linux

### Erreur rencontrée :
```
Kernel panic - not syncing: VFS: Unable to mount root fs on unknown-block(0,0)
```

### Causes possibles :
- Mauvais paramètre `root=` dans GRUB.
- Absence de disque EFI avec BIOS UEFI (OVMF).
- Contrôleur de disque incompatible (VirtIO SCSI Single requis pour iothread).
- Initramfs corrompu ou manquant.

### Modifications apportées :
- Changement du BIOS de **SeaBIOS** vers **OVMF (UEFI)**.
- Ajout d’un **disque EFI** via l’onglet Hardware > Add > EFI Disk.
- Modification du **type de machine** de `i440fx` vers `q35`.
- Ajustement du **contrôleur de disque** vers **VirtIO SCSI Single** pour compatibilité avec `iothread=on`.
- Vérification de l’ordre de boot et des partitions.

### Avertissement Proxmox :
```
WARN: iothread is only valid with virtio disk or virtio-scsi-single controller, ignoring
```
➡️ Résolu en utilisant un contrôleur compatible (VirtIO SCSI Single).

---

## 💡 Recommandations pour le ThinkPad E14 Gen 2
- Utiliser des modèles LLM quantifiés (Q4 ou inférieurs) : Mistral 7B Q4, Phi-2, TinyLlama.
- Utiliser **LM Studio** ou **llama.cpp** avec AVX2 + Vulkan.
- Éviter les modèles >10 Go sans GPU dédié.
- Prévoir un SSD externe si besoin de plus d’espace pour les modèles.

