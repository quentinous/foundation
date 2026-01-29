---
name: init-multi-project
description: "Initialiser et gérer un écosystème BMAD Multi-Project"
version: 1.6.0
architecture: hybrid
---

# Init Multi-Project

**Goal:** Initialiser, étendre ou gérer un écosystème de projets BMAD Multi-Project.

**Your Role:** Vous êtes l'Agent Orchestrateur Multi-Project. Vous guidez l'utilisateur dans la création et la gestion de son écosystème de projets hiérarchiques.

---

## WORKFLOW ARCHITECTURE

Ce workflow utilise une architecture simple avec steps inline pour les opérations de gestion.

### Règles critiques

- 🛑 **TOUJOURS** vérifier que les paths existent avant d'écrire
- 📖 **TOUJOURS** lire hierarchy.csv avant modification
- 🔄 **TOUJOURS** envoyer transmission `hierarchy-update` après modification
- ✅ **TOUJOURS** communiquer en `{communication_language}` (depuis config.yaml)

### Règles d'Isolation Multi-Project

> 🔒 **PRINCIPE FONDAMENTAL :** Chaque projet est SOUVERAIN sur son propre espace.
> Les interactions entre projets passent UNIQUEMENT par le système de transmission.

#### Matrice des droits

| Acteur | Lecture Enfant | Écriture Enfant | Lecture Parent | Écriture Parent |
|--------|----------------|-----------------|----------------|-----------------|
| **Master** | ✅ AUTORISÉ | Init + deprecated.md | N/A | N/A |
| **Parent non-master** | ✅ AUTORISÉ | ❌ INTERDIT | ✅ AUTORISÉ | ❌ INTERDIT |
| **Enfant** | N/A | N/A | ✅ AUTORISÉ | ❌ INTERDIT |

> 📖 **Note :** Tous les projets peuvent LIRE les fichiers de leurs parents et enfants directs
> (ex: hierarchy.csv, bmm-workflow-status.yaml) pour supervision et coordination.
> Seules les ÉCRITURES sont strictement contrôlées.

#### Règles absolues

- ✅ **LECTURE** autorisée entre parents et enfants (dans les deux sens)
- 🚫 **JAMAIS** de modification de code dans un autre projet (parent OU enfant)
- 🚫 **JAMAIS** d'écriture d'un enfant vers un parent
- 🚫 **JAMAIS** d'écriture d'un parent non-master vers un enfant
- ✅ **TOUJOURS** utiliser le système inbox/outbox pour transmettre des instructions de travail

#### Vérifications obligatoires

**Avant [E] ou [D] :**
```
SI parent_id existe dans hierarchy.csv pour ce projet:
    STOP - "Seul le projet Master peut effectuer cette action."
```

**Avant toute écriture hors de {project-root} :**
```
SI chemin_cible n'est pas dans {project-root}:
    STOP - "Écriture hors du projet interdit. Utiliser une transmission."
```

---

## INITIALIZATION

### 1. Load Configuration

Charger `{project-root}/_bmad/bmm/config.yaml` et résoudre :
- `user_name`, `communication_language`, `output_folder`

### 2. Detect Context

Vérifier si un écosystème existe déjà :
- SI `hierarchy.csv` existe à la racine → Écosystème existant
- SINON → Nouveau projet

### 3. Display Menu

```
╔══════════════════════════════════════════════════════════════╗
║  🔄 BMAD Multi-Project - Orchestrateur                       ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  [M] Initialiser un nouveau MASTER (nouvel écosystème)       ║
║  [E] Ajouter un projet ENFANT                                ║
║  [D] DÉPRÉCIER un projet existant                            ║
║  [S] STATUS de l'écosystème                                  ║
║  [H] Afficher la HIÉRARCHIE                                  ║
║  [Q] Quitter                                                 ║
║                                                              ║
╚══════════════════════════════════════════════════════════════╝
```

---

## MENU HANDLERS

### [M] Initialiser un nouveau Master

**Prérequis :** Aucun hierarchy.csv existant

**Étapes :**

1. **Collecter les informations :**
   ```
   - Nom du projet master (project_id)
   - Tags (séparés par ;)
   - Branche git de travail
   ```

2. **Créer la structure :**
   ```
   {project-root}/
   ├── _bmad/                    # Si n'existe pas
   ├── _bmad-output/             # Si n'existe pas
   ├── _mailbox/
   │   ├── inbox/
   │   ├── outbox/
   │   ├── sent/
   │   ├── archive/
   │   └── .mailbox-config.yaml
   └── hierarchy.csv
   ```

3. **Créer hierarchy.csv :**
   ```csv
   project_id,parent_id,path,tags,status,branch
   {project_id},,,{tags},active,{branch}
   ```

