---
stepsCompleted: [1, 2, 3, 4]
inputDocuments:
  - product-brief-tuttle-master-2026-01-27.md
  - prd.md (in_progress, steps 1-3)
session_topic: 'Teleport + PKI + AI Agent Identity + Gitea Project Management Integration for tuttle-master'
session_goals: 'Evaluate Teleport integration with step-ca/PKI stack, define AI agent cryptographic identity model, map BMAD artifacts to Gitea UI for full project visibility'
selected_approach: 'AI-Recommended'
techniques_used:
  - 'Structured exploration (multi-axis decomposition)'
  - 'Architecture decision comparison (A/B/C options)'
  - 'Threat modeling (blast radius analysis)'
  - 'Taxonomy building (identity classification)'
  - 'System mapping (BMAD → Gitea feature mapping)'
  - 'Source of truth analysis (Option A/B/C)'
ideas_generated:
  - 'Teleport as subordinate CA of step-ca (Approach C)'
  - 'PKI-as-Governance pattern'
  - 'Three-tier agent classification (Paper/Code/Operations)'
  - 'Escalation chain: dev auto-merge → tea validates staging → human validates main'
  - 'CI-triggered Access Request with auto-approval for tea staging access'
  - 'Five identity categories: Human, BMAD Agent, CI Service, Automated Bot, Application Service'
  - 'TTL inversely proportional to autonomy and power'
  - 'Hybrid PR model: PR mandatory for agents, optional for human operator'
  - 'BMAD = source of truth, Gitea = navigable mirror'
  - 'Epic → Milestone, Story → Issue, Sprint → Project Board, Docs → Wiki'
  - 'Master repo mirrors epics only (strategic view), child repos hold stories (execution view)'
  - 'Gitea issue templates for structured story/bug/spike/transmission creation'
  - 'Sprint gate = Gitea release with signed SBOM and auto-generated changelog'
  - 'Gitea webhooks → BMAD transmissions for event-driven feedback loop'
context_file: ''
---

# Brainstorming Session Results

**Facilitator:** Quentin
**Date:** 2026-01-30

## Session Overview

**Topic:** Teleport + PKI + AI Agent Identity + Gitea Project Management Integration for tuttle-master

**Goals:**
1. Evaluate opportunity to integrate Teleport with existing step-ca/PKI stack
2. Define AI agent cryptographic identity model with tiered access
3. Map BMAD planning artifacts to Gitea UI for full project visibility on git.pelerin.lan

### Context Guidance

Sources analysées :
- **Product Brief** (2026-01-27) : Dual-Path Sovereignty, Orphean ID, PKI hierarchy (Root CA → step-ca → Leaf), Warrant Canary, Zero-Knowledge, fail-closed patterns
- **PRD** (in progress, steps 1-3) : 64 critères techniques stratifiés S0/S1/S2 incluant PKI Hierarchy Deployed, Mandatory Git Commit Signing, BMAD Agent Cryptographic Identity, mTLS Inter-Services, Release Signing, Cryptographic Audit Trail

### Session Setup

Le PRD définit déjà une hiérarchie PKI ambitieuse :
- Root CA offline (HSM dédié, rotation 5-10 ans)
- step-ca comme Intermediate CA (load-balanced, HSM backend)
- Leaf certificates par projet/service/env
- Commit signing obligatoire (devs + agents BMAD)
- Release signing (binaires, containers cosign, packages)
- mTLS inter-services

**Ce qui n'était PAS couvert avant cette session :**
- Teleport comme couche d'accès infrastructure
- Le modèle précis d'identité autonome pour les agents IA
- Le mapping BMAD → Gitea pour la visibilité projet

---

# Part 1 — Teleport + PKI Integration

## Decision 1 — Teleport Integration Model

**Context:** Teleport replaces the traditional bastion for infrastructure access (SSH, K8s, DB). The question was how it coexists with step-ca.

**Decision: Approach C — Pragmatic subordination**

step-ca remains the sole root of trust. Teleport keeps its internal CA for ephemeral certificate issuance, but its CA root certificate is signed by step-ca. This unifies the trust tree while letting each tool do what it does best.

```
Root CA (HSM offline, 5-10 year rotation)
  └── step-ca (Intermediate CA, HSM load-balanced)
        ├── Teleport CA (subordinate, signed by step-ca)
        │     ├── Ephemeral SSH certs (minutes/hours)
        │     ├── Ephemeral K8s certs (minutes/hours)
        │     └── Ephemeral machine/agent certs (controlled duration)
        ├── mTLS inter-service certs (90 day rotation)
        ├── Code signing certs (developer + agent)
        └── Release signing certs (HSM-stored keys)
```

