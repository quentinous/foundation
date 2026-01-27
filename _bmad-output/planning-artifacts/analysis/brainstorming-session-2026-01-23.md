---
stepsCompleted: [1, 2, 3, 4]
inputDocuments:
  - _bmad-output/analysis/brainstorming-session-2026-01-20.md
session_topic: 'Tuttle Network Foundation - Vision Globale'
session_goals: 'Approfondir les idées clés du brainstorm précédent, définir architecture VPN, LightWeb et Infrastructure'
selected_approach: 'continuation'
techniques_used: ['Deep Dive', 'Challenge/Validation', 'Competitive Analysis', 'Architecture Design']
ideas_generated: 35
context_file: ''
status: in_progress
decisions:
  legislative_weather: 'Semi-automatique (RSS + AI + Admin approval)'
  native_apps: 'Natif pur, Mainstream First (Win → Android → iOS → macOS → Linux)'
  vpn_protocol: 'WireGuard + V2Ray/Xray obfuscation'
  lightweb_mesh: 'NetBird self-hosted (groupes dans ACLs = raison technique)'
  core_infra: 'Self-hosted Proxmox (Talos K8s + PostgreSQL VM)'
  cloud_strategy: 'Hybride - SANCTUARY (manuel) + BALANCED (Terragrunt/Hetzner)'
  iac_stack: 'Terragrunt + Ansible'
  node_auth: 'mTLS (PKI interne)'
  node_management: 'NetBird mesh + VPN API custom (Go)'
---

# Brainstorming Session Results

**Facilitator:** Quentin
**Date:** 2026-01-23
**Continuation de:** brainstorming-session-2026-01-20.md

---

## Session Overview

**Topic:** Tuttle Network Foundation - Vision Globale
**Goals:** Approfondir les idées clés, définir l'architecture VPN et apps natives

### Idées approfondies

1. ✅ **UX Tuyauterie / Pro** → Legislative Weather (pools dynamiques, scoring juridiction)
2. ✅ **Apps Natives VPN** → Architecture & Stack (natif pur, Mainstream First)
3. ✅ **Admin WireGuard Pro** → **NetBird** self-hosted (groupes dans ACLs = raison technique)
4. ✅ **Infrastructure Provisioning** → Hybride (self-hosted Core + cloud nodes)

---

## 1. Legislative Weather (UX Tuyauterie approfondi)

### Concept
Système de pools de serveurs dynamiques basés sur la législation par juridiction.

### Axes de scoring par pays

| Axe | Critères |
|-----|----------|
| **Privacy Score** | RGPD-like, data retention, surveillance |
| **Criminal Risk** | Peines encourues (téléchargement, VPN illégal) |
| **Crypto Friendly** | Légalité, taxation, exchanges |
| **Intel Alliance** | 5 Eyes, 9 Eyes, 14 Eyes |
| **Censorship Level** | Blocages DNS, DPI actif |

### Pools définis

```
🟢 SANCTUARY     → Privacy max + No Intel Alliance + Crypto OK
🔵 CRYPTO HAVEN  → Crypto friendly avant tout
🟡 BALANCED      → Compromis perf/privacy/légalité
🔴 RISKY EXIT    → Contournement géoblocage spécifique
⚫ STEALTH       → Anti-DPI, protocoles obfusqués
```

### Architecture du système

```
Sources RSS/APIs → Scraper Daily → AI Analyst → Admin UI (approve/reject) → Pool DB → Client Apps
```

### Sources de données identifiées

**ONG & Watchdogs:**
- EFF (Electronic Frontier Foundation)
- Access Now
- Freedom House
- Privacy International
- Reporters Without Borders
- Coin Center

### Décisions MVP vs Full

| MVP | Full Vision |
|-----|-------------|
| Scoring manuel (fichier config YAML) | Scraper RSS + AI Analyst |
| 5 pools statiques | Pools dynamiques temps réel |
| Changelog manuel | Push notifications si pool change |
| - | Warrant Canary par juridiction |
| - | Partenariats ONG |

### Format données
- Partie du contrat API (api-contracts)
- JSON signé pour intégrité

---

## 2. Apps Natives VPN

### Ordre de développement (Mainstream First)

