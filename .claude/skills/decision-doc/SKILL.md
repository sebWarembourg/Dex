---
name: decision-doc
description: Structure une décision importante en doc clair et défendable
context: fork
---

## Purpose

Transformer une décision floue ou un dilemme en document structuré, défendable et traçable. Utile avant un arbitrage important, une présentation à des stakeholders, ou simplement pour clarifier sa propre pensée.

Charge `.claude/reference/pm-frameworks.md` → section BLUF et Pyramid Principle.
Charge `.claude/reference/quality-standards.md` → section "Documents de décision".

## Usage

- `/decision-doc` — démarre en mode interactif
- `/decision-doc [contexte]` — démarre avec le contexte fourni

---

## Process

### Step 1 : Capturer le contexte

Si pas de contexte fourni :
> "Quelle décision tu dois prendre ? Décris la situation en 2-3 phrases."

Puis :
> "Quel est le contexte ? Pourquoi c'est une décision difficile ?"

**Ne pas proposer d'options avant d'avoir compris le problème.**

### Step 2 : Challenger le problème

**Checkpoint humain.**

Reformuler le problème en une phrase :
> "Si je comprends bien : [reformulation]. C'est bien ça ?"

Si c'est flou : demander quelle est la contrainte principale (temps / budget / risque / politique).

### Step 3 : Structurer les options

Demander :
> "Quelles options tu envisages ? Liste-les, même les mauvaises."

Compléter avec 1-2 options que l'utilisateur n'a pas mentionnées si pertinent (dont le statu quo).

Pour chaque option, évaluer :
- **Avantages** (2-3 points)
- **Inconvénients** (2-3 points)
- **Risque principal**
- **Réversibilité** : facilement réversible / difficilement / irréversible

### Step 4 : Recommandation

Formuler une recommandation claire :

```markdown
## Décision recommandée

**[Option recommandée]**

**Pourquoi :** [2-3 raisons, la plus forte en premier — BLUF]

**Ce qu'on abandonne :** [Ce que cette option ne permet pas]

**Condition de succès :** [Ce qui doit être vrai pour que ça marche]
```

**Checkpoint humain** :
> "Tu valides cette recommandation ou tu veux challenger quelque chose ?"

### Step 5 : Générer le doc

Format final :

```markdown
# Décision : [Titre court]

**Date :** YYYY-MM-DD
**Décideur :** [Nom]
**Statut :** Décidé / En discussion / À valider

---

## Contexte

[2-3 phrases max. Ce qu'on sait, ce qui a changé, pourquoi décider maintenant.]

## Décision

**[Option choisie]** — [Raison principale en 1 ligne]

## Options considérées

| Option | Avantages | Inconvénients | Risque | Réversible ? |
|--------|-----------|---------------|--------|--------------|
| [A] | ... | ... | ... | Oui / Non |
| [B] | ... | ... | ... | Oui / Non |
| [Statu quo] | ... | ... | ... | — |

## Hypothèses clés

- [Ce qui doit être vrai pour que la décision soit bonne]
- [Signal d'alerte si faux]

## Ce que ça ne règle pas

- [Problème adjacent non traité]

## Prochaines étapes

- [ ] [Action 1] — [Owner] — [Date]
- [ ] [Action 2] — [Owner] — [Date]
```

### Step 6 : Sauvegarder

Proposer de sauvegarder dans :
- `04-Projects/[Projet]/Decisions/YYYY-MM-DD_[sujet].md`
- Ou `06-Resources/Research/00-Decisions/` si décision transversale

---

## Règles

- Toujours inclure le statu quo comme option explicite
- Toujours mentionner ce que la décision ne règle pas
- Ne jamais produire la recommandation avant d'avoir listé les options
- Si la décision implique des personnes : vérifier leurs pages dans `05-Areas/People/`
