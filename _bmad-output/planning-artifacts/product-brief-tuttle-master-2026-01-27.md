---
stepsCompleted: [1, 2]
inputDocuments:
  - analysis/brainstorming-session-2026-01-20.md
  - analysis/brainstorming-session-2026-01-23.md
  - analysis/5_whys_analysis.md
  - analysis/chaos_monkey.md
  - analysis/genre_mashup.md
  - analysis/pre_mortem_analysis.md
  - analysis/scamper_innovations.md
  - analysis/support_theater.md
  - decisions/architecture_debate.md
  - decisions/change_plan.md
  - decisions/code_review_gauntlet.md
  - decisions/critical_challenge.md
  - decisions/performance_panel.md
  - decisions/stakeholder_round_table.md
  - research/market-public-adoption-cypherpunk-research-2026-01-22.md
  - source/product-brief-tuttle-network.md
  - source/project-context.md
  - source/architecture-decision-record.md
date: 2026-01-27
author: Quentin
status: in_progress
current_step: 2
next_step: 3
---

# Product Brief: tuttle-master

## Executive Summary

**tuttle-master** est le projet fondateur et orchestrateur de l'écosystème Tuttle Network. Ce n'est pas un projet d'implémentation mais le **cerveau stratégique** qui définit la vision, les concepts, et détermine quels sous-projets doivent être construits.

En tant que projet parent, tuttle-master :
- Définit la **vision de souveraineté numérique** que tous les projets enfants incarnent
- Établit les **principes directeurs** (architecture, sécurité, UX, éthique)
- Orchestre la **hiérarchie des sous-projets** via le système multi-project BMAD
- Maintient la **cohérence** entre Store, VPN, Hardware, Apps, et Infrastructure

**Mission** : Briser le cycle de la surveillance de masse en permettant à chaque individu de devenir un "souverain invisible".

---

## Core Vision

### Problem Statement

Le pouvoir moderne se nourrit de la prévisibilité des données pour maintenir sa domination. Le citoyen numérique est réduit à un état d'**esclavage administratif** :
- Identité formelle imposée vs identité propre
- Capture systématique de l'information au sein d'un Web centralisé
- Vulnérabilité totale aux règlements proliférants
- Érosion de la paix intérieure et de la liberté d'entreprendre

### Problem Impact

**Le Privacy Paradox** : 70-80% préoccupés, 15% adoptent. Causes :
- Terminologie crypto confuse (63%)
- Gestion des clés = raison #1 de churn
- Confiance difficile sans entité responsable

**Conséquences** : Surveillance normalisée, auto-censure, perte de souveraineté.

### Why Existing Solutions Fall Short

| Solution | Gap |
|----------|-----|
| VPN classiques | Tuyaux sans valeur ajoutée, confiance aveugle |
| VPN privacy | Logiciel seul, pas de hardware, fragmenté |
| dVPN | Complexité token, UX hostile |
| Hardware existant | Pas d'écosystème services |

**Le gap structurel** : Aucune solution n'offre **Hardware + Services + UX Zero-Friction + Communauté curée**.

### Proposed Solution: La Nébuleuse Tuttle

**Écosystème orchestré par tuttle-master** :

```
tuttle-master (VISION & GOUVERNANCE)
    │
    ├── 01-store      → E-commerce souverain (abonnements, hardware)
    ├── 02-vpn        → Infrastructure VPN + Legislative Weather
    ├── 03-proxy      → Shipping Proxy (anonymisation logistique)
    ├── 04-hardware   → Tuttle Key, Box, Rosette
    ├── 05-infra      → Infrastructure cloud/self-hosted
    └── 06-apps       → Clients natifs (Win, Android, iOS, macOS, Linux)
```

**Architecture conceptuelle en 3 couches** :

