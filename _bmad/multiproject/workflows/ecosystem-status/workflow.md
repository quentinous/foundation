---
name: ecosystem-status
description: "Dashboard masterclass pour visualiser le status de tout l'écosystème Multi-Project"
version: 1.1.0
master_only: true
---

# Ecosystem Status Dashboard

<critical>Ce workflow est réservé au projet MASTER (pas de parent_id dans hierarchy.csv)</critical>
<critical>Communiquer en {communication_language}</critical>
<critical>ISOLATION: Ce workflow LECTURE SEULE - aucune écriture dans les projets enfants</critical>

---

## INITIALIZATION

### 1. Verify Master Status

```
SI parent_id existe dans hierarchy.csv pour ce projet:
    Afficher erreur: "Ce workflow est réservé au projet master."
    Exit
```

### 2. Load Ecosystem Data

```
1. Charger hierarchy.csv complet
2. Construire l'arbre hiérarchique:
   - Identifier le master (parent_id vide)
   - Calculer la profondeur de chaque projet (depth)
   - Identifier les projets feuilles (is_leaf = aucun enfant)
   - Compter: total, intermediate (non-feuilles), leaf (feuilles)
3. Pour chaque projet accessible (path existe):
   - Lire {path}/planning-artifacts/bmm-workflow-status.yaml si existe
   - Extraire: phase actuelle, workflows complétés, blocages
   - Vérifier existence de _bmad/, _bmad-output/, _mailbox/
4. Charger transmissions status-report non traitées dans inbox
5. Fusionner les données (status-report plus récent que fichier local)
6. Trier les projets en DFS order pour affichage hiérarchique
```

> **RÈGLE FONDAMENTALE : Tous les nœuds sont des projets BMAD complets**
> Chaque projet (intermédiaire ou feuille) possède `_bmad/`, `_bmad-output/`, `_mailbox/`.
> Il n'y a PAS de "conteneur" - cette distinction n'existe pas.

---

## DASHBOARD DISPLAY

### 3. Render Dashboard

Afficher le dashboard avec ce format exact:

