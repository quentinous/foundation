---
name: 'step-01-transmit-init'
description: 'Select recipient, draft message, and dispatch transmission'

# File References
hierarchyData: '{project-root}/hierarchy.csv'
transmissionTemplate: '{project-root}/_bmad/multiproject/templates/transmission.md'
---

# Step 1: Transmit Initializer

## STEP GOAL:
Guider l'utilisateur dans la rédaction et l'envoi d'une transmission vers un projet cible ou en mode broadcast.

## MANDATORY EXECUTION RULES (READ FIRST):
### Universal Rules:
- 🛑 **NEVER** generate content without user input
- 📖 **CRITICAL:** Read the complete step file before taking any action
- 🔄 **CRITICAL:** When loading next step with 'C', ensure entire file is read
- 📋 YOU ARE A FACILITATOR, not a content generator

### Step-Specific Rules:
- 🎯 Focus only on transmission drafting and dispatch
- 🚫 **FORBIDDEN** to send to non-existent projects
- 💬 Approach: Efficient and precise communication

## EXECUTION PROTOCOLS:
- 🎯 Follow the MANDATORY SEQUENCE exactly
- 💾 Save transmission to outbox first
- 📖 Track dispatch status

## CONTEXT BOUNDARIES:
- Available context: {project-root}, hierarchy.csv
- Focus: Outgoing communication
- Limits: Inter-project scope

---

## MANDATORY SEQUENCE

### 1. Load Hierarchy
```
LOAD {hierarchyData}
LIST all active projects (project_id, path, tags)
```

### 2. Select Recipient
Demander à l'utilisateur de choisir le destinataire :
- Un projet spécifique par son `project_id`
- `*` pour un broadcast à tout l'écosystème

### 3. Draft Message
Demander à l'utilisateur :
- Le sujet de la transmission
- Le type (update, instruction, request, info)
- La priorité (low, medium, high, critical)
- Le corps du message

### 4. Generate Transmission
```
timestamp = current ISO datetime
suffix = random 8-char hex
filename = "TX_{user_name}_{date}_{suffix}.md"

CREATE {project-root}/_mailbox/outbox/{filename} from {transmissionTemplate}:
  - Populate frontmatter (from, to, type, priority, status: pending)
  - Populate body
```

### 5. Dispatch
```
IF to == "*":
    targets = all active projects in hierarchy (excluding self)
ELSE:
    targets = [selected project]

dispatched_to = []
FOR each project in targets:
    inbox_path = "{project.path}/_mailbox/inbox/"
    IF path_exists(inbox_path):
        COPY transmission TO inbox_path
        dispatched_to.push(project.id)

IF dispatched_to.length > 0:
    MOVE transmission FROM outbox TO {project-root}/_mailbox/sent/
```

### 6. Confirm
Afficher le résumé de l'envoi et les projets ayant reçu la transmission.

### 7. Present MENU OPTIONS
Display: "**Transmission terminée.** [M] Nouveau Message [Q] Quitter"

#### Menu Handling Logic:
- IF M: [Redisplay Menu Options](#7-present-menu-options) (Restart Step 1)
- IF Q: End workflow
- IF Any other: help user, then redisplay menu

#### EXECUTION RULES:
- ALWAYS halt and wait for user input after presenting menu
- ONLY proceed to next step (or end) when user selects an option
- After other menu items execution, return to this menu

## 🚨 SYSTEM SUCCESS/FAILURE METRICS:
### ✅ SUCCESS:
- Transmission créée dans l'outbox
- Fichier copié dans les inboxes cibles
- Fichier original déplacé vers sent/
### ❌ SYSTEM FAILURE:
- Envoi à un projet inexistant
- Fichier restant dans outbox après dispatch réussi
- Transmission mal formatée
