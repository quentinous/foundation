---
stepsCompleted: [1, 2, 3, 4, 5, 6]
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
date: 2026-01-28
author: Quentin
status: completed
current_step: 6
next_step: null
completed_at: 2026-01-28
enrichments_applied:
  - Pre-mortem Analysis
  - Architecture Decision Records
  - First Principles Analysis
  - Comparative Analysis Matrix
  - Red Team vs Blue Team
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

### Fiche Récap V1 (Active Recall)

*Mémoriser en 30 secondes*

```
TUTTLE V1 EN 30 SECONDES
────────────────────────
Quoi     : VPN + Clean Pipe (filtrage famille)
Pour qui : Parents protecteurs (Christophe)
Contre   : Le contrôle parental FAI gratuit
Message  : "Internet propre pour votre famille"
Tagline  : "Le contrôle parental de votre box, en mieux."
Prix     : 12€/mois
Distrib  : Web (pub Facebook/Google)
────────────────────────
PAS dans V1 : Hardware, Network, Référents, B2B
────────────────────────
V1.5     : + Tuttle-Key (HSM)
V2.0     : + Network + Box + Référents
```

**Pitch 10 mots :**
> "VPN + filtrage famille. Internet propre, partout, privé."

**Différenciateurs V1 (vs NordVPN) :**
1. Clean Pipe (filtrage famille dédié)
2. Valeurs / Manifeste
3. Transparence (open source, threat model)

**Concurrent réel V1 :**
> Pas NordVPN. Le **contrôle parental GRATUIT** de la Freebox/Livebox.

---

### Vue d'ensemble

L'écosystème Tuttle s'adresse à **6 profils utilisateurs**, fait face à **4 adversaires identifiés**, et évolue dans un **paysage concurrentiel de 4 acteurs**. Cette cartographie complète permet de concevoir un produit résilient et aligné avec sa mission.

---

### Utilisateurs Primaires

#### 1. Robert — L'Activiste Anonyme

| Attribut | Détail |
|----------|--------|
| **Âge** | 38 ans |
| **Profil** | Développeur freelance, payé en crypto, adresse inconnue |
| **OPSEC** | QubesOS/Tails, Monero exclusif, zéro compte |
| **Motivation** | Liberté absolue, tester les limites du système |

