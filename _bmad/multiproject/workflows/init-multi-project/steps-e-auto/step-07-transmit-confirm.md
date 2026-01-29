---
name: 'step-07-transmit-confirm'
description: 'Send hierarchy-update transmission and confirm completion'

# File References
mainMenuReturn: '../workflow.md#3-display-menu'
transmissionTemplate: '{project-root}/_bmad/multiproject/templates/transmission.md'

# Context from previous step
# creation_log: passed from step-06
# created_projects: passed from step-06
---

# Step 7: Transmit & Confirm

## STEP GOAL:

Create and dispatch a `hierarchy-update` transmission to all existing projects, then confirm successful completion to the user.

## MANDATORY EXECUTION RULES:

- 📤 **ALWAYS** create hierarchy-update transmission
- 📬 **ALWAYS** dispatch to accessible projects
- ✅ **ALWAYS** provide clear next steps
- 📁 **REMEMBER** all created nodes are full BMAD projects
- ✅ **ALWAYS** communicate in `{communication_language}`

---

## MANDATORY SEQUENCE

### 1. Generate Transmission ID

```
timestamp = current datetime
suffix = random 8-char hex
transmission_id = "{suffix}"
filename = "TX_{master_project_id}_{date}_{time}_{suffix}.md"
```

### 2. Create Transmission Document

Create in `{project-root}/_mailbox/outbox/{filename}` using template {transmissionTemplate}:

```markdown
---
transmission_id: {suffix}
created: {ISO timestamp}
from_project: {master_project_id}
to_project: "*"
type: hierarchy-update
priority: medium
status: pending
---

# Transmission: Structure Hiérarchique Créée

## Résumé

Une nouvelle structure hiérarchique de projets BMAD a été créée dans l'écosystème.

## Nouveaux projets

| Path | Focus | Status |
|------|-------|--------|
{#each created_project}
| {project.path} | {project.suggested_focus || "À définir"} | ✅ Créé |
{/each}

## Statistiques

- **Projets intermédiaires créés:** {intermediate_count}
- **Projets finaux (feuilles) créés:** {leaf_count}
- **Total projets BMAD:** {total_count}

## Structure de chaque projet

Chaque projet créé possède:
- `_bmad/` - Configuration BMAD
- `_bmad-output/` - Artéfacts de sortie
- `_mailbox/` - Communication inter-projets
- `hierarchy.csv` - Vue locale de la hiérarchie

## Actions requises

Pour chaque projet concerné:
1. Mettre à jour votre `hierarchy.csv` local
2. Vérifier les nouvelles relations parent/enfant
3. Configurer `.mailbox-config.yaml` si nécessaire

## Détails techniques

- **Date de création:** {timestamp}
- **Créé par:** init-multi-project workflow v1.6.0
- **Mode:** Automatique (basé sur architecture)
```

### 3. Dispatch Transmission

```
// Load existing projects from hierarchy.csv
existing_projects = LOAD hierarchy.csv
    .filter(p => p.status != "deprecated")
    .filter(p => p.path != "./")  // Exclude master

dispatched_to = []
dispatch_failed = []

FOR each project in existing_projects:
    target_inbox = "{project.path}/_mailbox/inbox/"

    IF path_exists(target_inbox):
        COPY transmission TO target_inbox
        dispatched_to.push(project.id)
    ELSE:
        dispatch_failed.push({project: project.id, reason: "inbox not accessible"})

// Remove from outbox after successful dispatch
IF dispatched_to.length > 0:
    MOVE transmission FROM outbox TO sent/
```

### 4. Display Confirmation

```
╔══════════════════════════════════════════════════════════════╗
║                                                              ║
║   ✅ STRUCTURE HIÉRARCHIQUE CRÉÉE AVEC SUCCÈS               ║
║                                                              ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║   📊 Résumé de la création:                                  ║
║                                                              ║
║   Projets BMAD créés: {total_count}                          ║
║     • Projets intermédiaires: {intermediate_count}           ║
║     • Projets finaux (feuilles): {leaf_count}                ║
║                                                              ║
║   📁 Structure:                                              ║
{structure_tree_preview}
║                                                              ║
║   Chaque projet possède: _bmad/, _bmad-output/, _mailbox/    ║
║                                                              ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║   📤 Transmission hierarchy-update:                          ║
║                                                              ║
║   • ID: {transmission_id}                                    ║
║   • Destinataires: * (broadcast)                             ║
║   • Dispatchée vers: {dispatched_to.length} projet(s)        ║
{IF dispatch_failed.length > 0}
║   • ⚠️ Non dispatchée: {dispatch_failed.length} (non accessible) ║
{/IF}
║                                                              ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║   🎯 Prochaines étapes:                                      ║
║                                                              ║
║   TOUS les projets créés sont des projets BMAD complets.     ║
║   Pour chaque projet (intermédiaire ou final):               ║
║                                                              ║
║   1. cd {project_path}                                       ║
║   2. Lancer le workflow approprié:                           ║
║      • /create-product-brief  → Définir le produit           ║
║      • /workflow-init         → Commencer le workflow BMAD   ║
║                                                              ║
║   3. Gérer les communications:                               ║
║      • /transmit              → Envoyer un message           ║
║      • /check-inbox           → Consulter les messages reçus ║
║                                                              ║
║   💡 Les projets intermédiaires (ex: app/, app/mobile/)      ║
║      peuvent définir la stratégie globale qui sera héritée   ║
║      par leurs enfants. C'est l'avantage de l'entonnoir!     ║
║                                                              ║
╚══════════════════════════════════════════════════════════════╝
```

### 5. Return to Main Menu

```
Display:
"
[M] Retour au menu principal
[Q] Quitter le workflow
"

IF M: Return to {mainMenuReturn}
IF Q: End workflow
```

#### EXECUTION RULES:
- ALWAYS halt and wait for user input after presenting menu
- After other menu items execution, return to this menu

---

## SUCCESS METRICS

### ✅ SUCCESS:
- Transmission created with correct format
- Dispatched to all accessible projects
- Clear confirmation that ALL are full BMAD projects
- Next steps for ALL projects (not just leaves)
- User knows what to do

### ❌ FAILURE:
- Referring to any node as "container"
- Suggesting some nodes don't need workflow
- No transmission created
- Forgetting to dispatch
- Confusing confirmation