**Responsibilities split:**
- **step-ca:** Root authority, mTLS, code signing, release signing, application certs, Teleport CA cert
- **Teleport:** Ephemeral access certs, session recording, audit trail, RBAC, Access Requests

**Note:** Teleport may support full external CA injection (Approach A). To be validated during architecture phase. If supported, migration from C to A is straightforward.

## Decision 2 — Human vs Agent Access Model

**Decision: Differentiated access based on trust level**

| Actor | Infrastructure Access | Approval |
|-------|----------------------|----------|
| **Human (Quentin)** | RBAC + short TTL, direct access all envs | No approval needed |
| **BMAD Agents** | Access Request per action, prod NEVER | Human approval required |
| **CI Services** | Auto-approved if triggered by CI pipeline | Automatic (CI-triggered) |

## Decision 3 — Agent Identity Granularity

**Decision: Option C — One key per agent instance + periodic rotation**

Each BMAD agent type has a persistent identity with a 30-day TTL certificate, automatically rotated by step-ca. This balances traceability (stable identity over time) with security (regular rotation, immediate revocation possible).

Not session-based (too many keys), not permanent (too risky).

## Decision 4 — Git Workflow: Hybrid PR Model

**Context:** The question was whether PRs are necessary or whether a branch-based model (dev/staging/main) suffices.

**Decision: PRs mandatory for agents, optional for human operator**

| Actor | dev | staging | main |
|-------|-----|---------|------|
| **Human** | Push direct (signed) | Push direct (signed) | Push direct (signed) |
| **Agent dev** | PR → CI green → auto-merge | PR → tea validates → merge | PR → human validates → merge |
| **Agent tea** | PR → CI green → auto-merge | PR → CI green → auto-merge | PR → human validates → merge |
| **Agent ops** | PR → CI green → auto-merge | PR → tea validates → merge | PR → human validates → merge |

**Escalation chain:**
```
dev:     agent auto-merges if CI green
staging: tea agent validates and merges
main:    human (Quentin) validates and merges — no exceptions
```

**Rationale:** PRs are the supervision mechanism for agents, not bureaucratic overhead. The human operator, with a step-ca signed certificate, has full trust. Agents earn trust progressively through the escalation chain.

## Decision 5 — Tea Staging Access: CI-Triggered Access Request

**Decision: Access Request auto-approved when triggered by CI**

The tea agent does not have permanent staging access. Each CI pipeline run triggers a Teleport Access Request that is auto-approved because the trigger is the CI system itself. Outside CI, staging access requires human approval.

**Result:** Every staging access = a line in the Teleport audit log. Zero permanent access. Maximum traceability.

## Decision 6 — Ops Agent Pattern: Plan Yes, Apply Never

**Decision: Future ops/infra agent follows the "prepare, never execute" pattern**

```
Agent ops: terraform plan → PR with diff → CI validates plan
Human:     reviews plan → terraform apply
```

The agent can read cluster state, prepare infrastructure changes, and open PRs. It can NEVER apply changes to any environment.

---

# Part 2 — Identity Taxonomy

## Five Categories of Machine Identity