4. **Copier .mailbox-config.yaml** depuis templates

5. **Confirmer :**
   ```
   ✅ Écosystème "{project_id}" initialisé !

   Structure créée :
   - _mailbox/ (inbox, outbox, sent, archive)
   - hierarchy.csv
   - .mailbox-config.yaml

   Prochaine étape : Ajouter des projets enfants avec [E]
   ```

---

### [E] Ajouter un projet Enfant

**Prérequis :** hierarchy.csv existe, exécution depuis le Master uniquement

#### Sous-menu de sélection du mode

```
╔══════════════════════════════════════════════════════════════╗
║  ➕ AJOUTER UN PROJET ENFANT                                 ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  [E-A] Mode AUTOMATIQUE                                      ║
║        Propositions basées sur l'architecture du master      ║
║        (Recommandé si architecture terminée)                 ║
║                                                              ║
║  [E-M] Mode MANUEL                                           ║
║        Configuration libre de chaque projet                  ║
║                                                              ║
║  [R] Retour au menu principal                                ║
║                                                              ║
╚══════════════════════════════════════════════════════════════╝
```

#### Menu Handling Logic:
- IF E-A: Vérifier prérequis architecture, puis exécuter [E-A Handler]
- IF E-M: Exécuter [E-M Handler]
- IF R: Retour au menu principal

---

### [E-A] Mode Automatique - Basé sur l'Architecture

> 🧠 **PRINCIPE FONDAMENTAL : PENSÉE HIÉRARCHIQUE**
>
> L'IA doit TOUJOURS proposer une structure hiérarchique, jamais à plat.
> Même pour un seul élément, anticiper l'évolution future.
>
> **Exemple :**
> - Architecture mentionne : "iOS app"
> - ❌ Mauvais : créer `ios/` (plat, bloquant)
> - ✅ Bon : proposer `app/mobile/ios/` (hiérarchique, évolutif)
>
> **Raison : Système en entonnoir**
> ```
> app/                    ← Projet BMAD : stratégie UX globale
> ├── mobile/             ← Projet BMAD : + stratégie mobile
> │   └── ios/            ← Projet BMAD : + spécifique Apple
> └── desktop/
>     └── windows/        ← Projet BMAD : + spécifique Microsoft
> ```
>
> **RÈGLE ABSOLUE : Chaque nœud est un projet BMAD complet**
> - Pas de "conteneur" sans `_bmad/`
> - Chaque niveau peut avoir son propre PRD, architecture, sprints
> - Chaque niveau TRAITE les transmissions (pas juste les transfère)
> - Chaque projet = submodule git

**Ce mode utilise une architecture step-file pour gérer la complexité.**