**Exigences non-négociables :**
- Zéro compte (pas d'email, pas de mot de passe)
- Paiement Monero uniquement
- App native signée (pas de web)
- Silent Auth (sa clé = sa session)
- Code auditable publiquement

**Valeur pour Tuttle :**
- Beta testeur extrême, crédibilité communauté hardcore, feedback technique impitoyable
- **Prescripteur high-trust** : Robert ne paiera peut-être jamais (il roule son propre setup), MAIS quand sa famille ou ses amis non-tech demandent "C'est quoi le meilleur VPN ?", il recommande Tuttle
- **Son pitch** : "Moi j'ai pas besoin de VPN commercial. Mais toi, prends Tuttle. C'est les seuls qui font les choses bien. Fiable pour les noobs, solide pour les geeks."
- **Effet de halo** : "Le VPN que même les paranos recommandent à leur mère"

---

#### 2. Jean-Marc — Le Consommateur Hybride

| Attribut | Détail |
|----------|--------|
| **Âge** | 45 ans |
| **Profil** | Cadre commercial, marié, 2 enfants (12 et 15 ans) |
| **Équipement** | Mac, iPhone, iPad famille |
| **Motivation** | Protection famille après harcèlement de son fils |

**Parcours :** A essayé NordVPN (trop marketing), ProtonVPN (trop complexe). Cherche "sérieux mais simple".

**Freins à lever :**
- "C'est pas un truc d'extrême droite ?" → Manifeste équilibré
- "C'est compliqué la crypto" → Stripe disponible
- "Ma femme va pas comprendre" → UX Aurora Day

**Valeur pour Tuttle :** Validation mainstream, bouche à oreille corporate, feedback UX grand public.

---

#### 3. Christophe — L'Éveillé Souverain (PRIMARY)

| Attribut | Détail |
|----------|--------|
| **Âge** | 42 ans |
| **Profil** | Responsable logistique, marié, 4 enfants (6-14 ans), Vendée |
| **Foi** | Catholique pratiquant |
| **Déclencheur** | Son fils de 12 ans tombe sur du contenu pornographique violent |

**Psychologie :**
- Protecteur viscéral — Sa famille est son royaume
- Traditionaliste modéré — Valeurs classiques sans intégrisme
- Homme d'action — Quand il décide, il agit

**Motivation MAXIMALE : Le Clean Pipe.** Pour Christophe, le VPN n'est pas le produit. La promesse "Internet familial nettoyé de la pornographie" l'est. Le reste vient avec.

**Aha Moment :** Premier rapport dashboard : "23 tentatives d'accès à du contenu adulte bloquées aujourd'hui". Les enfants n'ont rien vu. Il pleure presque.

**Valeur pour Tuttle :** Primary user archétypal, testimonial puissant, réseau paroissial, fidélité absolue.

---

#### 4. Marie-Bénédicte — La Matriarche Discrète

| Attribut | Détail |
|----------|--------|
| **Âge** | 30 ans |
| **Profil** | Mère au foyer, 5 enfants (2-10 ans), conseillère Guy Demarle |
| **Foi** | Catholique traditionaliste (FSSPX) |
| **Influence** | Discrète mais ses 15 copines suivent ses recommandations |

**Psychologie :**
- Stricte mais douce — Main de fer, gant de velours
- Intelligente — Comprend vite, pas besoin d'explications techniques
- Protectrice féroce — Le "monde à la dérive" ne touchera pas ses enfants

**Modèle mental :** Elle connaît Guy Demarle (réunions cuisine à domicile). Tuttle = "Guy Demarle de la sécurité numérique".

**Potentiel :** Si satisfaite, peut devenir Référente elle-même — "Réunions Tuttle" comme ses réunions cuisine.

**Valeur pour Tuttle :** Réseau féminin trad, influence invisible mais massive, modèle de distribution éprouvé.

---

#### 5. Sophie — L'Éveillée Malgré Elle

| Attribut | Détail |
|----------|--------|
| **Âge** | 32 ans |
| **Profil** | Journaliste presse mainstream (gauche), Paris, célibataire |
| **Trauma** | Viol par un "mineur isolé" de 35 ans d'apparence, relâché |
| **Double vie** | Journaliste le jour, lectrice Furia/Fdesouche la nuit |

**Psychologie :**
- Dissonance cognitive maximale — Écrit ce qu'elle ne croit plus
- Paranoïaque justifiée — Ses recherches nocturnes peuvent détruire sa carrière
- En reconstruction — Cherche la vérité même si ça détruit tout

**Besoin critique :** Séparer absolument son identité pro de ses recherches nocturnes. Si son historique fuite, sa carrière est morte.

**Aha Moment :** VPN activé pour ses lectures nocturnes. Elle peut enfin respirer. Personne ne saura.

**Valeur pour Tuttle :** Alliée médiatique potentielle (protection contre hit pieces, information sur attaques à venir).

---

### Utilisateurs Secondaires : Les Référents (V2.0+)

#### 6. Marc — Le Référent Technophile

| Attribut | Détail |
|----------|--------|
| **Âge** | 50 ans |
| **Profil** | Ancien DSI PME, consultant indépendant, Bretagne |
| **Compétences** | Expert réseaux (Cisco, WireGuard), Linux, sécurité |
| **Foi** | Catholique (Communauté de l'Emmanuel) |
| **Phase** | ⚠️ **Rôle actif à partir de V2.0 (Tuttle Network)** |

**Rôle V1.0-V1.5 :**
- Early adopter et testeur technique
- Feedback sur Tuttle-Key (V1.5)
- Pas encore Référent (pas de Network)

**Rôle V2.0+ (Référent Network) :**
- Organise des "Démos Techniques" (modèle vente à domicile)
- Aide à l'installation et à la "Cérémonie de Libération"
- Premier niveau de support pour son îlot
- Rémunéré par commission (~200€/mois pour 12 foyers)

**Îlot futur (V2.0) :** 12 foyers équipés, groupe Signal de support mutuel.

**Gouvernance (V2.0) :** Soumis au Consensus de Nettoyage — le Référent propose, l'îlot dispose (quorum de clés).

**Valeur pour Tuttle :** V1 = validation technique. V2 = Distribution organique, support décentralisé, feedback terrain.

---

### Adversaires Identifiés

#### 7. NEXUS Capital — L'Acquéreur Institutionnel

| Attribut | Détail |
|----------|--------|
| **Type** | Fonds M&A bancaire, €2B+ sous gestion |
| **Objectif** | Acquérir ou neutraliser les technologies disruptives |
| **Méthode** | Due diligence exhaustive, pentest déguisé, offre "impossible à refuser" |

**Comportement :** Contact "partenariat B2B", questions pointues sur architecture/gouvernance/funding, menace voilée si refus.

**Contre-mesures :** Transparency trap, architecture inopérable sans communauté, structure juridique anti-acquisition hostile.

---

#### 8. ARGUS Intelligence — L'Infiltré

| Attribut | Détail |
|----------|--------|
| **Type** | PME renseignement privé, 50-100 personnes ex-DGSE/DGSI |
| **Objectif** | Cartographier les réseaux dissidents pour clients étatiques |
| **Méthode** | Infiltration longue durée, social engineering, moyens illégaux |

**Comportement :** Client "très satisfait", questions techniques au support, tentative de relation personnelle, proposition de "partenariats".

**Contre-mesures :** Zero Trust absolu, cloisonnement support/architecture, canary traps, audit comportemental.

---

#### 9. Kermit — L'Hacktiviste Antifa

| Attribut | Détail |
|----------|--------|
| **Profil** | 26 ans, autodidacte, sphère antifa/cyberantifa |
| **Compétences** | Expert OSINT, database hacking, bruteforce, dox |
| **Motivation** | "Détruire les réseaux faf avant qu'ils ne grandissent" |

**Modus Operandi :**
1. Reconnaissance (scrape, stack analysis, cartographie personnes)
2. Dox & harcèlement (désanonymisation, signalements massifs)
3. Attaque technique (SQLi, bruteforce, dump & leak)
4. Déstabilisation (faux témoignages, DDoS, signalement "terrorisme")

**Contre-mesures :** Rate limiting agressif, WAF, auth résistante (Argon2, lockout, 2FA), logs anonymisés, canary tokens.

**Philosophie :** L'attaque de Kermit est une feature — si Tuttle résiste, c'est la preuve que le système marche.

---

#### 10. Étienne Verdier — Le Journaliste Hostile

| Attribut | Détail |
|----------|--------|
| **Profil** | 52 ans, rédacteur en chef adjoint Libération |
| **Réseau** | Fact-checkers, ministères, Élysée, DGSI |
| **Prisme** | Le système républicain est le meilleur possible |

**Ce qu'il écrira :** "Tuttle, le VPN qui séduit l'extrême droite... derrière ce vocabulaire libertarien se cache un projet politique inquiétant..."

**Conséquences orchestrées :** Signalement Pharos, pression hébergeurs/Stripe, question parlementaire.

**Contre-mesures :** Dossier presse préparé, témoignages variés, droit de réponse, mobilisation réseau, résilience infrastructure (déjà prévue).

**Judo :** L'attaque de Verdier = publicité gratuite dans la cible. Badge d'honneur.

---

### Paysage Concurrentiel

#### 11. Mullvad — Le Puriste

| Attribut | Détail |
|----------|--------|
| **Segment** | Privacy absolutiste |
| **Force** | Réputation impeccable, €5/mois fixe, pas de compte |
| **Faiblesse** | Aucun hardware, UX austère, pas de communauté |
| **Posture** | Observation silencieuse, potentiel allié |

---

#### 12. Proton AG — Le Mainstream Riche

| Attribut | Détail |
|----------|--------|
| **Segment** | "Privacy for the masses", écosystème complet |
| **Valorisation** | ~$1B+, 400+ personnes |
| **Force** | Marque forte, trésorerie massive |
| **Faiblesse** | Compromis privacy/mainstream, incidents passés (logs IP) |
| **Posture** | Étouffement soft, copie features, talent drain |

**Arsenal :** SEO warfare, FUD sponsorisé, recrutement devs Tuttle, lobbying réglementaire.

---

#### 13. NordVPN — Le Géant Mainstream

| Attribut | Détail |
|----------|--------|
| **Segment** | VPN grand public (streaming, geo-unblock) |
| **Valorisation** | ~$3B, 1500+ personnes |
| **Force** | Budget marketing démentiel, brand awareness |
| **Faiblesse** | Réputation privacy douteuse, incidents sécurité |
| **Nouveauté** | Cellule de veille IA (NLP temps réel sur réseaux sociaux) |

**Arsenal :** Astroturfing, pression influenceurs, price war, acquisition hostile via fonds.

---

#### 14. EnikmaVPN — Le Vrai Concurrent

| Attribut | Détail |
|----------|--------|
| **Segment** | VPN hardware dissident (sphère E&R) |
| **État** | En sommeil, base clients vieillissante, équipe 3-5 personnes |
| **Force** | Légitimité historique, relation E&R, premier sur le créneau |
| **Faiblesse** | Technologie datée, pas de dev actif depuis 2+ ans |
| **Posture** | Observation, vampirisation des idées Tuttle, comeback V2 |

**Scénario idéal :** Partenariat ou absorption — récupérer leur base clients et légitimité.

---

### User Journeys

#### Journey 1a : Christophe V1.0 (Acquisition web)

```
1. TRIGGER        → Fils tombe sur contenu inapproprié
2. RECHERCHE      → Google "VPN famille protection enfants"
3. DÉCOUVERTE     → Pub Facebook ou résultat "Tuttle - Internet propre"
4. LANDING        → Message clair, pas de jargon technique
5. ESSAI          → Souscription Stripe, app installée en 5 min
6. AHA MOMENT     → Dashboard : "23 contenus bloqués aujourd'hui"
7. FIDÉLITÉ       → Renouvellement automatique, bouche à oreille
```

#### Journey 1b : Christophe V2.0 (Via Référent)

```
1. DÉCOUVERTE     → Marc lui en parle après la messe
2. DÉMO           → Démonstration Tuttle-Box chez Marc
3. CÉRÉMONIE      → Génération des clés (Zero-Knowledge) avec Marc
4. CONNEXION      → Branchement Tuttle-Key, accès instantané
5. PAIX           → Environnement protégé, invisible, préservé
6. ÉVOLUTION      → Devient lui-même Référent
```

#### Journey 2 : L'Activiste Anonyme (Robert)

```
1. DÉCOUVERTE     → Mention sur forum .onion par contact de confiance
2. ANALYSE        → Lit TOUT : Manifeste, docs, GitHub, Threat Model
3. TEST           → Premier compte Monero, depuis Tails, via Tor
4. STRESS TEST    → Teste kill switch, leak DNS/WebRTC
5. HARDWARE       → Commande Tuttle-Key (point relais, faux nom)
6. VÉRIFICATION   → Firmware Verification obsessionnel
7. VALIDATION     → Remote Blind Signing fonctionne. Adoption.
```

#### Journey 3 : La Double Vie (Sophie)

```
1. DÉCOUVERTE     → Compte Twitter dissident mentionne Tuttle
2. RECHERCHE      → Manifeste résonne avec son histoire
3. TEST           → Abonnement crypto, depuis un café
4. USAGE          → VPN pour lectures nocturnes uniquement
5. LIBÉRATION     → Peut enfin respirer. Personne ne saura.
```

#### Journey 4 : Le Réseau Féminin (Marie-Bénédicte)

```
1. DÉCOUVERTE     → Marc (Référent paroisse) en parle après la messe
2. DÉMO           → Avec son mari chez Marc
3. DÉCISION       → Convainc son mari ("C'est comme Guy Demarle")
4. INSTALLATION   → Marc branche la Box, montre le Dashboard
5. AHA MOMENT     → "247 pubs, 12 trackers, 3 contenus adultes bloqués"
6. ÉVANGÉLISATION → Devient Référente, organise ses propres "Réunions Tuttle"
```

---

### Gouvernance

- **Consensus de Nettoyage** : Système de vote de proximité permettant aux membres d'un îlot d'exclure les éléments nuisibles par quorum de clés. Le Référent propose, l'îlot dispose.

- **Clean Pipe** : Politique de filtrage DNS par défaut (pornographie, publicités, trackers). Kill switch admin disponible pour désactivation rapide si nécessaire.

---

### Besoins cachés & Features demandées

*Issus du Focus Group utilisateurs (Christophe, Marie-Bénédicte, Jean-Marc, Sophie)*

#### Par persona

| Persona | Besoin caché | Feature/Réponse requise |
|---------|--------------|-------------------------|
| **Christophe** | Pérennité — "Et si Tuttle disparaît ?" | Architecture qui survit à Tuttle Corp (clés utilisateur = vraie propriété) |
| **Marie-Bénédicte** | Simplicité radicale + famille nombreuse | Mode multi-profils (7+ appareils), UX "Guy Demarle" sans jargon |
| **Jean-Marc** | Neutralité du message | Landing page "soft" sans vocabulaire militant, version corporate |
| **Sophie** | Invisibilité totale | Panic button, facturation anonyme ("Digital Services Ltd"), app sans logo reconnaissable |

#### Demandes unanimes

1. **Deux niveaux de dashboard**
   - **Mode Simple** : Indicateur vert/rouge "Votre foyer est protégé ✓" + chiffre global
   - **Mode Expert** : Détails complets (requêtes bloquées, logs, configs avancées)

2. **Message adaptable**
   - Version "Manifeste" : Militante, souveraineté, résistance (pour Christophe, Robert)
   - Version "Corporate" : Protection vie privée familiale, cybersécurité (pour Jean-Marc, entreprises)

3. **Plan B hardware**
   - Procédure claire si Tuttle-Key perdue/volée/confisquée
   - Seed phrase de récupération (déjà prévu)
   - Option de révocation à distance

4. **Facturation discrète**
   - Nom générique sur relevés bancaires ("TN Digital Services" ou similaire)
   - Crypto comme option par défaut pour les paranos
   - Pas de logo/nom Tuttle sur les factures PDF

5. **Trial sans friction**
   - Période d'essai sans carte bleue
   - Vérification compatibilité (Netflix, Teams, services courants)

6. **Clean Pipe configurable**
   - Option de voir les détails OU juste le résumé
   - Profils différenciés par âge (enfant 2-7 ans ≠ ado ≠ adulte)
   - Whitelist personnalisable

#### Frictions UX identifiées

| Friction | Verbatim | Solution |
|----------|----------|----------|
| Jargon technique | "Zero-Knowledge, HSM, Seed Phrase... vous parlez à qui ?" | Glossaire intégré + version "5 ans" de chaque concept |
| Perception MLM | "C'est quoi la différence avec du marketing de réseau ?" | Explication transparente du modèle Référent vs MLM |
| Prix vs NordVPN | "Pourquoi payer plus cher que 5€/mois ?" | Comparatif valeur (hardware, communauté, éthique) |
| Emballage militant | "Si je montre ça à ma femme, elle va penser complotiste" | Deux versions du site/onboarding |

---

### Threat Model & Durcissement

*Issu du Red Team vs Blue Team (Kermit, ARGUS, NEXUS vs Tuttle)*

#### Vulnérabilités identifiées

| Priorité | Vulnérabilité | Attaquant | Impact |
|----------|---------------|-----------|--------|
| 🔴 **CRITIQUE** | Infrastructure pas encore résiliente au déplatforming | Tous | Site down = mort du projet |
| 🔴 **CRITIQUE** | Structure juridique classique (SAS) = acquérable | NEXUS | Acquisition hostile possible |
| 🟠 **HAUTE** | Référents comme vecteur d'infiltration | ARGUS | Cartographie du réseau dissident |
| 🟠 **HAUTE** | Timing attaque réputation avant défenses prêtes | Kermit+ARGUS | Dégâts irréparables |
| 🟡 **MOYENNE** | Rate limiting bypassable via botnet distribué | Kermit | Bruteforce auth |
| 🟡 **MOYENNE** | Supply chain (prestataires) ciblable | ARGUS | Accès indirect aux données |

#### Matrice de durcissement

**Priorité 0 — AVANT lancement public**

| Action | Description | Responsable |
|--------|-------------|-------------|
| **Multi-provider infra** | Pas de SPOF : multi-cloud, multi-CDN, multi-registrar | Infra |
| **Structure juridique blindée** | SCIC, fondation, ou SAS avec pacte anti-acquisition | Legal |
| **Paiements crypto jour 1** | Monero + Bitcoin opérationnels, pas dépendant de Stripe | Dev |
| **War room documentée** | Procédures de crise : qui fait quoi si attaque coordonnée | Ops |
| **Canaux alternatifs** | Telegram, Signal, mailing list directe pour joindre clients si site down | Community |

**Priorité 1 — Premiers mois**

| Action | Description | Responsable |
|--------|-------------|-------------|
| **Vetting Référents** | Processus de vérification identité + motivation avant validation | Community |
| **Rate limiting avancé** | Par IP + par compte + par fingerprint navigateur | Dev |
| **Canary traps** | Faux comptes/données qui alertent si accédés | Security |
| **Analyse comportementale** | Détection patterns anormaux chez Référents et clients | Security |
| **Cloisonnement support** | Niveau 1 ne connaît rien de l'architecture technique | Ops |

**Priorité 2 — Consolidation**

| Action | Description | Responsable |
|--------|-------------|-------------|
| **Audit supply chain** | Vérification sécurité de tous les prestataires critiques | Security |
| **Bug bounty privé** | Programme pour chercheurs de confiance | Security |
| **Open source core** | Code critique ouvert = ne peut pas être "acquis" | Dev |
| **Conseil des Sages** | Référents seniors avec droit de véto sur décisions majeures | Governance |

#### Défenses par vecteur d'attaque

**Attaque technique (Kermit)**
```
Reconnaissance     → Cloudflare front, IP masquées, headers nettoyés
Bruteforce         → Rate limiting agressif, Argon2id, 2FA obligatoire admin
SQLi/Injection     → ORM exclusif, WAF OWASP, sanitization, DB chiffrée
DDoS               → Multi-CDN, failover automatique
```

**Infiltration (ARGUS)**
```
Social engineering → Support cloisonné, scripts standardisés, formation anti-SE
Faux Référent      → Vetting, données pseudonymisées, canary traps
Supply chain       → Multi-fournisseurs, audits prestataires
Insider threat     → Least privilege, audit logs, séparation des rôles
```

**Pression business (NEXUS)**
```
Due diligence      → Jamais de NDA asymétrique, métriques publiques uniquement
Réglementaire      → Conformité RGPD proactive, relation CNIL établie
Talent drain       → Equity + mission, bus factor réduit, documentation exhaustive
Acquisition        → Structure juridique résistante, communauté comme rempart
```

**Réputation (Kermit + Verdier)**
```
Dossier FUD        → Dossier presse préparé, témoignages diversifiés
Article hostile    → Alliés médiatiques, droit de réponse, documentation juridique
Déplatforming      → Multi-provider, paiements décentralisés, registrars résistants
```

#### Principe directeur : Antifragilité

> **L'attaque nous renforce.** Chaque tentative de déplatforming prouve notre valeur. Chaque article hostile est une publicité gratuite dans notre cible. Chaque audit de Kermit est un pentest gratuit. Architecture conçue pour que l'adversité soit une feature, pas un bug.

---

### Ajustements Personas & Résolutions

*Issus du Challenge Critique (avocat du diable)*

#### Reclassification des personas existants

| Persona | Rôle initial | Rôle ajusté | Raison |
|---------|--------------|-------------|--------|
| **Robert** | Primary User | Validateur technique + Prescripteur high-trust | Trop niche pour être client direct, MAIS recommande Tuttle à sa famille/amis non-geeks. "C'est le seul que je conseillerais à ma mère." |
| **Marie-Bénédicte** | Potentielle Référente | Prescriptrice | Pas le temps ni l'expertise technique, mais influence massive sur son réseau |
| **Sophie** | Utilisatrice | Utilisatrice (backstory simplifié) | Le besoin professionnel suffit, trauma non nécessaire pour justifier le persona |

#### Ajustements de positionnement

**Christophe — Message recentré**
> Proposition de valeur pour Christophe : **"Protection familiale complète, clé en main, sans compromis"**
> - NE PAS vendre la "souveraineté" (il ne comprend pas)
> - VENDRE le Clean Pipe + simplicité + communauté locale
> - Comparatif explicite vs Pi-hole/OpenDNS : "Tout configuré, support humain, pas de bidouillage"

**Jean-Marc — Comparatif Proton**
> Jean-Marc compare Tuttle à Proton. Différenciateurs explicites :
> - **Hardware** : Proton n'en a pas → Tuttle-Key = ancre de confiance physique
> - **Communauté locale** : Proton est impersonnel → Marc aide Jean-Marc en personne
> - **Clean Pipe intégré** : Proton ne filtre pas → Tuttle protège toute la famille par défaut
> - **Valeurs alignées** : Proton a cédé aux autorités → Tuttle architecture Zero-Knowledge

**Marc — Plan de succession**
> Le modèle Référent doit prévoir la rotation :
> - Rôle temporaire (2-3 ans recommandé), pas à vie
> - Formation de co-Référents dans chaque îlot
> - Îlots peuvent fusionner si Référent se retire
> - Motivation mission > motivation financière (200€ = bonus, pas driver)

#### Nouveaux personas ajoutés

---

**Persona #15 — David, Le Déçu**

| Attribut | Détail |
|----------|--------|
| **Âge** | 35 ans |
| **Profil** | Développeur web, early adopter, impatient |
| **Historique** | Client Tuttle pendant 3 mois, puis parti |

**Frustrations vécues :**
- Netflix et Disney+ bloqués par le VPN (géo-restrictions)
- Support trop lent (48h pour une réponse)
- Jargon incompréhensible ("Seed Phrase", "Derivation Path")
- App mobile bugguée au lancement

**Pourquoi il est parti :**
> "J'ai pas le temps de bidouiller. Chez NordVPN, je clique et ça marche. Tuttle c'est bien pour les militants, pas pour moi."

**Valeur pour Tuttle :** Comprendre le churn, prioriser la fiabilité et l'UX avant les features avancées.

---

**Persona #16 — Kevin, Le Troll**

| Attribut | Détail |
|----------|--------|
| **Âge** | 22 ans |
| **Profil** | Étudiant, très en ligne, provocateur |
| **Usage** | Tuttle pour harceler des "cibles" en ligne sans être tracé |

**Comportement problématique :**
- Utilise l'anonymat pour le harcèlement ciblé
- Se plaint bruyamment sur les forums si limité
- Menace de bad buzz : "Je vais dire partout que Tuttle censure"

**Risques pour Tuttle :**
- Association du nom Tuttle à des activités toxiques
- Questions légales si victime porte plainte
- Argument pour les détracteurs ("Tuttle protège les harceleurs")

**Valeur pour Tuttle :** Définir clairement les CGU, la politique de modération, et les limites du service. Réponse standard : "Tuttle protège la vie privée, pas les délits."

---

**Persona #17 — Cabinet Durand (B2B Bienveillant)**

| Attribut | Détail |
|----------|--------|
| **Nom** | Cabinet Durand & Associés |
| **Type** | Cabinet d'avocats, 15 personnes |
| **Localisation** | Bordeaux |
| **Spécialité** | Droit de la famille, affaires pénales |

**Contexte :**
Le cabinet traite des dossiers sensibles : divorces conflictuels, gardes d'enfants, défense pénale. La confidentialité client-avocat est sacrée. Me Durand a lu un article sur les failles des VPN commerciaux et cherche une solution de confiance.

**Besoins :**
- Protéger les communications avec les clients
- Garantir que les adversaires (ex-conjoints, parties civiles) ne peuvent pas intercepter
- Conformité Ordre des Avocats
- Facturation propre (pas de "Crypto VPN Ltd" sur les notes de frais)

**Budget :** Prêt à payer un premium pour une solution professionnelle (500-1000€/an)

**Valeur pour Tuttle :** Ouvrir le segment B2B légitime, références "sérieuses", revenus stables.

---

#### Résolution des contradictions

**Contradiction #1 : Communauté vs Anonymat**

| Mode | Pour qui | Caractéristiques |
|------|----------|------------------|
| **Mode Communauté** | Christophe, Marie-Bénédicte, Jean-Marc | Îlot, Référent attitré, entraide locale, dashboard social |
| **Mode Fantôme** | Robert, Sophie | Zéro îlot, zéro Référent, aucune donnée sociale, anonymat total |

> L'utilisateur choisit son mode à l'inscription. Migration possible dans un sens (Communauté → Fantôme) mais pas l'inverse (pour éviter l'infiltration).

---

**Contradiction #2 : Mainstream vs Militant**

| Élément | Approche unifiée |
|---------|------------------|
| **Site** | Un seul site neutre-positif (tuttle.net) |
| **Homepage** | Message sobre : "Reprenez le contrôle de votre vie numérique" |
| **Manifeste** | Accessible via /manifeste (lien footer, pas en avant) |
| **Campagne Militants** | Pub ciblée Telegram/X/forums → landing /manifeste |
| **Campagne Famille/Pro** | Pub ciblée Facebook/Google → landing /famille ou /pro |

> UN produit, UN site, DEUX campagnes pub. Le message s'adapte au canal d'acquisition, pas au produit.

---

**Contradiction #3 : Open Source vs Business**

| Composant | Licence | Raison |
|-----------|---------|--------|
| **Core crypto** | Open source (MIT/Apache) | Confiance, auditabilité, résistance acquisition |
| **Protocoles VPN** | Open source | Standards, interopérabilité |
| **Apps clients** | Open source | Confiance utilisateur |
| **Infrastructure** | Propriétaire | Valeur ajoutée opérationnelle |
| **Communauté/Îlots** | Propriétaire | Différenciateur non-copiable |
| **Support** | Propriétaire | Service, pas code |

> Modèle "Open Core" : le code est libre, la valeur est dans l'écosystème. Un fork peut exister, il n'aura pas la communauté ni l'infrastructure.

---

### Security Audit & Recommandations

*Issu de l'audit par trois personas : Viktor (Hacker), Diane (Defender), Hassan (Auditor)*

#### Vulnérabilités critiques identifiées

| Domaine | Vulnérabilité | Sévérité | Remediation |
|---------|---------------|----------|-------------|
| **Zero-Knowledge** | Claim non audité par tiers indépendant | 🔴 Critique | Audit Cure53/Trail of Bits pré-lancement |
| **Zero-Knowledge** | Metadata leakage possible (timestamps, fréquence, taille paquets) | 🟠 Haute | Documenter exactement ce qui est collecté |
| **Hardware** | Supply chain non formalisée | 🟠 Haute | Process ISO 28000 ou équivalent |
| **Hardware** | Pas de certification Secure Element | 🟡 Moyenne | EAL4+ minimum requis |
| **Infra VPN** | RAM-only = claim non prouvé | 🟠 Haute | Preuve technique publique |
| **Infra VPN** | Correlation attacks possibles | 🟡 Moyenne | Documenter honnêtement dans Threat Model |
| **Îlots** | Référent peut être compromis/pressé | 🟠 Haute | Architecture de fragmentation des données |
| **Îlots** | Statut légal Référent flou | 🟡 Moyenne | Formalisation juridique CGU |

#### Architecture de défense par domaine

**Zero-Knowledge**
```
Timing attacks      → Réponses à temps constant (constant-time comparison)
Metadata leakage    → Padding paquets, connexions persistantes, noise injection
Build compromise    → Reproducible builds, multi-party signing, attestation publique
Claim verification  → Audit indépendant + preuve cryptographique formelle
```

**Hardware (Tuttle-Key)**
```
Supply chain        → Fabricant certifié, firmware signé air-gapped, transport scellé
Fault injection     → Secure Element certifié CC EAL5+ (type ATECC608B)
Firmware attacks    → Anti-rollback enforced, signature vérifiée au boot
Physical extraction → Anti-tamper, effacement si intrusion détectée
Attestation         → Device prouve son authenticité via /verify
```

**Infrastructure VPN**
```
Node compromise     → RAM-only servers, reboot = purge totale
Rogue node          → Certificate pinning, attestation par node
BGP hijacking       → Multi-provider, monitoring routes, alertes
Correlation attacks → Traffic shaping, dummy traffic, timing jitter (partiel)
Legal seizure       → Géo-distribution, aucun node n'a toutes les données
```

**Système Îlots**
```
Référent malveillant → Pseudonymat, Référent ne voit pas les vraies identités
Social graph leak    → Données fragmentées (Référent ≠ Tuttle Corp ≠ paiement)
Référent compromis   → 2FA obligatoire, révocation sans cascade, migration membres
Pression légale      → Marc ne peut donner que des pseudos, pas de lien identité
```

#### Actions prioritaires pré-lancement (P0)

| Action | Description | Owner |
|--------|-------------|-------|
| **Audit sécurité indépendant** | Engager Cure53 ou Trail of Bits | Security |
| **Documentation metadata** | Lister précisément ce qui est collecté vs ce qui ne l'est pas | Legal + Tech |
| **Preuve RAM-only** | Publication technique démontrant l'absence de persistance | Infra |
| **Supply chain hardware** | Formaliser le process de fabrication et transport | Hardware |
| **Warrant Canary technique** | Heartbeat cryptographique prouvant intégrité infra | Security |

#### Actions post-lancement (P1)

| Action | Description | Owner |
|--------|-------------|-------|
| Certification Secure Element | Obtenir EAL4+ pour Tuttle-Key | Hardware |
| Bug bounty public | Programme pour chercheurs, récompenses crypto | Security |
| Statut Référents | Formaliser CGU et DPA (Data Processing Agreement) | Legal |
| Audit infra annuel | Audit récurrent par tiers indépendant | Security |
| Version "Pro" certifiée | FIPS 140-2 Level 3 pour clients institutionnels | Hardware |

#### Threat Model honnête (à publier)

**Ce que Tuttle protège :**
- ✅ Surveillance de masse (FAI, GAFAM, publicité ciblée)
- ✅ Interception opportuniste (wifi public, hackers amateurs)
- ✅ Identification par IP pour les services en ligne
- ✅ Contenu inapproprié pour les familles (Clean Pipe)
- ✅ Profilage comportemental par les plateformes

**Ce que Tuttle NE protège PAS contre :**
- ❌ Adversaire étatique avec capacités de surveillance globale (NSA, Five Eyes avec mandat)
- ❌ Correlation attacks si l'adversaire observe entrée ET sortie du VPN simultanément
- ❌ Compromission de votre device personnel (malware, keylogger)
- ❌ Vous-même (si vous vous identifiez volontairement sur un service)
- ❌ Analyse forensique si votre Tuttle-Key est saisie ET vous êtes forcé de donner le PIN

**Principe de transparence :**
> Tuttle ne prétend pas être invincible. Nous documentons honnêtement nos limites. Un utilisateur informé fait de meilleurs choix de sécurité qu'un utilisateur qui croit aveuglément des promesses marketing.

---

### Décisions Stratégiques (Round Table)

*Issus de la confrontation de tous les stakeholders*

#### Architecture produit : VPN vs Network

| Produit | Description | Prérequis | Cible |
|---------|-------------|-----------|-------|
| **Tuttle VPN** | Service VPN classique + Clean Pipe | Aucun | Tous (Jean-Marc, Christophe, Sophie) |
| **Tuttle Network** | Intranet souverain (LightWeb), mesh, services | Tuttle VPN actif | Avancés (Robert, Marc, communautés) |

**Règle d'accès :**
```
Tuttle VPN    → Accessible seul (produit de base)
Tuttle Network → Requiert Tuttle VPN actif (produit avancé)

Client peut avoir : VPN seul ✅
Client peut avoir : Network seul ❌ (impossible)
Client peut avoir : VPN + Network ✅
```

**Logique commerciale :**
- **VPN** = Cash cow, acquisition de masse, revenus récurrents
- **Network** = Upsell, différenciateur, lock-in communautaire
- Le VPN finance le développement du Network
- Le Network justifie le premium long terme

#### Décisions validées par consensus

| Décision | Soutien | Implémentation | Phase |
|----------|---------|----------------|-------|
| **Un produit, deux campagnes** | Unanime | Site unique neutre + campagnes pub ciblées | V1.0 |
| **VPN + Clean Pipe = MVP** | Unanime | Lancer simple, itérer sur feedback | V1.0 |
| **Distribution web d'abord** | Validé | Pub digitale, pas de Référents V1 | V1.0 |
| **Deux niveaux UX** | Unanime | Dashboard Simple vs Dashboard Expert | V1.0 |
| **Référents liés au Network** | Clarifié | Référents = distribution Network, pas VPN | V2.0 |
| **Hardware en deux temps** | Validé | Tuttle-Key V1.5, Tuttle-Box V2.0 | V1.5/V2.0 |
| **Structure juridique résistante** | Fort | Fondation, SCIC, ou SAS avec pacte blindé | V1.0 |
| **Recovery erreur humaine** | Critique | Procédure de récupération Seed documentée | V1.5 |
| **Offre B2B distincte** | Validé | Support pro, SLA, facturation entreprise | V2.0 |

#### Stratégie de marque unifiée

**UN produit : Tuttle**
```
Site unique : tuttle.net (ou .io, .one)
├── Message principal : "Reprenez le contrôle de votre vie numérique"
├── Ton : Sobre, professionnel, pas militant en homepage
├── Features : Clean Pipe, simplicité, communauté locale
└── Manifeste : Accessible en footer (pas en homepage)
```

**DEUX campagnes pub séparées :**

| Campagne | Cible | Canaux | Message | Landing |
|----------|-------|--------|---------|---------|
| **"Protégez votre famille"** | Jean-Marc, Marie-Bénédicte, B2B | Facebook, Google Ads | "Internet propre pour vos enfants" | /famille |
| **"Devenez invisible"** | Christophe, Robert, Sophie | Telegram, forums, X | "Souveraineté numérique" | /manifeste |

**Parcours personnalisé APRÈS inscription :**
```
Inscription → Questionnaire : "Qu'est-ce qui vous amène ?"
  □ Protéger ma famille
  □ Sécuriser mon activité pro
  □ Reprendre le contrôle de mes données
  □ Autre
→ Dashboard et emails adaptés selon réponse
→ Même produit, expérience personnalisée
```

#### Matrice des modes et campagnes

| | **Campagne "Résistance"** | **Campagne "Famille/Pro"** |
|---|---|---|
| **Mode Communauté** | Christophe, Marie-Bénédicte | Jean-Marc famille |
| **Mode Fantôme** | Robert, Sophie | Cabinet Durand (B2B) |

#### Avantages de l'approche unifiée

| Aspect | Bénéfice |
|--------|----------|
| **Dev** | Un seul produit à maintenir |
| **Support** | Une seule documentation |
| **Marque** | Identité claire, pas de confusion |
| **SEO** | Un domaine fort vs deux domaines faibles |
| **Communauté** | Unifiée, pas fragmentée |
| **Référents** | Peuvent servir tous les profils |

#### Tensions identifiées et résolutions

| Tension | Résolution |
|---------|------------|
| Militant vs Neutre | Un produit unifié, deux campagnes pub ciblées |
| Communauté vs Anonymat | Mode Fantôme disponible dès l'inscription |
| Sécurité vs Usabilité | Modes Simple/Expert + pédagogie intégrée |
| Croissance vs Intégrité | Structure juridique + open source core |
| VPN vs Network | Produits distincts avec prérequis clair |

#### Insights stratégiques des observateurs

| Source | Observation | Action Tuttle |
|--------|-------------|---------------|
| **NEXUS Capital** | "Communauté = seul moat défendable" | Investir massivement dans la communauté |
| **Mullvad** | "Problème d'identité assumé = force" | Ne pas chercher un compromis, assumer la dualité |
| **EnikmaVPN** | "Rester petit = mort lente" | Croissance maîtrisée avec jalons qualité |
| **David (Déçu)** | "Fiabilité > Features" | Qualité de base avant innovations |

---

### Pre-mortem : Risques existentiels

*Analyse rétroactive fictive — "Tuttle a échoué en 2028, pourquoi ?"*

#### Les 6 façons dont Tuttle peut mourir

| Scénario | Description | Probabilité | Impact |
|----------|-------------|-------------|--------|
| **A. Déplatforming** | Article hostile → Stripe/Cloudflare/OVH terminent → Infra s'effondre | 🟠 Moyenne | 🔴 Fatal |
| **B. Acquisition hostile** | NEXUS achète des parts, paralyse les décisions, récupère les assets | 🟡 Faible | 🔴 Fatal |
| **C. Churn massif** | Netflix/Disney bloqués, support débordé, Référents abandonnent | 🟠 Moyenne | 🔴 Fatal |
| **D. Scandale sécurité** | Faille exploitée, dump de base, perte de confiance irréversible | 🟡 Faible | 🔴 Fatal |
| **E. Épuisement fondateur** | Quentin fait tout, burnout, pas de succession, fermeture | 🔴 Haute | 🔴 Fatal |
| **F. Concurrence Enikma** | EnikmaVPN V2 copie tout, prix inférieur, légitimité historique | 🟠 Moyenne | 🟠 Grave |

#### Matrice de survie — Actions AVANT lancement

| Action | Prévient | Owner |
|--------|----------|-------|
| **Multi-provider infra** (pas de SPOF) | A. Déplatforming | Infra |
| **Paiements crypto opérationnels** (≥30% du mix) | A. Déplatforming | Finance |
| **Structure juridique blindée** (SCIC/fondation) | B. Acquisition | Legal |
| **Pacte d'actionnaires anti-cession** | B. Acquisition | Legal |
| **Audit sécurité indépendant** (Cure53) | D. Scandale | Security |
| **Beta test compatibilité** (Netflix, Disney, Teams) | C. Churn | QA |
| **Équipe core** (pas solo) | E. Épuisement | Founders |
| **War room documentée** | A, D. Crises | Ops |
| **Canaux alternatifs clients** (Telegram, Signal) | A. Déplatforming | Community |

#### Matrice de survie — 6 premiers mois

| Action | Prévient | Owner |
|--------|----------|-------|
| SLA support < 24h | C. Churn | Support |
| Bug bounty privé puis public | D. Scandale | Security |
| Approcher EnikmaVPN (partenariat/absorption) | F. Concurrence | Business |
| Features uniques livrées (LightWeb, Îlots) | F. Concurrence | Product |
| Documentation tous processus | E. Épuisement | Tous |
| Plan de succession publié | E. Épuisement | Governance |

#### ⚠️ Alerte risque #1 : Épuisement fondateur

> **Dans 4 scénarios sur 6, la santé ou la disponibilité du fondateur est un facteur aggravant.**
>
> Le risque le plus probable n'est pas technique ou business — c'est le burnout.
>
> **Action immédiate requise :**
> - Ne pas lancer seul
> - Trouver co-fondateur technique OU COO AVANT lancement public
> - Limites personnelles : max 50h/semaine
> - Référents seniors formés comme backup opérationnel

#### Scénarios détaillés et prévention

**Scénario A — Déplatforming**
```
Trigger    : Article Verdier "Le VPN de la haine"
Cascade    : Stripe suspend → Cloudflare termine → OVH suit → Latence x3
Issue      : Revenus s'effondrent, clients partent
Prévention : Infra multi-provider prête, crypto ≥30%, war room testée
```

**Scénario B — Acquisition hostile**
```
Trigger    : Refus offre NEXUS
Cascade    : FUD coordonné → Talent drain → Minoritaire vend → Blocage 35%
Issue      : Paralysie décisionnelle, équipe démissionne
Prévention : Structure juridique, pacte actionnaires, equity employés
```

**Scénario C — Churn massif**
```
Trigger    : Netflix/Disney bloqués
Cascade    : Plaintes → Support débordé (5j réponse) → Churn 25%/mois → Référents abandonnent
Issue      : 80 clients restants, fin de partie
Prévention : Test compatibilité, SLA support, formation Référents, promesses réalistes
```

**Scénario D — Scandale sécurité**
```
Trigger    : Kermit trouve SQLi
Cascade    : Dump 3000 emails + 500 adresses → Publication Twitter → Presse reprend
Issue      : Perte de confiance irréversible
Prévention : Audit indépendant, bug bounty, architecture Zero-Knowledge auditée, com de crise prête
```

**Scénario E — Épuisement fondateur**
```
Trigger    : Quentin fait tout seul
Cascade    : Burnout #1 → Pause → Burnout #2 → Hospitalisation → Retrait
Issue      : Pas de succession, fermeture
Prévention : Équipe core, documentation, délégation, plan de succession, limites personnelles
```

**Scénario F — Concurrence Enikma**
```
Trigger    : EnikmaVPN annonce V2 avec features Tuttle
Cascade    : Prix -30% + légitimité E&R → Clients hésitent → Tuttle coincé entre deux chaises
Issue      : Ni dissident (Enikma) ni mainstream (Proton)
Prévention : Features uniques non-copiables, vitesse exécution, approche Enikma, communauté fidélisée
```

---

### Stratégie de financement (Shark Tank)

*Issu du stress-test face à un panel d'investisseurs fictifs*

#### Verdict des investisseurs types

| Profil investisseur | Décision | Raison |
|---------------------|----------|--------|
| **VC early-stage** | ❌ OUT | Marché niche, pas de potentiel 10x, profil PME |
| **Ex-corporate tech** | ❌ OUT | Unit economics fragiles, churn incertain |
| **Impact investing** | ✅ INTÉRESSÉ | Mission alignée, gouvernance éthique |
| **Angel personnel** | ⚠️ CONDITIONNEL | Oui si CTO/COO trouvé |
| **Corporate VC** | ❌ OUT | Risque réputationnel trop élevé |

#### Points de friction identifiés

| Friction | Gravité | Solution requise |
|----------|---------|------------------|
| **Solo founder** | 🔴 Critique | Recruter CTO/COO AVANT ou PENDANT levée |
| **Unit economics non validés** | 🟠 Haute | Beta payante avant scale |
| **Marché niche** | 🟠 Haute | Assumer positionnement PME à impact |
| **Risque réputationnel** | 🟡 Moyenne | Plan com de crise + alliés médias |
| **Scalabilité Référents** | 🟡 Moyenne | Canaux complémentaires (web, retail) |

#### Stratégie de financement recommandée

| Source | Montant | Dilution | Fit |
|--------|---------|----------|-----|
| **Impact investing** | 200-400K€ | 15-25% | ✅ Aligné mission |
| **Crowdfunding communautaire** | 100-300K€ | Variable | ✅ Engage la communauté |
| **Préventes hardware** | 50-100K€ | 0% | ✅ Valide le marché |
| **Angels alignés** | 50-150K€ | 5-10% | ✅ Contrôle préservé |
| ~~VC classique~~ | ~~500K€+~~ | ~~15-25%~~ | ❌ Inadapté (pression exit) |

**Mix recommandé :**
```
Impact investing     250K€  (20%)
Crowdfunding         150K€  (récompenses, pas equity)
Préventes hardware   100K€  (0%)
Angels alignés        50K€  (5%)
─────────────────────────────
Total                550K€  (25% dilution)
```

#### Conditions non-négociables

1. **Mission inscrite dans les statuts** — Clause de mission sociale empêchant pivot vers modèle extractif
2. **Conseil des Référents** — Droit de véto sur acquisition ou changement stratégique majeur
3. **Structure résistante** — SCIC, fondation actionnaire, ou pacte blindé anti-cession
4. **Pas de pression exit** — Investisseurs acceptent horizon long (7-10 ans) ou pas d'exit

#### Checklist pré-levée

| Question | Statut | Bloquant |
|----------|--------|----------|
| CTO/COO identifié ? | ❌ À faire | 🔴 Oui |
| Beta payante validant unit economics ? | ❌ À faire | 🔴 Oui |
| Structure juridique finalisée ? | ❌ À faire | 🔴 Oui |
| Plan com de crise documenté ? | ❌ À faire | 🟠 Recommandé |
| Hardware en production (pas prototype) ? | ❌ À faire | 🟠 Recommandé |
| 15+ Référents formés et engagés ? | ✅ Fait | - |
| 200+ pré-inscriptions ? | ✅ Fait | - |

#### Unit economics à valider

| Métrique | Hypothèse optimiste | Hypothèse prudente | À valider |
|----------|---------------------|-------------------|-----------|
| Coût hardware | 35€ | 50€ | Beta production |
| Prix vente hardware | 79€ | 79€ | - |
| Marge hardware | 56% | 37% | ⚠️ |
| Abonnement mensuel | 12€ | 12€ | - |
| Coût infra/user | 2€ | 3€ | Beta scale |
| Churn mensuel | 5% | 10% | ⚠️ Critique |
| LTV (durée moyenne) | 20 mois (200€) | 10 mois (100€) | ⚠️ |
| CAC Référents | 15€ | 25€ | Beta |
| CAC Paid | 40€ | 60€ | Post-launch |
| LTV/CAC ratio | 5-13x | 1.7-4x | ⚠️ |

**Conclusion unit economics :**
> En hypothèse prudente, le modèle reste viable (LTV/CAC > 3x via Référents) mais avec marges réduites. La validation par beta payante est CRITIQUE avant tout investissement significatif.

---

### Gaps Support & Actions

*Issu du Customer Support Theater — 5 scénarios clients furieux*

#### Matrice des frustrations par persona

| Persona | Frustration | Gap révélé | Priorité |
|---------|-------------|------------|----------|
| **David (Déçu)** | Netflix bloqué, 48h sans réponse | App pas auto-update, SLA absent | 🔴 P0 |
| **Marie-Bénédicte** | Enfants sur YouTube malgré Clean Pipe | Onboarding incomplet, profils absents | 🔴 P0 |
| **Cabinet Durand** | Déploiement 12h pour 3 postes | Doc B2B insuffisante, facturation non conforme | 🟠 P1 |
| **Robert (Geek)** | Fingerprint non documenté | Transparence technique incomplète | 🟠 P1 |
| **Kevin (Troll)** | Banni "sans preuve" | Process modération pas public | 🟡 P2 |

#### Actions support prioritaires

**P0 — Avant lancement**

| Action | Problème résolu |
|--------|-----------------|
| **SLA formalisé** : < 24h (B2C), < 4h (B2B) | David attend 48h |
| **Auto-update app** avec notification push | David sur vieille version |
| **FAQ complète** : Streaming, Clean Pipe, Troubleshooting | Tickets évitables |
| **"Mode Enfant" proposé à l'installation** | Marie-Bénédicte croit être protégée |
| **Profils pré-configurés** : Famille stricte, Ado, Adulte | Configuration manuelle complexe |

**P1 — 3 premiers mois**

| Action | Problème résolu |
|--------|-----------------|
| **Guides sectoriels B2B** (Avocats, Médecins, PME) | Cabinet Durand perdu |
| **Session onboarding incluse** dans offre Pro | Stagiaire IT livré à lui-même |
| **Facturation conforme** dès souscription (TVA intracom) | Blocage comptable |
| **File support dédiée B2B** + Account Manager | Pro dans même queue que B2C |
| **Page "Ce que nous collectons"** exhaustive | Robert découvre le fingerprint |
| **Mode "Paranoïa"** sans fingerprint optionnel | Power users frustrés |

**P2 — Consolidation**

| Action | Problème résolu |
|--------|-----------------|
| **Process modération public** | Kevin accuse de censure arbitraire |
| **Procédure d'appel** en 2 étapes | Escalade systématique |
| **Script anti-troll** standardisé | Équipe déstabilisée par bad buzz |
| **Chatbot FAQ** pour questions simples | Charge support réduite |

#### Formation Référents obligatoire

| Module | Contenu | Durée |
|--------|---------|-------|
| **Clean Pipe** | Niveaux 1 & 2, Mode Enfant, configuration | 30 min |
| **Troubleshooting basique** | Netflix/Streaming, mise à jour app | 30 min |
| **Escalade** | Quand transférer au support central | 15 min |
| **Gestion frustrations** | Écoute, empathie, ne pas promettre l'impossible | 15 min |

#### KPIs support cibles

| KPI | Cible lancement | Cible 12 mois |
|-----|-----------------|---------------|
| Temps première réponse (B2C) | < 48h | < 24h |
| Temps première réponse (B2B) | < 12h | < 4h |
| Résolution premier contact | > 50% | > 70% |
| Tickets évités (FAQ/chatbot) | 20% | 40% |
| NPS support | > 30 | > 50 |

#### Verbatims à ne jamais oublier

> **David :** "Vous m'avez vendu du Plug & Play, c'est du Debug & Pray."

> **Marie-Bénédicte :** "Si c'est compliqué, je vais rater des choses. Vous pouvez faire un mode 'Famille Catho' où tout est bloqué par défaut ?"

> **Cabinet Durand :** "Votre documentation suppose des connaissances que nous n'avons pas."

> **Robert :** "Sur votre page Zero-Knowledge, y'a pas un mot sur ce fingerprint. C'est ça la vraie transparence."

---

### Vision Long Terme (Time Traveler)

*Conseil du Tuttle 2024 (passé), 2026 (présent) et 2030 (futur)*

#### Sagesses clés

**Du passé (2024) :**
| Conseil | Implication |
|---------|-------------|
| "Lance, n'attends pas la perfection" | MVP imparfait > produit parfait jamais lancé |
| "Le co-fondateur parfait n'existe pas" | Embauche quelqu'un d'aligné maintenant, pas demain |
| "Le Manifeste filtre, c'est une feature" | Ne pas édulcorer pour plaire — la polarisation est une force |

**Du futur (2030) :**
| Conseil | Implication |
|---------|-------------|
| "Garde une réserve cash 6 mois" | Les crises imprévues arrivent toujours |
| "Concentre-toi sur le churn, pas la croissance" | 2000 fidèles > 5000 qui churnent |
| "Les crises se survivent, pas s'évitent" | Prépare la résilience, pas l'évitement |
| "Intégrité > Croissance, toujours" | Un compromis sur les valeurs = mort lente |

#### Timeline probable des crises

| Période | Crise anticipée | Préparation requise |
|---------|-----------------|---------------------|
| **Mois 1-3** | Streaming bloqué (Netflix, Disney) | Infra streaming dédiée, serveurs rotatifs |
| **Mois 4-6** | Burnout fondateur | COO embauché AVANT, limites 50h/semaine |
| **Mois 9-12** | Article hostile mainstream | War room prête, crypto ≥35%, alliés médias activés |
| **Année 2** | Tentative acquisition / débauchage | Structure juridique blindée, equity employés |
| **Année 3+** | Stabilisation | Patience, qualité, croissance organique |

#### Les 3 lignes rouges — JAMAIS franchir

| Ligne rouge | Conséquence si franchie |
|-------------|-------------------------|
| ❌ **Compromettre Zero-Knowledge** | Une fuite de données = réputation morte, projet mort |
| ❌ **Dépendance à un provider unique** | Déplatforming = mort instantanée (Stripe, Cloudflare, etc.) |
| ❌ **Sacrifier les Référents pour "scaler"** | Perte de l'âme, devient un VPN comme les autres |

#### Destination 2030 (vision confirmée)

```
Abonnés actifs     : 12 000
Churn mensuel      : 3%
Série A levée      : Non (pas nécessaire)
Tuttle Network     : Lancé (LightWeb opérationnel)
Communauté         : Solide, Référents = piliers
Crises survécues   : Netflix, burnout, Verdier, NEXUS
```

#### Le pourquoi final

> *"En 2030, Christophe — le père de famille du Product Brief — est devenu Référent. Son fils aîné a 18 ans maintenant. Il m'a dit : 'Grâce à Tuttle, mes enfants ont grandi dans un internet propre. Merci.'"*
>
> C'est pour ça qu'on fait tout ça. Pas pour les métriques. Pour les Christophe.

---

### Clarifications Fondamentales (Feynman)

*Issus du Feynman Technique — expliquer Tuttle à un enfant de 10 ans*

#### Preuve de confiance (bootstrap, sans budget audit)

| Phase | Mécanismes de confiance |
|-------|-------------------------|
| **Phase 1 (lancement)** | Code open source sur GitHub/GitLab (vérifiable par la communauté) |
| | Bug bounty communautaire (récompenses symboliques + crédits) |
| | Documentation technique transparente |
| | Warrant canary public |
| **Phase 2 (avec revenus)** | Audit indépendant quand trésorerie > 50k€ |

#### Gouvernance évolutive

| Phase | Structure |
|-------|-----------|
| **Phase 1** | Fondateur solo (décisions rapides, responsabilité claire) |
| **Phase 2 (avec revenus)** | Création structure légale (association ou fondation) |
| | Intégration progressive des Référents dans les décisions |
| | Board quand l'écosystème le justifie |

#### Architecture multi-tunnel (comme Proton)

**Options de connexion :**
```
├── Tuttle VPN direct      (rapide, usage quotidien)
├── Tuttle via Tor         (anonymat maximum, plus lent)
└── Tor seul               (fallback si serveurs Tuttle down)
```

**Avantages :**
- **Tor = gratuit**, réseau existant, résilience externe
- **VPN+Tor = double protection** (Proton le fait déjà, validé marché)
- **Choix utilisateur** selon besoin du moment (vitesse vs anonymat)
- **Réduit dépendance** infrastructure propre au démarrage

**Différenciation vs Proton qui fait pareil :**

| | Proton | Tuttle |
|---|--------|--------|
| VPN+Tor | ✅ | ✅ |
| Corporate / cher | ✅ | ❌ |
| Communauté Référents | ❌ | ✅ |
| Network exclusif (LightWeb) | ❌ | ✅ |
| Prix accessible | ❌ | ✅ |
| Valeurs alignées dissidents | ⚠️ (incidents logs) | ✅ |

#### Stratégie Open Source (par couche)

**Principe :** Le code qui touche aux DONNÉES UTILISATEUR est open source. Le code qui protège l'INFRASTRUCTURE reste privé.

| Couche | Statut | Raison |
|--------|--------|--------|
| **Crypto & Auth** | 🟢 Open Source | Robert DOIT vérifier qu'il n'y a pas de backdoor |
| **App client** | 🟢 Open Source | Confiance utilisateur, pas de secrets dedans |
| **Protocoles réseau** | 🟢 Open Source | Standards, interopérabilité |
| **Format clés/seed** | 🟢 Open Source | Portabilité, auditabilité |
| **Config infra** | 🔴 Privé | Surface d'attaque réelle |
| **Règles WAF/firewall** | 🔴 Privé | Expose les défenses |
| **Logique anti-fingerprint** | 🔴 Privé | Expose les contournements |
| **Infrastructure routing** | 🔴 Privé | Cartographie réseau sensible |

**Message public (pour Robert et les paranos) :**
> "Le code qui touche à TES données est open source et auditable. Le code qui protège NOTRE infra reste privé — exactement comme tu ferais toi-même."

**Référence marché :** C'est le modèle de Mullvad et Signal — confiance sur la crypto, protection sur l'opérationnel.

---

### Chemin Critique (Reverse Engineering)

*Issu du Reverse Engineering — partir de 2028 pour tracer le chemin*

#### Actions bloquantes AVANT lancement

| Action | Persona bloqué si absent | Priorité |
|--------|--------------------------|----------|
| **Kit démo Référent** (matériel + script + FAQ) | Christophe (pas de découverte) | 🔴 P0 |
| **Code open source (crypto + app client)** | Robert (pas de confiance) | 🔴 P0 |
| **Offre Pro avec facturation conforme** | Cabinet Durand (pas de signature) | 🔴 P0 |
| **Clean Pipe fonctionnel jour 1** | Christophe, Marie-Bénédicte | 🔴 P0 |
| **Paiement crypto opérationnel** | Robert, Sophie | 🔴 P0 |
| **App avec auto-update** | David (churn évité) | 🔴 P0 |

#### Timeline inversée

```
2028: 10 000 abonnés, 150 Référents, Network lancé
  ↑
2027: 5 000 abonnés, survécu article Verdier, app V2 stable
  ↑
2026: 1 500 abonnés, 30 Référents, product-market fit
  ↑
NOW:  Beta payante 200 users, 10 Référents fondateurs, V1 app
```

#### Chemin critique par segment

```
              NOW
               │
  ┌────────────┼────────────┐
  │            │            │
  ▼            ▼            ▼
Kit         Code Open    Offre Pro
Référent    (crypto+app) (facturation)
  │            │            │
  ▼            ▼            ▼
Marc        Robert       Durand
équipé      teste        signe
  │            │            │
  ▼            ▼            ▼
Christophe  Robert       Durand
rejoint     recommande   témoigne
  │            │            │
  └────────────┼────────────┘
               │
               ▼
        TUTTLE 2028
```

**Insight :** Trois chemins parallèles indépendants. Si UN est bloqué, on perd un segment entier de marché.

---

### Inspirations Croisées (Genre Mashup)

*Issu du Genre Mashup — croiser Tuttle avec des domaines inattendus*

#### Quatre modèles d'inspiration

| Domaine | Concept clé | Application Tuttle |
|---------|-------------|-------------------|
| **Résistance Française** | Cellules cloisonnées, passeurs | Îlots isolés, Référents = passeurs, protocole si compromis |
| **Alcooliques Anonymes** | Parrainage, 12 étapes, meetings | Parcours progressif, réunions locales, témoignages anonymes |
| **Speakeasy (Prohibition)** | Exclusivité, mot de passe, bouche à oreille | Parrainage > pub, croissance organique, "members only" vibe |
| **Ordre Monastique** | Règle de vie, silence, ancrage matériel | Charte Tuttle, Zero-Knowledge = silence, Tuttle-Key = objet sacré |

#### Applications retenues

**Pour la résilience (Résistance) :**
- Îlots ne connaissent pas les autres îlots
- Protocole de compromission : si un îlot tombe, les autres ne sont pas exposés
- Référents remplaçables sans perte de réseau

**Pour l'engagement (AA) :**
- Parcours "étapes vers la souveraineté" (gamification)
- Réunions Tuttle locales animées par Référents
- Section témoignages anonymes sur le site

**Pour la croissance (Speakeasy) :**
- Parrainage valorisé > publicité massive
- Sentiment d'exclusivité ("Tu fais partie du club")
- Pas de promos criardes style NordVPN

**Pour la fidélité (Monastère) :**
- Charte Tuttle (document fondateur, valeurs)
- Clean Pipe = "jeûne numérique" (vocabulaire spirituel pour certains profils)
- Récompenses fidélité > promos nouveaux clients

---

### Vérités Fondamentales (First Principles)

*Issu du First Principles Analysis — reconstruire depuis zéro*

#### Les 5 vérités de Tuttle

| # | Vérité | Conséquence |
|---|--------|-------------|
| **V1** | Les gens veulent éviter des **problèmes concrets**, pas de la "privacy" abstraite | Vendre les conséquences évitées, pas le moyen technique |
| **V2** | Un VPN seul ne suffit pas (cookies, fingerprint, comportement) | Vendre l'écosystème complet, être honnête sur les limites |
| **V3** | La confiance se transmet entre humains, pas de marque à individu | Référents + bouche à oreille > publicité |
| **V4** | Les communautés structurées sont le terrain fertile | Cathos = premier segment d'un pattern reproductible |
| **V5** | Chaque segment a un "job to be done" différent | Personnaliser le message, pas le produit |

#### Ce que Tuttle est VRAIMENT

```
Tuttle n'est pas un VPN.

Tuttle est un système de protection numérique familiale
distribué par des humains de confiance,
qui utilise un VPN comme composant technique.

Le VPN est le moteur.
La communauté est le châssis.
Le Clean Pipe est l'habitacle.
La Tuttle-Key est la clé de contact.
```

#### Job to be done par segment

| Segment | Job to be done | Message |
|---------|----------------|---------|
| **Parents protecteurs** (Christophe, M-B) | "Que mes enfants ne voient jamais de contenu inapproprié" | "Internet propre pour votre famille" |
| **Dissidents discrets** (Sophie) | "Que personne ne sache ce que je lis vraiment" | "Votre vie privée reste privée" |
| **Paranos techniques** (Robert) | "Que le système soit mathématiquement sûr" | "Auditez le code vous-même" |
| **Pros sensibles** (Cabinet Durand) | "Que nos échanges clients restent confidentiels" | "Confidentialité niveau cabinet" |
| **Familles mainstream** (Jean-Marc) | "Que ma famille soit protégée sans y penser" | "Branchez, c'est protégé" |

#### Hypothèses corrigées

| Hypothèse initiale | Vérité découverte | Ajustement |
|--------------------|-------------------|------------|
| "Les gens veulent la privacy" | Ils veulent éviter des problèmes concrets | Messaging sur conséquences, pas sur moyens |
| "Le hardware est obligatoire" | Certains préfèrent zéro trace physique | Offre Software-only pour Mode Fantôme |
| "Cathos = LA cible" | Cathos = premier d'un pattern | Roadmap : autres communautés structurées |
| "Clean Pipe pour tous" | Inutile pour Robert, Sophie, B2B | Toggle visible, pas défaut universel |
| "Référents only" | Ne scale pas partout (B2B, international) | Modèle hybride Référents + digital |

#### Pattern "communauté structurée" (au-delà des cathos)

| Communauté | Structure | Valeurs alignées | Potentiel |
|------------|-----------|------------------|-----------|
| Catholiques pratiquants | Paroisses, groupes | Protection famille, méfiance système | ✅ Premier segment |
| Juifs pratiquants | Synagogues, communautés | Discrétion, protection | 🟡 Segment 2 |
| Musulmans conservateurs | Mosquées, associations | Famille, vie privée | 🟡 Segment 3 |
| Homeschoolers | Réseaux, coops | Autonomie, contrôle parental | 🟡 Segment 4 |
| Libertariens/Survivalistes | Forums, groupes locaux | Souveraineté, méfiance État | 🟡 Segment 5 |

---

### Scénarios Alternatifs (What If)

*Issu du What If Scenarios — explorer les réalités alternatives*

#### Les 6 scénarios explorés

| Scénario | Question | Conséquence principale |
|----------|----------|------------------------|
| **A. Acquisition Proton** | Et si Proton rachetait Tuttle ? | Communauté = trahie, churn massif, fork hostile |
| **B. Clean Pipe illégal** | Et si le filtrage devenait réglementé ? | Perte du différenciateur famille |
| **C. Référent criminel** | Et si un Référent était arrêté pour crime grave ? | Crise réputation existentielle |
| **D. Faille WireGuard** | Et si le protocole avait une CVE critique ? | Vulnérabilité systémique |
| **E. Succès viral** | Et si un témoignage faisait 500K vues ? | Infra/support submergés, qualité chute |
| **F. VPN interdit** | Et si la France interdisait les VPN ? | Illégalité, perte marché principal |

#### Actions préventives par scénario

| Scénario | Action préventive | Priorité |
|----------|-------------------|----------|
| **A. Acquisition** | Structure juridique anti-acquisition (SCIC, fondation, pacte) | 🔴 P0 |
| **B. Clean Pipe illégal** | Architecture côté client (listes locales, pas filtrage serveur) | 🟠 P1 |
| **C. Référent criminel** | Playbook crise rédigé AVANT lancement | 🔴 P0 |
| **D. Faille WireGuard** | Support multi-protocole V2 (WireGuard + OpenVPN + V2Ray) | 🟠 P1 |
| **E. Succès viral** | Plan de scaling documenté + waitlist + offre Software-only | 🔴 P0 |
| **F. VPN interdit** | Architecture ban-resistant (obfuscation, .onion, entité offshore) | 🟠 P1 |

#### Focus : Playbook crise "Référent criminel"

**Réponse préparée (à activer en < 2h) :**
```
1. Communiqué immédiat : "Nous avons appris avec horreur..."
2. Radiation immédiate du Référent (prévu dans CGU)
3. Collaboration avec autorités (Tuttle protège la vie privée, pas les crimes)
4. Rappel public : "Notre mission est de protéger les familles"
5. Témoignages solidaires d'autres Référents
6. Pas de silence — transparence totale
```

#### Focus : Plan de scaling viral

**Si +5000 demandes en 48h :**
```
1. Waitlist avec position claire ("Vous êtes #3247, ~2 semaines")
2. Auto-scaling infra (pas de timeout humiliant)
3. FAQ vidéo pour réduire charge support
4. Offre Software-only immédiate (pas de bottleneck hardware)
5. Recrutement Référents accéléré SANS baisser la qualité
6. Communication proactive ("On est submergés, on gère")
```

#### Les 3 scénarios les plus probables à court terme

| # | Scénario | Probabilité | Impact | Statut préparation |
|---|----------|-------------|--------|-------------------|
| 1 | **Succès viral non préparé** | 🟠 Moyenne | 🔴 Critique | ❌ À documenter |
| 2 | **Référent problématique** | 🟠 Moyenne | 🔴 Critique | ❌ À documenter (V2) |
| 3 | **Réglementation hostile** | 🟡 Faible | 🔴 Fatal | ⚠️ Architecture à prévoir |

---

### Architecture en Phases (Occam's Razor)

*Issu de l'Occam's Razor — simplifier impitoyablement*

#### Principe directeur

> "La solution la plus simple qui fonctionne est généralement la meilleure."
> Lancer le MVP minimal, itérer sur feedback réel.

#### Roadmap produit simplifiée

```
┌─────────────────────────────────────────────┐
│  V1.0 — VPN SIMPLE                          │
├─────────────────────────────────────────────┤
│  Produit     : VPN + Clean Pipe (software)  │
│  Distribution: Web classique (pub digitale) │
│  Paiement    : Stripe + Bitcoin             │
│  Protocole   : WireGuard only               │
│  Cible       : Familles (acquisition web)   │
│  Message     : "Internet propre"            │
├─────────────────────────────────────────────┤
│  ❌ PAS de : Hardware, Network, Référents   │
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│  V1.5 — + HARDWARE                          │
├─────────────────────────────────────────────┤
│  + Tuttle-Key (HSM, ancre de confiance)     │
│  VPN reste identique                        │
│  Distribution web + early adopters          │
├─────────────────────────────────────────────┤
│  ❌ PAS encore : Network, Référents         │
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│  V2.0 — NETWORK COMPLET                     │
├─────────────────────────────────────────────┤
│  + Tuttle Network (LightWeb, mesh)          │
│  + Tuttle-Box (hardware Network)            │
│  + Système Référents (distribution îlots)   │
│  Prérequis client : VPN actif               │
└─────────────────────────────────────────────┘
```

#### Triggers de passage entre phases

| Phase | Trigger pour passer à la suite |
|-------|-------------------------------|
| **V1.0 → V1.5** | 500 abonnés stables + demande hardware validée |
| **V1.5 → V2.0** | 1500 abonnés + communauté prête + archi Network validée |

#### Ce que V1 n'est PAS

| Élément | Statut V1 | Raison |
|---------|-----------|--------|
| Référents | ❌ V2 | Liés au Network, pas au VPN de base |
| Tuttle-Key | ❌ V1.5 | Hardware = complexité supply chain |
| Tuttle-Box | ❌ V2 | Lié au Network |
| Tuttle Network | ❌ V2 | Complexité énorme, pas de demande prouvée |
| Multi-protocole | ❌ V1.5+ | WireGuard suffit au début |
| Tor intégré | ❌ V1.5 | Nice-to-have |
| Monero | ❌ V1.5 | Bitcoin suffit pour signal privacy |
| B2B | ❌ V2 | Focus famille d'abord |

#### Ce que V1 EST (le sacré)

| Élément | Pourquoi c'est non-négociable |
|---------|-------------------------------|
| **VPN fonctionnel** | Core product |
| **Clean Pipe** | Différenciateur #1, raison d'achat Christophe |
| **Zero-Knowledge** | Promesse fondamentale |
| **Bitcoin** | Signal privacy crédible |
| **Open source (crypto+app)** | Confiance technique |
| **Manifeste** | Identité, même si pas poussé en V1 |

#### Rôle des personas par phase

| Persona | V1.0 | V1.5 | V2.0 |
|---------|------|------|------|
| **Christophe** | Client web direct | + Tuttle-Key | Référent potentiel |
| **Jean-Marc** | Client web direct | Client | Client VPN (pas Network) |
| **Marc** | ❌ Pas de rôle | Early adopter Key | Référent Network |
| **Marie-Bénédicte** | Cliente web | Cliente | Référente potentielle |
| **Robert** | Validateur technique | Teste la Key | Validateur Network |
| **Sophie** | Cliente web | Cliente | Cliente (Mode Fantôme) |
| **Cabinet Durand** | ❌ Pas de rôle | ❌ | Client B2B |

#### Business model par phase

| Phase | Acquisition | Revenus | Complexité |
|-------|-------------|---------|------------|
| **V1.0** | Pub digitale (Facebook/Google) | Abonnement VPN | Faible |
| **V1.5** | Idem + communauté early | Abo + vente hardware | Moyenne |
| **V2.0** | Référents + web | Abo VPN + Abo Network + hardware | Élevée |

#### Comparaison complexité

| Aspect | Plan initial | MVP Occam | Réduction |
|--------|--------------|-----------|-----------|
| Produits V1 | 3 | 1 | -66% |
| Distribution V1 | Référents | Web | Simplicité |
| Hardware V1 | Oui | Non | -100% |
| Protocoles V1 | 3 | 1 | -66% |
| Paiements V1 | 4 | 2 | -50% |

---

### Hypothèses Cachées (Socratic Questioning)

*Issu du Socratic Questioning — questions inconfortables pour révéler les angles morts*

#### Les 8 questions qui dérangent

| # | Question | Hypothèse cachée | Vérité révélée |
|---|----------|------------------|----------------|
| 1 | Pourquoi Christophe paierait 12€ au lieu du contrôle parental Orange gratuit ? | "Les gens comprennent pourquoi payer" | Christophe compare au **gratuit FAI** |
| 2 | Que se passe-t-il si NordVPN lance "NordFamily" ? | "Clean Pipe = moat durable" | **Copiable en 6 mois** |
| 3 | Quel budget pub et CAC cible ? | "Acquisition web = facile" | **CAC 30-60€** sans Référents |
| 4 | Qui fait le support V1 ? | "Support = on verra après" | **Burnout** si pas planifié |
| 5 | Qui décide vraiment dans la famille ? | "Christophe (père) décide" | **Marie-Bénédicte** peut être le vrai décideur |
| 6 | Comment Christophe sait que ça marche si rien n'est bloqué ? | "Dashboard = confiance" | **Silence = doute** |
| 7 | Quel % de Christophe sait utiliser Bitcoin ? | "Bitcoin = essentiel" | **Signal > Usage** (<5% réel) |
| 8 | Et si l'ado de 14 ans contourne le filtre ? | "Clean Pipe = infaillible" | Protège les **accidents**, pas les ados déterminés |

#### Réponses stratégiques

**Q1 — Concurrent réel = FAI gratuit**
```
Le vrai concurrent V1 n'est pas Mullvad ou NordVPN.
C'est le contrôle parental GRATUIT de la Freebox/Livebox.

Action : Page comparative "Tuttle vs Contrôle Parental FAI"
- FAI = filtrage basique, pas de VPN, logs opérateur
- Tuttle = filtrage avancé + VPN + Zero-Knowledge
```

**Q2 — Différenciateur V1 non-copiable**
```
Clean Pipe seul = copiable.
Ce qui n'est PAS copiable en 6 mois :
- Le Manifeste (identité)
- La transparence totale (Threat Model public)
- L'open source (crypto + app)
- La communauté early adopters

V1 différenciateur = Valeurs + Transparence + Open Source
V2 différenciateur = + Communauté Référents
```

**Q3 — Économie d'acquisition V1**
```
Hypothèses à valider par test pub AVANT lancement :
- CAC Facebook/Google famille : 30-60€ ?
- Taux conversion landing : 2-5% ?
- LTV (10 mois × 12€) : 120€
- LTV/CAC cible : >3x

Action : Campagne test 500€ pour valider CAC réel
```

**Q4 — Stratégie support V1**
```
Options viables pour solo founder :
1. Support asynchrone uniquement (email, pas de chat)
2. SLA honnête : réponse < 48h (pas 4h)
3. FAQ agressive (80% questions anticipées)
4. Embauche temps partiel dès 200 clients

Action : Documenter stratégie support AVANT lancement
```

**Q5 — Décideur réel = les deux parents**
```
Christophe décide symboliquement.
Marie-Bénédicte gère souvent :
- Budget mensuel
- Achats en ligne
- Inquiétude quotidienne enfants

Action : Landing page qui parle aux DEUX
- Pas juste "tech" (père)
- Aussi "protection famille" (mère)
```

**Q6 — UX rassurante même en silence**
```
Problème : Pas de blocage visible = doute
"Ça marche ou mes enfants n'ont rien cherché ?"

Solutions :
- Rapport hebdo même si zéro : "Semaine calme ✓"
- Indicateur permanent : "Clean Pipe actif depuis 14j"
- Bouton test : "Vérifier que le filtre fonctionne"
```

**Q8 — Honnêteté sur les limites**
```
Clean Pipe protège contre :
✅ Exposition accidentelle (jeunes enfants)
✅ Pubs inappropriées
✅ Erreurs de frappe (URLs similaires)

Clean Pipe NE protège PAS contre :
❌ Ado déterminé avec 4G perso
❌ VPN gratuit installé par l'ado
❌ Consultation chez un ami

Messaging : "Protection contre l'exposition accidentelle"
Pas : "Blocage 100% garanti"
```

#### Actions prioritaires révélées

| # | Action | Priorité | Raison |
|---|--------|----------|--------|
| 1 | Page "Tuttle vs Contrôle Parental FAI" | 🔴 P0 | Concurrent réel |
| 2 | Définir différenciateur V1 (Valeurs + Transparence) | 🔴 P0 | Clean Pipe copiable |
| 3 | Test campagne pub 500€ pour valider CAC | 🔴 P0 | Hypothèse non validée |
| 4 | Documenter stratégie support V1 | 🔴 P0 | Burnout sinon |
| 5 | Landing page pour les deux parents | 🟠 P1 | Marie-Bénédicte décide aussi |
| 6 | UX rassurante (rapport même si zéro) | 🟠 P1 | Silence = doute |
| 7 | Messaging honnête sur limites Clean Pipe | 🟠 P1 | Éviter déception |

---

### Analyse Concurrentielle (Comparative Matrix)

*Issu de la Comparative Analysis Matrix — évaluation pondérée pour Christophe*

#### Critères pondérés (perspective famille protectrice)

| Critère | Poids | Raison |
|---------|-------|--------|
| Filtrage contenu | 25% | Raison #1 d'achat |
| Simplicité | 20% | "Je veux pas bidouiller" |
| Prix | 15% | Budget famille |
| Confiance / Valeurs | 15% | "Pas une boîte louche" |
| Protection vie privée | 10% | Important mais secondaire |
| Support | 10% | "Qui m'aide si ça marche pas ?" |
| Réputation | 5% | "D'autres l'utilisent ?" |

#### Scores pondérés (échelle 1-5)

| Solution | Filtrage | Simple | Prix | Valeurs | Privacy | Support | Réput. | **TOTAL** |
|----------|----------|--------|------|---------|---------|---------|--------|-----------|
| **Tuttle V1** | 5 | 4 | 3 | 5 | 4 | 3 | 1 | **4.05** 🥇 |
| FAI Gratuit | 3 | 5 | 5 | 2 | 1 | 3 | 4 | 3.15 🥈 |
| NordVPN | 3 | 4 | 2 | 2 | 3 | 4 | 5 | 3.10 🥉 |
| Pi-hole | 4 | 1 | 5 | 4 | 3 | 1 | 3 | 3.10 |
| Proton | 2 | 3 | 3 | 4 | 4 | 3 | 4 | 3.05 |
| Mullvad | 1 | 2 | 4 | 5 | 5 | 2 | 3 | 2.75 |

#### Le vrai combat V1 : Tuttle vs FAI Gratuit

```
FAI : "C'est gratuit et déjà activé"
Tuttle : "C'est mieux parce que..."
```

| | Contrôle FAI | **Tuttle V1** |
|---|---|---|
| Prix | Gratuit | 12€/mois |
| VPN inclus | ❌ | ✅ |
| Fonctionne hors domicile | ❌ | ✅ |
| Votre FAI voit tout | ✅ 😱 | ❌ |
| Dashboard détaillé | ❌ | ✅ |
| Catégories configurables | Limité | ✅ |
| Support humain | ❌ | ✅ |

#### Positionnement V1 recommandé

**Message principal :**
> "Le contrôle parental de votre box, en mieux. Partout. Sans que votre FAI sache ce que font vos enfants."

**Forces à pousser :**
- Filtrage famille dédié (Clean Pipe) — unique parmi les VPN
- Valeurs alignées (Manifeste, transparence)
- Fonctionne partout (mobile, hors domicile)

**Faiblesses à mitiger :**
- Réputation (nouveau) → Témoignages early adopters
- Prix vs gratuit FAI → Page comparative avec valeur ajoutée
- Support solo → FAQ agressive, SLA clair

---

### Causes Racines (5 Whys)

*Issu du 5 Whys Deep Dive — creuser jusqu'à la racine*

#### Découvertes clés

| Question | Cause racine |
|----------|--------------|
| Pourquoi Christophe achète ? | Pour **restaurer son rôle de protecteur** (pas pour un VPN) |
| Pourquoi 85% n'utilisent pas de VPN ? | L'industrie **parle aux geeks**, pas aux familles |
| Pourquoi le Clean Pipe différencie ? | Tuttle optimise la **fidélité**, pas le volume |
| Pourquoi solo founder = risque #1 ? | Absence de **documentation**, pas le fait d'être seul |
| Pourquoi Référents en V2 ? | La **confiance** pour le Network demande du temps |

#### Principes directeurs Tuttle

```
1. ÉMOTIONNEL > TECHNIQUE
   Christophe achète un sentiment, pas une feature.
   → Messaging : "Protégez votre foyer" pas "Filtrage DNS"

2. HUMAIN > JARGON
   Parler comme un ami, pas comme une doc technique.
   → Zéro jargon en V1. Glossaire si nécessaire.

3. FIDÉLITÉ > VOLUME
   100 clients fidèles > 1000 clients qui churnent.
   → Ne JAMAIS entrer dans la guerre du streaming.

4. DOCUMENTER > CODER
   Si c'est pas écrit, ça n'existe pas.
   → Chaque process transmissible dès V1.

5. CONFIANCE = TEMPS
   VPN = confiance rapide (achat impulsif OK).
   Network = confiance lente (Référent nécessaire).
```

#### Implications produit

| Principe | Application V1 |
|----------|----------------|
| Émotionnel > Technique | Landing page centrée sur la famille, pas sur la tech |
| Humain > Jargon | Onboarding sans un seul terme technique |
| Fidélité > Volume | Pas de promo "Netflix débloqué", focus Clean Pipe |
| Documenter > Coder | Wiki interne avant lancement |
| Confiance = Temps | VPN en libre-service, Network via Référents |

---

### Checklist "Pas de Regrets" (Hindsight)

*Issu du Hindsight Reflection — regard depuis janvier 2027*

#### Ce qu'on doit avoir fait AVANT lancement

| # | Action | Regret évité si fait |
|---|--------|----------------------|
| 1 | **Beta payante 50 clients** | "On a lancé à l'aveugle" |
| 2 | **Test pub 500€ (CAC validé)** | "On savait pas combien ça coûtait" |
| 3 | **FAQ 30+ questions** | "Support a explosé" |
| 4 | **Analytics dashboard prêt** | "On voyait rien" |
| 5 | **Page limites Clean Pipe** | "Parents déçus par promesses excessives" |
| 6 | **Stratégie SEO/contenu** | "100% dépendant de la pub payante" |
| 7 | **Referral program simple** | "Clients satisfaits mais muets" |
| 8 | **Roadmap publique** | "C'est quand la V2 ?" |
| 9 | **Documentation interne** | "Quentin seul savait" |
| 10 | **Test Clean Pipe vs YouTube** | "Promesse cassée jour 1" |

#### Questions GO/NO-GO avant lancement

| Question | Réponse requise pour GO |
|----------|-------------------------|
| A-t-on 50 clients payants qui valident ? | Oui |
| Sait-on combien coûte un client (CAC) ? | Oui, testé |
| A-t-on une FAQ de 30+ questions ? | Oui |
| Le Clean Pipe bloque-t-il YouTube/TikTok ? | Oui, testé |
| Comment un client satisfait partage-t-il ? | Process clair |

#### Lettre depuis le futur

> *Janvier 2027 → Janvier 2026*
>
> Tu as fait un Product Brief incroyablement détaillé. 20 méthodes d'élicitation.
>
> Mais voici ce que tu DOIS faire avant de lancer :
>
> 1. **50 clients payants en beta.** Pas inscrits. PAYANTS.
> 2. **Teste la pub MAINTENANT.** 500€ = CAC réel.
> 3. **Le Clean Pipe doit bloquer YouTube.** Sinon promesse cassée.
> 4. **Documente TOUT.** Tu vas tomber malade. Prépare-toi.
> 5. **Le B2B viendra te chercher.** Dis pas non. Teste.
>
> Le reste, tu l'as bien préparé.

---

### Innovations SCAMPER

*Issu du SCAMPER Method — 7 lentilles créatives*

#### Top 3 idées à implémenter V1

| # | Idée | Source | Impact |
|---|------|--------|--------|
| 1 | **Dual-Path Sovereignty + Orphean ID** | Architecture | Deux parcours (Managed/Sovereign), données unlinkable |
| 2 | **Offre famille illimitée** | Modify | "Un foyer = un prix" peu importe les appareils |
| 3 | **Rapport hebdo push** | Adapt | Réassurance sans effort (style Apple Screen Time) |

#### Idées V1.5+

| Idée | Source | Description | Phase |
|------|--------|-------------|-------|
| **Mode Whitelist** | Reverse | "Seuls ces 20 sites accessibles" (mode devoirs/petits) | V1.5 |
| **Tuttle Light (DNS)** | Combine | 3€/mois, DNS seul, porte d'entrée | V1.5 |
| **Offre Paroisse** | Put | Prix groupé communauté, un admin | V2 |
| **Notifications push** | Substitute | Remplace dashboard pour les non-geeks | V1 |

#### Détail des innovations V1

**1. Dual-Path Sovereignty + Orphean ID**

Deux parcours d'authentification selon le profil :

| Parcours | Cible | Auth | Gestion Seed |
|----------|-------|------|--------------|
| **Managed (HSM-Backed)** | Jean-Marc, Christophe | Email/Pwd via Zitadel | HSM PKI, vault chiffré côté Tuttle |
| **Sovereign (Non-Custodial)** | Robert, Sophie | Silent Auth (seed = session) | Local, self-custody, 100% autonomie |

**Orphean ID v2 — Isolation cryptographique :**
```
Master Seed (généré localement)
    │
    ├── m/0' → Enrollment ID (shipping, commandes, commerce)
    │
    └── m/1' → Service ID (VPN, usage, connexions)

Résultat : Base shipping JAMAIS linkable à base VPN
           (même si l'une est compromise)
```

**Toggle au checkout :**
```
┌─────────────────────────────────────┐
│ Comment souhaitez-vous continuer ?  │
│                                     │
│ ○ Créer un compte (Email/Mot de passe)
│   → Pour Jean-Marc, récupération facile
│                                     │
│ ○ Mode Souverain (Génération Seed)  │
│   → Pour Robert, anonymat total     │
└─────────────────────────────────────┘
```

**Avantages :**
- Jean-Marc : UX familière, récupération possible, support facilité
- Robert : Zero-Knowledge total, seed = seule preuve d'identité
- Les deux : Shipping et VPN cryptographiquement séparés (Orphean ID)

**2. Offre famille illimitée**
```
Concurrent        : "Jusqu'à 5 appareils"
Tuttle            : "Un foyer, un prix. 2 ou 12 appareils, même tarif."

Cible : Marie-Bénédicte (5 enfants + 2 parents = 7+ appareils)
Message : "Toute la famille protégée, sans compter"
```

**3. Rapport hebdo push**
```
Contenu du rapport (email ou push) :
┌─────────────────────────────────┐
│ 🛡️ Semaine du 20-27 janvier    │
│                                 │
│ ✓ 47 contenus inappropriés     │
│ ✓ 234 trackers publicitaires   │
│ ✓ 12 tentatives de phishing    │
│                                 │
│ Votre famille est protégée.    │
└─────────────────────────────────┘

Pourquoi : Christophe veut savoir que ça marche, pas surveiller un dashboard.
```

**4. Mode Whitelist (V1.5)**
```
Inverse du filtrage classique :
- Blacklist : "Tout sauf X est autorisé"
- Whitelist : "Seul X est autorisé"

Usage : Mode "devoirs" ou très jeunes enfants
Liste suggérée : Wikipedia, sites éducatifs, email famille
Différenciateur : Aucun VPN grand public ne propose ça
```

---

### Raisonnement des Décisions Clés (Explain Reasoning)

*Issu du Explain Reasoning — rendre explicite le POURQUOI*

#### Décisions majeures et leur logique

| Décision | Logique principale | Alternative rejetée |
|----------|-------------------|---------------------|
| VPN seul V1 | Simplicité d'abord, valider avant scaler | Tout lancer d'un coup |
| Pas Référents V1 | Référents = distribution Network | Référents dès V1 |
| Concurrent = FAI | Christophe compare au gratuit existant | Se battre vs NordVPN |
| Un produit, deux campagnes | Éviter double complexité | Deux sites/produits |
| Open Source par couche | Confiance + sécurité infra | 100% open ou 100% fermé |
| **Dual-Path + Orphean ID** | Jean-Marc ET Robert servis | Un seul parcours pour tous |
| Famille illimitée | Familles nombreuses (M-B = 7 pers.) | Paliers par appareils |
| Bitcoin V1, Monero V1.5 | Signal > usage réel | Tout crypto d'emblée |

#### Focus : Dual-Path Sovereignty + Orphean ID

**Pourquoi deux parcours d'authentification ?**
```
Prémisse 1 : Jean-Marc veut un compte classique (récupération, support)
Prémisse 2 : Robert refuse tout compte (anonymat, self-custody)
Prémisse 3 : Les deux profils sont légitimes et importants
Prémisse 4 : Forcer un choix = exclure un segment
─────────────────────────────────────────────────────────
Conclusion : Toggle "Managed / Sovereign" au checkout
```

**Pourquoi Orphean ID (isolation shipping/VPN) ?**
```
Prémisse 1 : Le shipping nécessite une adresse physique
Prémisse 2 : Le VPN doit être Zero-Knowledge (pas de lien identité)
Prémisse 3 : Si même ID → compromission shipping = compromission VPN
Prémisse 4 : HD Derivation permet deux IDs unlinkable du même seed
─────────────────────────────────────────────────────────
Conclusion : m/0' (commerce) et m/1' (VPN) = isolation cryptographique
```

#### Méta-principe unifiant toutes les décisions

```
PRINCIPE : Servir Christophe ET Robert sans compromis

Christophe veut : Simple, familial, récupérable
Robert veut     : Anonyme, souverain, vérifiable

Solution : Deux chemins, même destination (protection)
```

---

### Leçons Héritées (Lessons Learned)

*Issu du Lessons Learned Extraction — expériences tuttle-store + cette session*

#### Du projet tuttle-store

| # | Leçon | Contexte | Action tuttle-master |
|---|-------|----------|----------------------|
| 1 | **Isolation shipping/VPN** | API transporteurs gardent des logs qu'on ne contrôle pas | Orphean ID (m/0' ≠ m/1') + Proxy shipping |
| 2 | **Paiements vraiment résistants** | Stripe + backup fiat = même risque Visa/MC | Vouchers via revendeurs tiers |
| 3 | **Récupération du Seed** | User perd backup → lockout permanent si déconnecté | Emergency Export en session active |
| 4 | **Warrant Canary avec grâce** | Admin oublie de signer → panique inutile | Période 24h ambre + alertes J-7/J-3/J-1 |
| 5 | **Architecture Fail-Closed** | Proxy crash → risque de bypass non-sécurisé | Toujours HALT, jamais contourner |

#### De cette session (22 méthodes)

| # | Leçon | Source | Action |
|---|-------|--------|--------|
| 6 | **Le vrai concurrent = FAI gratuit** | Socratic, Comparative | Page comparative dédiée |
| 7 | **Solo = documenter obligatoire** | Pre-mortem, Shark Tank | Wiki interne avant lancement |
| 8 | **Simplicité > Complétude** | Occam's Razor | VPN + Clean Pipe V1 |
| 9 | **Deux parcours légitimes** | First Principles | Toggle Managed/Sovereign |
| 10 | **Clean Pipe peut décevoir** | Socratic | Tester YouTube/TikTok avant promesses |

#### Patterns Fail-Closed obligatoires

| Composant | Si échec → | Jamais → |
|-----------|------------|----------|
| Shipping Proxy | HALT génération labels | Bypass vers transporteur direct |
| Clean Pipe | BLOCK ALL | Fallback "tout ouvert" |
| Auth | DENY access | Mode dégradé non-authentifié |
| Payment crypto | QUEUE (attendre confirmation) | Valider sans confirmation |

#### Système Vouchers (hérité)

```
Problème : Stripe ET backup fiat peuvent être coupés simultanément
Solution : Tuttle Vouchers

Flux :
1. Revendeur tiers achète vouchers en gros (crypto)
2. Revendeur vend vouchers contre cash/autre
3. Client entre code voucher sur tuttle.net
4. Crédit appliqué au compte

Avantage : Canal de paiement 100% indépendant du système bancaire
Phase : V1.5 (après validation modèle de base)
```

---

### Idées Créatives (Random Input)

*Issu du Random Input Stimulus — connexions inattendues*

#### Top idées retenues

| Source | Idée | Description | Phase |
|--------|------|-------------|-------|
| **Phare** | Tray icon permanent | 🟢 Vert = protégé, 🔴 Rouge = problème. Réassurance passive. | V1 |
| **Accordéon** | "Pause Tuttle" | Suspendre 1-3 mois sans annuler ni perdre config. Anti-churn. | V1.5 |
| **Potager** | Vocabulaire jardin | "Mauvaises herbes arrachées" au lieu de "menaces bloquées". | V1 |
| **Cicatrice** | Page "Nos cicatrices" | Transparence sur incidents passés et résolutions. | V1.5 |
| **Thermos** | Config portable | Export/import réglages Clean Pipe entre appareils. | V2 |

#### Détail : Tray icon permanent (V1)

```
Icône dans la barre système (Windows/Mac/Linux) :

🟢 Vert    : VPN connecté + Clean Pipe actif
🟡 Jaune   : VPN connecté, Clean Pipe désactivé
🔴 Rouge   : VPN déconnecté

Clic droit : Menu rapide (Connecter/Déconnecter/Ouvrir app)
Survol     : "Tuttle actif depuis 14 jours"

Valeur : Christophe voit en permanence que sa famille est protégée
         Sans ouvrir l'app, sans effort
```

#### Détail : "Pause Tuttle" (V1.5)

```
Problème : Client budget serré → annule → churn définitif
Solution : Option "Pause" au lieu d'annulation

Flux :
1. Client : "Je veux annuler"
2. Tuttle : "Préférez-vous une pause ? (1-3 mois, gratuit)"
3. Client : Choisit pause
4. Pendant pause : Pas de paiement, config conservée
5. Fin de pause : Reprise automatique ou rappel

Impact : Transforme un churn en rétention
         Client reconnaissant du geste
```

#### Vocabulaire "jardin" pour Clean Pipe

| Technique | Jardin |
|-----------|--------|
| Menaces bloquées | Mauvaises herbes arrachées |
| Trackers | Parasites |
| Contenu inapproprié | Plantes toxiques |
| Votre réseau | Votre jardin numérique |
| Rapport hebdo | Récolte de la semaine |
| Filtrage actif | Jardinier en service |

> "Cette semaine, votre jardinier a arraché 47 mauvaises herbes et éloigné 12 parasites. Votre jardin est sain."

---

## MVP Scope

tuttle-master est un **projet d'orchestration** qui ne produit pas de code exécutable. Son MVP définit la vision, coordonne les sous-projets, et établit les standards.

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

---

### Core Features

#### 1. Documentation Vision & Architecture Globale

- **Product Brief** complet (ce document)
- **PRD écosystème** avec tous les Functional Requirements cross-projets
- **Architecture Decision Record** définissant les principes techniques partagés
- **Project Context** pour chaque sous-projet (généré automatiquement)

#### 2. Cartographie des Flux de Communication

- **Flux stratégiques** (Master → Enfants) : Décisions architecturales, standards
- **Flux terrain** (Enfants → Master) : Besoins d'interface, dépendances découvertes
- **Diagrammes Excalidraw** : Entry Layer → Provisioner → LightWeb → Exit
- **Contrats API** : Émergent via transmissions (bottom-up), pas de spec formelle MVP

#### 3. Hiérarchie Git Multi-Project

```
tuttle-master/                          # Niveau 0 - Master
├── tuttle-store/                       # V1 Critical - E-commerce
│   ├── frontend/
│   └── backend/
├── tuttle-vpn/                         # V1 Critical - VPN Exit + Control Plane
│   ├── control-plane/
│   └── node-agent/
├── tuttle-provisioner/                 # V1 Critical - Paiement → Accès
├── tuttle-apps/                        # V1 Critical - Clients natifs
│   ├── mobile/
│   │   ├── android/
│   │   └── ios/
│   └── desktop/
│       ├── windows/
│       ├── macos/
│       └── linux/
├── tuttle-infra/                       # V1 Critical - Infrastructure centralisée
│   ├── core/
│   ├── cloud/
│   ├── flux/
│   ├── ansible/
│   ├── secrets/
│   ├── monitoring/
│   ├── backup/
│   └── dns/
├── tuttle-network/                     # V2 Placeholder - LightWeb Dashboard
├── tuttle-proxy/                       # V2 Placeholder - Shipping anonymization
├── tuttle-hardware/                    # V1.5 - Devices physiques
│   ├── tuttle-key/                     # Brownfield existant
│   └── tuttle-box/
└── tuttle-libs/                        # Shared - Composants partagés
    ├── ui/                             # Design system Nuxt
    └── auth/                           # Zitadel helpers
```

#### 4. Stack Technologique par Projet

| Projet | Type | Stack | Status | Init MVP |
|--------|------|-------|--------|----------|
| **tuttle-store** | Web App | Nuxt 3 + Medusa v2 | V1 Critical | ✅ Code actif |
| **tuttle-vpn** | Infrastructure | Go + WireGuard + tuttle-os | V1 Critical | ✅ Code actif |
| **tuttle-provisioner** | Microservice | Go | V1 Critical | ✅ Code actif |
| **tuttle-apps** | Native Apps | Swift/Kotlin/C#/Rust | V1 Critical | ✅ Code actif |
| **tuttle-infra** | DevOps | Terragrunt + Ansible + Talos + Flux | V1 Critical | ✅ Code actif |
| **tuttle-network** | Web App | Nuxt 3 | V2 Deferred | ✅ Placeholder |
| **tuttle-proxy** | Microservice | Go (RAM-only) | V2 Deferred | ✅ Placeholder |
| **tuttle-hardware** | Firmware | Rust + Embedded | V1.5 Deferred | ⚠️ Brownfield |
| **tuttle-libs** | Libraries | TypeScript + Go | Shared | ✅ Code actif |

#### 5. Système de Transmission BMAD

- **Mailbox** pour chaque projet (subscriptions, triggers, escalation)
- **Types de transmission** : architectural, feature-request, dependency, bug, hierarchy-update
- **Règles d'isolation** : Lecture autorisée parent/enfant, écriture via transmission uniquement
- **Contrats API bottom-up** : Les sous-projets remontent leurs besoins, master consolide

#### 6. Contrats Critiques V1 (Interfaces Minimales)

| Interface | Format | Contenu minimal |
|-----------|--------|-----------------|
| **Store → Provisioner** | Markdown | Webhooks Medusa attendus (payment.completed, subscription.*) |
| **Provisioner → VPN** | Markdown | Endpoints VPN API (POST /users, DELETE /users/{id}) |

#### 7. Règles d'Enforcement Structure

| Règle | Enforcement |
|-------|-------------|
| **Pas de /infra local** | Workflow init-multi-project ne crée pas `infra/` dans les enfants |
| **Lint structure** | Script CI qui échoue si Terraform/Ansible hors tuttle-infra |
| **Transmission dispatch** | Hook post-commit propose de dispatcher les transmissions pending |

#### 8. Documentation Onboarding

| Document | Contenu |
|----------|---------|
| **README.md** | Getting Started avec commandes exactes testées |
| **CONTRIBUTING.md** | Comment BMAD-ifier un projet existant (migration brownfield) |
| **docs/transmission-guide.md** | Comment créer, envoyer, recevoir des transmissions |

#### 9. Architecture Decision Records (ADRs)

| ADR | Décision | Rationale |
|-----|----------|-----------|
| **ADR-001** | 9 projets niveau 1 | Isolation et single responsibility > simplicité |
| **ADR-002** | tuttle-provisioner séparé | Découplage Store/VPN, évolutif V2+ |
| **ADR-003** | tuttle-network séparé du store | Isolation réseau (public vs VPN-only) |
| **ADR-004** | tuttle-libs partagé | DRY pour design system et auth Zitadel |
| **ADR-005** | Infra centralisée | Single source of truth, pas de drift |
| **ADR-006** | tuttle-proxy sorti du store | Fail-closed critique, déploiement indépendant |
| **ADR-007** | Repos privés (auth Gitea) | Contrôle et sécurité V1 |
| **ADR-008** | Niveau 3 accepté pour apps | Organisation logique, risque mitigé par CI |

#### 10. Infrastructure Principles

**Philosophie : Pragmatisme K8s vs VM**

| Critère | K8s (Flux) | VM (Ansible) |
|---------|------------|--------------|
| Stateless services | ✅ | - |
| Scalable horizontalement | ✅ | - |
| Databases production | ❌ | ✅ |
| Services avec état persistant | ❌ | ✅ |
| Dev/Staging databases | ✅ (simplicité) | - |

#### 11. GitOps & Deployment Stack

| Composant | Outil | Rôle |
|-----------|-------|------|
| **Git** | Gitea | Repos privés, MCP Gitea configuré |
| **CLI Git** | tea | Gitea CLI pour automation |
| **CI/CD** | Woodpecker | Pipelines (CLI installé) |
| **GitOps K8s** | Flux | Déploiement déclaratif K8s |
| **Config VM** | Ansible | Provisioning VM |
| **IaC** | Terragrunt | Infrastructure cloud |

#### 12. Secrets Management

| Phase | Outil | Transition |
|-------|-------|------------|
| **V1** | SOPS + Age | Secrets chiffrés dans git |
| **V1.5+** | Infisical | Secrets management centralisé |

#### 13. Environnements

| Env | Usage | Databases | Infra |
|-----|-------|-----------|-------|
| **dev** | Développement local/CI | K8s (PostgreSQL container) | K8s local ou staging cluster |
| **staging** | Tests intégration, preview | K8s (PostgreSQL container) | K8s staging cluster |
| **prod** | Production | VM dédiées (PostgreSQL VM) | K8s prod + VM databases |

#### 14. Databases Strategy

| Env | PostgreSQL | Redis | Autres |
|-----|------------|-------|--------|
| **dev/staging** | Container K8s | Container K8s | Container K8s |
| **prod** | VM dédiée (Proxmox) | VM ou K8s selon usage | VM si stateful |

#### 15. Tooling installé (pré-requis)

| Outil | Usage | Statut |
|-------|-------|--------|
| `tea` | Gitea CLI | ✅ Configuré |
| `woodpecker-cli` | CI/CD CLI | ✅ Installé |
| `flux` | GitOps K8s | ✅ À configurer |
| `sops` | Secrets V1 | ✅ À configurer |
| `terragrunt` | IaC | ✅ À configurer |
| `ansible` | Config VM | ✅ À configurer |

#### 16. tuttle-provisioner Resilience

| Mécanisme | Implémentation |
|-----------|----------------|
| **Queue persistante** | Redis/BullMQ ou PostgreSQL job queue |
| **Retry exponential** | 3 retries avec backoff (1s, 5s, 30s) |
| **Dead letter queue** | Webhooks échoués → table `failed_provisions` pour review admin |
| **Manual provision** | Endpoint admin `POST /admin/provision/{order_id}` |
| **Alerting** | Telegram/PagerDuty si queue > 10 pending ou > 5 failed |

#### 17. Legislative Weather V1 (transparence)

| Aspect | V1 | Documentation |
|--------|----|----|
| **Source** | Recherche manuelle admin | "Pools basés sur analyse janvier 2026" |
| **Mise à jour** | Trimestrielle minimum | Changelog public sur /legal-weather |
| **Disclaimer UI** | Affiché sur sélection pool | "Indicatif, vérifiez les lois locales" |
| **Engagement légal** | Aucune garantie | CGV claires "information, pas conseil juridique" |

#### 18. Continuité Opérationnelle

| Document | Contenu | Localisation |
|----------|---------|--------------|
| **RUNBOOK.md** | Procédures incidents (VPN down, DB crash, payment stuck) | tuttle-infra/docs/ |
| **OPERATIONS.md** | Déployer, rollback, scale, rotate secrets | tuttle-infra/docs/ |
| **SECRETS-RECOVERY.md** | Récupérer accès si SOPS keys perdues | tuttle-infra/docs/ (chiffré) |
| **ARCHITECTURE.md** | Vue d'ensemble pour nouveau dev | tuttle-master/docs/ |

#### 19. Developer Onboarding

| Étape | Temps | Outil |
|-------|-------|-------|
| Clone repo | 2 min | `git clone --recurse-submodules` + token Gitea |
| Setup script | 5 min | `./scripts/setup-dev.sh` |
| First build | 3 min | `make dev` ou `task dev` |
| **Total** | **~10-15 min** | Avec script automatisé |

#### 20. Payment Methods V1

| Méthode | Processor | Compte requis |
|---------|-----------|---------------|
| **Carte bancaire** | Stripe Checkout | Optionnel (guest checkout) |
| **Bitcoin** | BTCPay Server (self-hosted) | Non |
| **Monero** | BTCPay Server (self-hosted) | Non |

**Parcours "Robert" (Sovereign Path) :**

```
1. Robert → Store → "Payer en Monero"
2. Store → BTCPay → Invoice XMR générée (order_id en metadata)
3. Robert paie depuis son wallet (aucun compte, aucun email)
4. BTCPay webhook → tuttle-provisioner
5. Provisioner → VPN API → User créé
6. Page confirmation affiche : "Télécharger votre config WireGuard"
7. Robert télécharge .conf → importe dans app → connecté
```

---

### tuttle-apps V1 Scope

| Plateforme | V1 | V1.5 | V2 |
|------------|----|----- |----|
| **Android** | ✅ Priorité 1 | - | - |
| **Windows** | ✅ Priorité 2 | - | - |
| iOS | Config WireGuard manuelle | ✅ App native | - |
| macOS | Config WireGuard manuelle | ✅ App native | - |
| Linux | Config WireGuard manuelle | - | ✅ App native |

### tuttle-vpn V1 Scope

| Feature | V1 | V2 |
|---------|----|----|
| **WireGuard basic** | ✅ | ✅ |
| **tuttle-os** (image serveur) | ✅ | ✅ |
| **4 serveurs** (2 SANCTUARY, 2 BALANCED) | ✅ | + scale |
| **Legislative Weather déclaratif** | ✅ ENV/config | ✅ Dynamique |
| **Visuel web pools** | ✅ (basé sur config) | ✅ (temps réel) |
| LightWeb mesh (NetBird) | ❌ | ✅ |
| Multi-hop | ❌ | ✅ |
| V2Ray obfuscation | ❌ | ✅ |

---

### Out of Scope for MVP

| Élément | Raison | Quand |
|---------|--------|-------|
| **Wiki Gitea** | Documentation interne suffit | Post-MVP |
| **API Contracts formels (OpenAPI)** | Émergent via transmissions terrain | Quand services existent |
| **Code exécutable** | tuttle-master = orchestration pure | Jamais |
| **Gouvernance communautaire** | Après traction | M12+ |
| **Code dans tuttle-network** | V2 - LightWeb | Post-V1 |
| **Code dans tuttle-proxy** | V2 - Shipping | Post-V1 |
| **Legislative Weather dynamique** | Complexité scraper + AI | V2 |
| **Multi-payment processors** | BTCPay suffit | Post-V1 |

---

### MVP Success Criteria

| Critère | Mesure | Validation |
|---------|--------|------------|
| **Clone authentifié** | `git clone --recurse-submodules` avec token Gitea récupère tout | ☐ Test checkout |
| **CI Submodule Health** | Action CI valide le clone complet à chaque push sur main | ☐ Pipeline vert |
| **Getting Started** | README avec commandes exactes, premier build < 15 min | ☐ Test onboarding |
| **Inbox autonome** | Sous-projet peut `git pull` inbox et recevoir instructions | ☐ Test transmission offline |
| **Alerte transmission stagnante** | Script détecte transmissions outbox > 24h et alerte | ☐ Test alerte |
| **Hiérarchie sync** | Tous les `hierarchy.csv` locaux reflètent l'état réel | ☐ Script validation |
| **Contrats critiques V1** | Interfaces Store→Provisioner et Provisioner→VPN documentées | ☐ Review docs |
| **Pas d'infra locale** | Aucun projet sauf tuttle-infra ne contient Terraform/Ansible | ☐ Lint structure |
| **Brownfield BMAD-ified** | tuttle-key a `_bmad/`, `_mailbox/`, `hierarchy.csv` | ☐ Checklist migration |
| **9 projets initialisés** | Tous avec structure BMAD complète | ☐ Checklist structure |
| **Woodpecker pipelines** | Chaque projet a `.woodpecker.yml` fonctionnel | ☐ CI vert |
| **Flux déployé** | Cluster staging synchronisé via Flux | ☐ `flux get all` OK |
| **3 envs configurés** | dev, staging, prod pour chaque projet | ☐ Checklist envs |
| **SOPS fonctionnel** | Secrets chiffrés, déchiffrables en CI | ☐ Test secret rotation |
| **DB prod en VM** | PostgreSQL sur VM Proxmox, backup automatique | ☐ Test restore |
| **Gitea + tea** | Tous repos créés, tea CLI fonctionnel | ☐ `tea repo list` |
| **Provisioner queue** | Webhooks persistent même si service restart | ☐ Test kill provisioner |
| **Provisioner retry** | Failed webhook retenté 3x avec backoff | ☐ Test webhook timeout |
| **Manual provision** | Admin peut forcer provision via endpoint | ☐ Test endpoint admin |
| **Runbook testé** | Un tiers peut follow runbook et résoudre incident simulé | ☐ Test avec externe |
| **BTCPay opérationnel** | Paiement XMR end-to-end fonctionnel | ☐ Test achat Monero |
| **Sovereign path** | Robert peut acheter sans compte ni email | ☐ Test parcours complet |

---

### V1 Roadmap

#### Sprint 0 : Foundation

| Projet | Score | Livrable |
|--------|-------|----------|
| **tuttle-infra** | 3.10 | K8s cluster, Flux, SOPS, monitoring, DB VM |
| **tuttle-libs** | 2.75 | Design system Nuxt, auth helpers Zitadel |

**Gate :** Infra opérationnelle, premiers déploiements Flux fonctionnels

#### Sprint 1 : Core Product

| Projet | Score | Livrable |
|--------|-------|----------|
| **tuttle-store** | 4.15 | Landing, Medusa, paiement Stripe + BTCPay |
| **tuttle-vpn** | 4.05 | 4 serveurs tuttle-os, WireGuard, Legislative Weather config |
| **tuttle-provisioner** | 3.95 | Webhooks Medusa → création user VPN |

**Gate :** Tunnel complet : paiement → accès VPN automatique

#### Sprint 2 : Clients

| Projet | Score | Livrable |
|--------|-------|----------|
| **tuttle-apps/android** | 3.85 | App native Kotlin, connexion VPN |
| **tuttle-apps/windows** | 3.30 | App native C#/WinUI, connexion VPN |

**Gate :** Christophe peut acheter et se connecter depuis Android ou Windows

---

### 🚀 V1 LAUNCH

---

### V1.5 Roadmap

| Projet | Score | Livrable |
|--------|-------|----------|
| **tuttle-hardware/key** | 2.70 | Intégration brownfield, auth hardware |
| **tuttle-apps/ios** | 2.50 | App native Swift |
| **tuttle-apps/macos** | 2.00 | App native Swift (code partagé iOS) |

### V2 Roadmap

| Projet | Score | Livrable |
|--------|-------|----------|
| **tuttle-proxy** | 2.10 | Shipping anonymization |
| **tuttle-network** | 1.85 | LightWeb dashboard, Matrix, Mailu |
| **tuttle-hardware/box** | 1.85 | Tuttle Box hardware |
| **tuttle-apps/linux** | 1.50 | App native Rust + GTK |

---

### Priorités par Score

| Priorité | Score | Projets |
|----------|-------|---------|
| 🔴 **V1 Critical** | > 2.75 | store, vpn, provisioner, apps/android, apps/windows, infra, libs |
| 🟡 **V1.5** | 2.0 - 2.75 | hardware/key, apps/ios, apps/macos |
| 🟢 **V2** | < 2.0 | network, proxy, hardware/box, apps/linux |

---

### Future Vision

#### Phase 1 : Délégation IA (M6-M12)

- Agent autonome par sous-projet exécutant stories sans supervision
- Transmissions automatiques : détection bug → transmission → correction
- Sprint autonome : planification, exécution, rapport

#### Phase 2 : Orchestration Multi-Agent (M12-M24)

- Party Mode inter-projets : agents débattent des décisions architecturales
- Escalation intelligente vers décision humaine
- Self-healing ecosystem : correction automatique des incohérences

#### Phase 3 : Gouvernance Décentralisée (M24+)

- Contribution externe via fork et transmissions
- Consensus automatisé sur changements majeurs
- Documentation auto-générée vers Wiki Gitea

---

## Success Metrics (Écosystème)

### Métriques de Succès Utilisateur — Le "Moment de Paix"

Ce que l'utilisateur ressent quand tout fonctionne :

| Métrique | Objectif | Mesure |
|----------|----------|--------|
| **Fluidité Écosystème** | Zéro panne d'accès perçue | 99.9% uptime (hors maintenance planifiée), zéro déco involontaire |
| **Clean Pipe Efficace** | Internet "propre" constaté | Requêtes pub/porno bloquées au DNS (Pi-hole style) |
| **Interface Efficace** | Navigation intuitive | < 3 clics pour toute action principale |
| **Fonctionnement Cohérent** | Prévisibilité du système | Comportement identique sur tous les devices |
| **Accès Dissidence** | Liberté d'information | Zéro faux positif sur sources alternatives |

> "Christophe ne doit jamais douter de son bouclier."

### Objectifs Business — Chemin Critique Complet

| Phase | Échéance | Objectif | Métrique Quantitative | Métrique Qualitative |
|-------|----------|----------|----------------------|---------------------|
| **M0 Pre-launch** | Lancement | Beta privée validée | 10 early adopters, tunnel testé | Zéro friction critique identifiée |
| **M3 Validation** | 3 mois | 100 abonnés actifs | Acquisition ≥ 10/sem | **10 interviews approfondies** (pas juste des chiffres) |
| **M6 Scale** | 6 mois | 500 abonnés actifs | NPS > 30 (min 30 réponses) | **1 canal acquisition scalable** identifié |
| **M9 Pre-hardware** | 9 mois | 1,100 abonnés + liste bêta Key | > 100 intéressés Key | Feedback bêta documenté |
| **M12 Traction** | 12 mois | 1,500 abonnés actifs | Acquisition ≥ 40/sem | Croissance non dépendante du founder |
| **Continu** | Permanent | Rétention Militante | Churn < 5% | Raisons départ documentées |

**Checkpoints Go/No-Go (renforcés) :**

| Milestone | Conditions Go | Facteur Humain | Action si No-Go |
|-----------|---------------|----------------|-----------------|
| M0 → M3 | Tunnel conversion > 85% | Founder < 50h/sem | Revoir UX paiement |
| M3 → M6 | NPS > 25 (min 30 réponses), dette tech OK | Runway > 6 mois | Pause feature, focus stabilité |
| M6 → M9 | Infra 2x, canal scalable identifié | Support < 10h/sem | Scale infra ou automatiser support |
| M9 → M12 | Liste bêta Key > 50 | Cash pour production Key | Reporter Key si cash insuffisant |

**⚠️ Stress Test : Et si le churn est de 10-12% ?**

| Scénario | Impact sur M12 | Mitigation |
|----------|----------------|------------|
| Churn 10% | 1,500 → ~1,200 users nets | Acquisition +20% ou réduire objectif |
| Churn 12% | 1,500 → ~1,050 users nets | Pivot : focus rétention avant croissance |
| Cause probable | Bug sécurité, concurrent mieux positionné | Audit sécurité M3, veille concurrentielle mensuelle |

**Justifications Shark Tank :**

- **Acquisition sans budget pub** : Canaux organiques ciblés — présence sur réseaux dissidents (Telegram, forums privacy), contenu éducatif (tutoriels souveraineté), bouche-à-oreille Référents (V2). Pas de masse, mais qualité des early adopters.
- **Churn < 5% vs benchmark 8%** : Engagement idéologique > engagement transactionnel. Les militants ne partent pas pour économiser 2€. La promesse émotionnelle ("protéger l'âme") crée une fidélité supérieure au marché VPN commodity.

### Key Performance Indicators (Indicateurs Avancés)

Ces indicateurs prédisent le succès avant les métriques business :

| KPI | Description | Seuil d'Alerte (Progressif) |
|-----|-------------|----------------------------|
| **Vitesse d'Acquisition** | Nouveaux inscrits/semaine | M1-M3: < 10/sem, M4-M6: < 25/sem, M7-M12: < 40/sem |
| **Taux Conversion Bêta** | % abonnés VPN → demande Tuttle-Key | < 5% = revoir proposition hardware |
| **Stabilité Technique** | Zéro déconnexion involontaire par session | > 1 déco/session = problème critique |
| **Score Friction UX** | Abandons tunnel d'achat | > 15% = friction payment/auth |
| **NPS "Militant"** | Recommandation spontanée (Managed path) | < 40 = promesse non tenue |
| **Taux Renouvellement Sovereign** | % users seed-only qui renouvellent | < 90% = valeur non perçue |

**Clarifications méthodologiques :**

- **NPS limité au Managed Path** : Formulaire optionnel post-achat, découplé de l'identité VPN. Les users Sovereign (seed-only) sont injoignables par design → le "Taux de Renouvellement Sovereign" mesure leur satisfaction indirectement (ils paient = ils restent).
- **Seuil de validité NPS** : Minimum **30 réponses** avant de calculer/publier un NPS. En dessous, donnée non statistiquement significative. Avec 5-10% de taux de réponse, il faut ~400 users Managed pour un NPS fiable.
- **Uptime 99.9% = hors maintenance planifiée** : Le 0.1% de downtime autorisé = maintenance annoncée, jamais de coupure involontaire. Monitoring automatisé (Prometheus/Grafana) + failover infra + alertes SMS. Architecture "self-healing" prioritaire.
- **Interviews > Chiffres à M3** : 100 users ne prouve rien statistiquement. Les 10 interviews approfondies sont la vraie validation du product-market fit. Questions clés : "Pourquoi êtes-vous resté ?", "Qu'est-ce qui vous ferait partir ?", "À qui en avez-vous parlé ?"

> ⚠️ "Key management is the #1 reason for churn in decentralized tools" — Market Research 2026

### Métriques User-Facing (ce que l'utilisateur VOIT)

*Validé par Focus Group : Christophe (dissident) + Marie-Bénédicte (famille)*

| Métrique Visible | Persona Cible | Implémentation | Pourquoi |
|------------------|---------------|----------------|----------|
| **Compteur Protection** | Marie-Bénédicte | "127 menaces bloquées ce mois" dans dashboard | Validation émotionnelle : "ça marche" |
| **Warrant Canary** | Christophe | Indicateur vert/rouge (< 30 jours = vert) | Preuve de non-compromission |
| **Bouclier Actif** | Tous | Tray icon + statut "Protégé" | Zéro doute sur l'état de protection |
| **Audit Public** | Christophe | Lien vers rapports sécurité / code source | Vérifiabilité = confiance |

| Métrique Support | Persona Cible | Objectif | Pourquoi |
|------------------|---------------|----------|----------|
| **Temps Réponse Standard** | Marie-Bénédicte | < 24h | Elle a besoin de savoir qu'un humain répond |
| **Temps Réponse Urgence** | Tous | < 4h | Problème d'accès = urgence |

**⛔ Métriques à NE JAMAIS communiquer aux users :**

| Métrique Interne | Raison |
|------------------|--------|
| Nombre total d'utilisateurs | Christophe : "Plus gros = plus cible". Marie-B : s'en fiche |
| Churn rate | Jargon business, crée doute si communiqué |
| Objectifs d'acquisition | Donne l'impression de "croissance à tout prix" |
| Revenue/ARR | Perception mercantile vs mission |

### Critères de Succès MVP (V1.0)

| Critère | Mesure | Statut |
|---------|--------|--------|
| **Conversion Directe** | Tunnel d'achat "Zéro-Data" fluide | ☐ Crypto + Stripe sans friction |
| **Dual-Path Fonctionnel** | Managed + Sovereign opérationnels | ☐ Les deux chemins auth marchent |
| **Clean Pipe Actif** | Filtrage DNS déployé | ☐ Catégories configurables |
| **Uptime Garanti** | 99.9% hors maintenance, zéro déco involontaire | ☐ Monitoring + failover auto |

### Indicateurs de Cohérence (tuttle-master)

| Indicateur | Mesure | Objectif |
|------------|--------|----------|
| **Alignement Projets** | % sous-projets respectant principes directeurs | 100% |
| **Documentation Sync** | PRDs/Architectures à jour vs code | < 1 semaine de retard |
| **Transmission Efficacy** | Latence inbox → action implémentée | < 48h pour critiques |
| **Fail-Closed Compliance** | % systèmes qui HALT on failure | 100% (Proxy, Clean Pipe, Auth) |

---

<!-- WORKFLOW STATUS: COMPLETED - All 6 steps done -->
<!-- Next: /bmad:bmm:workflows:prd or /bmad:multiproject:workflows:init-multi-project -->
