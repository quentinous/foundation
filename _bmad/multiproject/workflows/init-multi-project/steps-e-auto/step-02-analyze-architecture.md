---
name: 'step-02-analyze-architecture'
description: 'Extract components and services from architecture document'

# File References
nextStepFile: './step-03-generate-hierarchy.md'
categoriesData: '../data/hierarchy-categories.md'

# Context from previous step
architecturePath: '{architecture_path}'
prdPaths:
  - '{project-root}/_bmad-output/planning-artifacts/prd.md'
  - '{project-root}/docs/prd.md'
---

# Step 2: Analyze Architecture

## STEP GOAL:

Extract all components, services, and structural elements from the architecture document to prepare hierarchical project generation.

## MANDATORY EXECUTION RULES:

- 📖 **ALWAYS** load the complete architecture document
- 🔍 **ALWAYS** extract ALL mentioned components
- 📋 **ALSO** check PRD for additional context
- ✅ **ALWAYS** communicate in `{communication_language}`

---

## MANDATORY SEQUENCE

### 1. Load Architecture Document

```
LOAD {architecturePath} completely

EXTRACT from document:
  - Project Structure section
  - Component/Service Boundaries
  - Technology decisions
  - Integration points
  - Data storage components
```

### 2. Load PRD (Optional Enhancement)

```
SEARCH for PRD:
  1. {project-root}/_bmad-output/planning-artifacts/prd.md
  2. {project-root}/docs/prd.md

IF found:
  EXTRACT:
    - Major features
    - Target platforms (web, mobile, desktop)
    - User types (may indicate separate apps)
    - Integration requirements
```

### 3. Extract Components

**Parse architecture for these patterns:**

| Pattern to detect | Component type |
|-------------------|----------------|
| "iOS app", "iPhone", "Apple" | Mobile iOS |
| "Android app", "Google Play" | Mobile Android |
| "React Native", "Flutter", "Expo" | Mobile Cross-platform |
| "Windows app", "Win32", "WPF" | Desktop Windows |
| "Linux app", "GTK", "Qt" | Desktop Linux |
| "macOS app", "Cocoa", "Mac" | Desktop macOS |
| "web frontend", "React", "Vue", "Angular" | Web Frontend |
| "REST API", "HTTP endpoints" | API REST |
| "GraphQL", "Apollo" | API GraphQL |
| "authentication", "auth service", "OIDC", "OAuth" | Core Auth |
| "PostgreSQL", "MySQL", "MongoDB" | Database |
| "Kubernetes", "K8s", "Docker Compose" | Deployment |
| "CI/CD", "GitHub Actions", "GitLab CI" | CI Pipeline |
| "shared library", "common utils" | Shared Lib |
| "SDK", "client library" | SDK |

### 4. Build Component List

```
FOR each detected component:
  CREATE entry:
    {
      name: "detected name",
      type: "category from table",
      source: "architecture|prd",
      section: "where found",
      technologies: ["list", "of", "tech"],
      dependencies: ["other", "components"]
    }
```

### 5. Present Extraction Results

Display:
```
╔════════════════════════════════════════════════════════════╗
║  🔍 ANALYSE DE L'ARCHITECTURE                              ║
╠════════════════════════════════════════════════════════════╣
║                                                            ║
║  Sources analysées:                                        ║
║  • {architecture_path}                                     ║
║  • {prd_path} (si trouvé)                                  ║
║                                                            ║
║  Composants détectés: {count}                              ║
║                                                            ║
║  📱 Applications:                                          ║
{#each app_components}
║     • {name} ({type}) - {technologies}                     ║
{/each}
║                                                            ║
║  🔧 Services:                                              ║
{#each service_components}
║     • {name} ({type}) - {technologies}                     ║
{/each}
║                                                            ║
║  🏗️ Infrastructure:                                        ║
{#each infra_components}
║     • {name} ({type}) - {technologies}                     ║
{/each}
║                                                            ║
║  📚 Librairies:                                            ║
{#each lib_components}
║     • {name} ({type}) - {technologies}                     ║
{/each}
║                                                            ║
╚════════════════════════════════════════════════════════════╝

[C] Continuer avec ces composants
[A] Ajouter un composant manuellement
[R] Retirer un composant de la liste
[X] Annuler et revenir au menu
```

#### Menu Handling Logic:
- IF C: Save component list to context, then load, read entirely, then execute {nextStepFile}
- IF A: Prompt for component details, add to list, redisplay
- IF R: Show numbered list, prompt for number to remove, redisplay
- IF X: Return to main menu
- IF Any other: help user, then redisplay menu

---

## CONTEXT OUTPUT

Pass to next step:
```yaml
extracted_components:
  - name: "..."
    type: "..."
    category: "app|services|infra|libs"
    subcategory: "mobile|desktop|web|api|core|data|deploy|ci|sdk"
    technologies: [...]
```

---

## SUCCESS METRICS

### ✅ SUCCESS:
- Architecture fully parsed
- All components extracted
- PRD consulted if available
- User can review and adjust list

### ❌ FAILURE:
- Missing obvious components from architecture
- Not allowing user to adjust list
- Proceeding with empty component list
