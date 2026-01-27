# Templates Paths

## Location

Les templates BMAD Multi-Project sont situés dans :

```
{project-root}/_bmad-output/bmb-creations/bmad-multi-project/templates/
```

## Fichiers disponibles

| Template | Path relatif | Usage |
|----------|--------------|-------|
| hierarchy.csv | `templates/hierarchy.csv` | Structure hiérarchique |
| transmission.md | `templates/transmission.md` | Document de transmission |
| mailbox-config.yaml | `templates/mailbox-config.yaml` | Config mailbox |
| deprecated.md | `templates/deprecated.md` | Projet déprécié |

## Utilisation dans le workflow

### Pour initialiser _mailbox/

```bash
# Créer structure
mkdir -p _mailbox/{inbox,outbox,sent,archive}

# Copier config
cp {templates}/mailbox-config.yaml _mailbox/.mailbox-config.yaml
```

### Pour créer hierarchy.csv

Utiliser le header du template et ajouter les lignes de projets :

```csv
project_id,parent_id,path,tags,status,branch
```

### Pour déprécier un projet

Copier et adapter `deprecated.md` à la racine du projet déprécié.

## Variables à résoudre

- `{project-root}` : Racine du projet master
- `{templates}` : `{project-root}/_bmad-output/bmb-creations/bmad-multi-project/templates`
- `{communication_language}` : Depuis `_bmad/bmb/config.yaml`
