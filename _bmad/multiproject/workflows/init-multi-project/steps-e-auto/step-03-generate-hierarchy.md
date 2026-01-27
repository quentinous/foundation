---
name: 'step-03-generate-hierarchy'
description: 'Generate hierarchical project structure from extracted components'

# File References
nextStepFile: './step-04-present-proposal.md'
categoriesData: '../data/hierarchy-categories.md'

# Context from previous step
# extracted_components: passed from step-02
---

# Step 3: Generate Hierarchical Structure

## STEP GOAL:

Transform the flat list of extracted components into a hierarchical project structure following the "funnel thinking" principle.

## MANDATORY EXECUTION RULES:

- 🧠 **ALWAYS** think hierarchically - NEVER create flat structures
- 🌳 **ALWAYS** create intermediate project nodes (every level is a full BMAD project)
- 📈 **ALWAYS** anticipate future evolution
- ✅ **ALWAYS** communicate in `{communication_language}`

---

## CRITICAL PRINCIPLE: EVERY NODE IS A BMAD PROJECT

> 🚨 **RÈGLE ABSOLUE : PAS DE "CONTENEUR"**
>
> Il n'existe PAS de "conteneur" dans BMAD Multi-Project.
> **Chaque nœud est un projet BMAD complet** avec :
> - `_bmad/` - Configuration et workflows
> - `_bmad-output/` - Artéfacts de sortie
> - `_mailbox/` - Communication inter-projets
> - `hierarchy.csv` - Vue locale de la hiérarchie
>
> **Chaque projet est un submodule git** (repo indépendant)

---

## CRITICAL PRINCIPLE: FUNNEL THINKING

> 🧠 **PENSÉE HIÉRARCHIQUE OBLIGATOIRE**
>
> L'IA doit TOUJOURS proposer une structure hiérarchique, jamais à plat.
> Même pour un seul élément, anticiper l'évolution future.
>
> **Exemple :**
> - Composant détecté : "iOS app"
> - ❌ Mauvais : créer `ios/` (plat, bloquant)
> - ✅ Bon : créer `app/mobile/ios/` (hiérarchique, évolutif)
>
> **Raison : Système en entonnoir**
> ```
> app/                    ← Projet BMAD : stratégie UX globale, design system
> ├── mobile/             ← Projet BMAD : stratégie mobile, SDK communs
> │   ├── ios/            ← Projet BMAD : app iOS spécifique
> │   └── android/        ← Projet BMAD : app Android spécifique
> └── desktop/            ← Projet BMAD : stratégie desktop
>     └── windows/        ← Projet BMAD : app Windows spécifique
> ```
>
> **Chaque niveau peut :**
> - Avoir son propre PRD, architecture, sprints
> - Traiter et enrichir les transmissions (pas juste les transférer)
> - Produire de la valeur ajoutée pour ses enfants

---

## MANDATORY SEQUENCE

### 1. Load Categories Reference

Load {categoriesData} for standard category mappings.

### 2. Categorization Algorithm

```
POUR chaque composant dans extracted_components:

    1. IDENTIFIER la catégorie principale:
       - app      → Applications utilisateur
       - services → Backend, APIs, microservices
       - infra    → Infrastructure, déploiement
       - libs     → Librairies partagées, SDKs

    2. IDENTIFIER la sous-catégorie:
       - app/mobile/     → iOS, Android, React Native
       - app/desktop/    → Windows, Linux, macOS
       - app/web/        → Frontend web
       - services/api/   → REST, GraphQL
       - services/core/  → Auth, Users, etc.
       - infra/data/     → Databases
       - infra/deploy/   → K8s, Docker
       - infra/ci/       → Pipelines
       - libs/shared/    → Common utils
       - libs/sdk/       → Client SDKs

    3. GÉNÉRER le chemin hiérarchique complet:
       component.path = "{category}/{subcategory}/{component_name}/"

    4. CRÉER les projets intermédiaires si nécessaire:
       SI path contient des niveaux non existants:
           CRÉER chaque niveau comme PROJET BMAD COMPLET
```

