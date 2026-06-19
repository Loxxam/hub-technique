# CLAUDE.md — Hub Technique

> Briefing pour Claude Code. Lire ce fichier en premier à chaque session.

## 1. Le projet en une phrase

Fichier HTML statique unique (`index.html`) servant de référence technique de bureau pour un DSI travaillant sur Mac : raccourcis clavier macOS AZERTY + modes opératoires (Git, Claude Code, et bientôt Docker / Homebrew / SSH).

Publié sur GitHub Pages : https://loxxam.github.io/hub-technique/ (repo : https://github.com/Loxxam/hub-technique).

## 2. L'utilisateur

- **Alexandre PASCAL** (prénom Alexandre, nom PASCAL), DSI Piscines Desjoyaux SA (Veauche, Loire, France)
- Compte Mac : `pascal` — email pro : `pascala@desjoyaux.fr`
- Stack pro : SAP ECC6, Salesforce, .NET 8 / Rider, Angular 19, Flutter, Docker
- Machine : MacBook Air M5, clavier Logitech MX Keys AZERTY français
- Profil dev : compréhension et supervision plutôt qu'écriture en production
- **Communication** : toujours en français, ton tutoyant, structuré et direct
- **Préférences** : prose fluide avec listes ciblées, pas de blabla, validations courtes par numérotation

## 3. Architecture du fichier

`index.html` est **un seul fichier HTML autonome** (CSS + JS inline, polices locales servies depuis `assets/fonts.css`). Aucune dépendance npm, aucun build step. Tout doit rester dans ce fichier unique, à l'exception des polices et de leurs fichiers placés dans `assets/`.

### Structure visuelle

```
┌───────────────┬──────────────────────────────────────────┐
│  SIDEBAR      │  MAIN                                    │
│  (260px)      │                                          │
│               │  [Header adaptatif]                      │
│  Brand        │                                          │
│  Search       │  [Page active]                           │
│               │    Section 1                             │
│  RACCOURCIS   │      Cards / Scenarios / Projects        │
│   - 10 items  │    Section 2                             │
│               │      ...                                  │
│  MOP          │                                          │
│   - Git       │                                          │
│   - Claude    │                                          │
│   - MD enrichi│                                          │
│                                                          │
│  À VENIR      │                                          │
│   - Docker    │                                          │
│   - Homebrew  │                                          │
│   - SSH       │                                          │
└───────────────┴──────────────────────────────────────────┘
```

### Pages existantes

Chaque page = un `<div class="page" data-page="X" data-type="Y">` avec `data-type="shortcuts"` ou `data-type="mop"`.

**Type `shortcuts` (10 pages)** : `system`, `chars`, `screenshot`, `finder`, `text`, `simulator`, `jetbrains`, `vscode`, `browser`, `terminal`

**Type `mop` (3 pages)** : `git`, `claude-code`, `md-enrichi`

### Header adaptatif

- Pages `shortcuts` → affichent `#shortcutsHeader` (titre "Raccourcis clavier macOS" + légende 2 colonnes des touches Mac)
- Pages `mop` → masquent `#shortcutsHeader`, affichent leur propre `.page-header` avec eyebrow + titre + intro

Géré par `switchPage()` dans le `<script>` final.

## 4. Conventions de design (à respecter strictement)

### Palette CSS (variables `:root`)

```css
--bg:          #0d0d0f      /* fond principal */
--bg-raised:   #17171a      /* cartes, sidebar */
--bg-card:     #1c1c20      /* hover */
--bg-code:     #0a0a0c      /* blocs code */
--text:        #ededea
--text-muted:  #8a8a92
--text-dim:    #5c5c64
--accent:      #d4a574      /* doré, couleur signature */
--accent-blue: #7aa7d9      /* callout info */
--accent-green:#7dc89a      /* callout tip, badge L1 */
--accent-red:  #d97a7a      /* callout warn, badge L3 */
```

### Typographie

- **Instrument Serif** → titres principaux (italique pour accents)
- **Inter** → corps de texte
- **JetBrains Mono** → code, kbd, eyebrows, labels techniques

### Composants

- **`.card`** → carte raccourci. Grille 4 colonnes. Format : description à gauche, touches `<kbd>` à droite.
- **`.scenario`** → bloc MOP. Cercle doré devant le titre. Description avec `<strong>` et `<code>` pour mise en valeur.
- **`.code-block`** → bloc code copiable. Toujours avec header (langage + bouton "📋 Copier"). Coloration syntaxique manuelle via classes `.sh-comment .sh-str .sh-func`.
- **`.callout`** → encarts info (bleu) / `.tip` (vert) / `.warn` (rouge). Toujours avec icône emoji.
- **`.project`** → carte projet Claude Code. Description **calibrée à 5 lignes / ~75 mots** + prompt copiable. Badges `.badge-l1` à `.badge-l4`.

### Règles de calibrage

1. **Cartes raccourcis** : description courte, max 2 lignes. Si tip nécessaire, utiliser `<span class="tip">`.
2. **Scenarios MOP** : titre clair, description ~3 phrases avec un code-block ou callout derrière.
3. **Projets** : description **exactement 5 phrases / ~75 mots** pour que toutes les cartes aient la même hauteur.
4. **Pas d'emojis dans le contenu** (sauf icônes sidebar et callouts) — rester sobre.

## 5. État actuel (Q2 2026)

### ✅ Fait