```
step-ca (Root of Trust)
│
├── HUMANS
│   └── Quentin (cert 90d, auto-rotation)
│       → Git: push main, merge, sign
│       → Teleport: RBAC admin, short TTL
│       → Infisical: all envs
│       → Prod: full access
│
├── BMAD AGENTS (cert 30d, auto-rotation)
│   ├── Tier 1 — Paper (analyst, pm, ux-designer, tech-writer, sm)
│   │   → Git: read + PR docs only
│   │   → K8s: none
│   │   → Infisical: none
│   │   → CI/CD: none (sm: read logs)
│   │   → Teleport: none
│   │
│   ├── Tier 2 — Code (dev)
│   │   → Git: read + PR code
│   │   → K8s: dev direct
│   │   → Infisical: dev read (fake data)
│   │   → CI/CD: trigger
│   │   → Teleport: dev direct, staging on request
│   │
│   ├── Tier 3 — Validation (tea, architect)
│   │   → Git: read + PR tests/CI config (tea), PR archi/infra (architect)
│   │   → K8s: dev direct, staging via CI-triggered Access Request
│   │   → Infisical: dev read, staging read
│   │   → CI/CD: trigger + configure (tea), read + validate (architect)
│   │   → Teleport: dev direct, staging CI-auto-approved request
│   │
│   └── Tier 3+ — Infrastructure (ops) [future]
│       → Git: read + PR infra (Terraform, Ansible, Helm)
│       → K8s: read cluster state
│       → Infisical: dev/staging read
│       → CI/CD: trigger + configure infra pipelines
│       → Teleport: staging on request, prod NEVER
│       → Specific: terraform plan YES, terraform apply NEVER
│
├── CI SERVICES (cert per job, TTL = job duration)
│   ├── Woodpecker workers
│   │   → Build, test, sign artifacts
│   │   → Staging access via Teleport auto-CI request
│   │   → Cert dies when job ends
│   │
│   └── Infra pipeline (Terraform plan CI)
│       → Read-only on state
│       → Plan authorized, apply NEVER
│
├── AUTOMATED BOTS (cert 30d, very limited scope)
│   ├── Renovate / Dependabot
│   │   → Git: PR only (dependency updates)
│   │   → No infra access
│   │
│   ├── Mailbox auto-triage
│   │   → Read/write _mailbox/ only
│   │   → No infra access, no code access
│   │
│   ├── Hierarchy sync (cron)
│   │   → Read hierarchy.csv, validate, alert
│   │   → No write except status reports
│   │
│   └── Weekly health check
│       → Read dashboards, Prometheus, statuses
│       → Write: report only
│
└── APPLICATION SERVICES (mTLS cert, 90d rotation)
    ├── store ↔ provisioner
    ├── provisioner ↔ vpn
    └── ... (inter-service mTLS via step-ca)
```

## TTL Policy

| Category | TTL | Rotation | Revocation |
|----------|-----|----------|------------|
| **Human** | 90 days | Auto step-ca | Manual (CRL/OCSP) |
| **BMAD Agent** | 30 days | Auto step-ca | Immediate possible |
| **CI Service** | Job duration | New cert per job | Expires naturally |
| **Automated Bot** | 30 days | Auto step-ca | Immediate possible |
| **Application Service (mTLS)** | 90 days | Auto step-ca + ACME | Immediate + CRL |

**Principle: TTL is inversely proportional to autonomy and power.** The more autonomous and powerful the entity, the shorter its certificate lives.

## Universal Rules (All Agents, All Tiers)

```
✗ Push direct on main        → NEVER, PR only (agents)
✗ Prod access                → NEVER, no exceptions
✗ Prod secrets               → NEVER
✗ Merge without green CI     → NEVER
✗ Bypass gates               → NEVER
✗ Create other identities    → NEVER (step-ca does not delegate)
```

---

# Part 3 — Emergent Pattern: PKI-as-Governance

This session revealed a pattern that goes beyond traditional certificate management. The PKI is not just securing communications — it is **enforcing governance** across the ecosystem:

| Layer | Governance Question | PKI Answer | Tool |
|-------|-------------------|------------|------|
| **Identity** | Who are you? | Signed cert from step-ca | step-ca |
| **Access** | Where can you go? | Ephemeral cert from Teleport | Teleport |
| **Authorization** | What can you do? | RBAC role per identity type | Teleport + Infisical + Gitea |
| **Audit** | What did you do? | Signatures + session recording | Crypto audit trail |
| **Temporality** | For how long? | TTL + rotation policy | step-ca + Teleport |

This directly extends PRD First Principle FP-004: *"Every master output is a contract or a trigger, never informational-only."* Here, **the certificate IS the access contract.** Not a document that says "you have the right" — a cryptographic proof that **enforces** the right.

---

# Part 4 — Gitea Project Management Integration

## Decision 7 — Source of Truth: BMAD writes, Gitea mirrors

**Decision: Option A — BMAD = source of truth, Gitea = navigable mirror**

- BMAD planning artifacts (markdown files in `_bmad-output/`) are authoritative
- Gitea issues, milestones, project boards, and wiki are mirrors for visibility
- Manual edits in Gitea are overwritten on next BMAD sync
- Issues carry a `🔄 Managed by BMAD` banner with source file reference

**Implication:** If someone closes an issue manually in Gitea, the next BMAD sync reopens it. Only the BMAD workflow (dev-story completed, PR merged with `Closes #XX`) can authoritatively close a story.

## Decision 8 — Mapping BMAD Concepts to Gitea Features

