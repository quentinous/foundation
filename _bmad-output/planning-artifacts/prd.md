---
stepsCompleted:
  - step-01-init
  - step-02-discovery
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
---

# Product Requirements Document - tuttle-master

**Author:** Quentin
**Date:** 2026-01-29

