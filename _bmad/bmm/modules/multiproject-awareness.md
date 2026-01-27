# Multi-Project Awareness Module

<critical>Ce module est chargé par les agents pour activer la conscience cross-projet</critical>

---

## Detection

```
SI fichier "hierarchy.csv" existe à {project-root}:
    multi_project_mode = true
    Charger hierarchy.csv → identifier projet courant et relations
    Charger _mailbox/.mailbox-config.yaml → connaître les dépendances
SINON:
    multi_project_mode = false
    Skip toute logique multi-projet
```

---

## Context Variables (si multi_project_mode = true)

| Variable | Source | Usage |
|----------|--------|-------|
| `{current_project}` | hierarchy.csv (path="./") | Identifiant du projet courant |
| `{parent_project}` | hierarchy.csv | Projet parent (vide si master) |
| `{sibling_projects}` | hierarchy.csv | Projets avec même parent |
| `{child_projects}` | hierarchy.csv | Projets enfants directs |
| `{dependencies}` | .mailbox-config.yaml | Projets dont on dépend |
| `{provides}` | .mailbox-config.yaml | Services fournis aux autres |

---

## Agent-Specific Guidelines

### Architect (Winston) 🏗️

**Quand créer une transmission :**
- Décision architecturale qui impacte des projets dépendants
- Changement d'API/interface consommée par d'autres projets
- Migration technologique (DB, framework, etc.)
- Nouveau pattern ou convention à propager

**Type de transmission :** `architectural`

**Destinataires suggérés :**
- `children` : si décision impacte les enfants
- `siblings` : si décision impacte les frères
- `tag:xxx` : si décision impacte un tag spécifique
- Projet spécifique si impact ciblé

**Template de détection :**
```
PENDANT création/révision architecture:
    SI décision touche:
        - API exposée/consommée
        - Base de données partagée
        - Conventions de nommage
        - Protocoles de communication
        - Patterns obligatoires
    ALORS:
        Proposer création transmission `architectural`
```

---

### PM (John) 📋

**Quand créer une transmission :**
- Feature qui nécessite travail dans un autre projet
- Dépendance bloquante identifiée
- Changement de scope impactant d'autres projets
- Nouvelle fonctionnalité cross-projet

**Types de transmission :**
- `feature-request` : demande de fonctionnalité à un autre projet
- `dependency` : notification de dépendance
- `info` : information générale

**Template de détection :**
```
PENDANT création PRD/Epics:
    SI fonctionnalité requiert:
        - API d'un autre projet (vérifier {dependencies})
        - Modification dans projet enfant/frère
        - Coordination multi-équipe
    ALORS:
        Proposer création transmission appropriée
```

---

### Dev (Amelia) 💻

**Quand créer une transmission :**
- Bug découvert dont l'origine est un autre projet
- Blocage par API/service d'un autre projet
- Incompatibilité détectée avec dépendance
- Feedback technique à remonter

**Types de transmission :**
- `bug` : rapport de bug pour un autre projet
- `dependency` : blocage par dépendance
- `info` : feedback technique

**Template de détection :**
```
PENDANT implémentation:
    SI problème rencontré:
        - Erreur venant d'API externe (vérifier {dependencies})
        - Comportement inattendu d'un service partagé
        - Documentation manquante d'un autre projet
    ALORS:
        Proposer création transmission `bug` ou `dependency`
```

---

## Création de Transmission

### Workflow simplifié

```
1. Agent détecte besoin de transmission
2. Proposer à l'utilisateur:
   "🔔 Impact cross-projet détecté. Créer une transmission vers {destination}? (y/n)"

3. SI user accepte:
   - Collecter: type, priority, destinataire(s)
   - Générer contenu depuis le contexte actuel
   - Créer fichier dans _mailbox/outbox/
   - Nommer: TX_{current_project}_{date}_{time}_{suffix}.md

4. Informer: "📤 Transmission créée. Sera dispatchée au prochain workflow-status."
```

### Template frontmatter

```yaml
---
transmission_id: TX_{current_project}_{YYYY-MM-DD}_{HHhMM}_{8chars}
from_project: {current_project}
to_project: {destination}
type: {architectural|feature-request|bug|dependency|info}
priority: {critical|high|medium|low}
status: pending
created_at: {ISO timestamp}
thread_id: {optional - for conversations}
tags: [{relevant tags}]
---
```

---

## Consultation Inbox

### Quand consulter

Chaque agent devrait vérifier l'inbox au démarrage si `multi_project_mode = true`.

```
PENDANT activation agent:
    SI multi_project_mode ET inbox non vide:
        Afficher résumé: "📬 {count} transmission(s) en attente"
        Proposer: "[I] Voir inbox | [C] Continuer"
```

### Intégration avec workflow-status

Le check inbox complet est géré par `workflow-status` avec son extension multi-projet.
Les agents peuvent juste afficher un rappel.

---

## Menu Item Optionnel

Pour agents souhaitant un accès direct :

```xml
<item cmd="MP or fuzzy match on multi-project">[MP] Multi-Project: Voir inbox / Créer transmission</item>
```

Handler:
```
1. Afficher inbox résumé
2. Options:
   [V] Voir détails transmission
   [C] Créer nouvelle transmission
   [D] Dispatch outbox maintenant
   [R] Retour menu principal
```

---

## Règles Critiques

1. **NE JAMAIS** créer de transmission sans confirmation utilisateur
2. **TOUJOURS** vérifier que le destinataire existe dans hierarchy.csv
3. **NE PAS** envoyer vers projets `deprecated`
4. **PRÉFÉRER** workflow-status pour le traitement complet
5. **INFORMER** l'utilisateur que le dispatch se fait via workflow-status