### 3. Build Hierarchy Tree

```
hierarchy_tree = {
    nodes: [],      // All nodes (ALL are projects)
    root: null      // Master project reference
}

POUR chaque composant:
    path_parts = component.path.split('/')

    POUR chaque niveau in path_parts:
        SI niveau n'existe pas dans nodes:
            is_leaf = (niveau == dernier élément)

            CRÉER node:
                id: generate_id(path_parts jusqu'à niveau)
                name: niveau
                path: construire_path(path_parts jusqu'à niveau)
                type: "project"  // TOUJOURS projet, jamais conteneur
                parent: niveau précédent ou root
                technologies: is_leaf ? component.technologies : []
                depth: profondeur dans l'arbre
                is_leaf: is_leaf  // Pour info seulement

            AJOUTER node à hierarchy_tree.nodes
```

### 4. Suggest Project Focus

Pour chaque projet intermédiaire (non-leaf), suggérer un focus :

```
POUR chaque projet non-leaf dans hierarchy_tree:
    SELON node.path:
        "app/" → focus: "Stratégie UX globale, design system, accessibilité"
        "app/mobile/" → focus: "Stratégie mobile, SDK communs, guidelines stores"
        "app/desktop/" → focus: "Stratégie desktop, installateurs, distribution"
        "app/web/" → focus: "SEO, performance web, compatibilité navigateurs"
        "services/" → focus: "Architecture backend, standards API, sécurité"
        "services/api/" → focus: "Versioning API, documentation, rate limiting"
        "infra/" → focus: "Cloud strategy, coûts, disaster recovery"
        "libs/" → focus: "Standards code, versioning, documentation"
```

### 5. Generate Structure Preview

Build the visual tree for next step:

```
structure_preview = []

POUR chaque node dans hierarchy_tree (ordre depth-first):
    indent = "│   " * node.depth
    connector = "├── " ou "└── " selon position

    // Tous sont des projets - indiquer le focus suggéré
    focus_hint = node.is_leaf ? "" : " → {node.suggested_focus}"

    AJOUTER à structure_preview:
        "{indent}{connector}📁 {node.name}/{focus_hint}"
```

### 6. Pass to Next Step

Save context:
```yaml
hierarchy_tree:
  nodes:
    - id: "app"
      name: "app"
      path: "./app/"
      type: "project"
      parent: null
      children: ["app-mobile", "app-web"]
      suggested_focus: "Stratégie UX globale, design system"
      is_leaf: false
    - id: "app-mobile"
      name: "mobile"
      path: "./app/mobile/"
      type: "project"
      parent: "app"
      children: ["app-mobile-ios", "app-mobile-android"]
      suggested_focus: "Stratégie mobile, SDK communs"
      is_leaf: false
    - id: "app-mobile-ios"
      name: "ios"
      path: "./app/mobile/ios/"
      type: "project"
      parent: "app-mobile"
      children: []
      technologies: ["Swift", "SwiftUI"]
      is_leaf: true
    # ... etc

structure_preview: |
  📁 app/                    → Stratégie UX globale
  ├── 📁 mobile/             → Stratégie mobile
  │   ├── 📁 ios/
  │   └── 📁 android/
  └── 📁 web/                → SEO, performance web
      └── 📁 frontend/
```

Then: Load, read entirely, then execute {nextStepFile}

---

## SUCCESS METRICS

### ✅ SUCCESS:
- All components mapped to hierarchical paths
- ALL nodes are projects (no containers)
- Intermediate projects have suggested focus
- No flat structures (single-level)
- Future evolution anticipated

### ❌ FAILURE:
- Creating flat structure (ios/ instead of app/mobile/ios/)
- Creating "containers" without full BMAD structure
- Not anticipating evolution
