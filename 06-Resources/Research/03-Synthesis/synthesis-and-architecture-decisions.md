# Synthese et Decisions d'Architecture

**Date:** 2026-04-02
**Contexte:** Re-appropriation de Dex (fork MIT) en setup personnel product-oriented.

---

## 1. Situation actuelle

### Ce qu'on a
- Fork de [davekilleen/Dex](https://github.com/davekilleen/Dex) base sur des commits MIT (pre-changement licence)
- Architecture 5 couches : CLAUDE.md (cerveau) + Hooks + MCP Servers + Skills + Vault PARA
- ~60 skills, 7 MCP servers Python, 10+ hooks JS/Shell
- Configure pour multi-role (11 role_groups), orientation "commercialisable"

### Ce qui ne va pas
1. **Identite** -- le projet est "Dex by Dave", pas le mien
2. **Perimetre** -- trop large (11 roles, integrations Granola/ScreenPipe/Things/Trello...), peu profond sur product
3. **Black box** -- MCP servers et hooks non audites, comportements implicites
4. **Dependencies inutiles** -- Granola, Calendar.app, ScreenPipe, QMD pour un setup initial
5. **Licence upstream** -- PolyForm Noncommercial depuis Feb 2026, ne peut plus merger les nouveautes

---

## 2. Recherche -- ce qu'on a appris

### Projets analyses

| Projet | Idee-cle a retenir |
|--------|-------------------|
| **carl-vellotti** | Skill auto-activator (59ms keyword hook), workflow-as-directory, profils personnes avec prefs communication, GOALS.md avec metriques |
| **phuryn/pm-skills** | Methodology-as-code (65 frameworks PM nommes), architecture 3 tiers (skills/commands/plugins), checkpoints humains |
| **Mehdibargach** | Prompt engineering par contraintes (rejets explicites, limites de pages, push-back sur le vague), 10 skills PM absentes de Dex |
| **everything-claude-code** | Rules hierarchiques (common + language), hooks de compaction, modes contextuels (dev/research/review) |
| **KotyV/pipeline** | Contraintes negatives (dire ce qu'on NE PEUT PAS faire), shared reference docs, quality gates, recovery protocol |
| **liza-mas/liza** | Circuit breakers, stop triggers, enforcement mecanique vs instructionnel, doer/reviewer separation |
| **carl-vellotti/product-os** (template) | Scaffolding structurel fort (BLANK-OS + EXAMPLE-OS), mais scaffolding comportemental faible -- confirme que la force de Dex est dans le CLAUDE.md, pas la structure de dossiers |
| **ArtemXTech/personal-os-skills** | Integration tools locaux via lecture directe des caches/SQLite (pas d'API), skill `/recall` avec graph HTML de sessions, distribution via marketplace plugin |

### Patterns transversaux identifies

1. **Enforcement > Instruction** -- Les systemes les plus fiables encodent les regles dans du code (hooks, state machines), pas dans des prompts
2. **Contraintes negatives > Consignes positives** -- Dire "ne fais PAS X" est plus efficace que "fais Y"
3. **Methodology-as-code** -- Injecter des frameworks nommes produit de meilleurs outputs que des prompts generiques
4. **Checkpoints humains** -- Les meilleurs workflows pausent aux points de decision
5. **Stateless skills + Stateful context = combo gagnant** -- Les skills de phuryn/Mehdibargach + le contexte persistent de Dex serait le meilleur des deux mondes
6. **Scaffolding structurel ≠ Scaffolding comportemental** -- carl-vellotti template prouve qu'une bonne structure de dossiers sans regles comportementales (CLAUDE.md riche) ne suffit pas. C'est le CLAUDE.md qui fait la difference, pas les dossiers.
7. **Local-first integrations** -- ArtemXTech montre qu'on peut integrer des tools en lisant directement leurs caches/DB locaux plutot que via des APIs ou MCPs complexes

---

## 3. Vision cible

### North Star
Un setup personnel Claude Code pour un PM product qui :
- **Accumule du contexte** au fil du temps (personnes, projets, decisions, meetings)
- **Execute des frameworks PM** avec profondeur methodologique
- **Reste leger** -- pas de dependances externes non essentielles
- **Est compris de A a Z** -- zero black box
- **Evolue avec moi** -- facile a etendre, pas de dette technique

### Ce que ce n'est PAS
- Un produit commercialisable multi-role
- Un clone de Dex rebrande
- Un systeme autonome (les checkpoints humains sont voulus)

---

## 4. Decisions d'architecture

### ADR-001: Garder l'architecture 5 couches

**Decision:** Conserver la structure CLAUDE.md + Hooks + MCP + Skills + Vault.

**Raison:** L'architecture est saine. Les problemes sont dans le contenu (trop large, pas assez profond), pas dans la structure. Chaque couche a un role clair :
- CLAUDE.md = regles comportementales
- Hooks = automatisations mecaniques (enforcement > instruction)
- MCP = operations deterministes sur le vault
- Skills = workflows utilisateur
- Vault = memoire persistante

**Ce qui change:** On nettoie chaque couche, on ne change pas l'architecture.

---

### ADR-002: Simplifier le vault (PARA allege)

**Decision:** Garder PARA mais reduire les dossiers aux essentiels.

**Garder tel quel:**
- `00-Inbox/` -- capture zone
- `03-Tasks/Tasks.md` -- backlog
- `04-Projects/` -- projets actifs
- `05-Areas/People/` -- pages personnes (mais enrichir le format)
- `06-Resources/` -- reference
- `System/` -- config

**Simplifier ou fusionner:**
- `01-Quarter_Goals/` + `02-Week_Priorities/` → integrer dans un seul `GOALS.md` avec metriques (pattern carl-vellotti)
- `05-Areas/Companies/` -- garder uniquement si necessaire
- `05-Areas/Career/` -- garder mais optionnel
- `07-Archives/` -- garder (zero effort)

**Supprimer:**
- `System/Demo/` -- inutile pour usage personnel

---

### ADR-003: Nettoyer les MCP servers

**Decision:** Auditer et reduire a 2-3 MCP servers essentiels.

| MCP | Verdict | Raison |
|-----|---------|--------|
| `work.json` (work_server.py) | **Garder** | Coeur du systeme : tasks, goals, priorities |
| `tasks.json` | **Evaluer** | Potentiellement redondant avec work |
| `calendar.json` | **Conditionnel** | Garder si Calendar.app est utilise |
| `career.json` | **Conditionnel** | Garder si /career-coach est desire |
| `granola.json` | **Supprimer** | Dependance externe non pertinente |
| `commitment.json` | **Supprimer** | Lie a ScreenPipe (beta non active) |
| `resume.json` | **Conditionnel** | Garder si /resume-builder est desire |

**Action:** Auditer le code de chaque serveur conserve pour comprendre exactement ce qu'il fait.

---

### ADR-004: Tailler les skills

**Decision:** Passer de ~60 skills a un core de ~15, avec un chemin clair pour en ajouter.

**Core skills (garder):**
- `/daily-plan` -- planning quotidien
- `/daily-review` -- review de fin de journee
- `/week-plan` + `/week-review` -- cycle hebdo
- `/meeting-prep` -- prep meetings (enrichir avec prefs personnes)
- `/process-meetings` -- traitement meetings
- `/project-health` -- sante projets
- `/product-brief` -- PRD (enrichir avec methodology-as-code)
- `/triage` -- routage inbox

**Skills a ajouter (inspires de la recherche):**
- `/product-teardown` -- analyse produit (Mehdibargach)
- `/prioritize` -- RICE/ICE scoring (Mehdibargach)
- `/market-sizing` -- TAM/SAM/SOM (Mehdibargach)
- `/ab-test-design` -- design d'experience (Mehdibargach)
- `/decision-doc` -- doc de decision (Mehdibargach)

**Skills a supprimer (exemples):**
- Tous les skills `_available/` role-specific non-product (sales, finance, operations...)
- Skills d'integration non pertinentes (things-setup, trello-setup, zoom-setup, ms-teams-setup...)
- Skills beta (screenpipe-setup, commitment-scan)
- Skills meta non necessaires (dex-demo, dex-whats-new, dex-rollback)

**Le principe:** Chaque skill conserve doit etre audite et compris.

---

### ADR-005: Enrichir les pages personnes

**Decision:** Ajouter un bloc "Communication & Working Style" aux pages personnes.

**Inspire de:** carl-vellotti (profils avec prefs communication)

**Format cible:**
```markdown
## Communication & Working Style
- **Preferred format:** BLUF / narrative / data-first
- **Channel preference:** Slack > email / email-first
- **Meeting style:** agenda-driven / free-form
- **Decision pattern:** quick decider / needs time / consensus-builder
- **What they care about:** [2-3 points]
- **How to work with them:** [2-3 points]
```

**Impact:** Le `/meeting-prep` adapte sa sortie en fonction du profil.

---

### ADR-006: Adopter le pattern "shared references"

**Decision:** Creer un dossier `.claude/reference/` avec des docs partages que les skills chargent au runtime.

**Inspire de:** KotyV/pipeline (7 reference docs), everything-claude-code (rules hierarchiques)

**Docs a creer:**
- `pm-frameworks.md` -- frameworks PM de reference (RICE, OSTs, lean canvas, pre-mortem...)
- `quality-standards.md` -- contraintes de qualite pour les outputs (limites pages, rejet du vague)
- `working-agreements.md` -- ce que Claude ne peut PAS faire (contraintes negatives)

**Avantage:** Les skills restent legers, les standards sont centralises.

---

### ADR-007: Implementer le skill auto-activator

**Decision:** Ajouter un hook `UserPromptSubmit` qui matche les prompts naturels aux skills.

**Inspire de:** carl-vellotti (skill-activator.py, 59ms)

**Pourquoi:** L'utilisateur ne devrait pas avoir a memoriser les noms de slash commands. "Prepare-moi pour le meeting avec Jean" devrait activer `/meeting-prep` automatiquement.

**Contrainte:** Le hook doit etre rapide (<100ms), sans appel AI, pur keyword matching.

**Priorite:** Phase 2 (apres le nettoyage).

---

### ADR-008: GOALS.md avec metriques concretes

**Decision:** Remplacer la hierarchie actuelle (Quarter_Goals.md separee + Week_Priorities.md) par un `GOALS.md` unique avec baseline/target/status.

**Inspire de:** carl-vellotti

**Format:**
```markdown
## Q2 2026 Goals

### 1. [Goal Title]
- **Baseline:** [current state with numbers]
- **Target:** [desired state with numbers]
- **Status:** on-track / at-risk / behind
- **Key Results:**
  - [ ] KR1 (metric)
  - [ ] KR2 (metric)
```

**Avantage:** Chaque skill qui touche aux goals a des chiffres concrets a reference, pas du vague.

---

### ADR-009: Ajouter des contraintes negatives au CLAUDE.md

**Decision:** Ajouter une section "What I Cannot Do" au CLAUDE.md.

**Inspire de:** KotyV/pipeline, liza

**Exemples:**
- Ne JAMAIS creer de fichier sans demander
- Ne JAMAIS modifier une page personne sans sourcer l'info
- Ne JAMAIS marquer une tache complete sans confirmation explicite
- Ne JAMAIS generer de contenu generique sans framework nomme
- Ne JAMAIS proposer plus de 3 priorites quotidiennes

**Pourquoi:** Plus efficace que les instructions positives pour prevenir les derives.

---

## 5. Plan d'execution

### Phase 1 -- Nettoyage (Sprint 1)
1. Supprimer les skills non pertinentes (~40 skills)
2. Supprimer les MCP servers non necessaires
3. Supprimer les hooks lies aux features supprimees
4. Auditer et documenter chaque composant restant
5. Nettoyer le CLAUDE.md (retirer refs a Dex/Dave, analytics, screenpipe, etc.)

### Phase 2 -- Restructuration (Sprint 2)
6. Creer `GOALS.md` avec format metriques
7. Enrichir le template pages personnes
8. Creer les shared reference docs (frameworks, quality, contraintes)
9. Ajouter les contraintes negatives au CLAUDE.md
10. Rebrander (optionnel -- nouveau nom, nouvelle identite)

### Phase 3 -- Enrichissement (Sprint 3)
11. Porter les skills PM manquantes (teardown, prioritize, market-sizing...)
12. Implementer le skill auto-activator hook
13. Enrichir les skills existantes avec methodology-as-code
14. Configurer les checkpoints humains dans les workflows longs

### Phase 4 -- Dogfooding
15. Utiliser le systeme au quotidien
16. Capturer les frictions et iterer

---

## 6. Risques et mitigations

| Risque | Impact | Mitigation |
|--------|--------|------------|
| Casser des dependances en supprimant | Moyen | Auditer les imports/references avant chaque suppression |
| Perdre des features utiles non identifiees | Faible | Archiver plutot que supprimer definitivement |
| Sur-engineering du setup | Moyen | Commencer par Phase 1, valider avant Phase 2 |
| Licence upstream | Faible | Notre base est MIT, on ne merge pas les nouveaux commits |

---

## 7. Sources

- [davekilleen/Dex](https://github.com/davekilleen/Dex) -- upstream, PolyForm Noncommercial
- [carlvellotti/pg-carl-vellotti-master-cc](https://github.com/carlvellotti/pg-carl-vellotti-master-cc) -- PM OS demo
- [phuryn/pm-skills](https://github.com/phuryn/pm-skills) -- 65 PM skills, MIT
- [Mehdibargach/claude-code-pm-skills](https://github.com/Mehdibargach/claude-code-pm-skills) -- 20 PM skills, MIT
- [affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code) -- framework 151+ skills
- [KotyV/claude-code-pipeline](https://github.com/KotyV/claude-code-pipeline) -- pipeline dev, MIT
- [liza-mas/liza](https://github.com/liza-mas/liza) -- multi-agent Go system
- [carlvellotti/carls-product-os](https://github.com/carlvellotti/carls-product-os) -- PM OS template (blank + example)
- [ArtemXTech/personal-os-skills](https://github.com/ArtemXTech/personal-os-skills) -- integration skills (Granola, Wispr, recall)
