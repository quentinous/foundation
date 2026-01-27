# Multi-Project Extension for workflow-status

<critical>Ce module est chargé conditionnellement si hierarchy.csv existe à la racine du projet</critical>
<critical>Il s'exécute AU DÉBUT (avant step 1) et À LA FIN (après step 4) du workflow-status</critical>

---

## PHASE 1: Inbox Check (début du workflow)

<step n="mp-1" goal="Detect Multi-Project context">
  <action>Check if file "hierarchy.csv" exists at {project-root}</action>

  <check if="hierarchy.csv NOT found">
    <action>Set multi_project_mode = false</action>
    <action>Skip to standard workflow (return to caller)</action>
  </check>

  <check if="hierarchy.csv found">
    <action>Set multi_project_mode = true</action>
    <action>Load hierarchy.csv</action>
    <action>Load _mailbox/.mailbox-config.yaml</action>
    <action>Identify current project (path="./" in hierarchy)</action>
    <action>Continue to mp-2</action>
  </check>
</step>

<step n="mp-2" goal="Check project status">
  <action>Get current project status from hierarchy.csv</action>

  <check if="status == deprecated">
    <output>
╔══════════════════════════════════════════════════════════════╗
║ ⚠️  CE PROJET EST DÉPRÉCIÉ                                   ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║ Voir deprecated.md pour plus d'informations.                 ║
║ Aucune modification ne devrait être effectuée.               ║
║                                                              ║
║ [C] Continuer quand même  [Q] Quitter                        ║
╚══════════════════════════════════════════════════════════════╝
    </output>
    <ask>Votre choix:</ask>
    <check if="response == Q">
      <action>Exit workflow</action>
    </check>
  </check>
</step>

