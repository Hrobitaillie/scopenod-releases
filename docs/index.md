---
layout: default
title: Accueil
nav_order: 1
permalink: /
description: "ScopeNod — logiciel de cadrage de projet assisté par IA. Documentation utilisateur : installation, cadrage, développement avec l'agent."
---

# ScopeNod
{: .fs-9 }

Le cadrage de projet, vivant, partagé entre vous et votre agent IA.
{: .fs-6 .fw-300 }

[Télécharger la dernière version](https://github.com/Hrobitaillie/scopenod-releases/releases/latest){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Installation & démarrage](installation.html){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## Qu'est-ce que ScopeNod ?

**ScopeNod** est une application desktop (Windows, macOS expérimental) de **cadrage de projet assisté
par IA**. Elle est *local-first* : pas de compte, pas de serveur — un projet ScopeNod est **un seul
fichier** (extension `.scopenod`, dans l'esprit du fichier unique `.ai` d'Adobe) qui contient tout ce
qui décrit un projet logiciel :

- un **graphe de cadrage** : modules et fonctionnalités (le *quoi*, chiffrable), pages et blocs pour
  l'arborescence écran (le *où*), services externes ;
- de la **documentation** : des pages markdown organisées par catégorie, embarquées dans le fichier ;
- un **chiffrage** : une estimation par fonctionnalité, regroupable en lots ;
- des **commentaires ancrés** : des fils de discussion posés directement sur une page, un bloc, ou même
  un passage de texte précis dans une description — dont des fils adressés explicitement à l'IA.

Ce qui distingue ScopeNod : **Claude (l'agent IA d'Anthropic) travaille directement sur ce graphe**, via
un serveur MCP dédié. Vous décrivez une intention en langage naturel ; Claude explore le cadrage
existant, crée ou modifie des modules, des fonctionnalités chiffrées, des liens et de la documentation —
que vous relisez, commentez et réarrangez ensuite librement dans l'app. Le fichier `.scopenod` est la
**source de vérité partagée** entre l'humain et l'IA, aussi bien en phase de cadrage qu'en phase de
développement.

## À qui ça s'adresse

- **Aux personnes qui cadrent un projet** (product owner, tech lead, freelance, agence) : poser le
  périmètre d'un produit — modules, fonctionnalités, écrans, chiffrage — seul·e ou assisté·e par un
  agent IA, avec un rendu visuel (le canvas) plutôt qu'un document Word qui se désynchronise vite.
- **Aux développeurs et développeuses** qui implémentent un projet déjà cadré, avec ou sans agent IA :
  le graphe donne à tout moment l'état d'une fonctionnalité (bloquée / débloquée / terminée), ses
  dépendances, et la documentation qui s'y rapporte — sans avoir à relire un cahier des charges entier.
- **Aux équipes** qui veulent garder le cadrage vivant après le premier jet : les commentaires ancrés
  et les fils « pour Claude » permettent d'itérer (revue humaine → traitement IA → nouvelle revue) sans
  jamais toucher le fichier à la main.

## Par où commencer

1. **[Installation & démarrage](installation.html)** — installer ScopeNod (Windows aujourd'hui, macOS
   expérimental) et ouvrir votre premier projet.
2. **[Cadrer un projet avec ScopeNod](cadrer-un-projet.html)** — le processus concret : graphe,
   pages/écrans, catalogue, chiffrage, documentation, commentaires.
3. **[Développer avec ScopeNod + l'agent IA](developper-avec-agent.html)** — connecter Claude Code en
   MCP à un projet déjà cadré et développer une fonctionnalité en s'appuyant sur le graphe comme
   contexte.
4. **[Concepts clés](concepts-cles.html)** — le vocabulaire et les invariants du format projet.

{: .note }
> ScopeNod est un logiciel jeune, développé activement. Le [changelog](changelog.html) et la page
> [Releases GitHub](https://github.com/Hrobitaillie/scopenod-releases/releases) listent les
> nouveautés à chaque version ; les mises à jour Windows s'installent automatiquement depuis l'app.
