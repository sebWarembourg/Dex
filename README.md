# sw_os — Personal Operating System for PMs

> Un système de gestion de vie professionnelle, propulsé par Claude.

Tu es PM, Head of Product, ou dans une fonction produit exigeante. Tu jongle entre meetings, projets, recrutements, décisions, veille — et rien ne se connecte vraiment. sw_os est ton OS personnel : il organise tout ça en un seul endroit, et ton AI ne repart jamais de zéro.

---

## Ce que c'est

**sw_os** est un vault structuré couplé à un assistant AI configuré pour le travail produit. Il tourne dans [Claude Code](https://claude.ai/code) — pas d'app supplémentaire, pas de SaaS, pas de sync cloud obligatoire.

C'est ton fichier. Tu le possèdes. L'AI lit ton contexte, se souvient de tes réunions, connaît tes projets.

---

## Pourquoi

La plupart des PMs utilisent l'AI comme un moteur de recherche amélioré. Tu poses une question, tu obtiens une réponse, tu fermes la fenêtre. Contexte perdu.

sw_os fonctionne différemment. Ton assistant :
- Sait ce que tu as discuté la semaine dernière
- Connaît tes stakeholders et leur style de travail
- Suit tes objectifs trimestriels et tes priorités hebdo
- Produit des livrables PM réels — briefs, priorisations, docs de décision

**Résultat :** Moins de charge cognitive. Plus de travail stratégique.

---

## Les 3 piliers

sw_os est organisé autour de 3 axes de vie professionnelle :

| Pilier | Ce qu'il couvre |
|--------|-----------------|
| **Évolution de carrière** | Job search, personal brand, réseau, promotions |
| **Projets personnels** | Side projects, entrepreneuriat, builds |
| **Veille & idées** | Second brain, capture, connexions entre idées |

Chaque tâche, projet et objectif est ancré dans un pilier. Rien ne flotte dans le vide.

---

## Ce que tu peux faire

### Rituels quotidiens et hebdomadaires
```
/daily-plan      → Plan ta journée avec tes tâches, meetings et priorités
/daily-review    → Fin de journée : ce qui a avancé, ce qui glisse
/week-plan       → Priorités de la semaine, alignées sur tes objectifs
/week-review     → Bilan hebdo : patterns, momentum, réajustements
```

### Meetings & inbox
```
/meeting-prep       → Prépare un meeting : contexte des participants, points clés
/process-meetings   → Traite tes notes de meeting → actions, pages personnes
/triage             → Vide ton inbox et route chaque élément au bon endroit
```

### Travail produit
```
/product-brief    → Transforme une idée en PRD structuré
/prioritize       → Priorise un backlog avec RICE ou ICE
/decision-doc     → Structure une décision difficile en doc défendable
/project-health   → Scan de tes projets actifs : statut, blockers, prochaines étapes
```

### Carrière
```
/career-coach     → Coach carrière : rapports hebdo, promo assessments, bilans
/resume-builder   → Génère ton CV et profil LinkedIn par interview guidée
```

---

## Comment ça marche

```
sw_os/
├── 00-Inbox/          ← Capture : meetings, idées en vrac
├── 01-Quarter_Goals/  ← Objectifs trimestriels par pilier
├── 02-Week_Priorities/← Priorités de la semaine
├── 03-Tasks/          ← Backlog de tâches
├── 04-Projects/       ← Projets actifs (brief, décisions, backlog)
├── 05-Areas/
│   ├── People/        ← Pages personnes (meetings, contexte, actions)
│   ├── Companies/     ← Organisations externes
│   └── Career/        ← Développement professionnel
├── 06-Resources/      ← Référence, frameworks, veille
├── 07-Archives/       ← Travail terminé
└── System/            ← Config (profil, piliers, préférences)
```

**Hiérarchie de planification :** Piliers → Objectifs trimestriels → Priorités hebdo → Plans quotidiens → Tâches

---

## Intelligence relationnelle

Pour chaque personne avec qui tu travailles, sw_os maintient une **page personne** :
- Rôle, entreprise, style de communication
- Historique des meetings (lié)
- Contexte clé et actions en cours

Quand tu prépares un meeting, l'assistant charge automatiquement le contexte de chaque participant. Tu n'as plus à te souvenir de ce qui a été dit la dernière fois.

---

## Quick Start

**Prérequis :** Compte [Claude](https://claude.ai) (plan Pro ou Team recommandé)

```bash
# 1. Clone le repo
git clone https://github.com/[ton-username]/sw_os.git
cd sw_os

# 2. Ouvre dans Claude Code
# claude.ai/code → ouvre le dossier sw_os

# 3. Configure ton profil
# Édite System/user-profile.yaml avec ton nom, rôle, email domain

# 4. Lance le setup
/getting-started
```

En 10 minutes, ton système est configuré pour ton rôle et ton style de travail.

---

## Rituels recommandés

| Fréquence | Commande | Durée |
|-----------|----------|-------|
| Lundi matin | `/week-plan` | 15 min |
| Mercredi midi | `/daily-plan` | 5 min |
| Vendredi matin | `/week-review` | 15 min |
| Après chaque meeting | `/process-meetings` | 5 min |
| Fin de trimestre | `/quarter-review` | 30 min |

---

## Stack

- **AI :** [Claude](https://claude.ai) (Anthropic)
- **Interface :** [Claude Code](https://claude.ai/code)
- **Stockage :** Markdown local (ton ordinateur, ton contrôle)
- **Optionnel :** MCP servers pour Gmail, Notion, Granola, calendrier

---

## License

MIT — fork, adapte, partage.

---

*sw_os est construit sur [Dex](https://github.com/dvk/dex), re-approprié pour le travail produit.*
