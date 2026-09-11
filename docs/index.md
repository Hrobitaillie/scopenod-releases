# ScopeNod — Documentation

**ScopeNod** est un logiciel de **cadrage de projet assisté par IA**. Application desktop, *local-first* :
un projet = **un seul fichier** (`.scopenod`) qui contient tout — le graphe de cadrage, la
documentation, le chiffrage et les commentaires. On ne pilote pas un site ; on cadre un produit.

Sa particularité : **Claude travaille directement sur votre graphe** via un serveur MCP. Vous décrivez
l'intention, Claude crée les modules, les fonctionnalités chiffrées, les liens et la documentation —
que vous réarrangez ensuite au canvas.

> Cette page est le guide utilisateur. Si vous voulez juste **tester un workflow complet en 15 min**,
> allez directement à [Tutoriel : un projet de zéro](#tutoriel--un-projet-de-zéro).

## Sommaire

1. [Installer ScopeNod](#1-installer-scopenod)
2. [Les concepts en 2 minutes](#2-les-concepts-en-2-minutes)
3. [Créer ou ouvrir un projet](#3-créer-ou-ouvrir-un-projet)
4. [Connecter Claude à un projet](#4-connecter-claude-à-un-projet)
5. [Installer le skill (travail efficace sur un graphe)](#5-installer-le-skill)
6. [Tutoriel : un projet de zéro](#tutoriel--un-projet-de-zéro)
7. [Les outils MCP (référence)](#6-les-outils-mcp-référence)
8. [Commentaires « pour Claude »](#7-commentaires-pour-claude)
9. [Dépannage](#8-dépannage)

---

## 1. Installer ScopeNod

1. Ouvrez la page [**Releases**](https://github.com/Hrobitaillie/pbones-updates/releases/latest).
2. Téléchargez le `…-setup.exe` de la dernière version et lancez-le (installeur Windows).
3. Au premier lancement, l'app se déclare comme éditeur des fichiers `.scopenod`.

**Mises à jour automatiques.** ScopeNod embarque un updater signé : à chaque nouvelle version publiée,
une bannière « Mettre à jour et relancer » apparaît en bas de la fenêtre. Rien à télécharger à la main.

---

## 2. Les concepts en 2 minutes

Un projet ScopeNod réunit quatre couches dans un même fichier :

| Couche | Contenu | L'atome |
|---|---|---|
| **Graphe de cadrage** | pages & blocs (l'arborescence écran, le *où*), modules, fonctionnalités, services | la **fonctionnalité** = le *quoi*, chiffrable |
| **Documentation** | pages markdown classées par catégorie (Contexte, Specs, Technique…) | le **doc** |
| **Chiffrage** | estimation par fonctionnalité (1 j = 7 h), regroupée par lots (Prototype / V1 / V2…) | l'**estimate** |
| **Commentaires** | fils ancrés sur une page/un bloc, dont des fils ✳ **adressés à Claude** | le **fil** |

Les liens structurent le tout : `dependsOn` (fonctionnalité → fonctionnalité), `realizedBy`
(fonctionnalité → page/bloc), `navigatesTo` (page → page).

Vous n'éditez jamais le JSON à la main : **l'app** (canvas, docs, chiffrage) et **Claude** (via MCP)
sont les deux seules façons de modifier le projet — le format est versionné et validé.

---

## 3. Créer ou ouvrir un projet

Depuis l'écran d'accueil :

- **Nouveau projet** — crée un `.scopenod` vide, prêt à être cadré (souvent avec Claude).
- **Ouvrir un fichier…** — ouvre un `.scopenod` existant (l'ancien `.cadrage` reste ouvrable).
- **Glisser-déposer** un fichier n'importe où sur l'accueil.

Les projets créés depuis l'app sont rangés dans un dossier de stockage interne et réapparaissent dans
**Projets récents**. Notez le **chemin** du fichier : il servira à connecter Claude depuis un terminal
externe.

---

## 4. Connecter Claude à un projet

Claude agit sur le graphe via le **serveur MCP `scopenod`**, qui lit et écrit *le fichier du projet
ouvert*. Il y a deux façons de le brancher.

### Option A — Terminal intégré (le plus simple)

Dans le panneau latéral droit → section **« Terminal Claude & MCP »**. Ce terminal est **déjà câblé
sur le projet ouvert** : la variable `SCOPENOD_PROJECT` pointe automatiquement sur votre fichier.

➡️ Tapez simplement :

```
claude
```

Claude démarre avec l'outil MCP `scopenod` connecté au bon projet. Zéro configuration.

### Option B — Depuis n'importe quel autre terminal (VS Code, etc.)

Vous travaillez sur le repo de votre projet web dans VS Code et voulez que Claude y pilote aussi le
graphe ScopeNod ? Utilisez le bouton **« Connecter Claude »** (barre du panneau *Terminal Claude & MCP*).

Il copie dans le presse-papiers une commande de ce type :

```
claude mcp add scopenod -s user -- node "…/packages/mcp-server/dist/index.mjs" "C:/…/mon-projet.scopenod"
```

1. Collez-la **une seule fois** dans n'importe quel terminal.
2. Lancez `claude` : l'outil MCP `scopenod` est disponible **partout** et pilote ce projet précis.

`-s user` enregistre le serveur au niveau de votre compte (scope *user*) : plus besoin de toucher au
moindre fichier de config, et pas de `.mcp.json` à créer. Pour cibler un autre projet plus tard,
recliquez sur « Connecter Claude » depuis ce projet (ou relancez la commande avec le nouveau chemin).

> **Prérequis (build dev actuel).** La commande référence le serveur MCP présent dans le repo ScopeNod.
> Lancez une fois `pnpm setup:claude` à la racine du repo (voir §5) pour construire ce serveur.

---

## 5. Installer le skill

Le skill **`cadrage-project`** apprend à Claude *comment* cadrer un projet ScopeNod : par où commencer,
comment naviguer sans tout charger, le vocabulaire d'écriture. Fortement recommandé pour un travail
efficace sur un graphe.

Une seule commande, à la racine du repo ScopeNod, installe tout (skill **et** serveur MCP) :

```
pnpm setup:claude
```

Elle :

- construit le serveur MCP bundlé (`packages/mcp-server/dist/index.mjs`) ;
- copie le skill dans `~/.claude/skills/` pour qu'il soit disponible **depuis n'importe quel terminal**.

Ensuite, dans une session Claude connectée à un projet, invoquez-le avec `/cadrage-project` — ou
laissez simplement Claude le déclencher quand vous parlez de cadrage.

---

## Tutoriel : un projet de zéro

Objectif : partir d'un fichier vide et obtenir un premier cadrage (modules, fonctionnalités chiffrées,
contexte documenté) piloté par Claude. ~15 min.

### Étape 0 — Préparer (une fois)

À la racine du repo ScopeNod :

```
pnpm setup:claude
```

### Étape 1 — Créer le projet

Ouvrez ScopeNod → **Nouveau projet**. Vous arrivez sur le canvas, vide.

### Étape 2 — Lancer Claude connecté

Panneau latéral droit → **Terminal Claude & MCP** → tapez `claude`.
(Ou, depuis un terminal externe : bouton **Connecter Claude**, collez la commande, puis `claude`.)

### Étape 3 — Donner le brief

Décrivez le produit en une ou deux phrases. Par exemple :

> « Cadre une petite application interne de **réservation de salles** : voir les créneaux, réserver,
> annuler, et une vue admin des salles. Propose les modules, les fonctionnalités chiffrées et le
> contexte. »

Claude va typiquement :

1. appeler `project_summary` (le projet est vide) et `read_comments` (aucune demande en attente) ;
2. vous confirmer l'intention si besoin ;
3. écrire le cadrage en **un lot atomique** `apply_ops` : modules → fonctionnalités (avec `code` et
   `estimate`) → liens `dependsOn` ;
4. ajouter un document *Contexte* ;
5. poser une estimation par fonctionnalité.

### Étape 4 — Voir le résultat dans l'app

Le fichier est réécrit par Claude ; **l'app se recharge à chaud** (quelques secondes). Les nouveaux
modules et fonctionnalités apparaissent au canvas, la doc dans l'onglet Documentation, les totaux dans
le Chiffrage. Réarrangez librement les cartes : les positions vous appartiennent, Claude ne les touche
jamais.

### Étape 5 — Itérer par commentaires

Ajoutez un **commentaire adressé à Claude** sur une page ou un bloc (fil ✳). À la prochaine sollicitation,
Claude lira vos fils via `read_comments`, appliquera les changements, **répondra** puis **résoudra** le fil.
C'est la boucle de travail : vous annotez dans l'app, Claude exécute.

Vous avez un projet cadré de bout en bout. 🎉

---

## 6. Les outils MCP (référence)

Le serveur `scopenod` expose des outils pensés « économie de contexte » — jamais tout le document d'un
coup. Vous n'avez pas à les appeler vous-même ; Claude s'en sert. Bon à connaître pour comprendre ce
qu'il fait :

**Lecture**

| Outil | Rôle |
|---|---|
| `guide` | rappelle le process de travail (Claude le lit au besoin) |
| `project_summary` | inventaire complet, une ligne par entité, avec les poignées courtes |
| `entity_card { handle, content? }` | fiche d'un élément (métadonnées + liens ; `content:true` = contenu riche) |
| `find_entities { query }` | retrouver une entité par nom / code / route |
| `docs_summary` · `doc_card { handle }` | sommaire puis corps d'un document |
| `read_comments` | les fils de commentaires (par défaut : fils ✳ non résolus « pour Claude ») |

**Écriture** — un seul point d'entrée, **atomique** :

| Outil | Rôle |
|---|---|
| `apply_ops { ops: [...] }` | applique un lot d'opérations validé (schéma strict + invariants) ; à la moindre erreur, **rien** n'est écrit |

Vocabulaire d'`ops` (extrait) : `create-module`, `create-feature`, `create-page`, `create-block`,
`create-service`, `link` / `unlink`, `set-content`, `create-doc`, `reply-comment`, `resolve-comment`…
Les cibles se désignent par poignée (id court, code de fonctionnalité, ou nom exact). Aucune coordonnée :
tout est auto-placé, vous réarrangez au canvas.

---

## 7. Commentaires « pour Claude »

Les fils marqués ✳ sont votre **feuille de route pour Claude**. Créez-les dans l'app, ancrés sur la page
ou le bloc concerné (« Ajoute une étape de paiement ici », « Chiffre cette fonctionnalité »). Claude :

1. les lit avec `read_comments` (c'est souvent son vrai point de départ) ;
2. exécute la demande via `apply_ops` ;
3. **répond** au fil (`reply-comment`) puis le **résout** (`resolve-comment`).

Vous gardez ainsi une trace claire de ce qui a été demandé et fait, directement dans le projet.

---

## 8. Dépannage

**« Lecture impossible » / Claude ne voit pas le projet.**
La cible n'est pas définie ou introuvable. En terminal intégré, vérifiez qu'un projet est bien ouvert.
En terminal externe, recliquez sur **Connecter Claude** (le chemin du `.scopenod` doit être correct) et
relancez `claude`.

**Le bouton « Connecter Claude » colle un chemin `dist/index.mjs` qui n'existe pas.**
Lancez `pnpm setup:claude` (ou `pnpm -C packages/mcp-server build`) à la racine du repo pour construire
le serveur MCP.

**Mes modifications Claude n'apparaissent pas dans l'app.**
L'app recharge le fichier automatiquement (~1,5 s de latence). Si rien ne bouge, le lot `apply_ops` a
peut-être échoué : demandez à Claude le message d'erreur (l'écriture est atomique, donc soit tout passe,
soit rien).

**La bannière de mise à jour ne s'affiche pas.**
Normal hors application desktop (elle n'est active que dans la fenêtre native) et tant qu'aucune version
plus récente n'est publiée.

---

<p align="center"><em>ScopeNod — cadrez vite, cadrez juste.</em></p>
