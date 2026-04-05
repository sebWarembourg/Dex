# Pattern : Analytics produit pour outil personnel / open source

**Contexte :** sw_os est un outil personnel qui a vocation à être partagé. L'implémentation initiale (Dex by Dave) utilisait Pendo pour tracker l'adoption des features par les utilisateurs.

**Pourquoi c'est une bonne idée à terme :** quand sw_os sera distribué, savoir quelles features sont réellement utilisées permet de prioriser le backlog et d'améliorer l'onboarding.

---

## L'approche originale (Pendo)

**Stack :** Pendo Track Events API + clé write-only bundlée dans le code
**Consent :** opt-in via `usage_log.md` → `Consent decision: opted-in`
**Ce qui était tracké :**
- Feature utilisée (ex: `daily_plan_completed`, `task_created`)
- Journey stage (new / exploring / established / power_user)
- Jours depuis setup
- Feature adoption score (sur 57 features)
- Role group + company size (depuis user-profile.yaml)
- Visitor ID = hash SHA-256 du nom (anonyme mais stable)

**Ce qui n'était PAS tracké :** contenu des notes, noms de personnes, texte des tâches.

**Pourquoi on l'a retiré :** la clé Pendo appartenait à Dave Killeen (fork original). Impossible de garder ça dans un projet indépendant.

---

## Réimplémentation future

Quand sw_os sera distribué et qu'on voudra de l'analytics :

**Option A — Posthog self-hosted**
- Open source, gratuit en self-hosted
- API compatible avec le pattern existant
- Clé configurable par l'utilisateur ou hébergée par sw_os

**Option B — Plausible**
- Privacy-first par design
- Bon pour les event counts simples
- Pas de visitor tracking individuel

**Option C — Fichier local uniquement**
- `System/analytics_log.jsonl` déjà implémenté comme backup
- Agrégation manuelle côté maintainer si les users partagent volontairement
- Friction plus haute mais zéro dépendance externe

**Recommandation :** Option A (Posthog) quand le projet dépasse 10 utilisateurs actifs. Avant ça, Option C suffit.

---

## Code de référence

L'implémentation complète était dans :
- `core/mcp/analytics_server.py` — MCP server
- `core/mcp/analytics_helper.py` — helpers (consent, fire_event, journey metadata)

Supprimés en avril 2026 (commit `security: remove Pendo analytics`). Récupérables depuis l'historique git si besoin.

**Pattern consent dans `usage_log.md` :**
```
**Consent asked:** true
**Consent decision:** opted-in
**Consent date:** YYYY-MM-DD
```

**Pattern appel dans un MCP server :**
```python
try:
    from analytics_helper import fire_event as _fire_analytics_event
    HAS_ANALYTICS = True
except ImportError:
    HAS_ANALYTICS = False
    def _fire_analytics_event(event_name, properties=None):
        return {'fired': False, 'reason': 'analytics_not_available'}

# Dans une tool call :
try:
    _fire_analytics_event('feature_used', {'context': 'value'})
except Exception:
    pass
```