| BMAD Concept | Gitea Feature | Detail |
|-------------|---------------|--------|
| **Epic** | Milestone | 1 epic = 1 milestone. Name: `Epic 1: Infrastructure Foundation`. Auto % completion |
| **Story** | Issue | 1 story = 1 issue. Title: `[E1.S3] Configure Infisical namespaces`. Body: acceptance criteria + task checklist |
| **Task/Subtask** | Checklist in issue body | `- [ ] Create namespace`. Gitea displays completion ratio |
| **Sprint** | Project Board (kanban) | 1 sprint = 1 board. Columns: Backlog → In Progress → Review → Done |
| **PRD / Architecture** | Wiki | Synced from `_bmad-output/planning-artifacts/` |
| **PR linked to story** | PR with `Closes #XX` | Auto-closes issue on merge. Native PR ↔ Story link in UI |
| **Labels** | Labels | Multi-dimensional categorization (see label system below) |

### Label System

```
By type:
  epic          (purple)
  story         (blue)
  bug           (red)
  chore         (grey)
  spike         (orange)

By priority:
  P0-critical   (bright red)
  P1-high       (orange)
  P2-medium     (yellow)
  P3-low        (green)

By sprint gate:
  S0-foundation (dark blue)
  S1-core       (blue)
  S2-clients    (light blue)

By workflow status:
  needs-review     (yellow)
  blocked          (red)
  ready-for-dev    (green)
  in-review        (orange)

By agent:
  agent:dev        (dark green)
  agent:tea        (cyan)
  agent:architect  (purple)
  agent:pm         (blue)
```

## Decision 9 — Master Repo: Strategic View (Epics Only)

**Decision: Approach B — Master mirrors epics only, not individual stories**

**Child repo (e.g., tuttle-store):**
- Milestones = epics (with stories as issues)
- Issues = stories (with tasks as checklists)
- Project boards = sprints (kanban)
- Wiki = PRD, Architecture, Epics, Sprint Status, Glossary

**Master repo (tuttle-master):**
- Issues = one per epic per child project (strategic tracking)
  - Title: `[store][Epic 1] Store Foundation`
  - Labels: project name + sprint gate
  - Body: epic summary + link to child repo milestone
- Milestones = sprint gates (S0, S1, S2) across all projects
- Project board = ecosystem kanban (epics across all projects)
- Wiki = master documents + per-project summary pages

**Example master issue list:**
```
git.pelerin.lan/tuttle/tuttle-master/issues

#10  [store] Epic 1: Store Foundation           store  S0  ████████░░ 80%
#11  [store] Epic 2: Payment Integration        store  S1  ████░░░░░░ 40%
#12  [vpn] Epic 1: VPN Foundation               vpn    S0  ██░░░░░░░░ 20%
#13  [infra] Epic 1: Infrastructure Base        infra  S0  ██████████ 100%
#14  [infra] Epic 2: K8s Production             infra  S1  ░░░░░░░░░░  0%
```

## Decision 10 — Automatic Sync BMAD → Gitea (via MCP Gitea)

**Decision: Agents sync to Gitea as part of workflow execution, not as a separate job**

| BMAD Event | Gitea Action (via MCP Gitea) |
|------------|------------------------------|
| `create-epics-and-stories` completes | Create milestones + issues in child repo. Create epic-level issues in master. Populate wiki |
| `create-story` completes | Create issue in child repo, attach to milestone, apply labels |
| `dev-story` completes (PR merged) | Issue closed via `Closes #XX` in PR. Tasks checked in body. Milestone % updates |
| `sprint-planning` completes | Create/update project board, place issues in columns |
| `correct-course` completes | Update impacted issues, add comment with changelog |
| PRD/Architecture updated | Sync wiki of concerned repo |
| `workflow-status` updated | Update epic-level issue in master |

## Decision 11 — Issue Templates (Structured Creation)

**Decision: Pre-configured Gitea issue templates in every repo**

Templates stored in `.gitea/issue_template/` (versioned, auditable, consistent cross-project via master template copied at init):

- **story.yaml** — BMAD story structure (acceptance criteria, tasks, source file reference)
- **bug.yaml** — Structured bug report (reproduce steps, expected/actual, environment)
- **spike.yaml** — Investigation/research template (question, scope, timebox)
- **transmission.yaml** — Issues linked to mailbox transmissions (type, source project, impact)
- **chore.yaml** — Technical debt, maintenance, tooling

Each template includes the `🔄 Managed by BMAD` banner and source file reference field when applicable.

