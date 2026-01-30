---
stepsCompleted:
  - step-01-init
  - step-02-discovery
  - step-03-success
inputDocuments:
  - product-brief-tuttle-master-2026-01-27.md
workflowType: 'prd'
documentCounts:
  briefs: 1
  research: 0
  brainstorming: 0
  projectDocs: 0
date: 2026-01-29
author: Quentin
status: in_progress
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

