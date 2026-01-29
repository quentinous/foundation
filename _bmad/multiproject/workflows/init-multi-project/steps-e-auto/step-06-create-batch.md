---
name: 'step-06-create-batch'
description: 'Create all selected projects in correct order (parents first)'

# File References
nextStepFile: './step-07-transmit-confirm.md'
mailboxConfigTemplate: '{project-root}/_bmad/multiproject/templates/mailbox-config.yaml'
hierarchyCsvTemplate: '{project-root}/_bmad/multiproject/templates/hierarchy.csv'

# Context from previous step
# selected_nodes: passed from step-05
# creation_order: passed from step-05
---

# Step 6: Create Batch

## STEP GOAL:

Create all selected projects in the correct order (parents first), updating hierarchy.csv as we go. Every node is a full BMAD project.

## MANDATORY EXECUTION RULES:

- 📁 **ALWAYS** create parents before children
- 🏗️ **EVERY** node gets full BMAD structure (no exceptions)
- 📄 **ALWAYS** update master hierarchy.csv
- 🔗 **EACH** project is a git submodule
- ✅ **ALWAYS** communicate in `{communication_language}`

---

## CRITICAL PRINCIPLE: NO CONTAINERS

> 🚨 **RÈGLE ABSOLUE : TOUT EST PROJET BMAD**
>
> Il n'existe PAS de "conteneur" sans `_bmad/`.
> Chaque nœud créé est un **projet BMAD complet** avec :
> - `_bmad/` - Configuration
> - `_bmad-output/` - Artéfacts
> - `_mailbox/` - Communication
> - `hierarchy.csv` - Vue locale
>
> **Pourquoi ?**
> - Chaque niveau peut avoir son propre cycle de vie BMAD
> - Chaque niveau peut TRAITER les transmissions (pas juste les transférer)
> - Chaque niveau est un repo git indépendant (submodule)

---

## MANDATORY SEQUENCE

### 1. Prepare Creation

```
// Sort by depth (parents first)
creation_order = selected_nodes.sort((a, b) => a.depth - b.depth)

// Load master hierarchy.csv
master_hierarchy = LOAD "{hierarchyCsvTemplate}" or "{project-root}/hierarchy.csv" if template is source
master_project_id = first line with empty parent_id

creation_log = []
errors = []
```

### 2. Display Progress Header

```
╔══════════════════════════════════════════════════════════════╗
║  🏗️ CRÉATION DES PROJETS                                     ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  Projets à créer: {creation_order.length}                    ║
║  (Tous sont des projets BMAD complets)                       ║
║                                                              ║
```

### 3. Create Each Project

```
FOR each node in creation_order:

    Display: "║  ⏳ Création du projet {node.path}..."

    TRY:
        create_bmad_project(node)

        // Add to master hierarchy.csv
        add_to_hierarchy(node)

        // Generate local hierarchy.csv for this project
        generate_local_hierarchy(node)

        Display: "║  ✅ {node.path} créé (projet BMAD complet)"
        creation_log.push({node: node, status: "success"})

    CATCH error:
        Display: "║  ❌ {node.path} - Erreur: {error}"
        errors.push({node: node, error: error})
        creation_log.push({node: node, status: "error", error: error})
```

### 4. Project Creation (ALL nodes)

```
FUNCTION create_bmad_project(node):
    path = "{project-root}/{node.path}"
    
    // 1. Create project root
    MKDIR path
    
    // 2. Install BMAD using CLI
    // Note: User can specify version, defaults to @alpha
    installer_version = "alpha" // or prompt user if needed
    
    Display: "Running installer for {node.name}..."
    RUN "npx bmad-method@{installer_version} install" inside {path}
    
    // 3. Post-Install Configuration Update
    // The installer creates _bmad/config.yaml, we need to enrich it
    config_path = "{path}/_bmad/config.yaml"
    
    UPDATE config_path:
        SET project_focus = "{node.suggested_focus}"
        SET parent_project = "{node.parent.name if exists else 'master'}"
        // communication_language and user_name are set by installer prompts or defaults

    // 4. Multi-Project Overlay (Mailbox)
    // Create _mailbox structure which is specific to multi-project
    MKDIR path/_mailbox
    MKDIR path/_mailbox/inbox
    MKDIR path/_mailbox/outbox
    MKDIR path/_mailbox/sent
    MKDIR path/_mailbox/archive

    COPY {mailboxConfigTemplate} TO path/_mailbox/.mailbox-config.yaml

    // 5. Git Initialization
    IF git_submodule_mode:
        git init path
        // User will need to: git submodule add <remote> <path>
```

