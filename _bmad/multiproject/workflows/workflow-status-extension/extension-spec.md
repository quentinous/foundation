---
name: workflow-status-multiproject-extension
description: "Extension du workflow-status pour support BMAD Multi-Project"
version: 1.0.0
extends: workflow-status
---

# Extension workflow-status pour Multi-Project

## Objectif

Étendre le workflow `workflow-status` existant pour :
1. Détecter si le projet fait partie d'un écosystème Multi-Project
2. Scanner `_mailbox/inbox/` pour les transmissions en attente
3. Appliquer les règles de triage automatique
4. Afficher les transmissions nécessitant attention
5. Dispatcher les transmissions en `outbox/` si destinations accessibles

---

## Point d'insertion

Cette extension s'insère **AU DÉBUT** du workflow-status, avant l'analyse standard du projet.

```
workflow-status (étendu)
│
├── 1. [EXTENSION] Vérifier Multi-Project
│   ├── hierarchy.csv existe ?
│   ├── Scanner inbox/
│   ├── Triage automatique
│   └── Afficher / traiter
│
├── 2. [STANDARD] Analyse projet BMAD
│   └── ... (workflow-status normal)
│
└── 3. [EXTENSION] Dispatcher outbox/ (fin)
```

---

## Algorithme détaillé

### Phase 1 : Détection Multi-Project

```pseudo
SI fichier "hierarchy.csv" existe à la racine du projet ALORS
    multi_project = true
    hierarchy = charger_csv("hierarchy.csv")
    current_project = trouver_projet_avec_path("./", hierarchy)
    mailbox_config = charger_yaml("_mailbox/.mailbox-config.yaml")
SINON
    multi_project = false
    → Continuer avec workflow-status standard
FIN SI
```

### Phase 2 : Vérification statut du projet

```pseudo
SI current_project.status == "deprecated" ALORS
    Afficher:
    ┌─────────────────────────────────────────────────────────┐
    │ ⚠️  CE PROJET EST DÉPRÉCIÉ                              │
    │                                                         │
    │ Voir deprecated.md pour plus d'informations.            │
    │ Aucune modification ne devrait être effectuée.          │
    │                                                         │
    │ [C] Continuer quand même  [Q] Quitter                   │
    └─────────────────────────────────────────────────────────┘

    SI user choisit [Q] → Terminer workflow
FIN SI
```

### Phase 3 : Scanner inbox/

```pseudo
transmissions = lister_fichiers("_mailbox/inbox/*.md")

POUR CHAQUE transmission DANS transmissions:
    data = parser_frontmatter(transmission)
    transmission.metadata = data
    transmission.filename = nom_fichier
FIN POUR

trier_par(transmissions, "priority", ["critical", "high", "medium", "low"])
```

### Phase 4 : Triage automatique

```pseudo
auto_handled = []
require_attention = []

POUR CHAQUE transmission DANS transmissions:
    type = transmission.metadata.type
    priority = transmission.metadata.priority

    # Chercher règle auto_handle
    rule = trouver_regle(mailbox_config.triage.auto_handle, type)

    SI rule existe ALORS
        executer_action(rule.action, transmission)
        auto_handled.append({
            transmission: transmission,
            action: rule.action
        })
    SINON
        # Chercher règle require_attention
        rule = trouver_regle(mailbox_config.triage.require_attention, type, priority)

        SI rule existe ET conditions_match(rule.conditions, transmission) ALORS
            require_attention.append({
                transmission: transmission,
                suggested_action: rule.action
            })
        SINON
            # Pas de règle → attention par défaut
            require_attention.append({
                transmission: transmission,
                suggested_action: "manual_review"
            })
        FIN SI
    FIN SI
FIN POUR
```

### Phase 5 : Actions automatiques

```pseudo
FONCTION executer_action(action, transmission):
    SWITCH action:
        CASE "archive":
            deplacer(transmission, "_mailbox/archive/{YYYY-MM}/")

        CASE "update_tracking_archive":
            # C'est un ACK - mettre à jour le tracking
            parent_id = transmission.metadata.parent_transmission
            SI parent_id ALORS
                # Trouver transmission originale dans sent/ ou archive/
                marquer_comme_acknowledged(parent_id)
            FIN SI
            deplacer(transmission, "_mailbox/archive/{YYYY-MM}/")

        CASE "add_to_backlog":
            # Créer entrée dans backlog ou todo
            ajouter_backlog(transmission.metadata)
            deplacer(transmission, "_mailbox/archive/{YYYY-MM}/")

        CASE "workflow_create_story":
            # Déclencher création de story
            creer_story_depuis_transmission(transmission)
            deplacer(transmission, "_mailbox/archive/{YYYY-MM}/")

        CASE "update_local_hierarchy":
            # Mettre à jour hierarchy.csv local
            mettre_a_jour_hierarchy(transmission.metadata)
            deplacer(transmission, "_mailbox/archive/{YYYY-MM}/")

        CASE "notify_and_prioritize":
            # Escalation - notifier et garder en haut
            afficher_notification_urgente(transmission)
            # Ne pas archiver - garder dans inbox
    FIN SWITCH
FIN FONCTION
```

