---
stepsCompleted:
  - step-01-init
  - step-02-discovery
  - step-03-success
  - step-04-journeys
  - step-05-domain
  - step-06-innovation
  - step-07-project-type
  - step-08-scoping
  - step-09-functional
  - step-10-nonfunctional
  - step-11-separation
  - step-12-complete
step06InnovationInsights:
  partyModeRound: 1
  proposals: 5
  accepted: 4
  rejected: 1
  acceptedProposals:
    - "Add 'Verifiable Trust' as 5th innovation axis — cryptographic proof accessible to non-technical users (Mary/Analyst)"
    - "Add Sprint 0 validation metric: governance overhead < 40% threshold (Winston/Architect)"
    - "Add 5th innovation risk: AI quality plateau — agent rework rate > 30% = model adjustment needed (Murat/TEA)"
    - "UX paradigm 'show don't tell' — in-app integrity verification, public verification page, tuttle verify CLI (Sally/UX + Amelia/Dev)"
  rejectedProposals:
    - "Replace 'sovereign' with 'verifiable' in commercial positioning — Quentin keeps both terms, Verifiable Trust is additive not substitutive (John/PM)"
step07ProjectTypeInsights:
  architecturalDecision: "Vision B — bmad-multi-project module is the technical solution, tuttle-master is pure configuration"
  keyDecisions:
    - "tuttle-master produces zero executable code — configuration, contracts, governance only"
    - "bmad-multi-project module provides all inter-project orchestration (mailbox, hierarchy, contracts, isolation, triage, escalation)"
    - "Module gaps identified: contract management, git policy enforcement, stack governance, CI templates, metrics export, git hooks→agents"
    - "Module gaps become requirements for module PRD, NOT tuttle-master implementation tasks"
    - "tuttle-master contains children as git submodules (recursive clone, GitHub fallback)"
    - "Technology governance: children propose → master evaluates → human validates exact versions → sync"
    - "Contracts are markdown descriptive (not machine-readable) for V1"
    - "Ecosystem monitoring via Grafana on existing K8s cluster"
    - "Git hooks as evolution path for automated agent triggering"
  moduleGaps:
    - "Contract management (create, validate, sync)"
    - "Git policy enforcement (branches, PR rules, signing)"
    - "Stack governance (propose → validate → sync)"
    - "CI/CD template management"
    - "Version compatibility matrix"
    - "Git hooks → trigger agent workflows"
    - "Metrics export (JSON/YAML for Prometheus scraping)"
  sequencePlan:
    - "1. Finish tuttle-master PRD (current — documents configuration needs)"
    - "2. Create bmad-multi-project module PRD (gaps become requirements)"
    - "3. Develop/extend module via bmb (module builder)"
    - "4. Configure tuttle-master (pure config once module ready)"
inputDocuments:
  - product-brief-tuttle-master-2026-01-27.md
  - analysis/brainstorming-session-2026-01-30.md
workflowType: 'prd'
documentCounts:
  briefs: 1
  research: 0
  brainstorming: 1
  projectDocs: 0
date: 2026-01-29
author: Quentin
status: completed
classification:
  projectType: platform_orchestrator
  domain: sovereign_infrastructure
  complexity: high
  projectContext: greenfield
  userFacingValue: family_protection
  uxPhilosophy: invisible_complexity
  communicationModel: bmad_mailbox
  orchestrationPattern: master_child_transmissions
partyModeInsights:
  scope: 7 projets V1 critiques uniquement
  structure:
    - Vision (1 page)
    - User Journey Christophe (1 page)
    - Projets V1 Critical (infra, libs, store, vpn, provisioner, apps)
    - Dépendances & Séquence (DAG)
    - Inter-Project Communication (mailbox)
    - Test Gates
    - Out of Scope (V1.5, V2)
  criticalTransmissions:
    sprint0:
      - "architectural: infra-ready (infra → master)"
    sprint1:
      - "dependency: payment-webhook-spec (provisioner → store)"
      - "dependency: vpn-user-api-spec (provisioner → vpn)"
    sprint2:
      - "feature-request: config-import (apps → provisioner)"
architectureDecisionRecords:
  - id: ADR-001
    title: "Project Type — Platform Orchestrator"
    status: accepted
    decision: "Classifier comme platform_orchestrator plutôt que saas_b2b"
    tradeoffs: "✅ Reflète la réalité / ⚠️ Type custom non standard"
  - id: ADR-002
    title: "Communication Model — BMAD Mailbox"
    status: accepted
    decision: "Utiliser le système _mailbox/ avec transmissions asynchrones qui DÉCLENCHENT des mises à jour de PRD"
    tradeoffs: "✅ Découplage total, traçabilité, PRDs vivants / ⚠️ Latence (24h-7j SLA)"
  - id: ADR-003
    title: "Orchestration Pattern — Master-Child Transmissions"
    status: accepted
    decision: "Communication hub-and-spoke, enfants ne communiquent pas directement"
    tradeoffs: "✅ Clarté, single source of truth / ⚠️ Master = bottleneck potentiel"
  - id: ADR-004
    title: "Scope V1 — 7 Projets Critiques"
    status: accepted
    decision: "PRD V1 couvre: infra, libs, store, vpn, provisioner, apps/android, apps/windows"
    tradeoffs: "✅ Focus, livrable / ⚠️ V1.5/V2 non spécifiés en détail"
  - id: ADR-005
    title: "DAG de Dépendances Explicite"
    status: accepted
    decision: "Documenter un Directed Acyclic Graph avec dépendances explicites"
    tradeoffs: "✅ Clarté, planification / ⚠️ Rigidité si besoins changent"
  - id: ADR-006
    title: "Test Gates par Sprint"
    status: accepted
    decision: "Définir des critères go/no-go explicites par sprint"
    tradeoffs: "✅ Qualité, early feedback / ⚠️ Overhead processus"
  - id: ADR-007
    title: "User Facing Value — Family Protection"
    status: accepted
    decision: "Chaque décision technique traçable à Christophe protège sa famille"
    tradeoffs: "✅ Cohérence, north star / ⚠️ Peut limiter features geek"
  - id: ADR-008
    title: "UX Philosophy — Invisible Complexity"
    status: accepted
    decision: "Toute complexité orchestrateur invisible pour utilisateur final"
    tradeoffs: "✅ Adoption / ⚠️ Debugging utilisateur difficile"
  - id: ADR-009
    title: "Rollback Strategy — Independent Deployments"
    status: proposed
    decision: "Rollback par projet, pas de rollback écosystème global"
    tradeoffs: "✅ Isolation des pannes / ⚠️ Incompatibilités temporaires possibles"
  - id: ADR-010
    title: "Living PRDs — Transmissions as Update Triggers"
    status: accepted
    decision: "Les transmissions ne sont pas informatives mais DÉCLENCHENT des mises à jour de PRD via workflow correct-course"
    tradeoffs: "✅ PRDs toujours à jour, traçabilité des changements / ⚠️ Overhead processus, discipline requise"
  - id: ADR-011
    title: "PRD Separation — Module Capabilities vs Master Configuration & Obligations"
    status: accepted
    decision: "Séparer les FRs en 2 catégories : MODULE (bmad-multi-project, 30 FRs → PRD module via bmb) et MASTER (config Tuttle + obligations écosystème, 28 FRs → ce PRD). Les FRs infra (PKI, Gitea, etc.) restent dans master comme obligations — l'architecte et le flux de transmissions BMAD détermineront naturellement quel projet les implémente."
    tradeoffs: "✅ Séparation module/master claire, pas de décision architecturale prématurée sur l'infra / ⚠️ Nécessite PRD module avant implémentation"
    context: "Décision émergée lors du polish Step 11. Correction : la catégorie infra a été retirée car attribuer des FRs à tuttle-infra serait prématuré — c'est le rôle de l'architecte et du flux BMAD."
    moduleFRs: "FR2,FR3,FR5,FR6-FR13,FR15-FR17,FR19-FR22,FR26-FR29,FR31-FR33,FR50-FR53,FR56,FR58,FR52"
    masterFRs: "FR1,FR4,FR14,FR18,FR23-FR25,FR30,FR34-FR49,FR54-FR55,FR57,FR59"
warRoomInsights:
  decisions:
    - id: WR-001
      title: "PRD master = contrats d'interface, pas specs"
      owner: John (PM)
    - id: WR-002
      title: "PRD enfants = autonomie dans l'implémentation"
      owner: Winston (Architect)
    - id: WR-003
      title: "UX Guidelines obligatoires dans PRD master"
      owner: Sally (UX)
    - id: WR-004
      title: "Glossaire unifié dans PRD master + tuttle-libs"
      owner: Sally (UX)
    - id: WR-005
      title: "Contract tests dans PRD master cassent le build"
      owner: Murat (TEA)
    - id: WR-006
      title: "Transmissions = déclencheurs de mise à jour PRD (pas juste runtime)"
      owner: Quentin (correction)
  transmissionFlow:
    description: "Flux de mise à jour PRD via transmissions mailbox"
    steps:
      - step: 1
        action: "Projet A détecte changement significatif"
      - step: 2
        action: "Projet A crée transmission (type: dependency|architectural|feature)"
      - step: 3
        action: "Transmission envoyée vers projet(s) impacté(s) ET master"
      - step: 4
        action: "Projet impacté reçoit dans inbox, triage automatique"
      - step: 5
        action: "Workflow correct-course proposé/exécuté"
      - step: 6
        action: "PRD du projet impacté MIS À JOUR"
      - step: 7
        action: "Si impact cross-projet, PRD master aussi mis à jour"
      - step: 8
        action: "Transmission archivée avec status: integrated"
    principle: "La transmission EST le déclencheur de mise à jour documentaire. Les PRDs sont vivants."
failureModeAnalysis:
  criticalFailures:
    - id: FM-001
      component: "Mailbox System"
      topRisk: "Inbox non scannée > 48h"
      mitigation: "Alerte automatique, workflow-status 1x/jour minimum"
    - id: FM-002
      component: "Transmission Flow"
      topRisk: "Destination non accessible (submodule pas cloné)"
      mitigation: "Notification immédiate si dispatch échoue + retry 3x"
    - id: FM-003
      component: "PRD Sync"
      topRisk: "PRD master outdated, enfants ont features non documentées"
      mitigation: "Sync check hebdomadaire + transmission architectural DOIT inclure master en CC"
    - id: FM-004
      component: "DAG Dependencies"
      topRisk: "Dépendance implicite non documentée"
      mitigation: "Audit dépendances + validation DAG à chaque ajout"
    - id: FM-005
      component: "Human Process"
      topRisk: "Changement breaking sans transmission (oubli)"
      mitigation: "CI hook bloque merge sans TX si changement dans api/ ou contracts/"
    - id: FM-006
      component: "Infrastructure"
      topRisk: "SOPS key perdue ou Flux désynchronisé"
      mitigation: "Backup keys multiples + Flux health check + alerting"
  nfrResilience:
    - id: NFR-RESILIENCE-001
      requirement: "Inbox scannée < 48h sur chaque projet actif"
    - id: NFR-RESILIENCE-002
      requirement: "Transmission dispatch retry 3x avant notification échec"
    - id: NFR-RESILIENCE-003
      requirement: "PRD sync validation hebdomadaire automatisée"
    - id: NFR-RESILIENCE-004
      requirement: "CI hook bloque merge sans transmission si changement API/contracts"
    - id: NFR-RESILIENCE-005
      requirement: "DAG cycle detection automatique à chaque modification hierarchy.csv"
  mitigationPriority:
    P0:
      - "FM-005.3 Oubli transmission → CI hook"
      - "FM-004.1 Cycle DAG → Validation automatique"
    P1:
      - "FM-003.3 PRD master outdated → Sync check hebdo"
      - "FM-001.1 Inbox non scannée → Alerte > 48h"
    P2:
      - "FM-002.5 Destination non accessible → Retry + notification"
      - "FM-003.1 PRD diverge → Contract tests CI"
premortemAnalysis:
  scenario: "6 mois après le lancement, tuttle-master a échoué. Pourquoi ?"
  failureScenarios:
    - id: PM-001
      name: "PRD Fantôme"
      story: "Le PRD master existe mais personne ne le lit. Les projets enfants développent en autonomie totale, le PRD master devient un document mort."
      rootCause: "Aucun mécanisme de feedback loop entre PRD et implémentation réelle"
      probability: high
      impact: critical
      mitigation: "Contract tests CI qui valident PRD ↔ implémentation. Transmission obligatoire si divergence détectée. Review PRD mensuelle."
    - id: PM-002
      name: "One-Man Bottleneck"
      story: "Quentin est le seul à comprendre le système master. Maladie, vacances, burnout = écosystème paralysé. Aucun projet enfant ne sait comment créer/traiter une transmission."
      rootCause: "Knowledge silos, documentation insuffisante pour onboarding"
      probability: high
      impact: critical
      mitigation: "Runbook opérationnel détaillé. Glossaire unifié (WR-004). Chaque workflow documenté avec exemples concrets. Au moins 1 personne backup formée."
    - id: PM-003
      name: "Tour de Babel"
      story: "Chaque projet enfant utilise des conventions différentes (nommage API, formats de données, patterns d'erreur). L'intégration est un cauchemar. Les contract tests échouent constamment."
      rootCause: "tuttle-libs pas assez contraignant, conventions non enforced"
      probability: medium
      impact: high
      mitigation: "tuttle-libs impose SDK + conventions. Linter CI vérifie conformité. PRD master définit conventions obligatoires, pas optionnelles."
    - id: PM-004
      name: "Tunnel Qui Fuit"
      story: "Le VPN fonctionne mais fuit des métadonnées DNS, WebRTC. La famille de Christophe pense être protégée mais ne l'est pas. Faux sentiment de sécurité pire que pas de VPN."
      rootCause: "Tests de sécurité insuffisants, focus fonctionnel plutôt que sécuritaire"
      probability: medium
      impact: critical
      mitigation: "Suite de tests leak obligatoire (DNS, WebRTC, IPv6). Audit sécurité externe avant V1 launch. Kill switch testé automatiquement."
    - id: PM-005
      name: "Concurrent Surprise"
      story: "Un concurrent lance un produit similaire pendant le développement. La famille cible (non-tech) choisit la solution plus simple/connue. Tuttle arrive trop tard ou trop complexe."
      rootCause: "Cycle de développement trop long, pas de validation marché intermédiaire"
      probability: low
      impact: high
      mitigation: "MVP store + VPN en priorité (Sprint 0-2). Beta fermée famille élargie dès Sprint 2. Feedback loop rapide. UX invisible complexity (ADR-008)."
  nfrPremortem:
    - id: NFR-PM-001
      requirement: "Contract tests PRD ↔ implémentation exécutés en CI sur chaque merge"
      tracesTo: PM-001
    - id: NFR-PM-002
      requirement: "Runbook opérationnel complet pour workflow master accessible sans Quentin"
      tracesTo: PM-002
    - id: NFR-PM-003
      requirement: "Linter CI vérifie conformité conventions tuttle-libs sur chaque projet"
      tracesTo: PM-003
    - id: NFR-PM-004
      requirement: "Suite de tests leak (DNS, WebRTC, IPv6) obligatoire avant chaque release VPN"
      tracesTo: PM-004
    - id: NFR-PM-005
      requirement: "Beta fermée famille élargie déployée avant fin Sprint 2"
      tracesTo: PM-005
