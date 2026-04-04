# sw_os — Guide d'utilisation

Guide pratique pour utiliser sw_os au quotidien. Pas de théorie — ce qui marche, dans quel ordre, avec quels résultats.

---

## Premiers pas

### 1. Configure ton profil

Ouvre `System/user-profile.yaml` et remplis :
- Ton nom, rôle, entreprise
- Ton domaine email (pour trier Internal vs External dans People)
- Tes préférences de communication

### 2. Définis tes piliers

Ouvre `System/pillars.yaml`. Les 3 piliers par défaut :
- **Évolution de carrière** — ce que tu veux accomplir professionnellement
- **Projets personnels** — ce que tu construis en dehors du bureau
- **Veille & idées** — ce que tu apprends et captures

Modifie-les pour qu'ils te ressemblent. Tout le reste s'y ancre.

### 3. Lance `/getting-started`

L'assistant te pose les questions nécessaires et configure le reste.

---

## Utilisation quotidienne

### Le matin — `/daily-plan`

Lance ça avant de commencer ta journée. L'assistant :
- Regarde tes tâches en attente
- Identifie les meetings du jour
- Propose 3 priorités max
- Signale les engagements qui risquent de glisser

**Durée : 5 minutes.**

### Après un meeting — `/process-meetings`

Donne tes notes brutes à l'assistant (ou colle le transcript). Il :
- Extrait les décisions et actions
- Met à jour ou crée les pages des participants
- Suggère les follow-ups

Tu n'as plus à re-lire tes notes pour retrouver "qu'est-ce qu'on avait décidé".

### Le soir — `/daily-review`

Ce qui a avancé, ce qui n'a pas bougé, ce qu'il faut reporter. 5 minutes, pas plus.

---

## Rituels hebdomadaires

### Lundi matin — `/week-plan`

15 minutes pour définir tes 3 priorités de la semaine, alignées sur tes objectifs trimestriels. L'assistant te challenge si tes priorités dérivent.

### Vendredi matin — `/week-review`

Bilan honnête : qu'est-ce qui a vraiment avancé ? Quels patterns reviennent ? Qu'est-ce qu'on ajuste la semaine prochaine ?

---

## Travail produit

### Prioriser un backlog — `/prioritize`

```
/prioritize

→ Tu listes tes initiatives
→ L'assistant choisit le framework (RICE ou ICE) selon tes données
→ Il score chaque item en te posant les bonnes questions
→ Tu obtiens un backlog classé avec les exclusions documentées
```

Les exclusions sont aussi importantes que les inclusions. Le "ce qu'on ne fait pas" est dans le doc.

### Structurer une décision — `/decision-doc`

Pour les décisions difficiles, floues, ou qu'il faut défendre devant des stakeholders :

```
/decision-doc

→ Tu décris la situation
→ L'assistant reformule le problème (checkpoint)
→ Vous listez les options ensemble
→ Il évalue chaque option : avantages, risques, réversibilité
→ Il formule une recommandation (checkpoint)
→ Tu obtiens un doc complet avec hypothèses clés et prochaines étapes
```

Le statu quo est toujours listé comme option.

### Rédiger un brief — `/product-brief`

L'assistant t'interviewe sur ton idée et génère un PRD. Utile quand tu as une vision claire dans la tête mais du mal à l'articuler pour l'équipe.

---

## Pages personnes

sw_os maintient une page pour chaque personne avec qui tu travailles régulièrement.

**Ce qu'une page contient :**
- Rôle, entreprise, style de communication
- Historique des meetings (liens)
- Actions en cours
- Contexte clé ("préfère les décisions par écrit", "sensible au budget Q4")

**Comment ça s'alimente :**
- Automatiquement via `/process-meetings`
- Manuellement si tu ajoutes un contexte après un échange informel

**Où elles sont :**
- `05-Areas/People/Internal/` — collègues (même domaine email)
- `05-Areas/People/External/` — partenaires, candidats, réseau

---

## Gestion de carrière

Si tu as un objectif de progression, crée `05-Areas/Career/` et active le suivi :

- `/career-coach` — sessions de coaching structurées (hebdo, mensuel, promo assessment)
- Tags `# Career: [skill]` sur tes tâches — trace les compétences que tu développes
- `/resume-builder` — génère CV et LinkedIn à partir de ce qui est déjà dans le vault

L'assistant détecte automatiquement les achievements chiffrables pendant `/daily-review` et te propose de les capturer.

---

## Capture d'idées

Quand tu as une idée :
- Tape-la dans le chat → l'assistant la route vers `00-Inbox/Ideas/`
- Ou crée directement un fichier dans `00-Inbox/Ideas/`

Périodiquement, `/triage` vide l'inbox et connecte les idées aux projets existants.

---

## Recherche dans le vault

**Recherche sémantique (si activée) :** `/enable-semantic-search` puis cherche en langage naturel.

**Recherche par mots-clés :** l'assistant grep à travers le vault.

**Lookup personne :** dis juste le prénom, l'assistant retrouve la page.

---

## Commandes système

```
/health-check      → Diagnostique les problèmes de config
/xray              → Comprends ce qui s'est passé sous le capot
/identity-snapshot → Génère ton profil de travail à partir du vault
/save-insight      → Capture un apprentissage ponctuel
/dex-improve       → Propose une amélioration du système
```

---

## Ce que sw_os ne fait pas

- Il ne remplace pas ton outil de gestion de projet d'équipe (Jira, Linear, etc.)
- Il ne syncronise pas automatiquement ton calendrier (sans MCP calendrier)
- Il ne traite pas de contenu confidentiel de ton entreprise — garde ça dans les outils corporate

sw_os est ton espace de travail personnel, pas un outil d'équipe.

---

## Dépannage

**L'assistant ne retrouve pas une personne**
→ Vérifie que la page existe dans `05-Areas/People/`
→ Lance `build_people_index` pour reconstruire l'index

**Une tâche n'apparaît pas dans le daily-plan**
→ Vérifie le format dans `03-Tasks/Tasks.md` (format : `^task-YYYYMMDD-XXX`)

**Le système semble lent ou confus**
→ `/health-check` — diagnostique et fixes suggérés

---

*Dernière mise à jour : 2026-04-04*
