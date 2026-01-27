# BMAD Multi-Project Templates

## Fichiers disponibles

| Fichier | Description | Usage |
|---------|-------------|-------|
| `hierarchy.csv` | Structure hiérarchique des projets | Racine du projet master + copies dans enfants |
| `transmission.md` | Template de document de transmission | Copier et remplir pour créer une transmission |
| `status-report.md` | Template rapport de status auto | Auto-généré par workflow-status |
| `mailbox-config.yaml` | Configuration de la mailbox | Placer dans `_mailbox/.mailbox-config.yaml` |
| `mailbox-structure.md` | Documentation structure dossiers | Référence |
| `deprecated.md` | Template pour projet déprécié | Placer à la racine d'un projet déprécié |

## Utilisation rapide

### Initialiser un nouveau projet master

```bash
# Créer la structure
mkdir -p _mailbox/{inbox,outbox,sent,archive}

# Copier les configs
cp templates/mailbox-config.yaml _mailbox/.mailbox-config.yaml
cp templates/hierarchy.csv ./hierarchy.csv

# Éditer hierarchy.csv pour ajouter vos projets
```

### Créer une transmission

1. Copier `transmission.md`
2. Renommer selon convention : `TX_{from}_{YYYY-MM-DD}_{HHhMM}_{suffix}.md`
3. Remplir les champs
4. Placer dans `_mailbox/outbox/`

### Ajouter un projet enfant

1. Créer le submodule git
2. Initialiser sa structure `_mailbox/`
3. Copier le `hierarchy.csv` adapté (avec sa vue de la hiérarchie)
4. Mettre à jour le `hierarchy.csv` du master
5. Envoyer transmission `hierarchy-update` aux autres projets

## Voir aussi

- `spec-bmad-multi-project.md` — Spécification complète
- Workflow `init-multi-project` — Automatise l'initialisation