### Phase 6 : Affichage

```pseudo
SI auto_handled.length > 0 OU require_attention.length > 0 ALORS
    Afficher:
    ┌─────────────────────────────────────────────────────────┐
    │ 📬 TRANSMISSIONS MULTI-PROJECT                          │
    ├─────────────────────────────────────────────────────────┤

    SI auto_handled.length > 0 ALORS
    │ ✅ Auto-traité ({auto_handled.length}):                 │
    POUR CHAQUE item DANS auto_handled:
    │   • {filename} [{type}] → {action}                      │
    FIN POUR
    FIN SI

    SI require_attention.length > 0 ALORS
    │                                                         │
    │ 🔴 Attention requise ({require_attention.length}):      │
    POUR CHAQUE item DANS require_attention (indexé i):
    │                                                         │
    │ [{i}] {filename}                                        │
    │     From: {from_project}                                │
    │     Type: {type} | Priority: {priority}                 │
    │     "{titre}"                                           │
    │     → Suggestion: {suggested_action}                    │
    FIN POUR
    │                                                         │
    │ Actions:                                                │
    │ [numéro] Traiter transmission spécifique                │
    │ [A] Traiter toutes avec suggestions                     │
    │ [S] Skip - continuer sans traiter                       │
    │ [D] Détails d'une transmission                          │
    └─────────────────────────────────────────────────────────┘

    ATTENDRE input utilisateur

    SI user choisit [numéro] ALORS
        traiter_transmission_interactive(require_attention[numero])
    SINON SI user choisit [A] ALORS
        POUR CHAQUE item DANS require_attention:
            executer_suggestion(item)
        FIN POUR
    SINON SI user choisit [S] ALORS
        → Continuer
    SINON SI user choisit [D] ALORS
        demander_numero()
        afficher_details(transmission)
    FIN SI
FIN SI
```

### Phase 7 : Traitement interactif

```pseudo
FONCTION traiter_transmission_interactive(item):
    transmission = item.transmission

    Afficher:
    ┌─────────────────────────────────────────────────────────┐
    │ 📄 {filename}                                           │
    ├─────────────────────────────────────────────────────────┤
    │ From: {from_project}                                    │
    │ Type: {type}                                            │
    │ Priority: {priority}                                    │
    │ Thread: {thread_id ou "N/A"}                            │
    ├─────────────────────────────────────────────────────────┤
    │ {contenu markdown tronqué}                              │
    ├─────────────────────────────────────────────────────────┤
    │ Actions disponibles:                                    │
    │                                                         │
    │ [B] Brainstorming - discuter cette demande              │
    │ [R] Research - investiguer avant décision               │
    │ [C] Créer story directement                             │
    │ [W] Workflow correct-course                             │
    │ [A] Archiver (traité manuellement)                      │
    │ [X] Rejeter (avec justification)                        │
    │ [S] Skip - traiter plus tard                            │
    └─────────────────────────────────────────────────────────┘

    SWITCH user_choice:
        CASE [B]:
            lancer_workflow("brainstorming", context=transmission)
            deplacer(transmission, archive)

        CASE [R]:
            lancer_workflow("research", context=transmission)
            deplacer(transmission, archive)

        CASE [C]:
            lancer_workflow("create-story", context=transmission)
            deplacer(transmission, archive)

        CASE [W]:
            lancer_workflow("correct-course", context=transmission)
            deplacer(transmission, archive)

        CASE [A]:
            mettre_status(transmission, "integrated")
            deplacer(transmission, archive)

        CASE [X]:
            demander_justification()
            creer_transmission_rejet(transmission, justification)
            mettre_status(transmission, "rejected")
            deplacer(transmission, archive)

        CASE [S]:
            → retour menu
    FIN SWITCH
FIN FONCTION
```

### Phase 8 : Dispatcher outbox (fin de workflow)