| Couche | Fonction | Projets enfants |
|--------|----------|-----------------|
| **Entry Layer** | VPN Exit (cash cow) - WireGuard + V2Ray + Legislative Weather | 02-vpn, 06-apps |
| **LightWeb Layer** | Intranet souverain - NetBird mesh + Îlots + Services | 02-vpn (future) |
| **Hardware Layer** | Ancre de confiance physique | 04-hardware, 01-store |

### Key Differentiators (Principes directeurs)

1. **Trust in Hardware, not Brands** : Remote Blind Signing - même Tuttle Corp ne peut pas trahir

2. **Legislative Weather** : Scoring juridictionnel unique (SANCTUARY, BALANCED, STEALTH)

3. **LightWeb** : Privacy du DarkWeb, légitimité du ClearWeb. Intranet d'entreprise pour particuliers

4. **Zero-Friction Sovereignty** : Complexité automatisée. Brancher = libération

5. **Clean Pipe** : Filtrage par défaut (pub, porn, trackers) avec **kill switch** pour désactivation rapide

6. **Économie de Résistance** : Système Référents/Îlots avec commission

---

## Target Users

### Primary: L'Éveillé Souverain (Profil "Christophe")

- **Qui** : 42 ans, cadre, père de famille, valeurs traditionnelles
- **Motivation** : Protéger foyer et intelligence d'un système oppressif
- **Comportement** : Veut Plug & Play moral et technique
- **Budget** : $10-20/mois pour solution "militante"

### Secondary: Le Référent (Profil "Marc")

- **Qui** : 50 ans, leader local, technophile
- **Rôle** : Anime démos, équipe sa communauté
- **Économie** : Commission sur ventes de son îlot

### Gouvernance

- **Consensus de Nettoyage** : Vote de proximité par quorum de clés
- **Clean Pipe** : ON par défaut, kill switch admin disponible

---

## Scope: tuttle-master

### Ce que tuttle-master DÉFINIT

| Domaine | Responsabilité |
|---------|----------------|
| **Vision** | Mission, valeurs, positionnement "State-Resistant" |
| **Concepts** | LightWeb, Legislative Weather, Îlots, Clean Pipe |
| **Architecture** | Principes techniques partagés (Zero-Knowledge, RAM-only, etc.) |
| **Hiérarchie** | Structure des sous-projets, dépendances, roadmap |
| **Standards** | Conventions, sécurité, UX, documentation |
| **Gouvernance** | Règles éthiques, Consensus de Nettoyage |

### Ce que tuttle-master NE FAIT PAS

- ❌ Code d'implémentation
- ❌ Infrastructure directe
- ❌ Déploiement de services

### Projets enfants à créer

| Projet | Type | Priorité | Description |
|--------|------|----------|-------------|
| **01-store** | Web App | P0 | Comptoir e-commerce (Nuxt + Medusa) |
| **02-vpn** | Infrastructure | P0 | Nodes VPN + Control Plane |
| **05-infra** | DevOps | P0 | Core self-hosted + Cloud nodes |
| **06-apps** | Native Apps | P1 | Clients VPN 5 plateformes |
| **04-hardware** | Hardware | P2 | Key, Box, Rosette |
| **03-proxy** | Microservice | P2 | Shipping anonymization |

---

## Success Metrics (Écosystème)

### Phase 1 - Ancrage

| Métrique | Objectif |
|----------|----------|
| Abonnés VPN | 1,000 |
| Bêta-testeurs Key | 50-100 |
| Churn | < 5% |

### Indicateurs de cohérence (tuttle-master)

| Indicateur | Mesure |
|------------|--------|
| Alignement projets | % sous-projets respectant les principes directeurs |
| Documentation sync | % PRDs/Architectures à jour |
| Transmission efficacy | Latence moyenne inbox → action |

---

<!-- WORKFLOW STATUS: Step 2 completed, ready for Step 3 (Target Users deep dive) -->
<!-- To resume: /bmad:bmm:workflows:create-product-brief -->