## Decision 12 — Sprint Gate = Gitea Release (Signed)

**Decision: Each validated sprint gate produces a Gitea release**

**Child repo release:**
```
git.pelerin.lan/tuttle/tuttle-store/releases

v0.1.0 — Sprint 0 Gate ✅
├── Changelog (auto-generated from closed milestone issues)
├── Sprint gate checklist (criteria validated)
├── Signed SBOM
├── Signed artifacts (cosign for containers)
└── Link to Gitea milestone (% completion)
```

**Master repo release (ecosystem gate):**
```
git.pelerin.lan/tuttle/tuttle-master/releases

v0.1.0 — Ecosystem Sprint 0 Gate
├── tuttle-infra v0.1.0 ✅
├── tuttle-libs v0.1.0 ✅
├── 26 S0 criteria validated
└── Signed by: Quentin (step-ca cert)
```

**Versioning convention:**
```
v{major}.{minor}.{patch}

v0.1.0    → Sprint 0 gate passed (foundation)
v0.2.0    → Sprint 1 gate passed (core product)
v0.3.0    → Sprint 2 gate passed (clients)
v1.0.0    → MVP release

Between gates:
v0.1.1    → Patch post-S0
v0.2.0-rc1 → Release candidate S1
```

## Decision 13 — Gitea Webhooks → BMAD Transmissions (Feedback Loop)

**Decision: Event-driven feedback from Gitea to BMAD via webhooks**

| Gitea Event | BMAD Action |
|-------------|-------------|
| Issue commented by human | No action (info only, BMAD stays source) |
| Issue closed manually | ⚠️ Alert: desync detected, re-sync needed (reopen on next sync) |
| PR merged with `Closes #XX` | Story marked completed in BMAD |
| Sprint milestone reaches 100% | Transmission `sprint-gate-ready` to master |
| Label `blocked` added | Transmission `blocker` to master |
| Release published | Update workflow-status in BMAD |

**Mechanism:** Gitea Webhooks → small service (or Woodpecker job) that translates events into BMAD actions. Event-driven push, not polling.

**Critical rule:** Manual issue closure in Gitea does NOT close the story in BMAD. Only the BMAD workflow (dev-story completed, PR merged) is authoritative. Manual closure triggers a desync alert and will be reverted on next sync.

## Wiki Structure

**Child repo wiki (e.g., tuttle-store):**
```
git.pelerin.lan/tuttle/tuttle-store/wiki

├── Home.md              ← Auto-generated index with links
├── PRD.md               ← Synced from _bmad-output/planning-artifacts/prd.md
├── Architecture.md      ← Synced from _bmad-output/planning-artifacts/architecture.md
├── Epic-1.md            ← Synced from epics/epic-1/
├── Epic-2.md            ← Synced from epics/epic-2/
├── Sprint-Status.md     ← Synced from sprint-status.yaml (rendered readable)
└── Glossary.md          ← Copied from master
```

**Master repo wiki:**
```
git.pelerin.lan/tuttle/tuttle-master/wiki

├── Home.md              ← Ecosystem dashboard
├── PRD-Master.md        ← Master PRD
├── Architecture-Master.md
├── Hierarchy.md         ← Readable render of hierarchy.csv
├── Identity-Taxonomy.md ← From this brainstorming session
├── PKI-Governance.md    ← PKI policies
├── Glossary.md          ← Source of truth for glossary (WR-004)
├── Runbook.md           ← Operational runbook (PM-002)
└── Projects/
    ├── tuttle-store.md  ← Summary + links to repo
    ├── tuttle-vpn.md
    ├── tuttle-infra.md
    └── ...
```

**Wiki sync mechanism:** Gitea wikis are git repos. Sync = `git push` to the wiki repo after each BMAD document update. Signed by the agent that made the modification.

---

# Part 5 — Impact on PRD

## New Criteria to Integrate

**Sprint 0 Gate — additions:**

| Criterion | Measure | Validation |
|-----------|---------|------------|
| **Teleport Deployed** | Teleport cluster operational with CA signed by step-ca | Trust chain verified |
| **Zero Static SSH Keys** | No long-lived SSH keys on any server/VM. All access via Teleport ephemeral certs | Key audit |
| **Identity Taxonomy Implemented** | Five identity categories provisioned in step-ca with correct TTLs | step-ca config verified |
| **Agent Identity Bootstrapping** | Documented process for creating first BMAD agent identity via step-ca | Onboarding test |
| **Gitea Issue Templates** | `.gitea/issue_template/` with story, bug, spike, transmission, chore templates on every repo | Template check |
| **Gitea Wiki Initialized** | Wiki populated with PRD, Architecture, Glossary on each active repo | Wiki content check |
| **Gitea Label System** | Standard label set (type, priority, gate, status, agent) created on every repo | Label audit |

