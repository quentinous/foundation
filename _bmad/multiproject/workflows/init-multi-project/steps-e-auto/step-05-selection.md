---
name: 'step-05-selection'
description: 'Allow user to select which projects to create from the hierarchy'

# File References
nextStepFile: './step-06-create-batch.md'
previousStepFile: './step-04-present-proposal.md'

# Context from previous step
# hierarchy_tree: passed from step-04
---

# Step 5: Project Selection

## STEP GOAL:

Allow the user to select which projects to create, with smart dependency handling. Remember: ALL nodes are full BMAD projects.

## MANDATORY EXECUTION RULES:

- ☑️ **ALWAYS** show checkbox-style selection
- 🔗 **ALWAYS** auto-select parents when child is selected
- ⚠️ **ALWAYS** warn about orphan selections
- 📁 **REMEMBER** all nodes are full BMAD projects (no containers)
- ✅ **ALWAYS** communicate in `{communication_language}`

---

## MANDATORY SEQUENCE

### 1. Initialize Selection State

```
FOR each node in hierarchy_tree.nodes:
    node.selected = true  // Default: all selected

selection_state = {
    total: nodes.length,
    selected: nodes.length,
    intermediate_selected: nodes.filter(n => !n.is_leaf).length,
    leaf_selected: nodes.filter(n => n.is_leaf).length
}
```

### 2. Display Selection Interface

```
╔══════════════════════════════════════════════════════════════╗
║  ☑️ SÉLECTION DES PROJETS À CRÉER                            ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  Tous les nœuds sont des projets BMAD complets.              ║
║  Cochez/décochez les projets à créer:                        ║
║                                                              ║
{#each node with index}
║  [{checkbox}] {index}. {indent}📁 {node.name}/               ║
║               {node.suggested_focus || ""}                   ║
{/each}
║                                                              ║
║  ──────────────────────────────────────────────────────────  ║
║  Sélection: {selected}/{total} projets                       ║
║  • Projets intermédiaires: {intermediate_selected}           ║
║  • Projets finaux (feuilles): {leaf_selected}                ║
║                                                              ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  Commandes:                                                  ║
║  • Entrez les numéros à toggle (ex: 3,5,7)                   ║
║  • [T] Tout sélectionner                                     ║
║  • [N] Tout désélectionner                                   ║
║  • [L] Sélectionner uniquement les feuilles                  ║
║  • [C] Confirmer la sélection                                ║
║  • [R] Retour à la proposition                               ║
║                                                              ║
╚══════════════════════════════════════════════════════════════╝
```

Where `{checkbox}` = `x` if selected, ` ` if not.
Where `{indent}` = appropriate tree indentation.

### 3. Handle Toggle Input

```
IF input matches number pattern (e.g., "3,5,7" or "3"):
    FOR each number in input:
        node = nodes[number - 1]
        node.selected = !node.selected

        // Dependency handling
        IF node.selected:
            // Auto-select all parents
            parent = node.parent
            WHILE parent:
                parent.selected = true
                parent = parent.parent

        ELSE: // Deselecting
            // Check if any children would be orphaned
            orphan_children = get_selected_children(node)
            IF orphan_children.length > 0:
                Display warning:
                "⚠️ Attention: {orphan_children.length} projet(s) enfant(s) sélectionné(s)
                 seront aussi désélectionnés car leur parent est requis.

                 [O] OK, désélectionner aussi les enfants
                 [K] Garder la sélection actuelle"

                IF O: deselect node and all children
                IF K: cancel deselection

    Recalculate selection_state
    Redisplay selection interface
```

### 4. Menu Handling Logic

#### IF [T] Select All:
```
FOR each node: node.selected = true
Redisplay selection interface
```

#### IF [N] Deselect All:
```
FOR each node: node.selected = false
Redisplay selection interface
```

#### IF [L] Leaves Only:
```
// WARNING: This creates leaf projects without their parent projects
// The hierarchy will be incomplete

Display warning:
"⚠️ ATTENTION: Sélectionner uniquement les feuilles signifie que
les projets intermédiaires (app/, app/mobile/, etc.) ne seront
pas créés. Les feuilles seront créées directement sous le master.

Cela brise la structure hiérarchique recommandée.

[O] OK, continuer (déconseillé)
[K] Garder la sélection actuelle"

IF O:
    FOR each node:
        node.selected = node.is_leaf
IF K:
    Cancel

Redisplay selection interface
```

#### IF [C] Confirm:
```
// Validate selection
selected_nodes = nodes.filter(n => n.selected)

IF selected_nodes.length == 0:
    Display: "⚠️ Aucun projet sélectionné. Sélectionnez au moins un projet."
    Redisplay selection interface

// Check for orphan projects (selected project without selected parent)
orphan_count = 0
FOR each selected project:
    IF project.parent AND NOT project.parent.selected:
        // Auto-fix: select parent
        project.parent.selected = true
        orphan_count++

IF orphan_count > 0:
    Display: "ℹ️ {orphan_count} projet(s) parent(s) auto-sélectionné(s) pour maintenir la hiérarchie."

// Confirm
Display:
"✅ Sélection confirmée: {selected_nodes.length} projets BMAD à créer

   Chaque projet aura:
   • _bmad/ (configuration)
   • _bmad-output/ (artéfacts)
   • _mailbox/ (communication)
   • hierarchy.csv (vue locale)

   Passage à la création..."
```
Then: Load, read entirely, then execute {nextStepFile}

#### IF [R] Return:
```
Display: "Retour à la proposition..."
```
Load, read entirely, then execute {previousStepFile}

#### IF Any other:
Help user, then redisplay selection interface.

---

## CONTEXT OUTPUT

Pass to next step:
```yaml
selected_nodes:
  - id: "app"
    path: "./app/"
    type: "project"
    is_leaf: false
    suggested_focus: "Stratégie UX globale"
    selected: true
  - id: "app-mobile"
    path: "./app/mobile/"
    type: "project"
    is_leaf: false
    suggested_focus: "Stratégie mobile"
    selected: true
  - id: "app-mobile-ios"
    path: "./app/mobile/ios/"
    type: "project"
    is_leaf: true
    technologies: ["Swift", "SwiftUI"]
    selected: true
  # ... only selected nodes

creation_order: ["app", "app-mobile", "app-mobile-ios", ...]  // Parents first
```

---

## SUCCESS METRICS

### ✅ SUCCESS:
- Clear checkbox interface showing all as projects
- Auto-select parents when child selected
- Warn about orphan selections
- Proper dependency handling
- Confirm that all will have full BMAD structure

### ❌ FAILURE:
- Referring to any node as "container"
- Allowing orphan projects (project without parent)
- Not auto-selecting parents
- Confusing interface