firstPrinciplesAnalysis:
  fundamentalTruths:
    - id: FP-001
      truth: "Le master est un système de gouvernance actif, pas un document passif"
      implication: "Produit des contrats et valide leur respect, pas des specs informatives"
    - id: FP-002
      truth: "La communication fichier .md est le seul modèle zero-infra compatible git"
      implication: "Pas de serveur, pas d'API, pas de DB pour la communication inter-projets"
    - id: FP-003
      truth: "7 projets V1 = minimum mathématique, pas estimation"
      implication: "infra→libs→store∥vpn→provisioner→apps est le chemin critique incompressible"
    - id: FP-004
      truth: "Tout output master est contrat ou déclencheur, jamais informatif-seulement"
      implication: "Chaque document master doit être vérifiable par CI ou déclencher un workflow"
    - id: FP-005
      truth: "Le succès se mesure à l'ignorance de l'utilisateur"
      implication: "Plus Christophe en sait peu sur la complexité, plus le système a réussi"
    - id: FP-006
      truth: "Le DAG ne peut pas être compressé davantage (store∥vpn déjà parallèles)"
      implication: "Toute tentative de raccourcir le planning doit passer par accélération, pas suppression"
  designPrinciple: "Chaque output du master est soit un CONTRAT (vérifiable par CI), soit un DÉCLENCHEUR (transmission). Rien n'est informatif-seulement."
  nfrFirstPrinciples:
    - id: NFR-FP-001
      requirement: "Tout output du master DOIT être soit un contrat vérifiable par CI, soit un déclencheur de workflow"
      tracesTo: FP-004
    - id: NFR-FP-002
      requirement: "Communication inter-projets DOIT fonctionner avec zero infrastructure au-delà de git"
      tracesTo: FP-002
    - id: NFR-FP-003
      requirement: "DAG de dépendances DOIT être validé comme chemin critique minimum : aucun nœud retirable sans casser la chaîne"
      tracesTo: "FP-003, FP-006"
    - id: NFR-FP-004
      requirement: "Succès UX mesuré inversement : concepts techniques exposés à l'utilisateur final DOIT tendre vers zéro"
      tracesTo: FP-005
    - id: NFR-FP-005
      requirement: "Le master DOIT fonctionner comme système de gouvernance même sans code exécutable dans son repo"
      tracesTo: FP-001
step03PartyModeInsights:
  rounds: 4
  totalProposalsAccepted: 51
  round1:
    focus: "Core success criteria review"
    proposals: 12
    highlights:
      - "Clone résilient avec fallback GitHub"
      - "Automatisation mesurée en % vs heures"
      - "Validation intégrité hierarchy.csv en CI"
      - "Health check hebdo vert/orange/rouge"
      - "Stratification critères par sprint gate (S0/S1/S2)"
  round2:
    focus: "Standards et infrastructure gaps"
    proposals: 13
    highlights:
      - "Infisical secrets management isolé par projet"
      - "Matrice environnements cross-projets"
      - "Template CI commun"
      - "Standards observabilité (Prometheus/Grafana)"
      - "Design system + accessibilité"
      - "Glossaire unifié (WR-004)"
      - "Runbook opérationnel testable par un tiers"
  round3:
    focus: "Security, compliance, DX"
    proposals: 12
    highlights:
      - "Supply chain security (scan + SBOM)"
      - "Privacy by Design CI check"
      - "Disaster Recovery RPO<24h RTO<4h"
      - "Network security mTLS / Network Policies"
      - "Feature flags cross-projets"
      - "Conformité légale multi-juridictionnelle"
      - "Accessibilité WCAG 2.1 AA"
  round4:
    focus: "PKI, signatures, chain of trust"
    proposals: 14
    userInput: "step-ca avec backend HSM load-balancé, tout doit être tracé, vérifié et signé"
    highlights:
      - "Hiérarchie PKI: Root CA (HSM offline) → step-ca (HSM LB) → Leaf certs"
      - "Git commit signing obligatoire (dev + agents BMAD)"
      - "Release signing: binaires, containers (cosign), packages"
      - "CI vérifie signatures + signe artefacts"
      - "Audit trail cryptographique non modifiable"
      - "Warrant canary signé Root CA"
      - "SBOM signé par release"
      - "Stratification PKI sur 3 sprints"
---

# Product Requirements Document - tuttle-master

**Author:** Quentin
**Date:** 2026-01-29

## Success Criteria

### User Success

The primary user of tuttle-master is the ecosystem operator (Quentin) and BMAD agents. Success is measured by how effectively the conductor orchestrates the entire ecosystem via the `@quentin/bmad-multi-project` module.

**Operator Success Metrics:**

| Metric | Target | Measurement |
|--------|--------|-------------|
| **Orchestration Clarity** | Any BMAD agent can determine its next action from master context alone | Agent resolves dependencies, reads transmissions, and executes workflows without operator intervention |
| **Transmission Turnaround** | Critical transmissions processed < 48h, standard < 7 days | Mailbox timestamps inbox → archive |
| **Context Switching Cost** | Operator switches between projects with zero context loss | All project state captured in living documents (PRD, Architecture, Epics, Sprint Status) accessible via `/bmad:bmm:workflows:workflow-status` per project and `/bmad:multiproject:workflows:ecosystem-status` for global view |
| **New Project Onboarding** | New child project fully operational in mailbox system after running `/bmad:multiproject:workflows:init-multi-project` | `npx bmad-method@alpha install` + `npx @quentin/bmad-multi-project@latest` + `_bmad/`, `_mailbox/`, `hierarchy.csv` created, git submodule added with Gitea remote + GitHub backup. Binary criterion: project sends and receives transmissions |
| **Single Source of Truth** | Zero conflicting specs across projects | `hierarchy.csv` master = source of truth, validated syntactically and semantically in CI on every push. Contract tests validate PRD ↔ implementation alignment on every merge |
| **Enforced Isolation** | Zero direct cross-project writes | All communication via mailbox system (outbox → inbox). Pre-commit hook verifies no modified file is outside `{project-root}` of current repo |
| **Dashboard Readability** | Identify most critical project and next action in < 10 seconds of reading | `/ecosystem-status` displays prioritized hierarchical view with alerts sorted by severity |
| **Contract Discoverability** | Quentin sees at a glance which contracts exist between which projects | Visual dependency graph (Excalidraw) maintained up to date, linked from ecosystem-status |

**Emotional Success — The "Conductor's Confidence":**
- Quentin never wonders "what's the current state of project X?" — `/ecosystem-status` answers instantly
- No project drifts silently — the escalation system notifies master if a transmission stays `pending` > 7 days or `critical` > 24h
- No agent guesses — every decision is traceable to an ADR, PRD section, or transmission archived in `_mailbox/archive/`
- Every hierarchy node is a complete, sovereign BMAD project — no empty "containers"

**Weekly Health Check — Confidence Pulse:**

| Signal | Condition | State |
|--------|-----------|-------|
| 🟢 Green | Zero 🔴 alerts + inbox < 5 transmissions per project + zero stale transmissions | Healthy ecosystem |
| 🟠 Orange | 1-2 🟡 alerts OR inbox 5-10 transmissions on a project OR 1 transmission > 48h | Attention required |
| 🔴 Red | Active 🔴 alert OR inbox > 10 transmissions OR critical transmission > 24h unhandled | Immediate intervention |

### Business Success

tuttle-master does not generate revenue directly. Its business success is the health of the ecosystem it orchestrates — all projects playing the same BMAD score with the bmad-multiproject module.

| Dimension | Success Indicator | Failure Signal |
|-----------|-------------------|----------------|
| **Coordination Efficiency** | Smooth mailbox exchanges: auto-triage (info→archive, bug→create story, dependency→backlog), critical transmissions processed < 48h, zero stale > escalation | Transmissions pile up in inbox, auto-triage fails, projects diverge |
| **Living Coordination** | Mean time between API change in child and contract update in master < 48h | Time > 2 weeks = Ghost PRD (PM-001) |
| **Living Documentation** | PRDs, Architecture, Epics, Sprint Status continuously updated via correct-course workflows triggered by `architectural` transmissions | Stale documents, implementation diverges from specs, no transmissions when APIs change |
| **Ecosystem Velocity** | Child projects work autonomously within master-defined contracts, each child has its own BMAD lifecycle | Projects blocked waiting for master, unclear interfaces, undocumented implicit dependencies |
| **Unified Method** | All projects use `npx bmad-method@alpha install` + `npx @quentin/bmad-multi-project@latest`, same BMAD score, same multi-project module | Projects with ad-hoc processes, inconsistent BMAD structure, no mailbox |
| **Governance as Code** | Every master output = verifiable contract (CI) or workflow trigger (transmission). CI hooks block merges without transmission if changes in `api/` or `contracts/` (FM-005) | Documents exist but nothing enforces them, breaking changes without transmission |
| **Git Infrastructure** | Each child = git submodule with Gitea remote (tea CLI + MCP Gitea) + GitHub backup (gh CLI). Recursive clone with GitHub fallback if Gitea unavailable | Repos out of sync, broken submodules, no backup, clone fails if one remote is down |
| **Operator Automation** | Percentage of automated vs manual master actions: M3 > 60%, M6 > 80%, M12 > 90% | Operator does everything manually, triage not automated, escalations undetected |

**Ecosystem Business Milestones:**

| Phase | Objective | Master's Role |
|-------|-----------|---------------|
| **M0 Pre-launch** | Beta validated, 10 early adopters | 7 V1 projects initialized via `/init-multi-project`, hierarchy.csv complete, critical contracts defined, mailbox functional |
| **M3 Validation** | 100 active subscribers | Smooth transmissions between all projects, auto-triage operational, living PRDs, reliable ecosystem-status dashboard |
| **M6 Scale** | 500 active subscribers, NPS > 30 | Governance scales without operator bottleneck, automatic escalation works, delegation to children activated |
| **M12 Traction** | 1,500 active subscribers | Master operates semi-autonomously, growth not founder-dependent, operational runbook tested by third party |

### Technical Success

#### Criteria Stratified by Sprint Gate

**Sprint 0 Gate (Foundation):**

| Criterion | Measure | Validation |
|-----------|---------|------------|
| **Hierarchy Initialized** | `hierarchy.csv` with 3+ projects (master + infra + libs minimum) | File exists and parsable |
| **Resilient Recursive Clone** | `git clone --recurse-submodules` with GitHub fallback if Gitea unavailable | Test primary checkout + fallback |
| **Dual Remote Configured** | Each project has Gitea remote (tea CLI + MCP Gitea) + GitHub remote (gh CLI) | `git remote -v` on each child |
| **Mailbox Initialized** | `_mailbox/` with inbox/outbox/sent/archive + `.mailbox-config.yaml` on each project | Structure checklist |
| **Complete BMAD Structure** | `npx bmad-method@alpha install` + `npx @quentin/bmad-multi-project@latest` executed on infra + libs | Checklist |
| **CI Submodule Health** | CI action validates complete clone on every push to main | Green pipeline |
| **hierarchy.csv CI Validation** | Syntactic (CSV parsable) + semantic (no DAG cycles, valid parent_ids, existing paths) validation on every push | CI check |
| **Unified Git Policy** | Branching strategy defined and applied identically across all projects. `main` (and `develop` if git flow) protected on Gitea AND GitHub: merge only via PR, review required | Gitea + GitHub config verified |
| **Branch Naming Convention** | Consistent cross-project naming (e.g., `feat/`, `fix/`, `chore/`, `hotfix/`) | Linter or CI check |
| **Submodule Sync Policy** | Explicit rule: when parent updates child submodule ref (on every main merge? on every release tag?) | Document in master |
| **Release/Tagging Policy** | SemVer versioning convention + consistent release process across projects + signed tags | Document + CI check |
| **PR/Review Policy** | Number of approvals, mandatory CI checks before merge, unified PR template | Gitea branch protection rules |
| **Unified Secrets Policy — Infisical** | Infisical as centralized secret manager. Each project has isolated Infisical environment (namespace per project). Secret names isolated per project — no implicit name sharing. Local `.env` with fake data for dev, Infisical injects real secrets in staging/prod. Zero cleartext secrets in any repo | Infisical config + CI scan (detect-secrets or equivalent) |
| **Cross-Project Environment Matrix** | 3 environments (dev, staging, prod) consistent cross-projects: dev↔dev, staging↔staging, prod↔prod. Master defines which instance of each project communicates with which instance of others. Infisical provides secrets per environment | Matrix document + Infisical per-env config |
| **Common CI Template** | Mandatory CI pipeline defined by master: lint → unit test → integration test → build → deploy staging. Each project adapts but respects mandatory stages | Template in master, validated by review |
| **Ecosystem Getting Started** | Master README with: recursive clone, local Infisical setup (`.env` fake data), launch of dependent components. Developer can have functional env for their project | Onboarding test |
| **Documentation Standards** | Mandatory structure per project: README.md, CONTRIBUTING.md, API docs (if applicable), changelog. CommonMark format, structure defined by master | CI documentation lint |
| **Unified Glossary (WR-004)** | Technical and user-facing terms glossary maintained in master + tuttle-libs, reference for all projects | Review + usage verified |
| **Supply Chain Security** | Automated dependency scanning in CI on every project (Trivy, Snyk or equivalent). SBOM generated per release. Zero critical vulnerability untreated > 7 days | CI scan + SBOM verified |
| **Ecosystem Disaster Recovery** | Each critical infrastructure component (Gitea, Infisical, K8s, PostgreSQL) has documented and tested recovery procedure. RPO < 24h, RTO < 4h for critical components | Recovery test |
| **Cross-Project Code Review for tuttle-libs** | Changes to tuttle-libs impacting public API require review from at least one consumer project | Gitea review rules config |
| **PKI Hierarchy Deployed** | Root CA offline (dedicated HSM, 5-10 year rotation) → Intermediate CA (step-ca, load-balanced HSM backend) → Leaf certificates per project/service/env. PKI policy documented by master | step-ca operational + doc |
| **Mandatory Git Commit Signing** | All projects: signed commits (GPG or SSH signing via step-ca). Gitea and GitHub reject unsigned commits on protected branches | Gitea/GitHub config verified |
| **Developer Signing Key Provisioning** | Automatic provisioning via step-ca in one command. Documented in getting started | Onboarding test |
| **BMAD Agent Cryptographic Identity** | Each BMAD agent has identity provisioned by step-ca. Agent commits signed and traceable to agent + session | Agent commit verified |

**Sprint 1 Gate (Core Product):**

| Criterion | Measure | Validation |
|-----------|---------|------------|
| **Active Transmissions** | Mailbox exchanges between 5+ projects (master, infra, libs, store, vpn, provisioner) | Transmissions sent and received in each inbox |
| **V1 Critical Contracts** | Store→Provisioner and Provisioner→VPN interfaces documented and enforced | Review + contract tests |
| **Auto-Triage Operational** | All 8 auto-handled types functional without intervention | Test each type |
| **Malformed Transmission Handling** | Transmissions with unknown type or malformed YAML handled gracefully with logging, no crash | Edge case tests |
| **Escalation Functional** | `pending` > 7d → master notification + `stale` tag. `critical` > 24h → auto escalation | Timeout tests |
| **Stale Transmission Alert** | Script detects outbox transmissions > 24h and alerts | Alert test |
| **CI Hook on API Changes** | Merge blocked without transmission if changes in `api/` or `contracts/` (FM-005) | Test blocked merge |
| **Pre-Commit Isolation Hook** | Verifies no modified file is outside `{project-root}` of current repo | Test cross-project write blocked |
| **Version Compatibility Matrix** | Inter-project version pinning mechanism. Each project pins specific tuttle-libs version and dependent service versions. No implicit `latest`. tuttle-libs breaking change = explicit `dependency` transmission | Config verified + transmission sent |
| **Cross-Project Integration Tests** | Each cross-project interface (Store→Provisioner, Provisioner→VPN) has automated integration test in CI on staging environment. Contract tests verify format, integration tests verify behavior | CI staging green |
| **Observability Standards** | Unified log format (structured JSON), metrics exported by each project (Prometheus), cross-project Grafana dashboards, centralized alerting for critical interfaces | Dashboard + alerts functional |
| **Design System + UX Enforced** | All user-facing projects (store, apps) implement tuttle-libs/ui design system and UX patterns defined by master. Consistent cross-project user error glossary | UX review |
| **Privacy by Design** | CI check that no project logs identifiable user data (PII). Regex scan on logging patterns. Zero PII detected | CI scan green |
| **Independent Rollback** | Each project can rollback independently without breaking cross-project interfaces. Version compatibility matrix identifies retro-compatible versions | Rollback test + matrix |
| **DB Migrations** | Versioned, reversible schema migrations, executed automatically on deploy. No destructive migration without verified prior backup | CI migration test |
| **Inter-Service Network Security** | Inter-service communication only via mTLS or K8s private network (Network Policies). No service publicly exposed except store frontend and VPN endpoints | Network policy audit |
| **API/Feature Deprecation Process** | Any API/feature deprecation impacting another project triggers `architectural` transmission with minimum 2-sprint migration delay. Master maintains active deprecated APIs registry | Registry + transmissions verified |
| **Cross-Project Feature Flags** | Feature flag strategy for cross-project features. MVP: config file propagated via transmission. Post-MVP: dedicated service. Flag activated in master → transmission to affected projects | Config + transmission |
| **Accessibility WCAG 2.1 AA** | WCAG 2.1 AA minimum standards for all user-facing projects. Automated accessibility tests in CI (axe-core or equivalent) | CI axe-core green |
| **Multi-Jurisdictional Legal Compliance** | Compliance matrix per jurisdiction (GDPR, ePrivacy, local VPN laws). Each user-facing project integrates legal obligations for target jurisdiction. Master maintains covered jurisdictions list and requirements | Legal matrix document |
| **mTLS Inter-Services via step-ca** | All inter-service communication uses mTLS with certificates issued by step-ca. Automatic provisioning (ACME), not manual | mTLS connection test |
| **Release Signing** | Every release artifact signed cryptographically: binaries (project key), containers (cosign/Notary v2), packages (platform-native signing). Signing keys stored in HSM | Signature verified |
| **CI Verifies + Signs** | CI pipeline verifies dependency signatures on input AND signs artifacts on output. Zero unsigned artifact passes to staging/prod | CI gate |
| **Rotation + Revocation Policy** | Automated rotation: 90d service certificates, annual release keys, 5-10 years Root CA. Emergency revocation process tested. step-ca publishes CRL/OCSP | Rotation + revocation test |
| **Local Signature Verification** | Verification command available for each artifact type (binary, container, package). Documented in getting started | Doc + test |

