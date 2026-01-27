---
# ═══════════════════════════════════════════════════════════════
# BMAD Multi-Project - Status Report Transmission
# ═══════════════════════════════════════════════════════════════
# Envoyé automatiquement au master quand le workflow status change
# Type: status-report
# ═══════════════════════════════════════════════════════════════

transmission_id: "TX_{{project_id}}_{{date}}_{{suffix}}"
from_project: "{{project_id}}"
to_project: "{{parent_project}}"
type: status-report
priority: medium
status: pending
created_at: "{{timestamp}}"
auto_generated: true

# Données de status structurées
status_data:
  project_id: "{{project_id}}"
  project_name: "{{project_name}}"

  # Phase et progression
  current_phase: "{{phase}}"           # Discovery | Planning | Solutioning | Implementation | Validation
  phase_number: {{phase_num}}          # 1-5
  progress_percent: {{progress}}       # 0-100

  # Workflows
  total_workflows: {{total}}
  completed_workflows: {{completed}}
  skipped_workflows: {{skipped}}

  # Dernier changement
  last_completed: "{{last_workflow}}"
  next_workflow: "{{next_workflow}}"
  next_agent: "{{next_agent}}"

  # Status
  status: "{{status}}"                 # active | blocked | stale | completed
  blockers: []
  # - "Waiting for API spec from frontend"
  # - "Database migration pending"

  # Métadonnées
  last_activity: "{{last_activity}}"
  workflow_path: "{{workflow_path}}"

tags:
  - status-report
  - auto-generated
---

# 📊 Status Report: {{project_name}}

## Résumé

| Métrique | Valeur |
|----------|--------|
| Phase | {{phase}} ({{phase_num}}/5) |
| Progression | {{progress}}% |
| Workflows | {{completed}}/{{total}} complétés |
| Status | {{status_badge}} |

## Dernier changement

**Workflow complété:** {{last_workflow}}

**Prochain:** {{next_workflow}} (Agent: {{next_agent}})

## Progression détaillée

```
{{progress_bar_detailed}}
```

{{#if blockers}}
## ⚠️ Blocages

{{#each blockers}}
- {{this}}
{{/each}}
{{/if}}

---
*Rapport généré automatiquement par workflow-status*