1. **Windows** (C# + WinUI 3 ou Rust)
2. **Android** (Kotlin)
3. **iOS** (Swift)
4. **macOS** (Swift, code partagé iOS)
5. **Linux** (Rust + GTK/Qt)

### Décision : Natif pur (pas cross-platform)

**Justification :**
- Couche réseau = intégration OS critique
- API système VPN direct (pas de bridges)
- Performance et battery drain maîtrisés
- App Store approval plus simple
- C'est ce que font Mullvad, Proton, IVPN

### Core VPN partagé

```yaml
Protocol: WireGuard (wireguard-go ou boringtun)
Obfuscation: V2Ray/Xray intégré
Architecture: Core Rust/Go + UI native par plateforme
```

### WireGuard - Validation "State-Resistant"

| Critère | Statut |
|---------|--------|
| Code auditable | ✅ ~4,000 lignes |
| Crypto solide | ✅ ChaCha20, Curve25519 |
| Open source | ✅ GPL |
| Backdoor connue | ❌ Aucune |
| Utilisé par les pros | ✅ Mullvad, IVPN, Proton |

**Mesures additionnelles state-resistant :**
- Obfuscation V2Ray/Xray (anti-DPI)
- Multi-hop optionnel
- RAM-only servers
- No-log architecture prouvable

### Fonctionnalités apps

```yaml
Base (Proton-like):
  - Configs WireGuard/OpenVPN downloadables
  - Tri par pays + taux occupation serveur
  - Quick connect

Différenciateurs Tuttle:
  - Tri par pool législatif (Legislative Weather)
  - Affichage: débit, entrée/sortie, menaces bloquées
  - Sync pools à chaque connexion + cache local
  - Mode Stealth (obfuscation auto)
```

### Approche développement

```
BMAD ultra-itératif → Specs NASA par composant → Agents AI ultra-cadrés → Test-driven
```

---

## 3. Admin WireGuard Pro (À EXPLORER)

### Questions ouvertes

- Solution open-source existante ? (Firezone, Headscale, Netbird)
- Custom AdminVPN ?
- Intégration Medusa ?

### Candidates identifiées

| Projet | Stack | Notes |
|--------|-------|-------|
| Firezone | Elixir/Phoenix | WireGuard management, SSO, policies |
| Headscale | Go | Self-hosted Tailscale control |
| Netbird | Go | WireGuard mesh, zero-config |
| Pritunl | Python | OpenVPN/WireGuard enterprise |

---

## 4. Infrastructure Provisioning (À EXPLORER)

### Questions ouvertes

- Comment provisionner les serveurs VPN ?
- IaC (Terraform, Pulumi, Ansible) ?
- Multi-cloud ou bare-metal ?
- Automatisation déploiement nouveaux nodes ?

---

## Prochaines étapes

1. ✅ Explorer Admin WireGuard → **NetBird** (groupes dans ACLs)
2. ✅ Définir stratégie infrastructure → Hybride (self-hosted + cloud)
3. 🔲 Détailler VPN API (specs OpenAPI, choix Go vs Rust)
4. 🔲 Détailler Ansible roles (WireGuard, V2Ray, monitoring)
5. 🔲 Exposition Core (Cloudflare Tunnel pour Zitadel/Store public)
6. 🔲 Approfondir autres idées (Witness Mode, Dead Man's Switch, etc.)
7. 🔲 Passer au Product Brief pour formaliser

---

## 5. 🚀 GAME-CHANGER : Le concept LightWeb

### Révélation

Le Tuttle Network n'est PAS un VPN classique (tunnel vers Internet).
C'est un **réseau privé interne** avec services et communautés.

### Le modèle LightWeb

```
┌─────────────────────────────────────────────────────────────────┐
│                      TUTTLE NETWORK (B)                         │
│                        "Le LightWeb"                            │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                                                          │  │
│  │   ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐   │  │
│  │   │ Service │  │ Service │  │  Îlot   │  │  Îlot   │   │  │
│  │   │  Chat   │  │  Files  │  │ Famille │  │ Activis │   │  │
│  │   └─────────┘  └─────────┘  └─────────┘  └─────────┘   │  │
│  │                                                          │  │
│  │              Services internes + Groupes                 │  │
│  │                 (Invisible de l'extérieur)               │  │
│  └──────────────────────────────────────────────────────────┘  │
│                              │                                  │
│                              ▼                                  │
│                      [ Exit vers Internet (C) ]                │
│                        (Optionnel / VPN classique)             │
└─────────────────────────────────────────────────────────────────┘
         ▲
         │
    Client (A)
```

### Clarification importante

> ⚠️ **Le VPN Exit (sortie Internet) est le PRODUIT PRINCIPAL** - c'est le cash cow.
> Le LightWeb (services internes + îlots) est le **DIFFÉRENCIATEUR** qui rend Tuttle unique.

### Comparaison

| Aspect | VPN Classique | Tuttle Network |
|--------|---------------|----------------|
| **Produit principal** | Tunnel vers Internet | **VPN Exit** (tunnel vers Internet) |
| **Valeur ajoutée** | Aucune | **LightWeb** (réseau interne + services) |
| **Services** | Aucun | Intégrés, cachés, communautaires |
| **Social** | Individuel | **Îlots** (groupes privés) |
| **Analogie** | Tuyau anonyme | VPN premium + Intranet souverain |

### LightWeb vs DarkWeb

- **DarkWeb** = Tor hidden services, .onion, anonyme mais réputation criminelle
- **LightWeb** = Même privacy, mais propre, curé, légitime, services de qualité

### Impact sur l'architecture

Les solutions **Mesh VPN** deviennent pertinentes :

| Solution | Utilité |
|----------|---------|
| **Headscale** | ✅ Parfait pour les îlots (groupes) |
| **Netbird** | ✅ Mesh + groupes + ACLs |
| **Tailscale model** | ✅ Exactement ça, mais souverain |

### Architecture révisée

```yaml
Tuttle Network:
  Entry Layer:
    - WireGuard entry points (le "VPN" visible)
    - Legislative Weather (routing exit)

  Core Network (Le LightWeb):
    - Mesh interne (Headscale/Netbird-like)
    - Îlots (groupes/VLANs privés)
    - Services internes (hidden services style)

  Exit Layer (optionnel):
    - Exit nodes classiques vers Internet
    - Multi-hop possible
```

### Services potentiels internes

- 📬 Messagerie chiffrée (Matrix/Element)
- 📁 Stockage fichiers (Nextcloud)
- 🎥 Streaming privé
- 💬 Forums/Wiki internes
- 🔐 Gestionnaire mots de passe (Vaultwarden)
- 📧 Email privé
- ... extensible à l'infini

### Insight clé

> **C'est un intranet d'entreprise, au service des particuliers.**
> By design, tout est caché de l'extérieur du VPN.

---

## 6. Admin & Infrastructure (À EXPLORER - contexte LightWeb)

### Questions révisées

Avec le concept LightWeb, les questions deviennent :

1. **Control Plane** - Gérer les îlots, les services, les users
2. **Mesh interne** - Headscale ou Netbird comme base ?
3. **Services deployment** - K8s interne au réseau ?
4. **Exit nodes** - Optionnels, pour sortir vers Internet

### Comparaison complète Mesh VPN Self-Hosted

| Critère | **Headscale** | **NetBird** | **Nebula** | **Netmaker** |
|---------|---------------|-------------|------------|--------------|
| **Licence** | BSD-3-Clause | BSD-3 + AGPLv3 | MIT | Apache 2.0 + Pro |
| **Self-hosted gratuit** | ✅ 100% illimité | ✅ 100% illimité | ✅ 100% illimité | ⚠️ Limité |
| **Protocole** | WireGuard (Tailscale) | WireGuard | Propre (Noise) | WireGuard kernel |
| **Web UI** | Via tiers | ✅ Intégrée | ❌ CLI only | ✅ Intégrée |
| **IdP/Zitadel** | OIDC manuel | ✅ Natif | ❌ Certificats | OIDC basique |
| **Relay/NAT** | DERP | ✅ Signal+Relay | ❌ Non | ✅ Oui |
| **Performance** | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |

### Décision : NetBird 🏆

**Justification technique (après analyse approfondie) :**

| Critère | Headscale | NetBird |
|---------|-----------|---------|
| **Groupes pour auth filter** | ✅ `allowed_groups` | ✅ JWT claim |
| **Groupes dans ACLs réseau** | ❌ **IMPOSSIBLE** | ✅ Supporté |
| **Management API** | ❌ Non | ✅ Zitadel Manager |
| **Sync groupes auto** | ❌ Non | ⚠️ Partiel (JWT refresh) |

**Pourquoi NetBird et pas Headscale :**
- Headscale ne peut **PAS** utiliser les groupes OIDC dans les ACLs réseau (limitation architecturale documentée, Issue #2366)
- Pour les îlots avec contrôle d'accès par groupe, c'est **bloquant**
- L'intégration "native" Zitadel = Management API pour CRUD users, pas juste OIDC
- Les deux nécessitent une Zitadel Action pour transformer roles → groups

### Architecture finale

```yaml
Tuttle Network:
  Entry Layer (VPN Exit - PRODUIT PRINCIPAL):
    - WireGuard natif (performance max)
    - Legislative Weather routing
    - V2Ray/Xray obfuscation

  LightWeb Layer (DIFFÉRENCIATEUR):
    - Control Plane: NetBird self-hosted
    - Auth: Zitadel (SSO unifié)
    - Îlots: NetBird Groups + ACLs
    - Services: Matrix, Nextcloud, Vaultwarden...
```

---

## 7. Infrastructure Provisioning

### Stack existante

```yaml
IaC: Terragrunt (wrapper Terraform)
Config Management: Ansible
Identity: Zitadel (déjà en place)
Mesh: NetBird (décidé)
```

### Core Self-Hosted (Bootstrap Phase)

```yaml
Hardware:
  CPU: AMD Ryzen 9 5950X
  RAM: 96 GB
  Storage: NVMe + ZFS RAID
  NAS: Synology DS1522+

Virtualisation: Proxmox
  VMs:
    - Talos (Kubernetes)
      - Zitadel
      - NetBird Control Plane
      - Store (Nuxt + Medusa)
      - Monitoring (Prometheus/Grafana)
    - PostgreSQL (VM dédiée)
```

### Architecture Cloud Hybride

```
┌─────────────────────────────────────────────────────────────────┐
│                    TUTTLE INFRASTRUCTURE (MVP)                   │
├─────────────────────────────────────────────────────────────────┤
│  CORE - Self-Hosted (Proxmox)                                   │
│  └─ Zitadel, NetBird CP, Store, PostgreSQL, Monitoring          │
│                              │                                   │
│              ┌───────────────┴───────────────┐                  │
│              ▼                               ▼                  │
│       ┌─────────────┐                 ┌─────────────┐           │
│       │  SANCTUARY  │                 │  BALANCED   │           │
│       │   (2 VPS)   │                 │   (2 VPS)   │           │
│       ├─────────────┤                 ├─────────────┤           │
│       │ 1984 (IS)   │                 │ Hetzner(DE) │           │
│       │ FlokiNET(RO)│                 │ Hetzner(FI) │           │
│       └─────────────┘                 └─────────────┘           │
└─────────────────────────────────────────────────────────────────┘
```

### Terraform Providers - Recherche

| Provider | Terraform | API | Automation |
|----------|-----------|-----|------------|
| **1984 Hosting** (IS) | ❌ | ❌ | Manuel + Ansible |
| **FlokiNET** (IS/RO) | ❌ | ❌ | Manuel + Ansible |
| **Njalla** (SE) | ⚠️ DNS only | ✅ | VPS manuel |
| **BuyVM** (LU) | ❌ | ✅ Stallion | Custom possible |
| **Hetzner Cloud** | ✅ Official | ✅ | Full IaC |
| **Vultr** | ✅ Official | ✅ | Full IaC |

**Stratégie :**
- SANCTUARY: Provisioning manuel + Ansible config
- BALANCED: Full IaC (Terragrunt + Ansible)

### Structure Terragrunt

```
tuttle-infra/
├── terragrunt.hcl
├── _envcommon/
│   └── vpn-node.hcl
├── environments/
│   ├── production/
│   │   ├── balanced/
│   │   │   ├── hetzner-de/
│   │   │   └── hetzner-fi/
│   │   └── sanctuary/
│   │       └── _manual.md
│   └── staging/
├── modules/
│   └── vpn-node/
└── ansible/
    ├── inventory/
    ├── playbooks/
    └── roles/
        ├── wireguard/
        ├── v2ray/
        └── netbird-peer/
```

### Intégration Nodes ↔ Core

**Problème :** Core self-hosted derrière NAT/CGNAT, pas de ports ouverts.

**Solution :** NetBird mesh pour l'administration

```yaml
NetBird Network:
  Peers:
    - core-services (Proxmox)
    - vpn-node-1984-is
    - vpn-node-flokinet-ro
    - vpn-node-hetzner-de
    - vpn-node-hetzner-fi

  Groups:
    - admin: [core-services]
    - vpn-nodes: [vpn-node-*]

  ACLs:
    - vpn-nodes → core:9090 (Prometheus)
    - vpn-nodes → core:8080 (VPN API)
    - admin → ALL
```

### VPN API (à développer)

Service Go qui gère les nodes et users :

```yaml
Endpoints:
  # Node management
  POST /api/v1/nodes/register
  GET  /api/v1/nodes/{id}/config
  POST /api/v1/nodes/{id}/health

  # User management
  POST /api/v1/users/{id}/config
  GET  /api/v1/users/{id}/devices

  # Legislative Weather
  GET  /api/v1/pools
  GET  /api/v1/pools/{pool}/nodes

Auth:
  Users: JWT Zitadel (OIDC)
  Nodes: mTLS (certificat par node)
```

### Authentification Nodes (mTLS)

```
Tuttle Root CA (offline, cold storage)
    └── Tuttle Nodes CA (intermédiaire)
            ├── vpn-node-1984-is.crt
            ├── vpn-node-flokinet-ro.crt
            ├── vpn-node-hetzner-de.crt
            └── vpn-node-hetzner-fi.crt
```

### Boot Sequence Node

```
1. [OS Boot]
2. [NetBird Agent] → Connecte au Control Plane → IP mesh (100.64.x.x)
3. [Tuttle Agent] → POST /api/v1/nodes/register (via NetBird)
4. [VPN API] → Retourne config (pool, peers, DNS)
5. [WireGuard] → Configure wg0, écoute :51820
6. [Health Loop] → POST /health toutes les 30s
```

### Schéma réseau

```
                         INTERNET
                             │
          ┌──────────────────┼──────────────────┐
          │                  │                  │
    ┌─────┴─────┐      ┌─────┴─────┐      ┌─────┴─────┐
    │  User A   │      │  User B   │      │  User C   │
    └─────┬─────┘      └─────┬─────┘      └─────┬─────┘
          │ WireGuard (:51820)                  │
    ┌─────┴─────┐      ┌─────┴─────┐      ┌─────┴─────┐
    │ VPN Node  │      │ VPN Node  │      │ VPN Node  │
    │ 1984 (IS) │      │Hetzner(DE)│      │Hetzner(FI)│
    └─────┬─────┘      └─────┬─────┘      └─────┬─────┘
          │    NetBird Mesh (admin)             │
          └──────────────────┼──────────────────┘
                             │
                     ┌───────┴───────┐
                     │  CORE (Home)  │
                     │   Proxmox     │
                     │ (NAT/no port) │
                     └───────────────┘
```

---

## Session Insights

> **Hybridité confirmée :** Sérieux technique (WireGuard, natif, Legislative Weather) + Vision souveraineté (state-resistant, no-log, transparence).

> **Solo-challenger approach :** BMAD specs NASA + Agents AI = viable pour 5 apps natives.

> **Différenciateur clé :** Legislative Weather - personne ne fait ça.

> **🚀 GAME-CHANGER :** Le LightWeb - un intranet souverain pour particuliers avec îlots et services internes. VPN = cash cow, LightWeb = différenciateur.

> **Bootstrap viable :** Self-hosted Core (Proxmox/Talos) + 4 VPN nodes cloud = MVP réaliste avec budget limité.

> **NetBird > Headscale :** Choix technique, pas marketing. Headscale ne supporte pas les groupes OIDC dans les ACLs réseau (bloquant pour îlots).

> **Infra as Code partiel :** SANCTUARY = manuel (pas d'API), BALANCED = full IaC (Hetzner/Vultr). Ansible unifie la config.