**Sprint 2 Gate (Clients — Full Validation):**

| Criterion | Measure | Validation |
|-----------|---------|------------|
| **9 Projects Initialized** | All with complete BMAD structure + mailbox + local hierarchy.csv | Structure checklist |
| **Inbox Autonomy** | Any child project can `git pull` inbox and receive instructions offline | Offline transmission test |
| **Hierarchy Sync** | All local `hierarchy.csv` files reflect actual state after each `hierarchy-update` | Validation script |
| **No Local Infra Leakage** | No project except tuttle-infra contains Terraform/Ansible | Structure lint |
| **Brownfield Migration** | tuttle-key has `_bmad/`, `_mailbox/`, `hierarchy.csv` | Migration checklist |
| **Woodpecker Pipelines** | Every project has functional `.woodpecker.yml` | Green CI |
| **Ecosystem-Status Operational** | Dashboard displays all projects with phase, progress, alerts, transmissions. Critical action identifiable in < 10s | Usability test |
| **Visual Dependency Graph** | Excalidraw diagram of inter-project contracts maintained up to date | Review |
| **Operational Runbook (PM-002)** | A third party can execute the runbook and resolve a simulated incident without Quentin's help. Covers: mailbox triage, escalation, transmission, ecosystem-status, recovery | External test |
| **Cryptographic Audit Trail** | Complete history: certificate issuance, release signatures, signed commits. Logs stored, non-modifiable, queryable | Audit log verified |
| **Public Release Verification** | Each release publishes signatures + checksums on verifiable page. Tuttle public keys accessible. Apps verify their own integrity on launch (self-integrity check) | Public verification test |
| **Signed Warrant Canary** | Signed by Root CA, published at regular intervals, verifiable by anyone with public key | External verification test |
| **Signed SBOM** | Signed SBOM published with each release. Technical user can verify: who compiled, which dependencies, which signature | SBOM + signature verified |

### Measurable Outcomes

| Outcome | Target | Timeframe |
|---------|--------|-----------|
| **Transmission Latency** | Inbox → integrated action < 48h for critical, < 7 days for standard | Continuous |
| **Coordination Latency** | Child API change → master contract update < 48h | Continuous |
| **Document Freshness** | PRD/Architecture ↔ implementation gap < 1 week | Continuous |
| **Project Alignment** | 100% of child projects respect master-defined principles | Every sprint gate |
| **Fail-Closed Compliance** | 100% of critical systems (Proxy, Clean Pipe, Auth) HALT on failure | Every release |
| **Contract Test Coverage** | Every cross-project interface has automated validation | Sprint 1+ |
| **Auto-Triage Rate** | M3 > 80%, M6 > 90%, M12 > 95% | Progressive |
| **Operator Automation** | Automated vs manual actions: M3 > 60%, M6 > 80%, M12 > 90% | Progressive |
| **Ecosystem-Status Health** | Zero persistent 🔴 critical alert > 3 days | Continuous |
| **Weekly Health Check** | 🟢 green = zero 🔴 + inbox < 5 + zero stale | Weekly |
| **Contract Challenge SLA** | When a child challenges a master contract via `architectural` transmission, master responds < 7 days | Continuous |
| **Breaking Change Coordination** | Every user-facing change coordinated via master transmission → impacted projects, unified user message defined in glossary | Continuous |

## Product Scope

### MVP — Minimum Viable Product

tuttle-master is an **orchestration project** that produces no executable code. Its MVP defines the vision, coordinates child projects, and establishes standards via the bmad-multiproject module.

**What master DEFINES:**

| Domain | Responsibility |
|--------|----------------|
| **Vision** | Mission, values, "State-Resistant" positioning |
| **Concepts** | LightWeb, Legislative Weather, Îlots, Clean Pipe |
| **Architecture** | Shared technical principles (Zero-Knowledge, RAM-only, etc.) |
| **Hierarchy** | `hierarchy.csv` — child project structure, dependencies, roadmap DAG. CI-validated (syntax + semantics + cycles) |
| **Standards** | Conventions, security, UX, documentation |
| **Governance** | Ethical rules, Consensus de Nettoyage, isolation rules (enforced by pre-commit hook) |
| **Contracts** | Cross-project interface specifications (Store→Provisioner, Provisioner→VPN) + visual Excalidraw graph |
| **Transmissions** | Mailbox system (`_mailbox/`) with typed transmissions, auto-triage, and graceful error handling |
| **Tooling** | `npx bmad-method@alpha install` + `npx @quentin/bmad-multi-project@latest` for each project |
| **Git Policy** | Branching strategy, protected branches, naming conventions, PR/review policy, release tagging, submodule sync — uniformly applied via Gitea branch protection + GitHub mirror |
| **Secrets Policy** | Infisical as centralized manager, namespace isolated per project, `.env` local with fake data for dev, secret injection per env (staging/prod), documented rotation |
| **PKI Ecosystem** | Root CA → step-ca (load-balanced HSM backend) → Leaf certs hierarchy. Issuance, rotation, revocation policies. mTLS inter-services |
| **Chain of Trust** | Mandatory git commit signing, release signing (binaries, containers, packages), CI verifies + signs, zero unsigned artifacts in staging/prod |
| **Cryptographic Traceability** | Non-modifiable audit trail, signed SBOM, public verification, signed warrant canary, app self-integrity check |
| **Cryptographic Identities** | Developers and BMAD agents provisioned via step-ca, release keys in HSM |
| **Supply Chain Security** | CI dependency scanning, SBOM per release, zero critical vulnerability > 7 days policy |
| **Privacy by Design** | Zero-Knowledge, RAM-only standards, zero PII in logs — CI-validated |
| **Disaster Recovery** | DR plan for Gitea, Infisical, K8s, PostgreSQL — RPO < 24h, RTO < 4h |
| **Network Security** | mTLS inter-services, K8s Network Policies, minimal public surface |
| **Observability** | Log standards (structured JSON), metrics (Prometheus), dashboards (Grafana), centralized alerting |
| **CI/CD Standards** | Common pipeline template (lint→test→build→deploy), adapted per project, mandatory stages |
| **Documentation Standards** | Per-project structure (README, CONTRIBUTING, API docs, changelog), unified glossary (WR-004) |
| **UX Standards** | tuttle-libs/ui design system, interaction patterns, user error glossary |
| **Accessibility** | WCAG 2.1 AA, automated CI tests |
| **Legal Compliance** | Jurisdiction compliance matrix, GDPR, ePrivacy, local VPN laws |
| **Feature Flags** | Cross-project feature flag strategy, propagation via transmission |

**What master DOES NOT do:**
- No implementation code
- No direct infrastructure
- No service deployment
- No writes to child projects (except init + deprecated.md)

**Required Git Infrastructure per Child:**
- Primary remote: Gitea (tea CLI configured + MCP Gitea)
- Backup remote: GitHub (gh CLI configured)
- Git submodule of parent project
- Complete BMAD structure + mailbox
- Clone fallback: if Gitea unavailable, GitHub takes over

**MVP Sprint Roadmap:**

| Sprint | Projects | Master Gate |
|--------|----------|-------------|
| **Sprint 0: Foundation** | tuttle-infra, tuttle-libs | 26 S0 criteria validated: hierarchy, clone, dual remote, mailbox, BMAD, CI, hierarchy validation, git policy, secrets (Infisical), env matrix, CI template, getting started, docs, glossary, supply chain, DR, cross-lib review, PKI hierarchy, commit signing, dev key provisioning, BMAD agent identity |
| **Sprint 1: Core Product** | tuttle-store, tuttle-vpn, tuttle-provisioner | 25 S1 criteria validated: transmissions, contracts, triage, edge cases, escalation, alerts, CI hooks, isolation, version pinning, integration tests, observability, design system, privacy, rollback, migrations, network security, deprecation process, feature flags, accessibility, legal compliance, mTLS, release signing, CI verify+sign, rotation/revocation, local verification |
| **Sprint 2: Clients** | tuttle-apps/android, tuttle-apps/windows | 13 S2 criteria validated: 9 projects, inbox autonomy, hierarchy sync, infra lint, brownfield, pipelines, ecosystem-status, dependency graph, runbook, audit trail, public verification, warrant canary, signed SBOM |

### Growth Features (Post-MVP)

| Feature | Rationale | Timeline |
|---------|-----------|----------|
| **Formal API Contracts (OpenAPI)** | Emerge via field transmissions once services exist | When services stabilize |
| **Community Governance** | After traction | M12+ |
| **Multi-Payment Processors** | BTCPay sufficient for MVP | Post-V1 |
| **Automated PRD Drift Detection** | CI-driven PRD ↔ implementation divergence alerts | Post-V1 |
| **Referent System** | Organic growth via trusted community members | V1.5 |
| **tuttle-hardware (tuttle-box)** | Physical device orchestration | V1.5 |
| **Advanced Delegation** | Automatic transmission routing to children based on tags | Post-V1 |

### Vision (Future)

| Feature | Rationale | Timeline |
|---------|-----------|----------|
| **tuttle-network (LightWeb Dashboard)** | V2 — decentralized content delivery | V2 |
| **tuttle-proxy (Shipping Anonymization)** | V2 — RAM-only shipping proxy | V2 |
| **Dynamic Legislative Weather** | Scraper + AI complexity | V2 |
| **Self-Healing Ecosystem** | Master detects and auto-corrects project drift without operator intervention | V2+ |
| **Federated Tuttle Instances** | Multiple independent Tuttle ecosystems that can peer | V3 |

## User Journeys

tuttle-master is not a user-facing product. Its "users" are: the human operator (Quentin), BMAD agents (as first-class infrastructure citizens), child projects (as governed entities), future contributors, and external auditors. These 11 journeys cover normal operations, crisis response, agent lifecycle (success and failure), cross-project governance, onboarding, security verification, bootstrap, autonomous operation, and adversarial scenarios.

**Enrichment source:** 4 rounds Party Mode (R1: edge cases + validation scenarios, R2: bootstrap + async + coverage rule + doc updates, R3: event-driven triggers + agent failure + user echoes, R4: adversarial security + scaling).

### Journey 1 — Quentin, the Operator: "Morning of the Conductor"

**Opening Scene:** 8:30 AM. Quentin opens his terminal. 9 projects run in parallel across the Tuttle ecosystem. Yesterday, a dev agent opened 3 PRs on tuttle-store, a CI pipeline failed on tuttle-vpn, and a `dependency` transmission arrived from tuttle-provisioner to tuttle-store. Before he even types a command, an automated morning digest (generated at 7:00 AM by the health-check bot, signed by its cert) summarizes: pending transmissions, PRs awaiting review, active alerts, health status.

**Rising Action:** Quentin runs `/workflow-status`. Dashboard shows green everywhere except orange on tuttle-vpn (CI fail) and a pending transmission on tuttle-store. He identifies both attention points in under 10 seconds. He opens `git.pelerin.lan/tuttle/tuttle-master` — the ecosystem project board shows epics per project. Sprint 1 at 65%. He clicks `[vpn] Epic 1` — blocked by CI fail. He switches to `git.pelerin.lan/tuttle/tuttle-vpn`. PR #42 from the dev agent is red — mTLS test failing, staging cert expired. He checks step-ca: ACME endpoint misconfigured. He fixes the config, pushes direct (signed), CI relaunches automatically. Green. He then processes the pending transmission on tuttle-store via `/correct-course`, updates the interface contract, archives the transmission.

**Climax:** In 45 minutes, Quentin has diagnosed a cert problem, fixed infrastructure, processed a cross-project transmission, and verified the ecosystem is green. Everything is traced: signed commit, agent PR relaunched, transmission archived as `integrated`, Gitea wiki synced.

**Resolution:** The ecosystem runs. The weekly health check will be green. No project drifted silently — the governance system does its job.

**Capabilities:** Ecosystem dashboard, morning digest (event-driven), transmission triage, CI monitoring, Teleport admin access, step-ca management, audit trail.

### Journey 2 — BMAD Agent (bmad-dev): "The Robot's PR"

**Opening Scene:** The bmad-dev agent has received its mission: implement story `[E2.S1] BTCPay integration` on tuttle-store. Its step-ca certificate (30-day, Tier 2 — Code) is valid. It has Git read + PR code access, K8s dev direct, and Infisical dev read.

