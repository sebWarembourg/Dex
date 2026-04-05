---
name: prioritize
description: Priorise une liste d'initiatives, features ou tâches avec RICE ou ICE selon le contexte
context: fork
---

## Purpose

Transformer une liste floue de "choses à faire" en backlog priorisé et défendable. Utilise RICE pour les décisions à enjeux élevés, ICE pour les arbitrages rapides.

Charge `.claude/reference/pm-frameworks.md` → sections RICE et ICE avant de commencer.

## Usage

- `/prioritize` — démarre une session de priorisation interactive
- `/prioritize [liste]` — priorise une liste fournie directement

---

## Process

### Step 1 : Capturer la liste

Si aucune liste n'est fournie, demander :
> "Qu'est-ce qu'on priorise ? Liste les initiatives, features ou tâches — une par ligne."

Accepter n'importe quel format (bullets, numéros, texte libre). Ne pas corriger ou reformuler avant que la liste soit complète.

### Step 2 : Choisir le framework

**Checkpoint humain — ne pas sauter.**

> "On a [N] éléments. Je propose **RICE** si on a des données d'usage ou d'audience, **ICE** si c'est plus qualitatif. Tu as des données chiffrées sous la main ?"

- Données dispo → RICE
- Estimation qualitative → ICE
- L'utilisateur choisit → respecter son choix

### Step 3 : Scorer

#### Mode RICE

Pour chaque item, demander ou estimer :
- **Reach** — combien de personnes impactées par mois ? (chiffre)
- **Impact** — 3 = massif / 2 = significatif / 1 = faible / 0.5 = minimal
- **Confidence** — % de certitude sur les estimations (100% / 80% / 50%)
- **Effort** — person-weeks

Score = (Reach × Impact × Confidence) / Effort

#### Mode ICE

Pour chaque item, noter de 1 à 10 :
- **Impact** — effet sur l'objectif principal
- **Confidence** — certitude de l'estimation
- **Ease** — facilité d'implémentation

Score = moyenne des 3

### Step 4 : Présenter le résultat

```markdown
## Backlog priorisé — [Framework utilisé]

| Priorité | Initiative | Score | Rationale |
|----------|-----------|-------|-----------|
| P0 | [Item] | [Score] | [1 ligne] |
| P1 | [Item] | [Score] | [1 ligne] |
| P2 | [Item] | [Score] | [1 ligne] |

### Ce qu'on NE fait PAS maintenant
- [Item déprioritisé] — [Raison courte]
```

### Step 5 : Checkpoint validation

> "Ça te semble juste ? Un score qui te choque ?"

Ajuster si l'utilisateur challenge. Documenter les overrides et leur raison.

### Step 6 : Sauvegarder (optionnel)

Si l'utilisateur valide :
- Sauvegarder dans `04-Projects/[Projet]/Backlog_Prioritise.md`
- Ou dans `03-Tasks/Tasks.md` si ce sont des tâches

---

## Règles

- Ne jamais scorer seul sans poser au moins 1 question par item sur le Reach ou l'Impact
- Toujours afficher le "ce qu'on ne fait pas" — l'exclusion est aussi importante que la sélection
- Si > 10 items : proposer de regrouper avant de scorer
- Référencer le pilier stratégique de chaque item si possible (`System/pillars.yaml`)
