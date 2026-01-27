---
name: multiproject-check
description: "Module à inclure au début de workflow-status pour support Multi-Project"
version: 1.0.0
---

# Multi-Project Check

**Ce module est exécuté au début de workflow-status.**

---

## 1. Détection

```
SI fichier "hierarchy.csv" existe à la racine du projet:
    → Mode Multi-Project activé
    → Continuer avec ce module
SINON:
    → Skip ce module
    → Continuer workflow-status standard
```

---

## 2. Chargement configuration

Charger :
- `hierarchy.csv` → structure des projets
- `_mailbox/.mailbox-config.yaml` → règles de triage

Identifier le projet courant (celui avec `path=./` dans hierarchy.csv).

---

## 3. Vérification statut projet

```
SI projet courant a status "deprecated":
    Afficher avertissement
    Demander confirmation pour continuer
```

---

## 4. Scanner inbox

Lister tous les fichiers `_mailbox/inbox/TX_*.md`

Pour chaque fichier :
- Parser le frontmatter YAML
- Extraire : type, priority, from_project, thread_id

Trier par priorité : critical > high > medium > low

---

## 5. Appliquer triage

### Auto-handle (traitement automatique)

| Type | Action | Résultat |
|------|--------|----------|
| `info` | archive | Déplacer vers archive/ |
| `ack` | update_tracking_archive | Màj tracking + archive |
| `dependency` | add_to_backlog | Ajouter au backlog + archive |
| `bug` | workflow_create_story | Créer story + archive |
| `hierarchy-update` | update_local_hierarchy | Màj hierarchy.csv + archive |
| `project-update` | update_local_hierarchy | Màj hierarchy.csv + archive |
| `escalation` | notify_and_prioritize | Notifier (garder en inbox) |

### Require attention (traitement manuel)

| Type | Condition | Suggestion |
|------|-----------|------------|
| `architectural` | toujours | propose_review |
| `feature-request` | priority critical/high OU impacts_multiple_projects | propose_brainstorm |
| `feature` | complexité détectée | propose_review |

---

## 6. Affichage

```
╔══════════════════════════════════════════════════════════════╗
║ 📬 TRANSMISSIONS MULTI-PROJECT                               ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║ ✅ Auto-traité (N):                                          ║
║   • {filename} [{type}] → {action}                           ║
║   • ...                                                      ║
║                                                              ║
║ 🔴 Attention requise (N):                                    ║
║                                                              ║
║ [1] {filename}                                               ║
║     From: {from_project} | Type: {type} | Priority: {prio}   ║
║     "{titre}"                                                ║
║     → Suggestion: {action}                                   ║
║                                                              ║
║ [1-n] Traiter  [A] Toutes  [S] Skip  [D] Détails             ║
╚══════════════════════════════════════════════════════════════╝
```

---

## 7. Actions utilisateur

### [numéro] Traiter une transmission

Afficher détails complets puis proposer :

| Action | Raccourci | Description |
|--------|-----------|-------------|
| Brainstorming | [B] | Lancer workflow brainstorming |
| Research | [R] | Lancer workflow research |
| Créer story | [C] | Lancer workflow create-story |
| Correct-course | [W] | Lancer workflow correct-course |
| Archiver | [A] | Marquer integrated + archiver |
| Rejeter | [X] | Demander justification + créer transmission rejet |
| Skip | [S] | Traiter plus tard |

### [A] Traiter toutes avec suggestions

Exécuter l'action suggérée pour chaque transmission.

### [S] Skip

Continuer vers workflow-status standard sans traiter.

### [D] Détails

Afficher le contenu complet d'une transmission.

---

## 8. Dispatch outbox (fin de workflow)

**À exécuter à la fin du workflow-status standard.**

```
SI _mailbox/outbox/ contient des fichiers:
    POUR CHAQUE transmission:
        Résoudre destination(s) depuis to_project
        Filtrer les projets deprecated

        POUR CHAQUE destination accessible (path existe):
            Copier vers destination/_mailbox/inbox/

        SI au moins une copie réussie:
            Déplacer transmission vers _mailbox/sent/

        Afficher résumé dispatch
```

---

## Intégration

### Dans workflow-status.md, ajouter au début :

```markdown
## 0. Multi-Project Check

Charger et exécuter `multiproject-check.md` si hierarchy.csv existe.
```

### À la fin de workflow-status.md, ajouter :

```markdown
## N. Dispatch Outbox (Multi-Project)

Si mode Multi-Project actif, dispatcher les transmissions en outbox.
```
