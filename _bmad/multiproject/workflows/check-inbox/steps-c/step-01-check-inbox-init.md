---
name: 'step-01-check-inbox-init'
description: 'List and read transmissions in the inbox'

# File References
inboxDir: '{project-root}/_mailbox/inbox/'
archiveDir: '{project-root}/_mailbox/archive/'
---

# Step 1: Inbox Manager

## STEP GOAL:
Afficher les messages en attente dans la Inbox et permettre leur traitement (lecture, archivage).

## MANDATORY EXECUTION RULES (READ FIRST):
### Universal Rules:
- 🛑 **NEVER** generate content without user input
- 📖 **CRITICAL:** Read the complete step file before taking any action
- 🔄 **CRITICAL:** When loading next step with 'C', ensure entire file is read
- 📋 YOU ARE A FACILITATOR, not a content generator

### Step-Specific Rules:
- 🎯 Focus only on incoming message management
- 🚫 **FORBIDDEN** to delete messages without archiving
- 💬 Approach: Structured review of pending items

## EXECUTION PROTOCOLS:
- 🎯 Follow the MANDATORY SEQUENCE exactly
- 💾 Move files to archive/ after reading/treatment
- 📖 Maintain a log of processed items

## CONTEXT BOUNDARIES:
- Available context: {inboxDir}
- Focus: Incoming communication
- Limits: Current project scope

---

## MANDATORY SEQUENCE

### 1. Scan Inbox
```
LIST all .md files in {inboxDir}
COUNT pending transmissions
```

### 2. Present Inbox Summary
Afficher la liste des messages trouvés avec :
- Nom du fichier
- Date de réception (si possible via métadonnées)
- Expéditeur (extrait du frontmatter)

Si vide :
Display: "📬 Votre Inbox est vide."
GOTO Step 5 (Present MENU OPTIONS)

### 3. Read Transmission
Demander à l'utilisateur quel message lire (numéro ou nom).
```
LOAD selected file
DISPLAY content (frontmatter + body)
```

### 4. User Action
Demander à l'utilisateur :
- **[A] Archiver** : Déplacer le fichier vers {archiveDir}
- **[R] Répondre** : (Suggérer de lancer le workflow `/transmit`)
- **[K] Garder** : Laisser dans la Inbox pour plus tard

### 5. Present MENU OPTIONS
Display: "**Gestion Inbox.** [L] Rafraîchir la liste [Q] Quitter"

#### Menu Handling Logic:
- IF L: [Redisplay Menu Options](#5-present-menu-options) (Restart Step 1)
- IF Q: End workflow
- IF Any other: help user, then redisplay menu

#### EXECUTION RULES:
- ALWAYS halt and wait for user input after presenting menu
- ONLY proceed to next step (or end) when user selects an option
- After other menu items execution, return to this menu

## 🚨 SYSTEM SUCCESS/FAILURE METRICS:
### ✅ SUCCESS:
- Messages listés correctement
- Contenu affiché lisiblement
- Archivage réussi (déplacement physique)
### ❌ SYSTEM FAILURE:
- Message perdu lors du traitement
- Inbox illisible
