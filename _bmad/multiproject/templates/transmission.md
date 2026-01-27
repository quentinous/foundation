---
# ═══════════════════════════════════════════════════════════════
# BMAD Multi-Project - Document de Transmission
# ═══════════════════════════════════════════════════════════════
#
# Convention de nommage du fichier:
#   TX_{from}_{YYYY-MM-DD}_{HHhMM}_{suffix}.md
#
# Exemple:
#   TX_site-web_2026-01-26_14h30_a7f3b2c1.md
#
# ═══════════════════════════════════════════════════════════════

# Identifiant unique (suffix 8 chars hex, utilisé pour grep et liens)
transmission_id: "{{SUFFIX_8CHARS}}"

# Métadonnées temporelles
created: "{{YYYY-MM-DDTHH:MM:SSZ}}"

# Source et destination
from_project: "{{PROJECT_ID}}"
to_project: "{{TARGET}}"              # project-id | master | * | [proj1, proj2] | tag:xxx

# Classification
type: "{{TYPE}}"                      # Voir types ci-dessous
priority: "{{PRIORITY}}"              # critical | high | medium | low
status: pending                       # pending | acknowledged | in_progress | integrated | rejected

# Types disponibles:
#   info              - Information, pas d'action requise
#   ack               - Accusé de réception
#   dependency        - Nouvelle dépendance
#   bug               - Bug à corriger
#   hierarchy-update  - Changement structure projets (ajout/suppression)
#   project-update    - Changement status/branch d'un projet
#   architectural     - Décision architecturale
#   feature           - Nouvelle fonctionnalité
#   feature-request   - Demande de fonctionnalité (peut nécessiter brainstorm)
#   escalation        - Escalade d'une transmission non traitée
#   status-report     - Rapport de status automatique (enfant → master)

# Thread / Conversation (optionnel)
thread_id: null                       # ID du thread (= transmission_id du 1er message si nouveau)
parent_transmission: null             # Suffix de la transmission parente si réponse

# Contexte accumulé (optionnel, ajouté à chaque hop)
thread_context: []
#  - from: project-a
#    summary: "Description courte"
#  - from: project-b
#    summary: "Décision prise"

# Tags pour filtrage
tags: []
#  - tag1
#  - tag2
---

# Transmission: {{TITRE}}

## Contexte

[Pourquoi cette transmission est créée - quel est le déclencheur]

## Source de la détection

- **Workflow**: [Phase/étape où c'est détecté]
- **Document**: [Référence au document source si applicable]
- **Agent**: [Agent qui a détecté/proposé]

## Impact détecté

[Ce qui doit changer dans le projet destinataire]

## Données transmises

[Détails techniques, décisions, configurations, etc.]

## Actions suggérées

- [ ] Action 1
- [ ] Action 2
- [ ] ...

## Réponses et commentaires

<!-- Zone pour les réponses du destinataire avant archivage -->