<step n="mp-3" goal="Scan inbox">
  <action>List all files in _mailbox/inbox/*.md</action>

  <check if="inbox is empty">
    <output>📬 Aucune transmission en attente.</output>
    <action>Skip to standard workflow (return to caller)</action>
  </check>

  <check if="inbox has files">
    <action>For each file, parse YAML frontmatter</action>
    <action>Extract: type, priority, from_project, thread_id, title</action>
    <action>Sort by priority: critical > high > medium > low</action>
    <action>Continue to mp-4</action>
  </check>
</step>

<step n="mp-4" goal="Apply triage rules">
  <action>Load triage rules from .mailbox-config.yaml</action>
  <action>Initialize: auto_handled = [], require_attention = []</action>

  <action>For each transmission:</action>

  <!-- Auto-handle rules -->
  <check if="type == info">
    <action>Move to archive/{YYYY-MM}/</action>
    <action>Add to auto_handled with action "archived"</action>
  </check>

  <check if="type == ack">
    <action>Update tracking if parent_transmission exists</action>
    <action>Move to archive/{YYYY-MM}/</action>
    <action>Add to auto_handled with action "tracking updated + archived"</action>
  </check>

  <check if="type == hierarchy-update OR type == project-update">
    <action>Parse new hierarchy data from transmission</action>
    <action>Update local hierarchy.csv</action>
    <action>Move to archive/{YYYY-MM}/</action>
    <action>Add to auto_handled with action "hierarchy updated"</action>
  </check>

  <check if="type == dependency">
    <action>Add to backlog/todo</action>
    <action>Move to archive/{YYYY-MM}/</action>
    <action>Add to auto_handled with action "added to backlog"</action>
  </check>

  <check if="type == bug">
    <action>Note: suggest create-story workflow</action>
    <action>Add to require_attention with suggestion "workflow_create_story"</action>
  </check>

  <check if="type == escalation">
    <action>Add to require_attention with suggestion "notify_and_prioritize"</action>
    <action>Keep in inbox (do not archive)</action>
  </check>

  <!-- Require attention rules -->
  <check if="type == architectural">
    <action>Add to require_attention with suggestion "propose_review"</action>
  </check>

  <check if="type == feature-request AND (priority == critical OR priority == high)">
    <action>Add to require_attention with suggestion "propose_brainstorm"</action>
  </check>

  <check if="type == feature-request AND priority == medium OR priority == low">
    <action>Add to require_attention with suggestion "add_to_backlog"</action>
  </check>

  <!-- Default: unknown types require attention -->
  <check if="type not matched above">
    <action>Add to require_attention with suggestion "manual_review"</action>
  </check>
</step>

<step n="mp-5" goal="Display transmissions">
  <output>
╔══════════════════════════════════════════════════════════════╗
║ 📬 TRANSMISSIONS MULTI-PROJECT                               ║
╠══════════════════════════════════════════════════════════════╣
{{#if auto_handled.length > 0}}
║                                                              ║
║ ✅ Auto-traité ({{auto_handled.length}}):                    ║
{{#each auto_handled}}
║   • {{filename}} [{{type}}] → {{action}}                     ║
{{/each}}
{{/if}}
{{#if require_attention.length > 0}}
║                                                              ║
║ 🔴 Attention requise ({{require_attention.length}}):         ║
{{#each require_attention indexed}}
║                                                              ║
║ [{{index}}] {{filename}}                                     ║
║     From: {{from_project}} | Type: {{type}} | Priority: {{priority}}
║     "{{title}}"                                              ║
║     → Suggestion: {{suggestion}}                             ║
{{/each}}
║                                                              ║
║ Actions:                                                     ║
║ [numéro] Traiter  [A] Toutes  [S] Skip  [D] Détails          ║
{{/if}}
╚══════════════════════════════════════════════════════════════╝
  </output>

  <check if="require_attention.length == 0">
    <action>Return to standard workflow</action>
  </check>

  <ask>Votre choix:</ask>

  <check if="response is number">
    <action>Go to mp-6 with selected transmission</action>
  </check>

  <check if="response == A">
    <action>Execute suggested action for each transmission</action>
    <action>Return to standard workflow</action>
  </check>

  <check if="response == S">
    <action>Return to standard workflow</action>
  </check>

  <check if="response == D">
    <ask>Numéro de la transmission:</ask>
    <action>Display full content of selected transmission</action>
    <action>Return to mp-5</action>
  </check>
</step>

<step n="mp-6" goal="Handle single transmission">
  <action>Display full transmission details</action>

  <output>
╔══════════════════════════════════════════════════════════════╗
║ 📄 {{filename}}                                              ║
╠══════════════════════════════════════════════════════════════╣
║ From: {{from_project}}                                       ║
║ Type: {{type}} | Priority: {{priority}}                      ║
║ Thread: {{thread_id or "N/A"}}                               ║
╠══════════════════════════════════════════════════════════════╣
║ {{content truncated}}                                        ║
╠══════════════════════════════════════════════════════════════╣
║ Actions:                                                     ║
║ [B] Brainstorming  [R] Research  [C] Create Story            ║
║ [W] Correct-course [A] Archiver  [X] Rejeter  [S] Skip       ║
╚══════════════════════════════════════════════════════════════╝
  </output>

  <ask>Votre choix:</ask>

  <check if="response == B">
    <action>Launch brainstorming workflow with transmission context</action>
    <action>Move transmission to archive</action>
  </check>

  <check if="response == R">
    <action>Launch research workflow with transmission context</action>
    <action>Move transmission to archive</action>
  </check>

  <check if="response == C">
    <action>Launch create-story workflow with transmission context</action>
    <action>Move transmission to archive</action>
  </check>

  <check if="response == W">
    <action>Launch correct-course workflow with transmission context</action>
    <action>Move transmission to archive</action>
  </check>

  <check if="response == A">
    <action>Update transmission status to "integrated"</action>
    <action>Move transmission to archive</action>
  </check>

  <check if="response == X">
    <ask>Raison du rejet:</ask>
    <action>Create rejection transmission in outbox</action>
    <action>Update original transmission status to "rejected"</action>
    <action>Move transmission to archive</action>
  </check>

  <check if="response == S">
    <action>Return to mp-5</action>
  </check>
</step>

---

## PHASE 2: Outbox Dispatch (fin du workflow)

<step n="mp-dispatch" goal="Dispatch outbox transmissions">
  <check if="multi_project_mode == false">
    <action>Skip this step</action>
  </check>

  <action>List all files in _mailbox/outbox/*.md</action>

  <check if="outbox is empty">
    <action>Skip - nothing to dispatch</action>
  </check>

  <check if="outbox has files">
    <output>📤 {{count}} transmission(s) en attente de dispatch</output>

    <action>For each transmission in outbox:</action>

    <action>Parse to_project from frontmatter</action>

    <!-- Resolve destinations -->
    <check if="to_project == '*'">
      <action>destinations = all non-deprecated projects from hierarchy.csv</action>
    </check>

    <check if="to_project == 'children'">
      <action>destinations = direct children of current project</action>
    </check>

    <check if="to_project == 'siblings'">
      <action>destinations = projects with same parent</action>
    </check>

    <check if="to_project starts with 'tag:'">
      <action>destinations = projects with matching tag</action>
    </check>

    <check if="to_project is list">
      <action>destinations = specified project IDs</action>
    </check>

    <check if="to_project is single project">
      <action>destinations = [to_project]</action>
    </check>

    <!-- Dispatch to each destination -->
    <action>Initialize: dispatched = [], not_accessible = []</action>

    <action>For each destination:</action>
    <action>Get path from hierarchy.csv</action>

    <check if="destination.status == 'deprecated'">
      <action>Skip this destination</action>
    </check>

    <check if="path exists on filesystem">
      <action>Copy transmission to {path}/_mailbox/inbox/</action>
      <action>Add to dispatched list</action>
    </check>

    <check if="path does NOT exist">
      <action>Add to not_accessible list</action>
    </check>

    <!-- After processing all destinations -->
    <check if="dispatched.length > 0">
      <action>Move transmission to _mailbox/sent/</action>
      <output>✅ Dispatché vers: {{dispatched}}</output>
    </check>

    <check if="not_accessible.length > 0">
      <output>⚠️ Non accessible (sync externe nécessaire): {{not_accessible}}</output>
    </check>
  </check>
</step>

---

## Integration Hook

Pour intégrer ce module dans workflow-status/instructions.md, ajouter:

**Au début (après step 0, avant step 1):**
```xml
<check if="file exists: {project-root}/hierarchy.csv">
  <action>Load and execute: multiproject-extension.md (PHASE 1: steps mp-1 to mp-6)</action>
</check>
```

**À la fin (après step 4):**
```xml
<check if="multi_project_mode == true">
  <action>Execute: multiproject-extension.md (PHASE 2: step mp-dispatch)</action>
</check>
```