**Sprint 1 Gate — additions:**

| Criterion | Measure | Validation |
|-----------|---------|------------|
| **Agent PR Workflow Enforced** | Gitea branch protection: agents cannot push direct, PR mandatory with CI gate | Branch protection config |
| **Escalation Chain Active** | dev: auto-merge, staging: tea validates, main: human validates | Merge policy test |
| **CI-Triggered Access Requests** | Teleport auto-approves staging access when triggered by CI pipeline | Access Request audit |
| **Ops Agent Pattern** | terraform plan via agent PR, terraform apply human-only | CI gate test |
| **BMAD → Gitea Sync Operational** | Stories created in BMAD appear as issues in Gitea within workflow execution | Sync test |
| **PR → Issue Linking** | All story PRs reference `Closes #XX`, auto-close on merge | PR audit |
| **Sprint Board Active** | Project board reflects current sprint with correct column placement | Board review |

**Sprint 2 Gate — additions:**

| Criterion | Measure | Validation |
|-----------|---------|------------|
| **Full Identity Audit** | All five identity categories active, TTL policy enforced, revocation tested | Audit report |
| **Bot Identities Operational** | Renovate, mailbox triage, hierarchy sync, health check — all with step-ca certs | Cert inventory |
| **Blast Radius Documented** | For each identity category: documented worst-case if compromised + mitigation steps | Security review |
| **Master Epic Tracking** | All child epics mirrored as issues in master with correct labels and milestone | Master issue audit |
| **Ecosystem Release** | Sprint gate = signed release on each repo + ecosystem release on master | Release audit |
| **Gitea Webhooks → BMAD** | Event-driven feedback loop operational (PR merge → story close, milestone 100% → transmission) | Webhook test |
| **Wiki Fully Synced** | All planning artifacts reflected in wiki across all repos | Wiki content audit |

## Existing PRD Criteria to Enrich

- **"BMAD Agent Cryptographic Identity"** (S0) → Expand with Tier classification, TTL policy, scope per tier
- **"Mandatory Git Commit Signing"** (S0) → Add: hybrid PR model (agent = PR mandatory, human = push OK)
- **"mTLS Inter-Services via step-ca"** (S1) → Clarify: Teleport handles access-layer certs, step-ca handles service-layer certs
- **"Rotation + Revocation Policy"** (S1) → Add: per-category TTL table, CI service cert = job-scoped
- **"Ecosystem-Status Operational"** (S2) → Add: Gitea master project board as complementary view to CLI workflow-status

---

# Part 6 — Open Questions for Architecture Phase

## PKI & Teleport

1. **Teleport CA injection:** Does Teleport support using an external CA (step-ca) as its signing authority? If yes, migrate from Approach C to Approach A.
2. **Agent identity bootstrapping:** Who creates the first agent identity? Quentin manually via step-ca? An init script? This is a chicken-and-egg problem.
3. **Access Request ↔ BMAD workflow integration:** Should a Teleport Access Request generate a mailbox transmission? Or is Teleport audit sufficient without polluting the mailbox system?
4. **Agent compromise response:** Exact runbook when an agent identity is suspected compromised (revoke cert, kill sessions, audit trail analysis, re-provision).
5. **Teleport licensing:** Teleport Community vs Enterprise — which features are needed? (Access Requests may require Enterprise or Teleport Cloud.)

## Gitea Integration

6. **MCP Gitea capabilities:** Validate that the MCP Gitea toolset covers all required operations (create milestone, create issue, manage project boards, push wiki, create release).
7. **Wiki sync mechanism:** Git push to wiki repo vs Gitea API. Git push is more natural (signed commits) but requires wiki repo clone management.
8. **Cross-repo project board:** Gitea doesn't natively support cross-repo boards. The master epic-level approach works, but explore Gitea plugins or API workarounds for richer cross-project views.
9. **Webhook service architecture:** Dedicated microservice vs Woodpecker job for Gitea → BMAD event translation. Consider reliability and audit requirements.
10. **GitHub mirror sync:** Deferred, but when implemented: mirror Gitea → GitHub including issues? Or repos only? GitHub as pure code backup vs full project mirror.