```
╔══════════════════════════════════════════════════════════════════════════════════════════╗
║                                                                                          ║
║   ███████╗ ██████╗ ██████╗ ███████╗██╗   ██╗███████╗████████╗███████╗███╗   ███╗        ║
║   ██╔════╝██╔════╝██╔═══██╗██╔════╝╚██╗ ██╔╝██╔════╝╚══██╔══╝██╔════╝████╗ ████║        ║
║   █████╗  ██║     ██║   ██║███████╗ ╚████╔╝ ███████╗   ██║   █████╗  ██╔████╔██║        ║
║   ██╔══╝  ██║     ██║   ██║╚════██║  ╚██╔╝  ╚════██║   ██║   ██╔══╝  ██║╚██╔╝██║        ║
║   ███████╗╚██████╗╚██████╔╝███████║   ██║   ███████║   ██║   ███████╗██║ ╚═╝ ██║        ║
║   ╚══════╝ ╚═════╝ ╚═════╝ ╚══════╝   ╚═╝   ╚══════╝   ╚═╝   ╚══════╝╚═╝     ╚═╝        ║
║                                                                                          ║
║   🏢 {{ecosystem_name}}                                         {{current_date}}         ║
║                                                                                          ║
╠══════════════════════════════════════════════════════════════════════════════════════════╣
║                                                                                          ║
║   📊 OVERVIEW                                                                            ║
║   ┌────────────────────────────────────────────────────────────────────────────────┐    ║
║   │  Total Projects: {{total}}  │  Intermediate: {{intermediate}}  │  Leaf: {{leaf}}│    ║
║   │  Active: {{active}}         │  Blocked: {{blocked}}            │                │    ║
║   │  Production: {{prod}}       │  R&D: {{rnd}}   │  Deprecated: {{depr}}          │    ║
║   └────────────────────────────────────────────────────────────────────────────────┘    ║
║                                                                                          ║
║   📁 HIERARCHY (All nodes are full BMAD projects)                                        ║
║   ┌────────────────────────────────────────────────────────────────────────────────┐    ║
{{#render_hierarchy_tree projects}}
║   │  {{indent}}{{icon}} {{name}} {{status_indicator}}                               │    ║
{{/render_hierarchy_tree}}
║   └────────────────────────────────────────────────────────────────────────────────┘    ║
║                                                                                          ║
╠══════════════════════════════════════════════════════════════════════════════════════════╣
║                                                                                          ║
║   📈 PROJECT STATUS                                                                      ║
║                                                                                          ║
{{#each projects}}
║   ┌──────────────────────────────────────────────────────────────────────────────────┐  ║
║   │ {{icon}} {{name}}                                           {{status_badge}}      │  ║
║   │                                                                                    │  ║
║   │   Phase: {{phase}}                                                                │  ║
║   │   Progress: {{progress_bar}}  {{progress_pct}}%                                   │  ║
║   │                                                                                    │  ║
║   │   ✅ Completed: {{completed_list}}                                                │  ║
║   │   🎯 Current:   {{current_workflow}}                                              │  ║
║   │   ⏳ Next:      {{next_workflow}}                                                 │  ║
║   │                                                                                    │  ║
{{#if blockers}}
║   │   🚨 BLOCKERS:                                                                    │  ║
{{#each blockers}}
║   │      • {{blocker}}                                                                │  ║
{{/each}}
{{/if}}
║   │                                                                                    │  ║
║   │   Last Update: {{last_update}}                                                    │  ║
║   └──────────────────────────────────────────────────────────────────────────────────┘  ║
║                                                                                          ║
{{/each}}
╠══════════════════════════════════════════════════════════════════════════════════════════╣
║                                                                                          ║
║   📬 TRANSMISSIONS RÉCENTES                                                              ║
║   ┌────────────────────────────────────────────────────────────────────────────────┐    ║
{{#each recent_transmissions}}
║   │  {{icon}} {{from}} → {{type}} ({{priority}})  {{time_ago}}                      │    ║
{{/each}}
{{#if no_transmissions}}
║   │  (Aucune transmission récente)                                                  │    ║
{{/if}}
║   └────────────────────────────────────────────────────────────────────────────────┘    ║
║                                                                                          ║
╠══════════════════════════════════════════════════════════════════════════════════════════╣
║                                                                                          ║
║   🚨 ALERTS                                                                              ║
{{#each alerts}}
║   │  {{severity_icon}} {{message}}                                                   │    ║
{{/each}}
{{#if no_alerts}}
║   │  ✅ Aucune alerte - Tout va bien !                                               │    ║
{{/if}}
║                                                                                          ║
╠══════════════════════════════════════════════════════════════════════════════════════════╣
║                                                                                          ║
║   Actions:                                                                               ║
║   [1] Détails projet    [2] Voir transmissions    [3] Refresh    [Q] Quitter            ║
║                                                                                          ║
╚══════════════════════════════════════════════════════════════════════════════════════════╝
```

---

## DATA STRUCTURES

### Project Status Object

```yaml
project:
  id: "project-name"
  name: "Project Display Name"
  parent_id: "parent-project"   # vide si master
  depth: 1                      # 0 = master, 1 = enfant direct, 2+ = sous-enfant
  is_leaf: false                # true si aucun enfant
  children: []                  # Liste des IDs enfants
  icon: "📁"                    # Icône selon contexte (voir Icon Logic)
  status: "active"             # active | blocked | deprecated | unknown
  status_badge: "🟢 Active"    # 🟢 Active | 🔴 Blocked | 🟡 Warning | ⚫ Deprecated
  status_indicator: "🟢"       # Version courte pour la hiérarchie
  phase: "Implementation"      # Discovery | Planning | Solutioning | Implementation | Validation
  phase_num: 4                 # 1-5
  progress_pct: 65             # 0-100
  progress_bar: "████████░░░░" # Visual bar
  completed_list: "PRD, Arch"
  current_workflow: "dev-story"
  next_workflow: "code-review"
  blockers: []
  last_update: "2026-01-26 14:30"
  last_transmission: "TX_..."
  accessible: true             # Si path existe
```

### Icon Logic (Hierarchical)

```
FONCTION get_project_icon(project):
    SI status == "deprecated":
        RETURN "📦"
    SINON SI depth == 0:
        RETURN "🏢"           # Master = building
    SINON SI is_leaf:
        RETURN "📄"           # Leaf = document
    SINON:
        RETURN "📁"           # Intermediate = folder
```

### Hierarchy Tree Rendering