```pseudo
# À la FIN du workflow-status standard

SI multi_project ALORS
    outbox_files = lister_fichiers("_mailbox/outbox/*.md")

    SI outbox_files.length > 0 ALORS
        Afficher: "📤 {outbox_files.length} transmission(s) en attente de dispatch"

        POUR CHAQUE transmission DANS outbox_files:
            destination = transmission.metadata.to_project

            SI destination == "*" ALORS
                # Broadcast
                destinations = tous_projets_non_deprecated(hierarchy)
            SINON SI destination == "children" ALORS
                destinations = enfants_directs(current_project, hierarchy)
            SINON SI destination == "siblings" ALORS
                destinations = freres(current_project, hierarchy)
            SINON SI destination commence par "tag:" ALORS
                tag = extraire_tag(destination)
                destinations = projets_avec_tag(tag, hierarchy)
            SINON SI destination est une liste ALORS
                destinations = destination
            SINON
                destinations = [destination]
            FIN SI

            dispatched = []
            not_accessible = []

            POUR CHAQUE dest DANS destinations:
                dest_path = trouver_path(dest, hierarchy)

                SI dest.status == "deprecated" ALORS
                    # Skip projets deprecated
                    CONTINUER
                FIN SI

                SI path_existe(dest_path) ALORS
                    copier(transmission, "{dest_path}/_mailbox/inbox/")
                    dispatched.append(dest)
                SINON
                    not_accessible.append(dest)
                FIN SI
            FIN POUR

            SI dispatched.length > 0 ALORS
                # Supprimer de outbox (ou déplacer vers sent/)
                deplacer(transmission, "_mailbox/sent/")
                Afficher: "✅ Dispatché vers: {dispatched}"
            FIN SI

            SI not_accessible.length > 0 ALORS
                Afficher: "⚠️ Non accessible (sync externe nécessaire): {not_accessible}"
            FIN SI
        FIN POUR
    FIN SI
FIN SI
```

---

## Fichiers modifiés/créés

### Nouveau fichier : check-multiproject.md

À inclure au début de workflow-status :

```markdown
# Check Multi-Project

SI hierarchy.csv existe:
  - Charger configuration multi-project
  - Scanner inbox
  - Appliquer triage
  - Afficher interface
  - Retourner au workflow standard

SINON:
  - Continuer workflow standard directement
```

### Modification workflow-status.md

Ajouter au début de l'initialisation :

```markdown
### 0. Multi-Project Check (si applicable)

SI fichier "hierarchy.csv" existe à la racine ALORS
    Exécuter extension multi-project (voir extension-spec.md)
FIN SI
```

---

## Intégration avec workflows existants

| Workflow BMAD | Intégration |
|---------------|-------------|
| `brainstorming` | Peut être lancé depuis transmission |
| `research` | Peut être lancé depuis transmission |
| `create-story` | Crée story à partir des données transmission |
| `correct-course` | Lancé pour changements architecturaux |

---

## Configuration requise

Le fichier `_mailbox/.mailbox-config.yaml` doit exister avec au minimum :

```yaml
triage:
  auto_handle:
    - type: info
      action: archive
  require_attention:
    - type: feature-request
      action: propose_brainstorm
```

---

## Exemple de session

```
$ workflow-status

╔══════════════════════════════════════════════════════════════╗
║ 📬 TRANSMISSIONS MULTI-PROJECT                               ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║ ✅ Auto-traité (3):                                          ║
║   • TX_infra_2026-01-26_09h00_a1b2.md [info] → archivé       ║
║   • TX_master_2026-01-26_10h00_c3d4.md [dependency] → backlog║
║   • TX_site-web_2026-01-26_11h00_e5f6.md [bug] → story créée ║
║                                                              ║
║ 🔴 Attention requise (1):                                    ║
║                                                              ║
║ [1] TX_site-web_2026-01-27_14h30_g7h8.md                     ║
║     From: site-web                                           ║
║     Type: feature-request | Priority: HIGH                   ║
║     "Besoin système chat SAV"                                ║
║     → Suggestion: propose_brainstorm                         ║
║                                                              ║
║ [1-n] Traiter  [A] Toutes  [S] Skip  [D] Détails             ║
╚══════════════════════════════════════════════════════════════╝

> 1

╔══════════════════════════════════════════════════════════════╗
║ 📄 TX_site-web_2026-01-27_14h30_g7h8.md                      ║
╠══════════════════════════════════════════════════════════════╣
║ From: site-web                                               ║
║ Type: feature-request | Priority: HIGH                       ║
║ Thread: th_g7h8i9j0                                          ║
╠══════════════════════════════════════════════════════════════╣
║ # Besoin système chat SAV                                    ║
║                                                              ║
║ ## Contexte                                                  ║
║ L'équipe site-web a besoin d'intégrer un système de chat... ║
║                                                              ║
║ [B] Brainstorm  [R] Research  [C] Story  [W] Correct-course  ║
║ [A] Archiver    [X] Rejeter   [S] Skip                       ║
╚══════════════════════════════════════════════════════════════╝

> B

Lancement workflow brainstorming avec contexte transmission...
```

---

## Tests de validation

- [ ] Détection correcte de multi-project (hierarchy.csv)
- [ ] Scan inbox fonctionne
- [ ] Triage auto_handle exécute les bonnes actions
- [ ] Triage require_attention affiche correctement
- [ ] Actions interactives fonctionnent
- [ ] Dispatch outbox vers destinations accessibles
- [ ] Skip destinations deprecated
- [ ] Gestion destinations non accessibles