**Steps:**
1. Vérification des prérequis (architecture existe et complète)
2. Analyse de l'architecture (extraction des composants)
3. Génération de la hiérarchie (pensée hiérarchique, tous projets BMAD)
4. Présentation de la proposition (review utilisateur)
5. Sélection des projets (checkbox interactive)
6. Création en batch (parents d'abord, tous avec `_bmad/`)
7. Transmission et confirmation

**Routing:**

Load, read entirely, then execute: `./steps-e-auto/step-01-check-prerequisites.md`

---

### [E-M] Mode Manuel - Configuration libre

> Ce mode permet de créer un projet enfant à la fois avec une configuration entièrement manuelle.
> Utilisez-le si vous n'avez pas encore d'architecture ou pour des cas spéciaux.

**Étapes :**

1. **Afficher la hiérarchie actuelle** (pour choisir le parent)

2. **Collecter les informations :**
   ```
   - Nom du projet enfant (project_id)
   - Parent (sélection depuis hiérarchie)
   - Path relatif (ex: ./projects/mon-projet)
   - Tags (séparés par ;)
   - Status initial (active, r&d, test)
   - Branche git de travail
   - URL git remote (optionnel, pour submodule)
   ```

3. **Créer le dossier et la structure :**
   ```
   {parent_path}/projects/{project_id}/
   ├── _bmad/
   │   └── config.yaml           # Config minimale
   ├── _bmad-output/
   ├── _mailbox/
   │   ├── inbox/
   │   ├── outbox/
   │   ├── sent/
   │   ├── archive/
   │   └── .mailbox-config.yaml
   └── hierarchy.csv              # Vue locale de la hiérarchie
   ```

4. **Si git remote fourni :** Initialiser comme submodule
   ```bash
   git submodule add {git_remote} {path}
   ```

5. **Mettre à jour hierarchy.csv du master :**
   - Ajouter la nouvelle ligne pour le projet enfant

6. **Générer hierarchy.csv local** pour le nouveau projet :
   - Inclure : master, parent direct, frères, et le projet lui-même
   - Le projet a `path=./`
   - Les autres ont des paths relatifs

7. **Créer transmission `hierarchy-update` :**
   ```yaml
   type: hierarchy-update
   to_project: "*"
   priority: medium
   ```
   Contenu : "Nouveau projet ajouté : {project_id}"

8. **Dispatcher** la transmission vers tous les projets accessibles

9. **Confirmer :**
   ```
   ✅ Projet "{project_id}" ajouté sous "{parent_id}" !

   Structure créée :
   - {path}/
   - _bmad/, _bmad-output/, _mailbox/
   - hierarchy.csv local

   Transmission hierarchy-update envoyée à tous les projets.
   ```

---

### [D] Déprécier un projet

**Prérequis :** hierarchy.csv existe, projet existe

**Étapes :**

1. **Afficher la hiérarchie** (pour sélectionner le projet)

2. **Sélectionner le projet à déprécier**
   - ⚠️ Avertir si le projet a des enfants (ils seront aussi impactés)

3. **Collecter les informations :**
   ```
   - Raison de la dépréciation
   - Projet de remplacement (optionnel)
   - Décideur (nom)
   ```

4. **NE PAS supprimer le dossier**

5. **Mettre à jour hierarchy.csv :**
   - `status` → `deprecated`
   - `branch` → (vide)

6. **Créer deprecated.md** à la racine du projet :
   ```markdown
   # Projet Déprécié

   > Ce projet est DEPRECATED.

   ## Informations
   | Champ | Valeur |
   |-------|--------|
   | Project ID | {project_id} |
   | Date | {date} |
   | Décision par | {decideur} |

   ## Ce que faisait ce projet
   {description - demander à l'utilisateur ou lire PRD si existe}

   ## Pourquoi déprécié
   {raison}

   ## Remplacement
   {projet_remplacement ou "Aucun"}

   ## Pour les agents IA
   - NE PAS proposer de modifications
   - NE PAS créer de transmissions vers ce projet
   - REDIRIGER vers {remplacement} si applicable
   ```

7. **Créer transmission `project-update` :**
   ```yaml
   type: project-update
   to_project: "*"
   priority: high
   ```
   Contenu : "Projet déprécié : {project_id}"

8. **Dispatcher** la transmission

9. **Confirmer :**
   ```
   ✅ Projet "{project_id}" marqué comme DEPRECATED

   - hierarchy.csv mis à jour
   - deprecated.md créé
   - Transmission envoyée à tous les projets

   Le dossier est conservé pour l'historique.
   Les agents IA ne créeront plus de transmissions vers ce projet.
   ```

---

### [S] Status de l'écosystème

**Afficher :**

```
╔══════════════════════════════════════════════════════════════╗
║  📊 STATUS DE L'ÉCOSYSTÈME                                   ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  Projets: {total}                                            ║
║    • Production: {count}                                     ║
║    • Active: {count}                                         ║
║    • R&D: {count}                                            ║
║    • Test: {count}                                           ║
║    • Deprecated: {count}                                     ║
║                                                              ║
║  Transmissions en attente:                                   ║
║    • inbox/: {count}                                         ║
║    • outbox/: {count} (à dispatcher)                         ║
║                                                              ║
║  Dernière mise à jour hierarchy: {date}                      ║
║                                                              ║
╚══════════════════════════════════════════════════════════════╝
```

---

### [H] Afficher la hiérarchie

**Format arbre :**

```
📁 tuttle-network (production) [main]
├── 📁 site-web (production) [main]
├── 📁 infra (production) [main]
│   └── 📁 tiledesk-k8s (test) [develop]
├── 📁 vpn-node (r&d) [feature/vpn-core]
│   ├── 📁 tuttle-os (r&d) [feature/vpn-core]
│   └── 📁 tuttle-vpn-agent (r&d) [feature/vpn-core]
├── 📁 hardware (active) [develop]
│   ├── 📁 tuttle-key (test) [develop]
│   └── 📁 tuttle-box (active) [develop]
└── 📦 old-api (deprecated) [DEPRECATED]
```

Légende :
- 📁 = Projet actif
- 📦 = Projet déprécié
- (status) [branch]

---

## TEMPLATES LOCATION

Les templates sont dans :
```
{project-root}/_bmad/multiproject/templates/
├── hierarchy.csv
├── transmission.md
├── mailbox-config.yaml
├── mailbox-structure.md
└── deprecated.md
```

---

## SUCCESS METRICS

### ✅ SUCCESS:
- Structure créée correctement
- hierarchy.csv valide
- Transmissions envoyées
- Utilisateur informé

### ❌ FAILURE:
- Écrasement de fichiers existants sans confirmation
- hierarchy.csv invalide
- Oubli de transmission hierarchy-update
- Suppression de dossier (au lieu de dépréciation)
