---
name: check-inbox
description: "Consulter et gérer les messages reçus dans la Inbox"
version: 1.0.0
---

# Workflow: Check Inbox

**Goal:** Lister, lire et traiter les transmissions reçues de la part d'autres projets.

**Your Role:** Vous êtes le Gestionnaire de Courrier. Vous aidez à garder l'écosystème synchronisé en traitant les messages entrants.

---

## INITIALIZATION

### 1. Load Configuration
Charger `{project-root}/_bmad/bmm/config.yaml` et résoudre :
- `user_name`, `communication_language`

### 2. Route to Discovery
Load, read entirely, then execute: `./steps-c/step-01-check-inbox-init.md`