- 10 catégories de raccourcis complètes
- Section Git : 7 sous-sections + cheat sheet (concepts, config, quotidien, branches, erreurs, collab)
- Section Claude Code : 6 sous-sections (installation, premiers pas, scénarios, slash, bonnes pratiques, projets)
- Section Markdown enrichi : convention de balisage couleur VS Code (légende 6 marqueurs avec aperçu couleur + config `settings.json` copiable, extension fabiospampinato.vscode-highlight)
- 13 idées de projets calibrées en 4 niveaux (L1 vert, L2 jaune, L3 rouge, L4 violet)
- Recherche globale dans la sidebar (filtre cards + scenarios + projects + lignes de table)
- Header adaptatif (légende visible uniquement pour les pages shortcuts)
- Responsive mobile (menu burger, grilles qui passent à 2 puis 1 colonne)
- Boutons "Copier" sur tous les blocs code
- Navigation par ancres depuis les sous-éléments de sidebar

### 🔜 À faire (par ordre de priorité)

1. **Docker** — le plus utile (CinéFamille tourne sur QNAP en Docker)
   - Sections suggérées : concepts (image vs container vs volume), installation Docker Desktop Mac, commandes essentielles (`run`, `ps`, `exec`, `logs`), `docker-compose` (notamment pour le QNAP), dépannage courant
2. **Homebrew** — gestionnaire de paquets Mac
   - Sections : installation, formulae vs casks, commandes courantes (`install`, `upgrade`, `cleanup`), bundle, services, dépannage
3. **SSH** — connexions distantes
   - Sections : génération de clés, fichier `~/.ssh/config`, agents, tunnels, copie de clés sur serveur, troubleshooting

### Mécanique d'ajout d'un mode opératoire

Pour ajouter une nouvelle catégorie MOP (ex. Docker) :

1. **Sidebar** : déplacer `Docker` du groupe "À venir" vers le groupe "Modes opératoires", ajouter un `<div class="nav-sub">` avec les ancres
2. **Page** : créer un `<div class="page" data-page="docker" data-type="mop">` dans `<main>`, copier la structure exacte de la page Git (page-header + sections)
3. **Pas de CSS à toucher** : tout existe déjà
4. **JS** : aucun changement nécessaire, `switchPage()` et la recherche fonctionnent automatiquement

## 6. Conventions de contenu

### Pour les MOP

- Niveau cible : **développeur qui débute** sur la techno mais connaît C# / .NET / Angular
- Toujours expliquer le **pourquoi** avant le **comment**
- Préférer les **scénarios concrets** ("Je veux corriger un bug") aux listes de commandes brutes
- Chaque commande dans un `code-block` avec bouton Copier
- Au moins un `callout.tip` ou `callout.warn` par section pour souligner les pièges

### Pour les exemples de code

- Préférer des exemples liés aux projets réels d'Alexandre quand pertinent : GLPI Ticket Viewer, CinéFamille, macros VBA Desjoyaux, contexte SAP/Salesforce
- Rester généraliste quand c'est plus pédagogique

## 7. À ne pas faire

- ❌ Découper le fichier en plusieurs fichiers (tout doit rester dans `index.html`)
- ❌ Ajouter des dépendances JS externes (pas de framework, pas de bundler)
- ❌ Toucher à la palette de couleurs ou aux polices
- ❌ Modifier la structure de la sidebar sans en discuter
- ❌ Ajouter du contenu sans respecter le calibrage des composants existants
- ❌ Mettre des emojis partout — rester sobre

## 8. Premier prompt suggéré pour reprendre

```
Lis index.html en entier pour bien comprendre la structure.
Puis lis CLAUDE.md pour connaître les conventions du projet.

On va ajouter la catégorie Docker dans le groupe "Modes opératoires"
de la sidebar (déplacer depuis "À venir"). Structure proposée :

1. Concepts essentiels (image, container, volume, network, registry)
2. Installation sur Mac (Docker Desktop, alternatives)
3. Commandes du quotidien (run, ps, exec, logs, stop, rm)
4. Docker Compose (cas d'usage CinéFamille sur QNAP)
5. Dépannage courant
6. Cheat sheet finale

Respecte exactement le style des pages Git et Claude Code :
- même structure HTML (page-header, sections, scenarios, code-blocks, callouts)
- mêmes classes CSS, pas de CSS supplémentaire
- pareil pour le ton et le niveau de vulgarisation

Propose-moi d'abord le plan détaillé de chaque section
(titres + 1 phrase par scenario) avant d'écrire quoi que ce soit.
On valide ensemble avant l'écriture.
```

## 9. Workflow recommandé

```bash
# Avant chaque session
cd ~/Development/hub-technique
git status                              # vérifier l'état
git pull                                # si tu travailles depuis plusieurs machines

# Sécuriser un point de reprise avant gros changement
git add .
git commit -m "WIP avant ajout Docker"

# Lancer Claude Code
claude

# Tester en parallèle dans Safari
open index.html
# (raccourci ⌘R pour rafraîchir après chaque modif)

# Publier les modifs en ligne (GitHub Pages se met à jour automatiquement)
git push

# À la fin de la session
git add .
git commit -m "Ajoute catégorie Docker (concepts + install)"
```

## 10. Historique des versions

- **v1** — `raccourcis-mac.html` : 10 catégories de raccourcis, design dark, recherche
- **v2** — `modes-operatoires.html` : ajout Git + Claude Code, sidebar dédiée
- **v3** — `hub-technique.html` : fusion des deux, sidebar unifiée, recherche globale, header adaptatif
- **v4** — `index.html` (actuel) : renommage pour publication GitHub Pages, polices locales, mise en ligne sur https://loxxam.github.io/hub-technique/
- **v4.1** — ajout du MOP « Markdown enrichi » (convention de balisage couleur VS Code)

---

*Fichier maintenu à jour à chaque évolution majeure du projet. Dernière mise à jour : 19 juin 2026.*
