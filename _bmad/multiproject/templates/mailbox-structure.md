# Structure _mailbox/

## Convention de dossiers

Chaque projet BMAD Multi-Project doit avoir cette structure :

```
_mailbox/
├── inbox/                    # Transmissions reçues (à traiter)
├── outbox/                   # Transmissions à envoyer (zone de transit)
├── sent/                     # Transmissions envoyées (historique local, optionnel)
├── archive/                  # Transmissions traitées
│   ├── 2026-01/
│   ├── 2026-02/
│   └── ...
└── .mailbox-config.yaml      # Configuration locale
```

## Rôle de chaque dossier

### inbox/

**Transmissions reçues en attente de traitement.**

- Scannée par `workflow-status` à chaque lancement
- Fichiers traités selon règles de triage dans `.mailbox-config.yaml`
- Après traitement → déplacé vers `archive/YYYY-MM/`

### outbox/

**Zone de transit pour les transmissions à envoyer.**

- L'agent crée la transmission ici
- Si destination accessible (path existe) → copie vers `destination/_mailbox/inbox/` + suppression
- Si destination non accessible → reste ici jusqu'à sync externe (git push/pull)

**Important :** L'outbox ne doit pas grossir. C'est une zone de transit, pas de stockage.

### sent/ (optionnel)

**Historique local des transmissions envoyées.**

- Copie des transmissions dispatchées depuis outbox
- Utile pour traçabilité locale
- Peut être désactivé pour économiser l'espace

### archive/

**Transmissions traitées, organisées par mois.**

- Structure : `archive/YYYY-MM/TX_xxx.md`
- Rétention configurable dans `.mailbox-config.yaml`
- Compression automatique après X jours (optionnel)

### .mailbox-config.yaml

**Configuration du comportement de la mailbox.**

- Règles de triage automatique
- Subscriptions et filtres
- Escalation et timeouts
- Voir template `mailbox-config.yaml`

## Convention de nommage des fichiers

```
TX_{from}_{YYYY-MM-DD}_{HHhMM}_{suffix}.md
```

| Composant | Description |
|-----------|-------------|
| `TX` | Préfixe fixe |
| `{from}` | ID projet source |
| `{YYYY-MM-DD}` | Date |
| `{HHhMM}` | Heure (ex: 14h30) |
| `{suffix}` | 8 chars hex unique |

**Exemples :**
```
TX_site-web_2026-01-26_14h30_a7f3b2c1.md
TX_master_2026-01-27_09h15_b2c4d6e8.md
```

## Projets dépréciés

Quand un projet est déprécié (`status: deprecated` dans hierarchy.csv) :

1. **Le dossier `_mailbox/` est conservé** — pour l'historique
2. **Plus aucune transmission n'est envoyée** vers ce projet
3. **Les transmissions entrantes sont rejetées** automatiquement (sauf `hierarchy-update`)
4. **Un fichier `deprecated.md`** est créé à la racine expliquant :
   - Ce que faisait le projet
   - Pourquoi il a été déprécié
   - Le projet de remplacement (si applicable)

Les agents IA qui rencontrent `deprecated.md` comprennent le contexte sans chercher à interagir.

## Initialisation

Pour initialiser la structure mailbox dans un projet :

```bash
mkdir -p _mailbox/{inbox,outbox,sent,archive}
cp templates/mailbox-config.yaml _mailbox/.mailbox-config.yaml
```

Ou utiliser le workflow `init-multi-project`.
