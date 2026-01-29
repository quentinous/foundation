---
name: 'step-04-present-proposal'
description: 'Present hierarchical structure proposal to user for review'

# File References
nextStepFile: './step-05-selection.md'
mainMenuReturn: '../workflow.md#3-display-menu'

# Context from previous step
# hierarchy_tree: passed from step-03
# structure_preview: passed from step-03
---

# Step 4: Present Proposal

## STEP GOAL:

Present the generated hierarchical structure to the user for review, explaining the benefits of the funnel architecture where every node is a full BMAD project.

## MANDATORY EXECUTION RULES:

- 📊 **ALWAYS** show the complete tree structure
- 💡 **ALWAYS** explain that every node is a full BMAD project
- ✏️ **ALWAYS** allow user to modify
- ✅ **ALWAYS** communicate in `{communication_language}`

---

## MANDATORY SEQUENCE

### 1. Count Statistics

```
total_projects = hierarchy_tree.nodes.length
leaf_projects = nodes.filter(n => n.is_leaf).length
intermediate_projects = nodes.filter(n => !n.is_leaf).length
max_depth = max(nodes.map(n => n.depth))
```

### 2. Display Proposal

```
╔══════════════════════════════════════════════════════════════╗
║  🌳 PROPOSITION DE STRUCTURE HIÉRARCHIQUE                    ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  Basée sur: architecture.md {+ prd.md si utilisé}            ║
║                                                              ║
║  {master_name}/                                              ║
{structure_preview}
║                                                              ║
║  Statistiques:                                               ║
║  • Total projets: {total_projects}                           ║
║  • Projets intermédiaires: {intermediate_projects}           ║
║  • Projets finaux (feuilles): {leaf_projects}                ║
║  • Profondeur max: {max_depth} niveaux                       ║
║                                                              ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  📁 TOUS LES NŒUDS SONT DES PROJETS BMAD COMPLETS            ║
║                                                              ║
║  Chaque projet (intermédiaire ou final) possède:             ║
║  • _bmad/        → Configuration et workflows                ║
║  • _bmad-output/ → Artéfacts (PRD, architecture, etc.)       ║
║  • _mailbox/     → Communication inter-projets               ║
║  • hierarchy.csv → Vue locale de la hiérarchie               ║
║                                                              ║
║  Chaque projet peut avoir son propre cycle de vie BMAD:      ║
║  • Son propre PRD et architecture                            ║
║  • Ses propres sprints et stories                            ║
║  • Traiter (pas juste transférer) les transmissions          ║
║                                                              ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  💡 SYSTÈME EN ENTONNOIR (pourquoi c'est puissant):          ║
║                                                              ║
{#for each top_level_category}
║  📁 {category}/                                              ║
║     Focus suggéré: {suggested_focus}                         ║
║     Peut recevoir: {transmission_types}                      ║
{/for}
║                                                              ║
║  → Chaque niveau TRAITE et ENRICHIT les informations         ║
║  → Les enfants héritent + ajoutent leur spécificité          ║
║  → Structure évolutive: ajout facile de nouveaux projets     ║
║                                                              ║
╚══════════════════════════════════════════════════════════════╝
```

### 3. Display Menu Options

```
Que souhaitez-vous faire ?

[A] Accepter cette structure → Passer à la sélection
[E] Éditer un projet        → Modifier nom ou supprimer
[D] Ajouter un projet       → Créer manuellement un nouveau projet
[V] Voir les détails        → Afficher focus et technologies
[R] Rejeter et revenir      → Retour au menu principal
```

### 4. Menu Handling Logic

#### IF [A] Accept:
```
Display: "Structure acceptée. Passage à la sélection des projets à créer..."
```
Then: Load, read entirely, then execute {nextStepFile}

#### IF [E] Edit:
```
Display numbered list of projects:

Sélectionnez le projet à modifier (numéro):

1. 📁 app/                    → Stratégie UX globale
2. 📁 app/mobile/             → Stratégie mobile
3. 📁 app/mobile/ios/         (feuille)
4. 📁 app/mobile/android/     (feuille)
...

Entrez le numéro:
```

On selection, display:
```
Modification de: {node.name}
Path actuel: {node.path}
Focus suggéré: {node.suggested_focus}

[N] Renommer
[F] Modifier le focus suggéré
[S] Supprimer ce projet (et ses enfants)
[C] Annuler
```

Handle sub-menu, then redisplay main proposal.

#### IF [D] Add Project:
```
Ajouter un nouveau projet:

Parent (chemin ou numéro): ___
Nom du projet: ___
Focus suggéré (optionnel): ___
```

Add to hierarchy_tree, then redisplay proposal.

#### IF [V] View Details:
```
╔══════════════════════════════════════════════════════════════╗
║  📋 DÉTAILS DES PROJETS                                      ║
╠══════════════════════════════════════════════════════════════╣
{#each node in hierarchy_tree.nodes}
║                                                              ║
║  📁 {node.path}                                              ║
║     Niveau: {node.is_leaf ? "Feuille" : "Intermédiaire"}     ║
║     Focus: {node.suggested_focus || "À définir"}             ║
║     Technologies: {node.technologies.join(', ') || "N/A"}    ║
║     Source: {node.source_section || "Généré"}                ║
{/each}
╚══════════════════════════════════════════════════════════════╝

[Entrée] Retour à la proposition
```

Then redisplay proposal.

#### IF [R] Reject:
```
Display: "Structure rejetée. Retour au menu principal..."
```
Return to {mainMenuReturn}

#### IF Any other:
Help user understand options, then redisplay menu.

#### EXECUTION RULES:
- ALWAYS halt and wait for user input after presenting menu
- ONLY proceed to next step when user selects 'A' (Accept)
- After other menu items execution, return to this menu

---

## CONTEXT OUTPUT

Pass to next step:
```yaml
hierarchy_tree: {updated tree with any modifications}
user_accepted: true
```

---

## SUCCESS METRICS

### ✅ SUCCESS:
- Clear presentation that ALL nodes are full projects
- Benefits of funnel architecture explained
- User can modify before accepting
- All options functional

### ❌ FAILURE:
- Suggesting some nodes are "containers" without _bmad/
- Not explaining that every node has full BMAD structure
- Confusing presentation
