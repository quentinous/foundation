---
name: 'step-01-check-prerequisites'
description: 'Verify architecture document exists and is complete before auto mode'

# File References
nextStepFile: './step-02-analyze-architecture.md'
manualModeReturn: '../workflow.md#e-m-mode-manuel---configuration-libre'
mainMenuReturn: '../workflow.md#3-display-menu'

# Data References
architecturePaths:
  - '{project-root}/_bmad-output/planning-artifacts/architecture.md'
  - '{project-root}/docs/architecture.md'
---

# Step 1: Check Prerequisites

## STEP GOAL:

Verify that the master project has a completed architecture document before proceeding with automatic child project generation.

## MANDATORY EXECUTION RULES:

- 🛑 **ALWAYS** check for architecture document existence
- 📖 **ALWAYS** verify architecture completeness
- ⚠️ **NEVER** block manual mode - only discourage
- ✅ **ALWAYS** communicate in `{communication_language}`

---

## MANDATORY SEQUENCE

### 1. Search for Architecture Document

```
SEARCH in order:
1. {project-root}/_bmad-output/planning-artifacts/architecture.md
2. {project-root}/docs/architecture.md

IF found:
    architecture_path = found_path
    GOTO Step 2
ELSE:
    GOTO Step 3 (No Architecture)
```

### 2. Verify Architecture Completeness

```
LOAD architecture document
CHECK for required sections:
  - [ ] Project Structure / Directory Structure
  - [ ] Component Boundaries / Service Boundaries
  - [ ] Technology Stack
  - [ ] Data Flow / Integration Points

CHECK frontmatter if exists:
  - stepsCompleted array
  - status field

CALCULATE completeness_score:
  - 4/4 sections = COMPLETE
  - 2-3/4 sections = PARTIAL
  - 0-1/4 sections = INSUFFICIENT
```

**IF COMPLETE:**

Display:
```
╔════════════════════════════════════════════════════════════╗
║  ✅ ARCHITECTURE VALIDÉE                                   ║
╠════════════════════════════════════════════════════════════╣
║                                                            ║
║  Document: {architecture_path}                             ║
║  Sections trouvées: {list}                                 ║
║  Status: COMPLET                                           ║
║                                                            ║
║  Le mode automatique peut analyser cette architecture      ║
║  pour proposer une structure de projets hiérarchique.      ║
║                                                            ║
╚════════════════════════════════════════════════════════════╝
```

Then: Load, read entirely, then execute {nextStepFile}

**IF PARTIAL:**

Display:
```
╔════════════════════════════════════════════════════════════╗
║  ⚠️  ARCHITECTURE INCOMPLÈTE                               ║
╠════════════════════════════════════════════════════════════╣
║                                                            ║
║  Document: {architecture_path}                             ║
║  Sections trouvées: {found_list}                           ║
║  Sections manquantes: {missing_list}                       ║
║                                                            ║
║  Le mode automatique fonctionne mieux avec une             ║
║  architecture complète pour éviter les restructurations.   ║
║                                                            ║
║  [C] Continuer quand même (analyse partielle)              ║
║  [M] Basculer vers mode MANUEL                             ║
║  [R] Retour au menu principal                              ║
║                                                            ║
╚════════════════════════════════════════════════════════════╝
```

#### Menu Handling Logic:
- IF C: Load, read entirely, then execute {nextStepFile}
- IF M: Return to {manualModeReturn}
- IF R: Return to {mainMenuReturn}
- IF Any other: help user, then redisplay menu

### 3. No Architecture Found

Display:
```
╔════════════════════════════════════════════════════════════╗
║  ⚠️  ARCHITECTURE NON TROUVÉE                              ║
╠════════════════════════════════════════════════════════════╣
║                                                            ║
║  Chemins recherchés:                                       ║
║  • _bmad-output/planning-artifacts/architecture.md         ║
║  • docs/architecture.md                                    ║
║                                                            ║
║  Le mode automatique nécessite un document d'architecture  ║
║  pour proposer une structure de projets cohérente.         ║
║                                                            ║
║  Recommandation: Terminer le workflow create-architecture  ║
║  avant de créer des projets enfants.                       ║
║                                                            ║
║  [M] Basculer vers mode MANUEL (déconseillé)               ║
║  [R] Retour au menu principal                              ║
║                                                            ║
╚════════════════════════════════════════════════════════════╝
```

#### Menu Handling Logic:
- IF M: Return to {manualModeReturn}
- IF R: Return to {mainMenuReturn}
- IF Any other: help user, then redisplay menu

---

## SUCCESS METRICS

### ✅ SUCCESS:
- Architecture document located (or absence properly handled)
- Completeness verified
- User informed of status
- Appropriate routing based on findings

### ❌ FAILURE:
- Proceeding without checking architecture
- Blocking manual mode entirely
- Not informing user of missing sections
