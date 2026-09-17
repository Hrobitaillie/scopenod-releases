---
layout: default
title: Développer avec ScopeNod + l'agent IA
nav_order: 4
has_children: true
description: "Connecter Claude Code en MCP à un projet ScopeNod pour développer une fonctionnalité déjà cadrée, en s'appuyant sur le fichier projet comme source de vérité partagée."
---

# Développer avec ScopeNod + l'agent IA
{: .no_toc }

## Sommaire
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Une fois un périmètre cadré (voir [Cadrer un projet avec ScopeNod](cadrer-un-projet.html)), la phase de
développement peut elle aussi s'appuyer sur le fichier projet : c'est la **même source de vérité** qui
sert à l'humain (dans l'app) et à l'agent IA (via MCP). Pas de synchronisation manuelle entre un cahier
des charges et le code — le graphe *est* le cahier des charges, à jour.

## Le principe : un serveur MCP branché sur le fichier ouvert

ScopeNod embarque un **serveur MCP** (Model Context Protocol) nommé `scopenod`. Branché à Claude Code,
il donne à l'agent un accès structuré, en lecture et en écriture, au fichier `.scopenod` actuellement
ouvert dans l'app — jamais en éditant le JSON à la main : chaque lecture et chaque écriture passe par
des outils qui valident le format et ses invariants.

### Connexion

Deux façons de brancher Claude Code sur un projet :

- **Terminal intégré de ScopeNod** — dans le panneau latéral droit, la section « Terminal Claude &
  MCP » lance un terminal **déjà câblé** sur le projet ouvert (la variable d'environnement qui cible le
  fichier est posée automatiquement). Il suffit d'y taper `claude`.
- **N'importe quel autre terminal** (VS Code, terminal système…) — le bouton **« Connecter Claude »** du
  même panneau copie dans le presse-papiers une commande `claude mcp add scopenod -s user -- …` prête à
  coller, qui enregistre le serveur MCP pour votre compte Claude Code (scope *user* — pas de fichier de
  config à créer à la main) et le fait pointer sur le chemin exact de votre fichier projet. Collez-la
  une fois, relancez `claude`, et l'outil `scopenod` est disponible dans n'importe quel terminal.

Pour cibler un autre projet plus tard, recliquez sur « Connecter Claude » depuis ce nouveau projet, ou
relancez la commande avec le nouveau chemin.

## Le flux de travail type

Un agent connecté au projet suit typiquement cette boucle avant d'écrire du code :

1. **S'orienter** — un appel d'inventaire donne la liste de toutes les entités du projet (pages,
   blocs, modules, fonctionnalités, services, documents), une ligne chacune, avec une **poignée
   courte** pour la désigner ensuite (id court, code de fonctionnalité, ou nom). Aucun contenu chargé à
   ce stade : le but est de ne **jamais avaler le fichier entier**, même sur un gros projet.
2. **Lire les demandes en attente** — les fils de commentaires marqués « pour Claude » sont la feuille
   de route explicite laissée par l'humain.
3. **Se renseigner sur une fonctionnalité précise avant de coder** — pour une fonctionnalité donnée
   (désignée par son code, ex. `AUTH-02`), l'agent récupère sa description complète, ses dépendances et
   surtout son **état de déblocage** :
   - 🔒 **bloquée** — un prérequis n'a pas encore atteint le seuil « en développement » : l'agent
     traite d'abord ce prérequis, ou le signale ;
   - **débloquée** — prête à développer ;
   - **terminée** — déjà recettée/livrée.
4. **Retrouver ce que la documentation dit de cette fonctionnalité** — plutôt que de charger des
   documents entiers, l'agent recherche les occurrences du **code** ou de l'**id** de la fonctionnalité
   dans toute la documentation du projet, et ne récupère que de courts extraits autour de chaque
   occurrence (fenêtre étroite, optimisée en tokens). Si un extrait est trop court, il peut l'élargir en
   redemandant une tranche plus large **du même document, au même endroit** — sans jamais recharger le
   document entier tant que ce n'est pas nécessaire.
5. **Développer**, en tenant le fichier projet à jour : cocher les étapes de suivi (phase `dev`) au fur
   et à mesure, faire progresser le statut de la fonctionnalité.
6. **Ajouter les tests manuels à faire** — après un développement, l'agent consigne dans le projet les
   tests que l'humain doit valider (instructions précises : étapes, résultat attendu, cas d'erreur),
   dans la **Vue Test** de l'app. L'humain valide ou renvoie un commentaire d'ajustement ; les échecs
   repartent vers l'agent.

Le détail des outils MCP disponibles (lecture, écriture, tests) est sur la page
[Référence des outils MCP](reference-mcp.html).

## Pourquoi le fichier unique fonctionne comme source de vérité partagée

Trois propriétés du format rendent ce flux fiable :

- **Écriture atomique et validée.** Toute modification passe par un seul point d'entrée qui applique un
  **lot d'opérations** (créer une fonctionnalité, la relier, documenter, répondre à un commentaire…) de
  façon *tout ou rien* : le lot est validé (schéma strict + cohérence référentielle) avant d'être écrit
  sur le disque. À la moindre erreur, rien n'est appliqué — pas d'état intermédiaire corrompu.
- **Verrou d'écriture côté serveur.** Plusieurs agents peuvent travailler en parallèle sur le même
  fichier (une équipe d'agents, un par module) sans se marcher dessus : les écritures concurrentes sont
  sérialisées automatiquement, sans perte.
- **Rechargement à chaud.** Dès qu'un agent écrit dans le fichier, l'app ScopeNod ouverte le recharge
  en quelques secondes : l'humain voit apparaître les changements sur le canvas en direct, sans rouvrir
  le projet.

Concrètement, cela veut dire que le graphe que vous voyez dans l'app **est** le contexte que l'agent
utilise pour développer — et inversement, ce que l'agent développe se reflète immédiatement dans ce que
vous voyez. Il n'y a qu'une seule vérité, jamais deux documents à recroiser.

## Prochaine étape

- [Référence des outils MCP](reference-mcp.html) — le détail de chaque outil exposé par le serveur
  `scopenod`.
- [Concepts clés](concepts-cles.html) — pour comprendre le format de fichier et son versioning.