### 5. Add to Master Hierarchy

```
FUNCTION add_to_hierarchy(node):
    // Determine parent_id
    IF node.parent:
        parent_id = node.parent.id
    ELSE:
        parent_id = master_project_id

    // Determine tags based on path
    tags = generate_tags(node)

    // Add line to master hierarchy.csv
    APPEND TO master_hierarchy:
        "{node.id},{parent_id},{node.path},{tags},active,main"
```

### 6. Generate Local Hierarchy

```
FUNCTION generate_local_hierarchy(node):
    // Each project needs its own view of the hierarchy
    local_hierarchy = []

    // Add master
    local_hierarchy.push(master_line with path adjusted)

    // Add all ancestors
    ancestor = node.parent
    WHILE ancestor:
        local_hierarchy.push(ancestor with relative path)
        ancestor = ancestor.parent

    // Add siblings
    IF node.parent:
        FOR each sibling in node.parent.children:
            IF sibling != node:
                local_hierarchy.push(sibling with relative path)

    // Add self with path="./"
    local_hierarchy.push(node with path="./")

    // Add children
    FOR each child in node.children:
        local_hierarchy.push(child with relative path)

    WRITE "{node.path}/hierarchy.csv" with local_hierarchy
```

### 7. Display Summary

```
║                                                              ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  📊 RÉSUMÉ DE LA CRÉATION                                    ║
║                                                              ║
║  Total projets: {creation_order.length}                      ║
║  ✅ Succès: {success_count}                                  ║
║  ❌ Erreurs: {error_count}                                   ║
║                                                              ║
{IF errors.length > 0}
║  Erreurs détaillées:                                         ║
{#each error in errors}
║  • {error.node.path}: {error.error}                          ║
{/each}
{/IF}
║                                                              ║
║  📁 Chaque projet créé avec:                                 ║
║     • _bmad/ (configuration)                                 ║
║     • _bmad-output/ (artéfacts)                              ║
║     • _mailbox/ (communication)                              ║
║     • hierarchy.csv (vue locale)                             ║
║                                                              ║
║  hierarchy.csv du master mis à jour                          ║
║                                                              ║
╚══════════════════════════════════════════════════════════════╝
```

### 8. Git Submodule Reminder

```
Display:
"
💡 RAPPEL GIT SUBMODULES:

Chaque projet créé est un dossier local. Pour le transformer en
submodule git (recommandé), vous devrez :

1. Créer un repo distant pour chaque projet
2. Exécuter: git submodule add <remote-url> <path>

Exemple:
  git submodule add git@github.com:org/app.git app/
  git submodule add git@github.com:org/app-mobile.git app/mobile/
"
```

### 9. Proceed to Transmission

```
IF errors.length > 0:
    Display:
    "⚠️ Des erreurs sont survenues. Voulez-vous continuer ?
     [C] Continuer (ignorer les erreurs)
     [R] Réessayer les erreurs
     [X] Annuler"

    IF C: proceed
    IF R: retry failed nodes, then proceed
    IF X: return to main menu

ELSE:
    Display: "Passage à l'envoi des transmissions..."
```

#### EXECUTION RULES:
- ALWAYS halt and wait for user input after presenting menu (if errors occur)
- ONLY proceed to next step when user selects 'C' or no errors
- After other menu items execution, return to this menu

Then: Load, read entirely, then execute {nextStepFile}

---

## CONTEXT OUTPUT

Pass to next step:
```yaml
creation_log:
  - node: {id, path}
    status: "success|error"
    error: null|"message"

created_projects: [list of successfully created projects]
master_hierarchy_updated: true
all_have_bmad: true  # Confirmation that all have full BMAD structure
```

---

## SUCCESS METRICS

### ✅ SUCCESS:
- ALL projects have full _bmad/ structure
- ALL projects have _bmad-output/
- ALL projects have _mailbox/
- Master hierarchy.csv updated
- Each project has local hierarchy.csv
- Proper creation order (parents first)
- Git submodule instructions provided

### ❌ FAILURE:
- Creating any node WITHOUT _bmad/ (no containers!)
- Not updating master hierarchy.csv
- Creating children before parents
- Not generating local hierarchies
