# Setup local — sw_os

> Document de transition cloud → local. À supprimer une fois le setup terminé.

---

## Contexte

Jusqu'ici tu travaillais depuis une session Claude cloud (claude.ai/code) sur le repo `sebWarembourg/SW_os`.
Le vault existait uniquement dans l'environnement cloud — pas de copie locale.

Ce doc te guide pour reprendre proprement depuis ton Mac.

---

## Ce que contient le repo

**Framework (toujours dans git) :**
- `.claude/` — skills, hooks, config, reference docs
- `core/` — MCP servers Python
- `System/Templates/` — templates pages personnes, projets
- `CLAUDE.md` — instructions système sw_os
- `LICENSE` — MIT, Seb Warembourg

**Données personnelles (dans git car trackées) :**
- `03-Tasks/Tasks.md` — backlog tâches
- `01-Quarter_Goals/Quarter_Goals.md` — objectifs Q2 2026
- `02-Week_Priorities/Week_Priorities.md` — priorités semaine
- `System/pillars.yaml` — tes 3 piliers
- `System/user-profile.yaml` — ton profil
- `06-Resources/Learnings/` — patterns erreurs, préférences

**Données migrées via branche temporaire `claude/personal-data-migration-OpLPb` :**
- `System/Session_Learnings/` — 6 fichiers (janv → avril 2026)

---

## Étapes de setup

### 1. Cloner le repo

```bash
git clone https://github.com/sebWarembourg/SW_os.git
cd SW_os
```

### 2. Récupérer les Session Learnings

```bash
git fetch origin claude/personal-data-migration-OpLPb
git checkout origin/claude/personal-data-migration-OpLPb -- System/Session_Learnings/
```

Les fichiers sont maintenant dans ton dossier local. Ils restent gitignorés (normal).

### 3. Configurer iCloud sync (optionnel mais recommandé)

Pour synchroniser entre Mac et mobile (Obsidian) :

```bash
# Déplace le vault dans iCloud Drive
mv ~/SW_os ~/Library/Mobile\ Documents/iCloud~md~obsidian/Documents/SW_os
```

Puis dans Obsidian mobile : ouvrir vault depuis iCloud → `SW_os`.

### 4. Installer les dépendances MCP

```bash
cd ~/SW_os   # ou le chemin iCloud
./install.sh
```

### 5. Ouvrir dans Claude Code

```bash
claude ~/SW_os
```

La session charge automatiquement le contexte sw_os via `CLAUDE.md` + hooks.

---

## Architecture en suspens (projet planifié)

**Projet : sw_os Global Migration** — priorité haute

Objectif : séparer le framework sw_os du vault Dex pour que les skills/hooks soient disponibles dans tous les projets locaux.

- Skills → `~/.claude/skills/`
- Hooks → `~/.claude/settings.json`
- CLAUDE.md système → `~/.claude/CLAUDE.md`
- SW_os garde uniquement : vault + CLAUDE.md léger avec paths

À traiter en session dédiée.

---

## Repo GitHub

- **URL :** https://github.com/sebWarembourg/SW_os
- **Licence :** MIT (Seb Warembourg) — fork original était MIT, Dave Killeen a changé ensuite en PolyForm Noncommercial, non applicable rétroactivement
- **Lien de fork :** toujours actif (ticket support GitHub à faire si besoin de couper). On veut en tout cas complètement s'isoler du fonctionnement initial et de ces dépendances.

---

## Rappel workflow cloud vs local

| | Local (Mac) | Cloud (claude.ai/code) |
|--|-------------|------------------------|
| Accès vault Obsidian | ✅ direct | ❌ |
| `~/.claude/` global | ✅ | ❌ |
| Skills sw_os | ✅ | ✅ (dans repo) |
| Push/pull git | ✅ libre | ⚠️ branches `claude/` uniquement |

**Règle simple : 80% local, cloud uniquement si nécessaire.**