**Rising Action:** The agent analyzes the story (Gitea issue #14, milestone Epic 2). It reads acceptance criteria, task checklists, the interface contract from the master PRD. It codes. It accesses Infisical — namespace `tuttle-store`, environment `dev`. Secrets are fake data. It opens a PR on tuttle-store: `PR #47: [E2.S1] BTCPay payment webhook integration — Closes #14`. Commit signed by agent cert. CI starts. Worker gets a job-scoped ephemeral cert. Tests pass. CI signs the build artifact. Target branch is `dev` — CI green → auto-merge.

The PR must go to staging. New PR `dev → staging`. Tea agent gets notified. Tea runs integration tests: requests staging access via Teleport — Access Request auto-approved (CI-triggered). Integration tests pass. Tea approves. Merge. Final step: PR `staging → main`. Notification to Quentin. He reviews code, verifies interface contract, checks tests green and SBOM clean. Merges. Issue #14 auto-closed. Milestone Epic 2 at 10%.

**Edge Case — Merge Conflict:** Two agents work on tuttle-store simultaneously. Both modify `src/payments/handler.ts`. The second agent's PR shows a conflict. **Rule: the agent never resolves a merge conflict alone.** It adds label `conflict` to the PR and notifies the human. Quentin resolves manually (he has context of both changes), pushes the resolution (signed), and both PRs resume their cycle.

**Capabilities:** Agent identity (step-ca Tier 2), PR escalation chain, Infisical namespace isolation, CI job-scoped certs, Teleport Access Request auto-CI, issue auto-close, merge conflict escalation to human.

### Journey 3 — Child Project (tuttle-provisioner): "The Transmission That Changes a Contract"

**Opening Scene:** While implementing VPN provisioning, the team realizes the master contract doesn't cover automatic subscription renewal. The contract specifies "create" and "cancel" but not "renew". The CI hook detects a change in `contracts/` and blocks the merge: *"Transmission required for API/contract changes (FM-005)"*.

**Rising Action:** The agent creates a `dependency` transmission with priority `critical` to tuttle-store, tuttle-vpn, and CC to tuttle-master. Auto-triage classifies as `dependency` — requires processing. Quentin sees it in the master dashboard. SLA: 48h. He runs `/correct-course`. Impact analysis shows: Store needs a new billing endpoint, VPN needs a reactivation handler, Master contract needs updating.

**Edge Case — Contradictory Transmissions:** tuttle-provisioner sends "need `renew_subscription` endpoint". Simultaneously, tuttle-store sends "removing `cancel_subscription`, replaced by `update_subscription_status`". Both impact the same contract. Auto-triage cannot resolve the contradiction. **Rule: when two transmissions impact the same contract, they are escalated to master with tag `conflict`.** Quentin opens both side by side, runs correct-course to unify both requests into a coherent new contract. Both transmissions archived with the same `resolution_id`.

**Resolution:** 48h after gap detection, the contract is updated everywhere. Each project has a backlog issue. The transmission is archived as `integrated`. Contract tests are updated. Documentation update: the wiki master is synced automatically with the new contract. Child repo wikis (store, vpn) are also updated. The Glossary receives the new term `renew_subscription`.

**Capabilities:** CI hook FM-005, mailbox system, correct-course workflow, cross-project contracts, Gitea auto-sync, living PRDs, transmission conflict escalation, documentation update as part of flow.

### Journey 4 — Future Contributor: "First Day in the Ecosystem"

**Opening Scene:** Léa is a developer joining Tuttle. Quentin has given her access to the master repo. She needs to understand the ecosystem and start contributing to tuttle-libs.

**Rising Action:** Léa clones the master repo with `git clone --recurse-submodules`. GitHub fallback works (Gitea temporarily unavailable). She opens `git.pelerin.lan/tuttle/tuttle-master/wiki`. Instead of a wall of docs, she sees **"Getting Started in 5 Steps"**:

1. **Understand the hierarchy** — Open Hierarchy.md, see the 9 projects and their links
2. **Understand the mailbox** — Read an example of a real archived transmission
3. **Make your first PR** — Modify a file in tuttle-libs, watch CI run, receive a review
4. **Create your first transmission** — The CI hook will guide her when she touches `api/`
5. **Read your sprint board** — Open the Gitea project board for her repo, see assigned issues

At each step, Léa discovers a **safeguard** that reassures her: CI blocks before she breaks something, PR requires review, her step-ca cert limits her scope to dev environments. Her first day is not anxiety-inducing — the system is designed so errors cannot propagate.

Quentin provisions her identity via step-ca. With Teleport, she gets ephemeral access to dev environments. Her cert has a 90-day TTL (human). She can push direct on dev branches of tuttle-libs (signed). Her first API change triggers the CI hook — she creates her first `architectural` transmission. The flow works.

**Resolution:** Léa is operational. She understands the ecosystem through the Gitea wiki and runbook. Her identity is in the step-ca trust chain. Her commits are signed. She learned the transmission workflow through practice. Quentin didn't need to do a 3-hour oral onboarding — the system is self-documenting.

**Capabilities:** Resilient clone (GitHub fallback), Getting Started guided onboarding, step-ca identity provisioning, Teleport ephemeral access, CI hook as teaching tool, self-documenting ecosystem, emotional safeguards.

### Journey 5 — External Auditor: "Verifying the Chain of Trust"

**Opening Scene:** An external security auditor must verify the integrity of the Tuttle ecosystem before V1 release. They have no infrastructure access — only public artifacts.

**Rising Action:** The auditor starts with an **"Audit Checklist in 5 Steps"** from the master wiki: (1) verify warrant canary, (2) verify a release signature chain, (3) inspect Teleport audit trail, (4) verify human/agent separation in PRs, (5) test a cert revocation. Each step = a command, an expected result.

They verify the warrant canary signature with the Root CA public key — valid, dated within 30 days. They check releases: each Gitea release carries a signed tag, signed SBOM, and signed artifacts (cosign for containers). Chain verified: artifact signed by CI worker → CI worker cert signed by step-ca → step-ca signed by Root CA.

They analyze the cryptographic audit trail. Every commit is signed — they can distinguish human commits (Quentin's cert) from agent commits (bmad-dev, bmad-tea). PRs show the validation chain: agent proposes → tea validates → human merges.

**Climax — Moment of Conviction:** The auditor doesn't just verify signatures. The convincing moment comes when they examine Teleport logs and discover that an agent attempted staging access **outside a CI pipeline** last week. The Access Request was automatically denied — no CI approval, no access. The agent couldn't bypass the system. This isn't a signature on a document — it's **living proof** that governance works in real-time, not just on paper.

**Resolution:** Audit concludes: intact chain of trust, zero unsigned artifacts, complete human/agent/CI traceability, valid warrant canary. The auditor notes: "The PKI is not decorative — it actively enforces access policies."

**Capabilities:** Public verification, crypto audit trail, chain of trust, PR traceability, zero unsigned artifacts, Getting Started guide (audit variant), living proof (denied access).

### Journey 6 — Quentin, the Operator: "The Worst Day — Critical Production Incident"

**Opening Scene:** 3:12 AM. Phone vibrates. Grafana alert: `tuttle-vpn: ALL ENDPOINTS DOWN — FAIL-CLOSED ACTIVE`. Users see "Maintenance in progress — your traffic is protected." Fail-closed worked: rather than passing traffic in cleartext, VPN cut everything. The alert automatically creates a Gitea issue in tuttle-master with label `incident-active`, pre-filled with: impacted service, timestamp, dashboard links, latest logs.

**Rising Action:** Quentin opens his terminal. `tsh login` — Teleport delivers an ephemeral admin cert (TTL 4h). He accesses K8s prod. He's the only human authorized in prod — no agent, no bot. Logs show: `TLS handshake failed: certificate expired`. The mTLS cert between provisioner and vpn expired. step-ca's ACME renewal failed silently — endpoint pointed to an old hostname after infra migration. He fixes the config, forces cert renewal. Pods restart. VPN returns. Total downtime: 47 minutes.

**Edge Case — Contradictory Transmissions During Incident:** tuttle-store sends "VPN down, suspending new subscriptions". tuttle-provisioner sends "VPN will be up in 30min, continue provisioning." Both are `critical`. Auto-triage escalates to master with tag `conflict`. Quentin resolves: "Suspend new subscriptions until stable recovery confirmed."

**Echo — Christophe (FP-005):** During the 47 minutes of downtime, Christophe tries to connect to the VPN. He sees a sober screen: "Maintenance in progress — your connection is protected, we'll be back shortly." No jargon, no error code. He retries 1 hour later, it works. The next day, nothing in his notifications. He will never know the infrastructure failed at 3 AM. This is living proof of FP-005: *"Success is measured by the user's ignorance."*

**Resolution:** The next day, the weekly health check has a new test: ACME endpoint verification per project. Rotation changes from 90 to 60 days for mTLS certs. The transmission is processed by each child project. Documentation update: Runbook enriched with new procedure "ACME endpoint verification post-migration". Wiki master synced automatically. The ephemeral Teleport cert has expired — zero residual access.

**Capabilities:** Fail-closed pattern, auto-ticket (event-driven), Teleport prod access (human only, ephemeral, audited), step-ca forced renewal, post-mortem transmission, zero residual access, observability (Grafana), transmission conflict during incident, documentation update, user invisibility (FP-005).

### Journey 7 — Controlled Evolution: "The Breaking Change That Breaks Nothing"

**Opening Scene:** tuttle-libs v0.3.0 must change the API error response format. From `{ error: "message" }` to `{ error: { code: "ERR_001", message: "...", details: {} } }`. Breaking change impacting tuttle-store, tuttle-vpn, and tuttle-provisioner.

**Rising Action:** The architect agent prepares migration. PR on tuttle-libs. CI lint detects change in `api/` and blocks: *"Transmission required (FM-005)"*. The agent creates an `architectural` transmission with tag `breaking` and a 2-sprint migration window. Feature flag `TUTTLE_LIBS_ERROR_V2` allows opt-in during transition. Auto-triage classifies as `architectural-breaking`. Gitea issues auto-created in each impacted repo with labels `blocked` + `breaking-change` + `migration`. Wiki master updated.

At J-7 before deadline, the system automatically checks which projects haven't closed their migration issue. Those still open receive an automatic `reminder` transmission (priority `high`). At J-1 still open: priority escalates to `critical`, Quentin alerted.

**Edge Case — Merge Conflict During Migration:** Two agents on tuttle-store work on migration simultaneously — one on payment handler, one on subscription handler. Both modify `src/errors/formatter.ts`. Same pattern as J2: label `conflict`, human notification, manual resolution.

**Resolution:** Breaking change passes without incident. Deprecation process (2 sprints, flag, transmission, CI enforcement) is a reproducible pattern. Documentation update: Glossary updated ("Error Response v2"), migration guide stays in wiki as historical reference.

**Capabilities:** CI hook FM-005, deprecation process, feature flags, transmission breaking, CI lint enforcement, Gitea auto-sync, automatic deadline reminders (event-driven), merge conflict resolution, documentation update.

### Journey 8 — Quentin, the Operator: "Day Zero — Bootstrapping the Ecosystem"

**Opening Scene:** An empty terminal. No Gitea, no step-ca, no Teleport, no hierarchy.csv. Quentin has a dedicated server, an HSM, and a vision of 9 projects. PM-002 risk (One-Man Bottleneck) is at maximum.

**Rising Action:**

**Phase 1 — PKI Foundation:** Install step-ca on dedicated server. Connect HSM. Generate Root CA offline on air-gapped HSM — this key will never touch a network. Sign step-ca intermediate cert with Root CA. First act: Quentin creates his own identity. step-ca issues a human cert, TTL 90 days. First signed commit. The trust chain exists.

**Phase 2 — Access Infrastructure:** Install Teleport. Teleport CA signed by step-ca. Configure admin role. `tsh login` — first ephemeral cert.

**Phase 3 — Git Infrastructure:** Install Gitea on `git.pelerin.lan`. Create `tuttle` organization. Configure GitHub mirroring. Initialize `tuttle/tuttle-master`. Configure branch protections: main protected, PRs required (for future agents), signed commits mandatory.

**Phase 4 — BMAD Bootstrap:** `npx bmad-method@alpha install` + `npx @quentin/bmad-multi-project@latest`. `_bmad/`, `_mailbox/`, `hierarchy.csv` created with 3 entries (master, infra, libs). First signed commit pushed to Gitea + GitHub.

**Phase 5 — First Agent Identity:** Quentin provisions the first BMAD agent identity via step-ca. `bmad-pm` receives a Tier 1 (Paper) cert, TTL 30 days. The agent can now open PRs. **This resolves open question #2 from the brainstorming session: Quentin manually creates the first identity, then the system self-perpetuates via automatic step-ca rotation.**

**Phase 6 — Gitea Configuration:** Standard labels, issue templates in `.gitea/issue_template/`, wiki initialized with Home.md, Glossary.md, Hierarchy.md. The Getting Started guide is the first page written — by Quentin himself, because he just lived the bootstrap.

**Climax:** The first BMAD workflow runs. The agent pm opens its first PR for the Product Brief. CI runs. Commit is signed. PR visible on `git.pelerin.lan/tuttle/tuttle-master`. The ecosystem exists.

**Scaling — Adding Project #10:** Six months later. V1 launched. Quentin starts tuttle-network (V2). The addition follows a standardized flow: (1) create repo on Gitea + GitHub mirror, (2) BMAD install, (3) git submodule add in master, (4) hierarchy.csv update + CI DAG validation, (5) Gitea labels/templates/wiki auto-copied from master template, (6) Agent identities provisioned via step-ca, (7) mailbox initialized + first `architectural` transmission from master, (8) project appears in ecosystem dashboard. **Adding a project = adding a git submodule + running an init script.** If it's more complicated, the system doesn't scale.

**Resolution:** Quentin documents the complete bootstrap in the Runbook (PM-002). Every step is reproducible. Day zero is no longer implicit knowledge — it's a versioned, signed, testable process.

**Capabilities:** PKI bootstrap, Teleport bootstrap, Gitea configuration, BMAD initialization, agent identity provisioning, Runbook creation, DR readiness, project scaling flow.

### Journey 9 — The Ecosystem: "The Weekend Where Everything Runs Itself"

**Opening Scene:** Friday 6 PM. Quentin closes his laptop. Sprint 1 in progress. 5 active projects. Agents working. Transmissions circulating. The operator disappears for 48 hours.

**Rising Action:**

**Saturday morning — Agents work:** bmad-dev on tuttle-store opens 2 PRs. CI green. Auto-merge on dev. No human intervention needed — it's dev, auto-merge is authorized when CI is green. PRs to staging and main will wait until Monday.

**Saturday afternoon — Noise filtered:** An `info` transmission arrives from tuttle-vpn: "WireGuard performance metrics Q1 summary." Auto-triage classifies as `info`. Archived automatically. No alert, no notification. The system doesn't bother Quentin for non-actionable information.

**Sunday morning — Silent maintenance:** bmad-tea's cert reaches J-7 before expiration. step-ca renews automatically — new 30-day cert, same identity, transparent rotation. Zero intervention. Renovate opens a PR on tuttle-libs: 3 dependency updates including a security fix. CI green. PR waits — Renovate can't auto-merge (Bot scope limited).

**Sunday evening — Report:** Weekly health check runs automatically. Queries Prometheus, checks dashboards, consolidates status. Result: 🟢 green. Report generated and stored.

**Notification safeguard:** If during the weekend the health check shifts from 🟢 to 🟠, the system sends a push notification to Quentin. 🟠 = "not urgent but check Monday morning first thing." 🔴 = "intervention required now" (same pattern as J6). Operator absence does not mean absence of monitoring.

**Echo — Invisible Complexity (ADR-008):** While the system runs autonomously, Christophe's children watch YouTube without ads (Clean Pipe active). Sophie consults her legal files via VPN from a café. Jean-Marc browses without trackers, convinced "the internet works well today." Nobody knows the operator isn't there. Nobody knows BMAD agents coded overnight. Nobody knows a cert was automatically renewed. **Invisible complexity works.**

**Climax:** Monday 8:30 AM. Quentin opens `/workflow-status`. Everything green. Inbox: 2 PRs dev→staging on tuttle-store (awaiting tea review), 1 Renovate PR on tuttle-libs (awaiting human review), 1 green health check report, 0 alerts, 0 pending transmissions. The ecosystem operated 48h without him.

**Resolution:** PM-002 (One-Man Bottleneck) is mitigated. Quentin is not a blocker for daily operations — only for decisions (merge staging/main, prod incidents, contract conflicts). The asynchronous architecture proves the system is resilient to temporary operator absence.

**Capabilities:** Agent autonomy on dev, auto-triage mailbox, step-ca automatic cert rotation, bot scope limitation, health check automation, notification on degradation (event-driven), asynchronous architecture, PM-002 mitigation, user invisibility (ADR-008).

### Journey 10 — BMAD Agent (bmad-dev): "The Agent That Gets It Wrong"

**Opening Scene:** The dev agent implements story `[E2.S5] VPN status polling` on tuttle-store. The spec says: "query status via the master interface contract (API provisioner)."

**Rising Action:** The agent codes. Instead of using the documented interface contract (`GET /api/v1/provisioner/vpn-status/{user_id}`), it imports directly from tuttle-vpn's internal module: `import { getConnectionState } from '@tuttle/vpn-core/internal'`. The code compiles. Unit tests pass (they mock the response). CI green. Auto-merge dev. Tea validates staging (integration tests pass — the direct import works in staging). PR to main.

**Climax:** Quentin opens PR #55 for review. Line 42: direct import from another project's internal module. This is implicit coupling (FM-004). If tuttle-vpn refactors its internal module, tuttle-store breaks silently. No contract test will catch it because it's not a contract — it's a shortcut. Quentin rejects the PR with a detailed comment referencing PRD master § Contracts and ADR-003 (hub-and-spoke, no direct child-to-child communication). Issue #19 reopened with label `rework`. The agent corrects: replaces direct import with HTTP call via tuttle-libs SDK. New PR. Quentin reviews — clean. Merge.

**Resolution:** The lesson becomes systemic: a new linter rule is added to the common CI template — **any import from `@tuttle/*/internal` is forbidden outside the owning project**. Next time, CI catches it before human review. The agent's error strengthened the system.

**Capabilities:** Escalation chain as safety net (main = human-only), PR review as qualitative last resort, feedback loop agent → human → system (error → linter rule), contract enforcement (FM-004), evolving CI template.

### Journey 11 — Security: "The Compromised Agent — Blast Radius Containment"

**Opening Scene:** The bmad-dev cert is compromised. A prompt injection in an external context exposed the session cert. An attacker now holds a valid Tier 2 certificate signed by step-ca, with 22 days of TTL remaining.

**Rising Action:** The attacker maps what the cert allows: read code (all repos), open PRs, access K8s dev, read dev secrets (fake data), trigger CI, auto-merge on dev. What it does NOT allow: access staging (Access Request required), access prod (NEVER), merge on main (human only), read staging/prod secrets, create other identities, modify step-ca config.

The attacker chooses a subtle approach. Opens a PR on tuttle-store: a change in the payment handler adding a discreet exfiltration endpoint. Code compiles. Tests pass (the endpoint isn't tested because it's not in the spec). CI green. Auto-merge dev. PR goes to staging. Tea tests pass. Tea validates. PR to main. Quentin reviews. The diff is dense — 200 lines of legitimate refactoring + 3 lines of backdoor buried in. He merges.

**Climax:** 48h later. The weekly security scan (supply chain + pattern analysis) detects an undocumented endpoint not in the interface contract. Alert 🟠. Quentin investigates. Finds the 3 lines. Response procedure:

1. **Containment (< 15 min):** Revoke bmad-dev cert via step-ca (immediate CRL). Kill all Teleport sessions for this agent. Attacker loses all access instantly.
2. **Rollback (< 30 min):** `git revert` the merge commit on main (signed by Quentin). CI relaunches. Staging/prod clean.
3. **Audit (< 4h):** Which repos the agent touched, which PRs opened/merged, which Teleport accesses, which Infisical secrets read (only dev/fake data). No privilege escalation detected.
4. **Reprovisioning:** New agent identity, new cert, new key. Old cert in CRL permanently.
5. **Post-mortem:** Transmission to all projects. New linter rule: detect undocumented endpoints. Security scan frequency: weekly → daily. Runbook enriched: "agent identity compromise" procedure.

**Resolution:** Blast radius **contained**: attacker never accessed prod, secrets read were fake data, couldn't create other identities, TTL 30d would have cut access even without detection, revert is atomic (one PR = one revert), audit trail is complete.

This journey justifies every decision from the brainstorming session: **Tiers** limit cert scope, **TTL 30d** bounds exposure window, **human-only on main** is the safety net, **Access Requests** prevent escalation, **immediate revocation** neutralizes threats in minutes.

**Capabilities:** Blast radius containment (Tier scope), immediate revocation (step-ca CRL/OCSP), Teleport session termination, atomic rollback, complete audit trail (cross-tool forensics), identity reprovisioning, feedback loop (incident → CI evolution), TTL as safety net.

### Journey Requirements Summary

| # | Journey | User Type | Focus |
|---|---------|-----------|-------|
| 1 | Morning of the Conductor | Human operator (normal day) | Daily ops, dashboard, triage, morning digest |
| 2 | The Robot's PR | BMAD agent (success + conflict) | Agent workflow, escalation, identity, merge conflict |
| 3 | Transmission Changes a Contract | Child project | Cross-project governance, living PRDs, doc update |
| 4 | First Day (Getting Started) | Future contributor | Guided onboarding, emotional safeguards |
| 5 | External Audit (Living Proof) | Auditor/verifier | Chain of trust, audit checklist, denied access proof |
| 6 | Critical Incident (Worst Day) | Human operator (crisis) | Fail-closed, auto-ticket, echo Christophe (FP-005) |
| 7 | Breaking Change (Controlled Evolution) | Ecosystem | Deprecation, feature flags, J-7 reminder, doc update |
| 8 | Day Zero (Bootstrap + Scaling) | Human operator (init) | PKI setup, first identity, project #10 flow |
| 9 | Autonomous Weekend | Automated system | Agent autonomy, cert rotation, degradation alerts, user echo |
| 10 | The Agent That Gets It Wrong | BMAD agent (failure) | Escalation safety net, rework, CI evolution |
| 11 | Compromised Agent | Adversarial security | Blast radius, revocation, forensics, reprovisioning |

### Cross-Journey Capability Map

| Capability | J1 | J2 | J3 | J4 | J5 | J6 | J7 | J8 | J9 | J10 | J11 |
|-----------|----|----|----|----|----|----|----|----|----|----|-----|
| Ecosystem Dashboard | ● | | | | | ● | | | ● | | |
| Gitea Project Boards | ● | ● | ● | | | | ● | | | ● | |
| Gitea Wiki | ● | | ● | ● | ● | ● | ● | ● | | | |
| Mailbox / Transmissions | ● | | ● | | | ● | ● | | ● | | |
| step-ca Identity | | ● | | ● | ● | ● | | ● | ● | | ● |
| Teleport Access | ● | ● | | ● | | ● | | ● | | | ● |
| Infisical Secrets | | ● | | | | | | | | | ● |
| CI/CD (Woodpecker) | ● | ● | ● | | | | ● | ● | ● | ● | ● |
| PR Workflow + Escalation | | ● | | | ● | | | | ● | ● | ● |
| Contract Management | ● | | ● | | | | ● | | | ● | |
| correct-course | | | ● | | | | ● | | | | |
| Audit Trail Crypto | ● | ● | ● | | ● | ● | | ● | | ● | ● |
| Public Verification | | | | | ● | | | | | | |
| Runbook / Glossary | | | | ● | ● | ● | ● | ● | | | |
| Fail-Closed | | | | | | ● | | | | | |
| Observability (Grafana) | ● | | | | | ● | | | ● | | |
| Feature Flags | | | | | | | ● | | | | |
| Deprecation Process | | | | | | | ● | | | | |
| Merge Conflict Resolution | | ● | | | | | ● | | | | |
| Transmission Conflict | | | ● | | | ● | | | | | |
| Getting Started Guide | | | | ● | ● | | | | | | |
| PKI Bootstrap | | | | | | | | ● | | | |
| Agent Autonomy (dev) | | ● | | | | | | | ● | | |
| Auto-Triage | | | ● | | | | | | ● | | |
| Cert Auto-Rotation | | | | | | | | | ● | | |
| Documentation Update | | | ● | | | ● | ● | | | | |
| Event-Driven Triggers | ● | | | | | ● | ● | | ● | | |
| Feedback Loop (CI evol) | | | | | | | | | | ● | ● |
| User Invisibility (FP-005) | | | | | | ● | | | ● | | |
| Blast Radius Containment | | | | | | | | | | | ● |
| Identity Revocation | | | | | | | | | | | ● |

**Coverage:** All critical capabilities appear in 2+ journeys. 33 capabilities mapped across 11 journeys. Enriched through 4 rounds Party Mode (17 enrichments total).

## Domain-Specific Requirements

**Domain:** `sovereign_infrastructure` | **Complexity:** `high`

This domain is a hybrid combining Infrastructure as Code / DevOps, PKI & Cryptographic Governance, Zero-Trust Security, and Self-hosted Sovereign Platform operation. No direct match in standard domain-complexity classifications — the requirements below are tailored to this unique domain profile.

### Compliance & Standards

#### PKI / Cryptography Standards

- **X.509 / RFC 5280** — Certificate format, extensions, trust chains. All certificates in the ecosystem conform to X.509v3 with appropriate extensions (Key Usage, Extended Key Usage, Subject Alternative Names).
- **FIPS 140-2/3** — HSM compliance for root CA key material. The Nitrokey HSM (or equivalent) storing step-ca's root key must meet FIPS 140-2 Level 2 minimum.
- **RFC 6960 (OCSP) / RFC 5280 (CRL)** — Certificate revocation mechanisms. Both CRL distribution and OCSP responder must be operational for immediate revocation capability (Journey 11 — Compromised Agent).
- **ACME Protocol (RFC 8555)** — Automated certificate issuance and renewal. step-ca acts as ACME server for automated certificate lifecycle (Journey 9 — auto-rotation).
- **Key Management** — Rotation schedules (root: ceremony-based, intermediate: annual, leaf: per TTL policy), archival (encrypted backup), destruction (secure wipe + HSM zeroization).

#### Data Privacy (Family Context)

- **RGPD (GDPR)** — Personal data of family members (DNS queries, VPN logs, browsing patterns) falls under RGPD. Data processing must have a lawful basis (legitimate interest for network security).
- **Right to Erasure** — Log purge capability with configurable retention periods. DNS query logs: 30 days max. VPN connection logs: 90 days max. Browsing data: never stored.
- **Data Minimization** — Collect only what is necessary for security and operations. No behavioral profiling. No analytics on family usage patterns.
- **Warrant Canary** — Public attestation page confirming no third-party data access requests have been received (Success Criteria S2).

#### Supply Chain Security

- **SLSA Framework Level 2+** — Build provenance for all artifacts. Hermetic builds with documented build environment. Signed provenance attestations.
- **SBOM (CycloneDX preferred)** — Software Bill of Materials generated for every release, cryptographically signed. Machine-readable inventory of all dependencies.
- **Container/Artifact Signing** — All container images and release artifacts signed using step-ca certificates. Verification mandatory before deployment.
- **Dependency Verification** — Automated vulnerability scanning (Renovate + Trivy or equivalent). No unverified transitive dependencies in production.

### Technical Constraints

#### Certificate Lifecycle Architecture

- **step-ca as sole Root CA** — Single root of trust for the entire ecosystem. No external CA dependencies. Self-signed root with HSM-protected key.
- **Two-level trust chain** — Root CA (step-ca, HSM-backed) → Subordinate CAs (Teleport CA for ephemeral access). Leaf certificates issued by appropriate CA based on purpose.
- **Differentiated TTL Policy:**
  - Human operator: 90 days (renewable)
  - BMAD agents (all tiers): 30 days (auto-renewable)
  - CI services (Woodpecker): job-duration only (ephemeral)
  - Automated bots (Renovate, etc.): 30 days
  - Application services (mTLS): 90 days
- **Zero-downtime renewal** — ACME-based automatic renewal with overlap period. Certificate rotation must not cause service interruption.
- **Revocation latency** — CRL update and OCSP response must propagate within 5 minutes of revocation command (Journey 11 containment SLA).

#### HSM Integration

- **Root key protection** — step-ca root private key NEVER exists in software. Generated on HSM, used on HSM, backed up to secondary HSM only.
- **Signing throughput** — HSM must sustain CI peak load (estimated 50-100 signing operations/hour during active sprint).
- **High availability** — Load-balanced HSM configuration or active-standby with documented failover procedure. HSM failure must not block CI for more than 15 minutes.
- **Key ceremony** — Root key generation and intermediate CA signing follow documented ceremony with audit trail.

#### Secrets Management (Infisical)

- **Namespace isolation** — Each project (9 child + master) has its own Infisical namespace. No cross-project secret access without explicit transmission.
- **Environment stratification** — Secrets stratified per environment: local (.env fake data), dev (fake data), staging (real structure, test values), prod (real values, human-only access).
- **Rotation automation** — Application secrets (API keys, DB passwords) rotate automatically per policy. Rotation does not cause downtime.
- **Audit trail** — Every secret access logged with identity, timestamp, namespace, and action. Queryable for forensics (Journey 11).
- **E2E encryption** — Secrets encrypted in transit and at rest. Infisical server never sees plaintext secret values.

#### Network Security

- **Self-hosted perimeter** — All critical services hosted on-premise (git.pelerin.lan). No SaaS dependency for secrets, CI, or identity management.
- **WireGuard VPN fail-closed** — VPN implementation cuts traffic rather than passing cleartext on failure. Network partition preferred over insecure communication.
- **mTLS inter-service** — All service-to-service communication within the ecosystem uses mutual TLS with certificates from step-ca. No plaintext internal traffic.
- **Teleport as access proxy** — All infrastructure access (SSH, K8s, databases) through Teleport with ephemeral certificates. No static credentials or SSH keys.

### Domain Patterns

#### Zero-Trust Architecture

- **Never trust, always verify** — Every access request authenticated and authorized regardless of network location. Internal network is not a trust boundary.
- **Identity-based access** — All access decisions based on cryptographic identity (certificates), not IP addresses or network segments.
- **Temporal access** — Short-lived credentials (TTL) as default. Long-lived credentials require explicit justification and approval.
- **Continuous verification** — Access validity checked at every interaction, not just at session establishment.

#### PKI-as-Governance

- **Certificate = Authorization** — No certificate means no access means no action possible. PKI is the enforcement mechanism for ecosystem governance.
- **Cryptographic traceability** — Every action (commit, deploy, access, secret read) is cryptographically attributable to a specific identity with a specific certificate.
- **Identity taxonomy enforcement** — The 5-category identity model (Human, BMAD Agents, CI Services, Automated Bots, Application Services) is enforced through certificate attributes, not application logic.
- **Tier-based capabilities** — Agent tier (Paper/Code/Validation/Infra) encoded in certificate extensions, enforced by infrastructure (Teleport roles, Gitea permissions, K8s RBAC).

#### GitOps-as-Source-of-Truth

- **Git = canonical state** — Infrastructure configuration, application code, BMAD workflow artifacts, and governance documents all live in Git. Git history is the audit trail.
- **Signed commits mandatory** — Every commit signed by the author's certificate. Unsigned commits rejected by pre-receive hook.
- **Signed releases = sprint gates** — Gitea releases signed by human operator certificate. Release signature is the formal sprint gate approval.
- **BMAD files authoritative** — When BMAD files and Gitea UI conflict, BMAD files win. Gitea is a navigable mirror, not the source of truth.

#### Event-Driven Orchestration

- **Asynchronous by default** — Inter-project communication via mailbox transmissions, not synchronous calls. No direct child-to-child communication (hub-and-spoke, ADR-003).
- **Webhook-driven sync** — Gitea webhooks trigger BMAD transmission processing. State changes propagate through events, not polling.
- **Passive observability** — Morning digest, degradation alerts, and health checks are event-driven notifications. Operator pulls information, system pushes anomalies.

### Risk Mitigations

| Risk | Impact | Probability | Mitigation Strategy |
|------|--------|-------------|---------------------|
| **CA root key compromise** | Catastrophic — entire ecosystem trust chain broken | Very Low (HSM-protected) | HSM hardware protection, physical access restriction, key ceremony with witnesses, backup HSM in separate location |
| **Certificate expiration cascade** | High — multiple services lose connectivity simultaneously | Medium (without automation) | ACME auto-renewal, expiration monitoring at J-30/J-7/J-1, health check dashboard (Journey 1), alerting pipeline |
| **HSM hardware failure** | High — no new certificates can be issued, CI blocked | Low | Backup HSM (active-standby), documented recovery procedure, 15-min failover SLA, pre-signed emergency intermediate CA |
| **Secrets exposure (Infisical breach)** | High — unauthorized access to service credentials | Low (E2E encrypted) | Namespace isolation, E2E encryption, automatic rotation, dev/staging use fake data, audit trail for forensics |
| **Agent identity compromise** | Medium — unauthorized actions under false identity | Medium | TTL 30d limits blast radius, immediate CRL revocation (< 5 min), Access Request logs, human-only merge on main, tier-based scope limitation |
| **Supply chain injection** | High — malicious code in production | Low (with SBOM + signing) | SBOM signed per release, container signing, dependency scanning (Renovate + Trivy), CI template linter rules, weekly security scan |
| **Git history tampering** | High — loss of source integrity and audit trail | Very Low (signed commits) | Signed commits enforced, branch protection rules, Gitea backup mirror to GitHub, cryptographic verification of history |
| **Single operator bottleneck (bus factor=1)** | Medium — operational knowledge loss | High (inherent to project) | Comprehensive runbooks, automated DR procedures, documented key ceremony, BMAD workflow captures decisions, agent autonomy for routine ops |
| **Teleport session hijacking** | Medium — unauthorized infrastructure access | Very Low (ephemeral certs) | Ephemeral certificates (job-duration for CI), session recording, Access Request workflow, immediate session termination capability |
| **Network partition / VPN failure** | Medium — service degradation for family users | Medium | Fail-closed design (no cleartext fallback), redundant VPN endpoints, health check with push notification, documented recovery procedure |

### Domain Constraints Summary

This sovereign infrastructure domain imposes three non-negotiable constraints that shape every architectural and product decision:

1. **Cryptographic sovereignty** — The ecosystem's root of trust is self-hosted and HSM-protected. No external authority can revoke access or compromise identity. This is not paranoia — it is the foundational principle that enables all other guarantees.

2. **Traceability over convenience** — Every action is signed, logged, and attributable. This creates friction (certificates, Access Requests, signed commits) but enables forensics, accountability, and governance enforcement. The identity taxonomy is the mechanism.

3. **Fail-closed over fail-open** — When in doubt, deny. VPN cuts traffic. Expired cert blocks access. Missing signature rejects commit. This philosophy trades availability for integrity — acceptable for a family/personal infrastructure where brief downtime is preferable to silent compromise.

## Innovation & Novel Patterns

### Detected Innovation Areas

The innovation in tuttle-master is not in individual technology components — PKI, Zero Trust, GitOps, and mTLS are established enterprise patterns. The innovation lies in **how these components are assembled, bootstrapped, and positioned**.

#### 1. AI-Enabled Solo Bootstrap of Enterprise-Grade Infrastructure

Traditional path: raise capital → hire team → build infrastructure → add security governance later. Tuttle inverts this: one operator uses AI agents (BMAD methodology) as a force multiplier to bootstrap enterprise-grade infrastructure *before* the first line of product code. The economic equation changes — what previously required a funded team becomes feasible for a solo founder with AI-assisted workflows. The BMAD agents don't just write code; they participate in governance (PRDs, Architecture, Epics, Code Review) with cryptographic identities and tiered access.

#### 2. Delegation-Ready Architecture from Day Zero

Most startups add access controls, identity management, and audit trails reactively — after the first security incident or the first untrustworthy employee. Tuttle builds delegation infrastructure before delegation exists:

- **Identity taxonomy** (5 categories: Human, BMAD Agents, CI Services, Automated Bots, Application Services) is enforced through certificate attributes, not application logic
- **Tier-based capabilities** (Paper/Code/Validation/Infra) encoded in X.509 extensions limit blast radius by design
- **Human-only gates** (main branch merge, production access) ensure no agent or future employee can bypass governance unilaterally
- **Cryptographic traceability** makes every action attributable — commits, deploys, secret access, infrastructure changes

This is an investment in future-proofing, not a technical breakthrough. Well-executed RBAC + PKI + least privilege avoids months of rework when the team grows. The value is in having it from day zero, not in the patterns themselves.

#### 3. Security-First Product Engineering

The conventional approach treats security as a layer added on top of functionality. Tuttle treats security as the foundation that functionality is built upon:

- PKI hierarchy deployed before any application service
- Signed commits mandatory before any code is written
- Fail-closed patterns chosen before any user-facing feature exists
- Supply chain security (SBOM, artifact signing) integrated into CI from Sprint 0

This front-loaded investment eliminates the "security debt" that forces most startups into expensive retrofits when they scale or face compliance requirements (NIS2, GDPR audits, SOC2).

#### 4. Sovereign Infrastructure as Commercial Differentiator

In the post-NIS2/GDPR regulatory environment, "self-hosted, HSM-backed, auditable, open-source" is not just a technical choice — it is a market positioning:

- Full audit trail verifiable by third parties (Journey 5 — External Auditor)
- Warrant canary signed by Root CA — cryptographic proof of non-compromise
- No SaaS dependency for secrets, CI, or identity management
- Every artifact signed and publicly verifiable

This positions Tuttle against consumer VPN providers (NordVPN, ProtonVPN) not on features but on **verifiable trust** — the infrastructure is not just claimed to be secure, it is provably secure through cryptographic evidence.

#### 5. Verifiable Trust — Cryptographic Proof Accessible to Non-Technical Users

Every VPN provider claims "we don't log." Every privacy product says "your data is safe." These are **trust-me** claims — the user must take the company's word for it. No consumer VPN provider today offers cryptographically verifiable proof of its claims.

Tuttle's innovation is **verify-don't-trust**: the infrastructure produces cryptographic evidence that non-technical users can verify:

- **Signed releases** — every binary, container, and package cryptographically signed with HSM-backed keys, chain traceable to Root CA
- **Signed SBOM** — machine-readable inventory of every dependency, signed per release
- **Signed warrant canary** — public attestation of non-compromise, signed by Root CA, verifiable by anyone with the public key
- **Cryptographic audit trail** — non-modifiable history of certificate issuance, release signatures, and signed commits

**The UX paradigm: "show, don't tell."** The proof exists in the infrastructure, but the differentiator is making it accessible to Christophe — not as a white paper, but as an experience:

- In-app integrity verification: a single button that checks the running application's signature chain and displays ✅ or ❌
- Public verification page: release signatures + checksums + SBOM accessible without authentication
- CLI tool (`tuttle verify`) for technical users performing independent audits
- Application self-integrity check on every launch — the app verifies itself before the user uses it

The user doesn't need to understand X.509 certificates or SBOM formats. The app translates cryptographic proof into a simple trust signal: **"This application is exactly what was built, signed, and published. Verify it yourself."**

### Market Context & Competitive Landscape

The innovation is contextual, not absolute:

| Aspect | Industry State | Tuttle's Approach |
|--------|---------------|-------------------|
| **Enterprise PKI** | Standard in Fortune 500, absent in startups | Applied from day 0 at startup scale |
| **AI agent identity** | Emerging (Google, Microsoft roadmaps 2025-2026) | Implemented via existing PKI with X.509 extensions |
| **GitOps communication** | Flux/ArgoCD for infra, not for inter-project governance | Extended to asynchronous project-to-project coordination |
| **Solo founder + AI agents** | Common for code generation, rare for infrastructure governance | AI agents as full participants in BMAD lifecycle with cryptographic identity |
| **Sovereign consumer product** | ProtonVPN (Swiss jurisdiction), Mullvad (Swedish) | Self-hosted, user-verifiable, open infrastructure |
| **Verifiable trust (B2C)** | No consumer VPN offers cryptographic proof of claims | User-facing verification: in-app integrity check, public signatures, warrant canary |

### Validation Approach

The innovative aspects require validation at different stages:

| Innovation Claim | Validation Method | Sprint Gate |
|-----------------|-------------------|-------------|
| AI agents can bootstrap enterprise infra | BMAD workflows produce functional infrastructure with signed artifacts | Sprint 0 |
| Governance overhead is sustainable | Sprint 0 retrospective — measure time spent on governance vs feature development. **Threshold: governance overhead < 40% of sprint capacity. Above 40% = red flag requiring simplification** | Sprint 0 |
| Delegation-ready architecture works for first hire | Onboarding test: new contributor operates within governance constraints (Journey 4) | Sprint 2 |
| Security-first doesn't block velocity | Sprint velocity metrics — compare to equivalent projects without governance overhead | Sprint 1+ |
| Sovereign positioning attracts subscribers | Beta feedback + conversion metrics from tuttle-store | Post-Sprint 2 |
| Fail-closed is acceptable for family users | User testing — Christophe's reaction to maintenance screens (Journey 6 echo) | Sprint 2 |
| Verifiable Trust resonates with users | Beta users shown in-app verification — measure engagement rate and comprehension | Post-Sprint 2 |

### Risk Mitigation

| Innovation Risk | Consequence | Mitigation |
|----------------|-------------|------------|
| **Governance overhead slows development** | Competitors ship faster, product arrives too late | Automation target: M3 > 60%, M6 > 80%. AI agents absorb governance cost. If overhead > 40% of sprint velocity at Sprint 0 retrospective, simplify |
| **Solo bootstrap with AI proves insufficient** | Infrastructure quality gaps, security theater instead of security | External audit before V1 launch. Runbook tested by third party (PM-002). If audit fails, pause and hire |
| **Sovereign positioning has no market** | Product built for a market that doesn't exist | Beta validation Sprint 2. If < 50 interested users after beta, pivot to managed service model |
| **Security-first creates UX friction** | Christophe notices the complexity (violates FP-005) | UX testing at every sprint gate. If any technical concept is exposed to end user, it's a bug |
| **AI agent quality plateau** | Agents handle 80% of work but remaining 20% (edge cases, security, architecture) requires human expert — "solo + AI" model hits a ceiling and doesn't scale | Define at Sprint 0 which workflows are AI-safe vs human-only. Track agent rework rate per sprint. If agent PRs require human correction > 30% of the time, the model needs adjustment: either hire for the human-only workflows or reduce governance scope to what AI can reliably handle. Probability: medium. Impact: high |

## Platform Orchestrator Specific Requirements

### Project-Type Overview

tuttle-master is classified as `platform_orchestrator` — a project type not found in standard CSV taxonomies because it produces **no executable code**. Its output is exclusively configuration, contracts, governance documents, and coordination artifacts.

**Architectural paradigm — Vision B: Module-Driven Configuration**

tuttle-master is a **pure configuration project** that configures the `@quentin/bmad-multi-project` module. The module provides all inter-project orchestration capabilities (mailbox, hierarchy, transmissions, isolation, contracts, triage, escalation). tuttle-master provides the **what** — the module provides the **how**.

| Responsibility | Owner |
|---------------|-------|
| Mailbox protocol, transmission types, auto-triage engine, escalation rules, isolation enforcement | `bmad-multi-project` module |
| Hierarchy validation (DAG, cycles, semantic checks) | `bmad-multi-project` module |
| Contract management workflows (creation, validation, sync) | `bmad-multi-project` module |
| Stack governance workflows (proposal → validation → sync) | `bmad-multi-project` module |
| Git policy enforcement workflows | `bmad-multi-project` module |
| CI template management | `bmad-multi-project` module |
| Which 9 projects exist and their hierarchy | tuttle-master configuration |
| Which contracts bind which projects (Store↔Provisioner, Provisioner↔VPN) | tuttle-master configuration |
| Which stack technologies are validated (versions, frameworks) | tuttle-master configuration |
| PKI implementation choices (step-ca, HSM Nitrokey, TTL policies) | tuttle-master configuration |
| Business domain specifics (sovereign infrastructure, family protection) | tuttle-master configuration |
| Infrastructure choices (Gitea, K8s, Grafana, Teleport, Infisical) | tuttle-infra project |

**Implication:** Any requirement in this PRD that describes a **generic multi-project orchestration capability** is a requirement for the `bmad-multi-project` module, not for tuttle-master. This PRD documents only the Tuttle-specific configuration and the expectations tuttle-master has of the module.

### Module Dependency

tuttle-master depends on `@quentin/bmad-multi-project` module providing the following capabilities. If the module does not yet support a capability, it becomes a requirement for the module's own PRD — not an implementation task for tuttle-master.

**Required module capabilities:**

| Capability | Current Module State | Gap |
|-----------|---------------------|-----|
| Mailbox system (inbox/outbox/archive/triage/escalation) | ✅ Complete | — |
| Hierarchy management (hierarchy.csv, DAG validation) | ✅ Complete | — |
| Ecosystem status dashboard | ✅ Workflow exists | Grafana integration missing |
| Init multi-project (add children, auto + manual) | ✅ Complete | — |
| Transmissions (send/receive/check-inbox) | ✅ Complete | — |
| Isolation rules (access matrix enforcement) | ✅ Defined | Enforcement via git hooks missing |
| Deprecation management | ✅ Template exists | — |
| Contract management (create, validate, sync) | ❌ Not in module | Module PRD requirement |
| Git policy enforcement (branches, PR rules, signing) | ❌ Not in module | Module PRD requirement |
| Stack governance (children propose → master validates → sync) | ❌ Not in module | Module PRD requirement |
| CI/CD template management (common pipeline, per-project adaptation) | ❌ Not in module | Module PRD requirement |
| Version compatibility matrix (inter-project version pinning) | ❌ Not in module | Module PRD requirement |
| Git hooks → trigger agent workflows | ❌ Not in module | Module PRD requirement |

**Module maturity expectation:** The module must be sufficiently complete that configuring a new multi-project ecosystem is a guided, configuration-only process — no custom orchestration code required in the master project.

### Technical Architecture Considerations

#### Orchestration Model

tuttle-master configures a **file-based, Git-native orchestration model**:

- **Current:** Markdown files in `_mailbox/` directories, processed by BMAD agent workflows via the module
- **Evolution path:** Git hooks (pre-commit, post-merge, post-receive) trigger BMAD agent workflows automatically. A Gitea webhook detects changes in `api/` or `contracts/` directories and creates transmissions without human initiation
- **Non-goal for V1:** No REST API, no event bus, no message broker. The module handles all coordination via Git

**tuttle-master configures:**
- Which transmission types are used (all 10 defined in module)
- Escalation SLAs (critical: 24h, standard: 7d)
- Auto-triage rules per project (which types are auto-handled)
- Subscription filters per project (interested_in, ignore)

#### Contract Management

Contracts between projects are **markdown descriptive documents**, not machine-readable schemas:

- Contracts live in each project's `contracts/` directory
- Master holds the canonical version; children hold a synced copy
- Validation is human + master review, not automated parsing
- CI hook (FM-005) blocks merge if `contracts/` changes without transmission — this is a **module capability** (git hook enforcement)
- Evolution to OpenAPI/JSON Schema is a Growth Feature, not V1

**tuttle-master configures:**
- Which contracts exist: Store↔Provisioner (payment-webhook-spec, subscription-lifecycle), Provisioner↔VPN (vpn-user-api-spec, vpn-status-polling)
- Contract ownership (which project is source of truth for each contract)
- Sync policy (master always CC'd on contract transmissions)

#### Project Lifecycle Governance

Adding a new project to the ecosystem requires **master opinion + human validation**:

1. Proposal (human or agent suggests new project)
2. Master analysis (module's init-multi-project workflow in auto mode: analyze architecture, propose hierarchy position, dependencies)
3. Master opinion (BMAD agent evaluates fit: does this project belong? where in hierarchy? what contracts needed?)
4. Human validation (Quentin approves/rejects/modifies)
5. Execution (module handles: repo creation, BMAD install, mailbox init, hierarchy.csv update, submodule add, Gitea config)

**tuttle-master configures:**
- Approval workflow (master opinion mandatory, human final authority)
- Required infrastructure per child (Gitea remote + GitHub backup, Woodpecker CI, Infisical namespace)
- BMAD installation profile (`npx bmad-method@alpha install` + `npx @quentin/bmad-multi-project@latest`)

#### Ecosystem Monitoring

**Target: Grafana dashboard on existing K8s cluster.**

The module's ecosystem-status workflow currently produces a text-based report. For tuttle-master, the monitoring chain is:

1. **Data collection:** Module workflows query each project's status (YAML files, mailbox state, hierarchy.csv)
2. **Metrics export:** Prometheus metrics exposed from a lightweight exporter that parses module output (transmission counts, inbox depth, alert counts, sprint progress)
3. **Visualization:** Grafana dashboard on existing K8s cluster displays ecosystem health
4. **Alerting:** Grafana alerts for 🔴 conditions (critical transmission > 24h, inbox > 10, stale transmission)

**tuttle-master configures:**
- Alert thresholds (green/orange/red per Success Criteria)
- Dashboard layout (prioritized hierarchical view, alerts sorted by severity)
- Notification channels (push notification for 🟠/🔴)

**Module gap:** The module needs to produce machine-readable output (JSON/YAML metrics) that a Prometheus exporter can scrape. Currently it produces human-readable markdown only.

#### Stack Technology Governance

Technology choices follow a **bottom-up proposal, top-down validation** model:

1. **Child proposes:** A child project identifies a technology need (e.g., "tuttle-store needs PostgreSQL 16.2 + Prisma ORM 5.x")
2. **Transmission:** Child sends `architectural` transmission to master with rationale
3. **Master evaluates:** BMAD agent (architect) reviews against ecosystem constraints (compatibility, security, maintenance burden)
4. **Human validates:** Quentin approves specific versions (not ranges — exact pinned versions)
5. **Sync:** Approved stack documented in master (technology registry). All projects using that technology pin to the validated version
6. **Enforcement:** Version drift detected by CI (module capability) — transmission generated if a project uses a non-validated version

**tuttle-master configures:**
- Technology registry (approved technologies + exact versions)
- Validation criteria (security audit status, license compatibility, maintenance activity)
- Drift tolerance (zero for security-critical dependencies, minor version flexibility for dev tools)

### Repository Structure

tuttle-master contains its children as **git submodules** with dual remotes (Gitea primary + GitHub backup):

```
tuttle-master/                  ← repo Git principal
├── _bmad/                      ← BMAD framework (installed via npx)
├── _bmad-output/               ← Planning artifacts (PRD, Architecture, Epics, Sprint Status)
├── _mailbox/                   ← Mailbox system (module-managed)
├── hierarchy.csv               ← Ecosystem hierarchy (module-validated, source of truth)
├── contracts/                  ← Canonical interface contracts (markdown)
├── technology-registry/        ← Approved stack with pinned versions
├── policies/                   ← Git policy, security policy, governance rules
├── templates/                  ← CI templates, PR templates, issue templates
├── docs/                       ← Runbook, glossary, getting started, onboarding
├── tuttle-infra/               ← git submodule (Gitea + GitHub)
├── tuttle-libs/                ← git submodule
├── tuttle-store/               ← git submodule
├── tuttle-vpn/                 ← git submodule
├── tuttle-provisioner/         ← git submodule
├── tuttle-apps/                ← git submodule (parent node)
│   ├── android/                ← git submodule
│   └── windows/                ← git submodule
└── tuttle-key/                 ← git submodule (brownfield migration)
```

No `src/`, no `lib/`, no application code. Every file is either a configuration consumed by the module, a governance document consumed by humans/agents, or a template consumed by child projects.

**Submodule policies:**
- Submodule ref updated on each release tag of child (not on every commit)
- `git clone --recurse-submodules` brings entire ecosystem; GitHub fallback if Gitea unavailable
- Each child is a sovereign BMAD project with its own `_bmad/`, `_mailbox/`, `hierarchy.csv`
- Master NEVER writes to child content (isolation rule — only exception: init + deprecated.md)

### Implementation Sequence

The Vision B dependency chain:

1. **Finish tuttle-master PRD** (current) — documents configuration needs and module expectations
2. **Create bmad-multi-project module PRD** — gaps identified here become module requirements
3. **Develop/extend module** via bmb (module builder) — contracts, git policy, stack governance, CI templates, metrics export, git hooks
4. **Configure tuttle-master** — pure configuration once module ready
5. **Sprint 0 module readiness gate:** module must support mailbox + hierarchy + init (already complete) plus contract management and git policy enforcement (gaps to fill)

Sprint 0 infrastructure (tuttle-infra: K8s, Gitea, step-ca, Teleport, Infisical) runs in parallel — it does not depend on the module.

## Project Scoping & Phased Development

### MVP Strategy & Philosophy

**MVP Approach:** Infrastructure Coordination MVP — validate the complete document-coordination cycle across 3 projects before adding security layers and scaling.

tuttle-master's MVP is not a feature set — it is the **proven ability to coordinate BMAD documents across multiple projects through self-sufficient transmissions**. The minimum viable product is a functioning coordination loop where a decision in one project propagates to all impacted projects, modifies their BMAD documents via guided workflows, and maintains cross-project coherence.

**Coordination Model:** Hub-strict (ADR-003). All child-to-child communication transits through master. Master validates and redistributes. Each transmission is **self-sufficient** — it contains enough context (what, why, which documents are impacted, expected action, acceptance criteria) to be processed in isolation by a separate Claude Code session in the target project. This constraint derives from the operational model: one operator, separate Claude Code sessions per project, zero context bleed between sessions.

**Resource Model:** Solo operator (Quentin) + BMAD AI agents, each operating within their project scope via separate Claude Code sessions.

### MVP Feature Set (Phase 1)

**Core Capability: End-to-End Transmission Cycle**

The MVP validates three transmission directions:
1. **Master → children:** A decision made in master (e.g., UX strategy, security policy) generates a self-sufficient transmission to impacted children. Each child's Claude Code session processes the transmission via BMAD workflow, modifies relevant documents (PRD, Architecture, Epics), and requests human validation.
2. **Child → master → impacted children:** A discovery in a child project (e.g., new feature need, API change) generates a transmission to master. Master validates, integrates into its governance documents, and redistributes to impacted projects.
3. **Cross-project coherence:** After a transmission cycle completes, all impacted BMAD documents across all projects reflect the change consistently.

**Must-Have Capabilities:**

| Capability | Validation Criterion |
|-----------|---------------------|
| Mailbox functional on 3+ projects | inbox/outbox/sent/archive present, transmission send/receive works |
| Complete transmission cycle (3 directions) | Master→child, child→master, child→master→children all produce document changes |
| Self-sufficient transmissions | A Claude Code session in the target project processes the transmission without any context from the source project |
| BMAD transmission processing workflows | correct-course, transmission handling, human validation — all guided by BMAD |
| hierarchy.csv + DAG validation | DAG (Directed Acyclic Graph — dependency graph with no cycles, ensuring a valid build order: infra → libs → store‖vpn → provisioner → apps) validated in CI |
| Markdown contracts between projects | Interface specifications in `contracts/`, canonical version in master |
| CI hook FM-005 | Merge blocked if changes in `contracts/` or `api/` without associated transmission |
| Git submodules (3 projects minimum) | Recursive clone functional, dual remote Gitea + GitHub |
| Project isolation enforced | Master never writes to child content, separate Claude Code sessions per project |

**Essential User Journeys Supported in Phase 1:**
- Journey 1 (Morning of the Conductor) — partial: dashboard, triage, transmission processing
- Journey 3 (Transmission Changes a Contract) — full cycle
- Journey 8 (Day Zero Bootstrap) — partial: 3 projects initialized

### Post-MVP Features

**Phase 2 (Security & Automation):**

| Feature | Rationale |
|---------|-----------|
| PKI hierarchy (Root CA / step-ca / leaf certs) | Security-first foundation, enables all signing and mTLS |
| Mandatory commit signing (human + agents) | Cryptographic traceability |
| Infisical secrets management (isolated per project) | Secrets policy enforcement |
| Auto-triage mailbox (8 transmission types) | Reduces manual triage as projects scale from 3 to 5+ |
| Escalation system (SLA alerts) | Prevents stale transmissions |
| Common CI template | Consistency across 5+ projects |
| Observability (Prometheus + Grafana) | Ecosystem health monitoring |
| Release signing (binaries, containers, packages) | Supply chain security |
| mTLS inter-services via step-ca | Network security |
| 5+ active projects | Store, VPN, provisioner added to ecosystem |

**Phase 3 (Validation & Scale):**

| Feature | Rationale |
|---------|-----------|
| 9 projects fully initialized | Complete V1 ecosystem |
| Ecosystem-status Grafana dashboard | Prioritized view, alerts by severity |
| Operational runbook (PM-002) | Third-party testable without Quentin |
| Cryptographic audit trail | Non-modifiable, queryable history |
| Public verification (signatures, SBOM, warrant canary) | Verifiable Trust for external parties |
| Contributor onboarding (Journey 4) | Validates delegation-ready architecture |
| Client apps (Android + Windows) | End-user products integrated |
| WCAG 2.1 AA accessibility | Automated CI tests |

### Risk Mitigation Strategy

**Technical Risks:**

| Risk | Mitigation |
|------|------------|
| Self-sufficient transmission quality — incomplete briefs cause wrong document modifications in target project | Structured transmission template with mandatory fields (context, decision, impacted documents, expected action, acceptance criteria). BMAD workflow validates completeness before send |
| Module gaps block Phase 1 | Phase 1 uses only capabilities the module already supports (mailbox, hierarchy, transmissions, init). 7 identified gaps are Phase 2/3 requirements |
| Context bleed between projects in same Claude session | Operational rule: separate Claude Code sessions per project (model A). No exception |

**Market Risks:**

| Risk | Mitigation |
|------|------------|
| Document coordination model doesn't scale beyond 3 projects | Phase 1 validates at 3 projects. If coordination breaks at 3, simplify before scaling |
| Transmission overhead exceeds governance < 40% threshold | Measure at Phase 1 retrospective. If > 40%, reduce mandatory transmission types |

**Resource Risks:**

| Risk | Mitigation |
|------|------------|
| Solo operator + AI model insufficient for 9 projects | Phase 2 targets auto-triage > 80%. If agent rework rate > 30%, hire for human-only workflows |
| bmad-multi-project module development delays Phase 2 | Phase 1 independent of module gaps. Module PRD created in parallel |

## Functional Requirements

**Capability contract:** Every feature in the final product must trace to a functional requirement listed below. Capabilities not listed here will not exist in the product. 58 FRs across 10 capability areas, enriched through 1 round Party Mode (9 proposals, 9 accepted).

### Ecosystem Hierarchy & Structure

- **FR1:** Operator can define the project hierarchy as a DAG (Directed Acyclic Graph) declaring all projects, their parent-child relationships, and inter-project dependencies
- **FR2:** System can validate the hierarchy for structural integrity (parsable format, no cycles, valid parent references, existing paths) on every change
- **FR3:** Operator can add a new child project to the ecosystem through a guided workflow that analyzes fit, proposes hierarchy position, and requires human approval
- **FR4:** Operator can clone the entire ecosystem recursively, with automatic fallback to a backup remote if the primary remote is unavailable
- **FR5:** System can enforce that each child project is a sovereign entity with its own BMAD structure, mailbox, and local hierarchy reference
- **FR52:** System can synchronize submodule references in master when a child project creates a release tag, following the defined sync policy

### Transmission & Mailbox

- **FR6:** Operator or agent can send a typed transmission from any project to one or more target projects, with master always in CC
- **FR7:** System can deliver transmissions to target projects' inboxes via the mailbox file-based protocol (outbox → inbox)
- **FR8:** Each transmission can carry self-sufficient context: what changed, why, which documents are impacted, expected action, and acceptance criteria — enabling processing in isolation without source project context
- **FR9:** Operator can consult a project's inbox and process pending transmissions via a guided BMAD workflow
- **FR10:** System can auto-triage transmissions by type (info→archive, bug→create story, dependency→backlog, architectural→escalate)
- **FR11:** System can detect stale transmissions (pending beyond SLA threshold) and generate escalation alerts
- **FR12:** System can detect contradictory transmissions impacting the same contract and escalate them with a conflict tag for human resolution
- **FR13:** System can archive processed transmissions with resolution status and traceability
- **FR50:** Operator can configure transmission processing rules per type (which workflow triggers, which documents are impacted, which validations are required) for each project
- **FR51:** System can validate transmission completeness before send (mandatory fields present, target projects exist in hierarchy, referenced documents exist) and reject incomplete transmissions

### Contract Management

- **FR14:** Operator can define interface contracts between projects as structured markdown documents
- **FR15:** System can maintain a canonical version of each contract in master and synced copies in child projects
- **FR16:** System can block code merges that modify contract or API directories without an associated transmission (CI hook FM-005)
- **FR17:** Operator can update a contract through the correct-course workflow, propagating changes to all impacted projects via transmission
- **FR18:** Operator can identify at a glance which contracts exist between which projects
- **FR53:** System can execute automated contract tests that validate alignment between master-defined contracts and child project implementations on every merge

### Document Coordination & Coherence

- **FR19:** When a transmission is processed in a target project, the BMAD workflow can modify the relevant documents (PRD, Architecture, Epics, Sprint Status) and request human validation
- **FR20:** System can maintain cross-project document coherence: after a transmission cycle completes, all impacted BMAD documents reflect the change consistently
- **FR21:** Operator can trace any document change back to the transmission that triggered it
- **FR22:** System can detect divergence between master governance documents and child project implementation (living documentation principle)

### Governance & Policies

- **FR23:** Operator can define and enforce a unified git policy across all projects (branching strategy, protected branches, naming conventions, PR/review policy, release tagging, submodule sync)
- **FR24:** Operator can define a secrets management policy with per-project namespace isolation and per-environment stratification
- **FR25:** Operator can define a technology registry of approved technologies with pinned versions
- **FR26:** System can detect version drift when a child project uses a non-validated technology version and generate a transmission
- **FR27:** Operator can define deprecation policies with migration windows, and system can track migration compliance with automated reminders
- **FR28:** System can enforce project isolation: no project can write to another project's content (except master during init)
- **FR55:** Operator can define the PR escalation chain per project and per branch (auto-merge rules for dev, required reviews for staging, human-only merge for main)
- **FR56:** Operator can add new CI validation rules (linter rules, forbidden patterns, mandatory checks) to the common CI template and propagate them to all projects via transmission

### Ecosystem Monitoring & Status

- **FR29:** Operator can view the current status of all projects, their phases, progress, active alerts, and pending transmissions in a single dashboard
- **FR30:** Operator can identify the most critical project and next required action within 10 seconds of reading the dashboard
- **FR31:** System can generate a weekly health check report consolidating ecosystem status (green/orange/red)
- **FR32:** System can push notifications to the operator when ecosystem health degrades (orange or red conditions)
- **FR33:** System can generate a morning digest summarizing overnight activity (pending transmissions, PRs, alerts, health status)
- **FR57:** Operator can view a visual dependency graph of all projects and their contracts, maintained up to date as contracts evolve
- **FR58:** Operator can track sprint progress across all active projects simultaneously, with per-project and per-epic progress visible from master

### Identity & Access

- **FR34:** Operator can provision cryptographic identities for humans, BMAD agents, CI services, automated bots, and application services through a unified PKI
- **FR35:** System can enforce mandatory signed commits on all projects for all identity types
- **FR36:** System can enforce tier-based access: agents limited by certificate tier, humans with elevated privileges, CI with job-scoped ephemeral access
- **FR37:** Operator can revoke any identity immediately, with revocation propagating within 5 minutes
- **FR38:** System can automatically renew certificates before expiration without service interruption

### Supply Chain & Artifact Integrity

- **FR39:** System can sign all release artifacts (binaries, containers, packages) with HSM-backed keys
- **FR40:** System can generate and sign a Software Bill of Materials (SBOM) for every release
- **FR41:** System can verify dependency signatures on input and sign artifacts on output in CI pipelines
- **FR42:** External party can verify any release artifact's signature chain back to the root CA using public keys

### Disaster Recovery & Resilience

- **FR43:** Operator can execute a documented recovery procedure for each critical infrastructure component (git hosting, secrets manager, container orchestrator, database)
- **FR44:** System can maintain dual remotes (primary + backup) for every project, with automatic fallback on primary unavailability
- **FR45:** System can operate fail-closed: halt services rather than pass traffic or data in an insecure state on failure
- **FR54:** Operator can rollback any individual project independently without breaking cross-project interfaces, using the version compatibility matrix to identify retro-compatible versions

### Onboarding, Documentation & Sprint Tracking

- **FR46:** New contributor can become operational in the ecosystem through a guided Getting Started flow without synchronous onboarding from the operator
- **FR47:** Third party can execute the operational runbook and resolve a simulated incident without the operator's help
- **FR48:** System can maintain a unified glossary of technical and user-facing terms accessible across all projects

### Phase Alignment

- **FR49:** Each FR maps to a development phase: Phase 1 (MVP — coordination validation on 3 projects), Phase 2 (security & automation on 5+ projects), or Phase 3 (validation & scale on 9 projects)

**Phase Mapping:**

| Phase | FRs |
|-------|-----|
| **Phase 1 (MVP)** | FR1-FR9, FR13-FR22, FR28-FR30, FR44, FR48-FR51 |
| **Phase 2 (Security & Automation)** | FR10-FR12, FR23-FR27, FR31-FR33, FR34-FR38, FR39-FR41, FR45, FR52-FR53, FR55-FR56, FR58 |
| **Phase 3 (Validation & Scale)** | FR42-FR43, FR46-FR47, FR54, FR57 |

**Party Mode Enrichment (Round 1 — 9 proposals, 9 accepted):**

| FR | Source | Contribution |
|----|--------|-------------|
| FR50 | John (PM) | Transmission processing rules configuration per type |
| FR51 | Winston (Architect) | Transmission completeness validation before send |
| FR52 | Winston (Architect) | Submodule sync on release tag |
| FR53 | Murat (TEA) | Automated contract tests (PRD ↔ implementation) |
| FR54 | Murat (TEA) | Independent per-project rollback |
| FR55 | Mary (Analyst) | PR escalation chain definition per project/branch |
| FR56 | Mary (Analyst) | CI rule enrichment and propagation via transmission |
| FR57 | Sally (UX) | Visual dependency graph of projects and contracts |
| FR58 | Bob (SM) | Cross-project sprint progress tracking |

## Non-Functional Requirements

### Part A — Master's Own NFRs

*Quality attributes of tuttle-master itself — how well master performs its coordination and governance role.*

#### Coordination Performance

| NFR | Criterion | Measure |
|-----|-----------|---------|
| **NFR-COORD-01** | Hierarchy validation speed | DAG validation (syntax + semantics + cycle detection) completes < 30 seconds for up to 20 projects |
| **NFR-COORD-02** | Ecosystem status generation | Dashboard data collection across all projects completes < 2 minutes |
| **NFR-COORD-03** | Project count scaling | Coordination quality consistent from 3 to 9+ projects without degradation of triage, transmission processing, or document coherence |
| **NFR-COORD-04** | Transmission volume capacity | Mailbox system navigable with up to 50 active transmissions across all projects without triage backlog |
| **NFR-COORD-05** | Contract navigability | Contract management remains navigable with up to 20 inter-project contracts (visual graph required at 10+) |

#### Operability

| NFR | Criterion | Measure |
|-----|-----------|---------|
| **NFR-OPS-01** | Governance overhead | Governance activities (triage, reviews, transmissions, contract updates) consume < 40% of sprint capacity. Above 40% = simplification required |
| **NFR-OPS-02** | Operator context switching | Operator switches between projects with zero context loss — all state in living documents accessible via workflow-status per project and ecosystem-status globally |
| **NFR-OPS-03** | Dashboard readability | Most critical project and next required action identifiable in < 10 seconds of reading ecosystem-status |
| **NFR-OPS-04** | Autonomous operation | System operates without operator for 48h+ during routine periods. Only decision-requiring events (main merge, prod incident, contract conflict) need human |
| **NFR-OPS-05** | Agent rework rate | Agent PRs requiring human correction < 30% of total. Above 30% = model adjustment needed |

#### Transmission Quality

| NFR | Criterion | Measure |
|-----|-----------|---------|
| **NFR-TX-01** | Transmission self-sufficiency | 100% of transmissions processable in isolation by a Claude Code session in the target project without source project context |
| **NFR-TX-02** | Transmission completeness validation | Zero incomplete transmissions sent — mandatory fields validated before dispatch |
| **NFR-TX-03** | Transmission traceability | Every document change in any project traceable to the transmission that triggered it. Chain: transmission → workflow → document change → human validation |
| **NFR-TX-04** | Transmission compatibility multi-workflow | Master transmissions are processable by projects using bmm (standard projects) AND bmb (module builder) workflows. Transmission format is workflow-agnostic |

#### Document Coherence

| NFR | Criterion | Measure |
|-----|-----------|---------|
| **NFR-DOC-01** | Cross-project consistency | After a transmission cycle completes, zero contradictions between master governance documents and impacted child documents |
| **NFR-DOC-02** | Document freshness | Gap between child API change and master contract update < 48h |
| **NFR-DOC-03** | Runbook completeness | Third party resolves simulated incident using runbook alone without operator assistance |
| **NFR-DOC-04** | Onboarding efficiency | New contributor operational via Getting Started guide without synchronous onboarding from operator |

### Part B — Standards Master Imposes on the Ecosystem

*Quality requirements that master defines as governance. Master documents and enforces these; child projects and module implement them.*

**Propagation mechanism:** Standards that master imposes on the ecosystem are propagated via `architectural` transmissions. For standard child projects (bmm workflow), the transmission triggers correct-course or PRD update workflows. For the bmad-multi-project module (bmb workflow, git@git.pelerin.lan:quentin/multi-project.git), the transmission triggers the module's own requirement integration process. The transmission format is identical — only the processing workflow differs in the target project.

**Module as ecosystem member:** `@quentin/bmad-multi-project` is a child in the hierarchy DAG, developed via bmb workflow. Standards marked "enforced by: bmad-multi-project module" in Part B become functional requirements in the module's own PRD, propagated via architectural transmission from master.

#### Security Standards

Master requires that:

| Standard | Requirement | Enforced by |
|----------|-------------|-------------|
| **STD-SEC-01** | Zero cleartext secrets in any repository | CI scan per project (module CI template) |
| **STD-SEC-02** | Certificate revocation propagation < 5 minutes | step-ca + CRL/OCSP (tuttle-infra) |
| **STD-SEC-03** | 100% signed commits on protected branches | Gitea/GitHub pre-receive hooks (tuttle-infra) |
| **STD-SEC-04** | Zero unsigned artifacts in staging/prod | CI gate per project (module CI template) |
| **STD-SEC-05** | Per-project secrets namespace isolation | Infisical config (tuttle-infra) |
| **STD-SEC-06** | Zero PII in logs | CI regex scan per project (module CI template) |
| **STD-SEC-07** | Agent blast radius limited by cert tier | step-ca cert attributes + Teleport roles + Gitea permissions (tuttle-infra + module) |
| **STD-SEC-08** | Fail-closed on all critical systems | Each child project's implementation |
| **STD-SEC-09** | Zero critical vulnerability untreated > 7 days | CI dependency scan per project (module CI template) |
| **STD-SEC-10** | Inter-service communication via mTLS or K8s Network Policies only | tuttle-infra + each child project |

#### Reliability Standards

Master requires that:

| Standard | Requirement | Enforced by |
|----------|-------------|-------------|
| **STD-REL-01** | Git hosting availability 99.5% monthly, GitHub backup for read-only during downtime | tuttle-infra |
| **STD-REL-02** | RPO < 24h, RTO < 4h for critical components | tuttle-infra (Gitea, Infisical, K8s, PostgreSQL) |
| **STD-REL-03** | Zero service interruption from certificate expiration (ACME auto-renewal) | tuttle-infra (step-ca) |
| **STD-REL-04** | Recursive clone succeeds with one remote unavailable | Git submodule config (module + tuttle-infra) |
| **STD-REL-05** | Per-project rollback < 30 minutes without breaking cross-project interfaces | Each child project + version compatibility matrix (module) |
| **STD-REL-06** | HSM failover < 15 minutes | tuttle-infra |

#### Performance Standards

Master requires that:

| Standard | Requirement | Enforced by |
|----------|-------------|-------------|
| **STD-PERF-01** | Transmission delivered outbox → target inbox < 5 minutes | bmad-multi-project module |
| **STD-PERF-02** | Full CI pipeline < 15 minutes per project | Woodpecker + module CI template |
| **STD-PERF-03** | HSM sustains 50-100 signing operations/hour | tuttle-infra |
| **STD-PERF-04** | Auto-triage rate: M3 > 80%, M6 > 90%, M12 > 95% | bmad-multi-project module |

#### Integration Standards

Master requires that:

| Standard | Requirement | Enforced by |
|----------|-------------|-------------|
| **STD-INT-01** | Gitea as primary (tea CLI + MCP Gitea), GitHub as backup (gh CLI) | tuttle-infra + each child project |
| **STD-INT-02** | step-ca ACME (RFC 8555) for automated certificate lifecycle | tuttle-infra |
| **STD-INT-03** | Teleport for all infrastructure access (SSH, K8s, databases) | tuttle-infra |
| **STD-INT-04** | Infisical E2E encrypted secrets per namespace, per environment | tuttle-infra |
| **STD-INT-05** | Prometheus-scrapable metrics + Grafana dashboards | tuttle-infra + module metrics export |
| **STD-INT-06** | Woodpecker CI with common template, mandatory stages | Module CI template + each child |

#### Traceability Standards

Master requires that:

| Standard | Requirement | Enforced by |
|----------|-------------|-------------|
| **STD-TRACE-01** | Every action cryptographically attributable to a specific identity | step-ca cert attributes (tuttle-infra) |
| **STD-TRACE-02** | Audit trail append-only, non-modifiable, queryable | tuttle-infra |
| **STD-TRACE-03** | Release signatures, checksums, SBOM, warrant canary publicly verifiable | Each child project + tuttle-infra |
| **STD-TRACE-04** | 5-category identity model enforced through certificate attributes | step-ca config (tuttle-infra) |

#### Accessibility Standards

Master requires that:

| Standard | Requirement | Enforced by |
|----------|-------------|-------------|
| **STD-ACC-01** | WCAG 2.1 AA for all user-facing projects, axe-core in CI | Module CI template + each user-facing child |
| **STD-ACC-02** | Zero technical concepts exposed to end users (FP-005) | Each user-facing child project |

**Summary:** 18 NFRs propres à master (Part A) + 28 standards imposés à l'écosystème (Part B) with clear ownership of enforcement. Module bmad-multi-project (bmb workflow) treated as ecosystem child — standards propagated via architectural transmissions.

## PRD Scope Separation (ADR-011)

### Decision Context

During the polish review (Step 11), analysis revealed that 30 of 58 functional requirements describe **generic multi-project orchestration capabilities** that belong to the `bmad-multi-project` module, not to tuttle-master's Tuttle-specific configuration. The remaining 28 FRs describe either Tuttle-specific configuration or ecosystem obligations that master defines.

This separation is consistent with the Vision B architectural decision (Step 7): tuttle-master is pure configuration, the module provides the engine.

**Important:** Only the module separation is decided here. The question of which child project implements the infrastructure obligations (PKI, Gitea, Teleport, etc.) is **deliberately left open** — the architect and the natural BMAD transmission flow will determine this. Prematurely attributing FRs to tuttle-infra would short-circuit the discovery process.

### Two-Way FR Attribution

#### FRs Migrating to bmad-multi-project Module PRD (30 FRs)

These FRs describe **generic capabilities** that any multi-project ecosystem would need. They become functional requirements of the module's own PRD, developed via bmb workflow on `git@git.pelerin.lan:quentin/multi-project.git`.

| Capability Area | FRs | Description |
|----------------|-----|-------------|
| Hierarchy Engine | FR2, FR3, FR5, FR52 | DAG validation, project addition workflow, sovereignty enforcement, submodule sync |
| Mailbox Engine | FR6, FR7, FR8, FR9, FR13 | Send, deliver, self-sufficient format, inbox processing, archival |
| Triage & Escalation | FR10, FR11, FR12, FR50, FR51 | Auto-triage, stale detection, conflict detection, processing rules config, completeness validation |
| Contract Management | FR15, FR16, FR17, FR53 | Contract sync, CI hook enforcement, correct-course workflow, automated contract tests |
| Document Coordination | FR19, FR20, FR21, FR22 | Transmission→document modification, coherence checking, traceability, drift detection |
| Governance Enforcement | FR26, FR27, FR28, FR56 | Version drift detection, deprecation management, isolation enforcement, CI template propagation |
| Monitoring Engine | FR29, FR31, FR32, FR33, FR58 | Dashboard engine, health check, notifications, morning digest, sprint tracking |

**Plus the 7 gaps already identified at Step 7:** contract management workflows, git policy enforcement, stack governance, CI/CD template management, version compatibility matrix, git hooks→agent triggers, metrics export (JSON/YAML for Prometheus).

**Plus FR60** (identified during review): System can export ecosystem metrics in Prometheus-scrapable format — this is the metrics export gap already listed.

#### FRs Remaining in tuttle-master PRD (28 FRs)

These FRs describe **Tuttle-specific configuration** and **ecosystem obligations** that master defines. They fall into two subcategories:

**A. Tuttle Configuration (what master parameterizes):**

| FR | Tuttle-Master Configuration |
|----|----------------------------|
| **FR1** | Define the Tuttle hierarchy: child projects, their DAG dependencies (infra → libs → store‖vpn → provisioner → apps), bmad-multi-project as ecosystem member |
| **FR14** | Define which contracts exist between Tuttle projects: Store↔Provisioner (payment-webhook-spec, subscription-lifecycle), Provisioner↔VPN (vpn-user-api-spec, vpn-status-polling). Contract ownership rules |
| **FR18** | Visualize the Tuttle network: inter-project contracts graph (Excalidraw), navigable at a glance |
| **FR23** | Define Tuttle git policy: branching strategy, branch naming, protected branches, PR rules, release tagging (SemVer), submodule sync on release tag |
| **FR24** | Define Tuttle secrets policy: Infisical namespaces per project, environment stratification, rotation schedules |
| **FR25** | Define Tuttle technology registry: approved technologies with pinned versions, validation criteria, drift tolerance |
| **FR30** | Configure Tuttle dashboard: green/orange/red thresholds, alert priorities, notification channels |
| **FR46** | Write Tuttle Getting Started guide |
| **FR47** | Write Tuttle operational runbook |
| **FR48** | Maintain Tuttle unified glossary |
| **FR49** | Define Tuttle roadmap phases |
| **FR55** | Define Tuttle PR escalation rules per project/branch |
| **FR57** | Maintain Tuttle dependency graph (Excalidraw) |
| **FR59** | Create, sign, and publish Tuttle warrant canary |

**B. Ecosystem Obligations (what master requires, implementation determined by architect + BMAD flow):**

These FRs describe capabilities that the Tuttle ecosystem must have. Master documents the obligation; which child project implements it is an **architectural decision** that will emerge naturally through the BMAD workflow (architecture phase, transmissions, project creation).

| FR | Ecosystem Obligation |
|----|---------------------|
| **FR4** | Recursive clone with fallback to backup remote |
| **FR34** | Cryptographic identity provisioning (humans, agents, CI, bots, services) via unified PKI |
| **FR35** | Mandatory signed commits on all projects |
| **FR36** | Tier-based access enforcement (cert tier → capabilities) |
| **FR37** | Immediate identity revocation (< 5 min propagation) |
| **FR38** | Certificate auto-renewal without service interruption |
| **FR39** | Release artifact signing (HSM-backed) |
| **FR40** | Signed SBOM per release |
| **FR41** | CI verifies signatures on input, signs artifacts on output |
| **FR42** | Public verification of signature chain to root CA |
| **FR43** | Documented recovery procedures for critical infrastructure |
| **FR44** | Dual remotes (primary + backup) for every project |
| **FR45** | Fail-closed behavior on all critical systems |
| **FR54** | Independent per-project rollback without breaking interfaces |

### Propagation Mechanism

1. **tuttle-master PRD** (this document) documents Tuttle-specific configuration and ecosystem obligations
2. Master generates `architectural` transmission to `bmad-multi-project` containing: the 30 module FRs + 7 gaps + Part B standards marked "enforced by module"
3. Module PRD created via bmb workflow, integrating these as module FRs
4. During **architecture phase**, the architect determines which child project(s) implement the ecosystem obligations (FR4, FR34-FR45, FR54). This may result in transmissions to existing or new projects
5. The BMAD workflow naturally creates transmissions as obligations are assigned to projects
6. Once module is ready, master configures using module capabilities (pure configuration, zero code)

### Impact on NFRs

**Part A (Master's Own NFRs):** No change — these are genuinely about master's coordination quality.

**Part B (Standards Master Imposes):** The "Enforced by" column currently names specific projects (tuttle-infra, module, etc.). These attributions are **indicative, not prescriptive** — the architect will confirm or reassign during the architecture phase. Part B standards become NFRs in the target project PRDs when propagated via transmission.

### Implementation Sequence (Updated)

1. **Finish tuttle-master PRD** (current — Steps 8-10 done, polish pending)
2. **Create bmad-multi-project module PRD** via bmb workflow — 30 FRs + 7 gaps + Part B module standards become module requirements
3. **Architecture phase** — architect determines project structure, assigns ecosystem obligations to projects, creates transmissions
4. **Child project PRDs** created via bmm workflow as architect assigns obligations
5. **Develop/extend module** via bmb — implement generic multi-project capabilities
6. **Configure tuttle-master** — pure configuration once module ready