```
FONCTION render_hierarchy_tree(projects):
    // Build tree structure
    tree = build_tree(projects)

    // Render with proper indentation
    FOR each project in tree (DFS order):
        indent = get_tree_indent(project)
        // indent examples:
        // depth 0: ""
        // depth 1: "├── " or "└── " (last child)
        // depth 2: "│   ├── " or "│   └── " or "    ├── "

        OUTPUT "║   │  {indent}{icon} {name} {status_indicator}"

FONCTION get_tree_indent(project):
    SI depth == 0:
        RETURN ""

    indent = ""
    // Add parent's continuation lines
    FOR each ancestor from root to parent:
        SI ancestor has more siblings after it:
            indent += "│   "
        SINON:
            indent += "    "

    // Add own prefix
    SI project is last child of parent:
        indent += "└── "
    SINON:
        indent += "├── "

    RETURN indent
```

### Progress Bar Generation

```
FONCTION generate_progress_bar(pct):
    filled = floor(pct / 10)
    empty = 10 - filled
    RETURN "█" * filled + "░" * empty
```

### Status Badge Logic

```
SI status == "deprecated":
    badge = "⚫ Deprecated"
SINON SI blockers.length > 0:
    badge = "🔴 Blocked"
SINON SI progress_pct < 100 AND last_update > 7 jours:
    badge = "🟡 Stale"
SINON:
    badge = "🟢 Active"
```

### Phase Detection

```
FONCTION detect_phase(workflow_status):
    SI aucun workflow complété:
        RETURN "Discovery", 1
    SI prd complété AND NOT architecture:
        RETURN "Planning", 2
    SI architecture complété AND NOT epics:
        RETURN "Solutioning", 3
    SI epics complété:
        RETURN "Implementation", 4
    SI tous complétés:
        RETURN "Validation", 5
```

---

## ALERTS GENERATION

### Alert Rules

```yaml
alerts:
  critical:
    - condition: "project blocked > 3 days"
      message: "🔴 {{project}} bloqué depuis {{days}} jours"
    - condition: "transmission critical non traitée"
      message: "🔴 Transmission critique en attente: {{transmission}}"
    - condition: "project missing _bmad/ structure"
      message: "🔴 {{project}} manque la structure BMAD complète"

  warning:
    - condition: "project stale > 7 days"
      message: "🟡 {{project}} sans mise à jour depuis {{days}} jours"
    - condition: "inbox > 5 transmissions"
      message: "🟡 {{count}} transmissions en attente dans inbox"
    - condition: "intermediate project without workflow started"
      message: "🟡 {{project}} (intermédiaire) n'a pas encore de workflow BMAD démarré"

  info:
    - condition: "project completed phase"
      message: "🟢 {{project}} a terminé la phase {{phase}}"
    - condition: "new project added to hierarchy"
      message: "ℹ️ Nouveau projet ajouté: {{project}} ({{level}})"
```

> **Note sur les projets intermédiaires:**
> Les projets intermédiaires (non-feuilles) sont des projets BMAD complets.
> Ils peuvent définir leur propre stratégie (PRD, architecture) qui sera
> héritée/affinée par leurs enfants. L'alerte ci-dessus rappelle de lancer
> un workflow sur ces projets si ce n'est pas encore fait.

---

## INTERACTIONS

### [1] Détails projet

```
Afficher menu des projets:
  [1] backend
  [2] frontend
  [3] mobile-app
  ...

Sur sélection, afficher:
- Historique des workflows complétés avec dates
- Toutes les transmissions envoyées/reçues
- Contenu bmm-workflow-status.yaml complet
```

### [2] Voir transmissions

```
Afficher toutes les transmissions status-report:
- Triées par date (plus récent en haut)
- Avec possibilité d'archiver
```

### [3] Refresh

```
Recharger toutes les données et ré-afficher le dashboard
```

---

## INTEGRATION

### Menu Item pour Master

Ajouter au menu des agents (si projet master):

```xml
<item cmd="ES or fuzzy match on ecosystem-status"
      exec="{project-root}/_bmad/multiproject/workflows/ecosystem-status/workflow.md"
      condition="is_master_project">
  [ES] 📊 Ecosystem Status Dashboard
</item>
```

### Auto-refresh on workflow-status

Quand workflow-status est lancé sur le master, proposer d'afficher le dashboard:

```
SI projet master ET enfants existent:
    Afficher: "📊 Voulez-vous voir le dashboard écosystème? [E] Ecosystem / [S] Skip"
```
