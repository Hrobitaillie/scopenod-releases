---
layout: default
title: Installation & démarrage
nav_order: 2
description: "Installer ScopeNod sur Windows ou macOS, ouvrir votre premier projet, activer les mises à jour automatiques."
---

# Installation & démarrage
{: .no_toc }

## Sommaire
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Windows

ScopeNod est distribué en priorité pour Windows, avec mises à jour automatiques.

1. Ouvrez la page [**Releases**](https://github.com/Hrobitaillie/scopenod-releases/releases/latest) de
   ce dépôt.
2. Téléchargez le `…-setup.exe` de la dernière version et lancez-le (installeur NSIS classique).
3. Au premier lancement, ScopeNod se déclare comme éditeur des fichiers `.scopenod` (double-clic sur un
   fichier projet = ouverture directe dans l'app).

{: .tip }
> **Mises à jour automatiques.** ScopeNod embarque un updater signé : dès qu'une nouvelle version est
> publiée, une bannière « Mettre à jour et relancer » apparaît en bas de la fenêtre. Rien à
> retélécharger à la main — un clic suffit.

## macOS (expérimental)

{: .warning }
> Le support macOS est **expérimental**. Il n'y a **pas encore de build signé/notarié publié en
> continu** sur les Releases de ce dépôt, ni de mise à jour automatique sur Mac.

Ce qui existe aujourd'hui :

- Un pipeline de build macOS existe côté projet et produit un `.dmg` **universel** (Apple Silicon +
  Intel), déclenché manuellement — il n'est pas encore publié automatiquement à chaque version comme
  le build Windows.
- Ce build **n'est pas signé ni notarié** par Apple : au premier lancement, macOS (Gatekeeper) affiche
  un avertissement de sécurité (« application non identifiée »).
- Un script `install.command` accompagne le `.dmg` : il copie l'app dans `/Applications` et lève la
  quarantaine Gatekeeper (`xattr -dr com.apple.quarantine`) pour permettre le premier lancement sans
  passer par les réglages système un par un.
- **Aucun auto-update sur Mac pour l'instant** — chaque nouvelle version doit être retéléchargée et
  réinstallée manuellement de la même façon.

Si vous voulez tester ScopeNod sur Mac, ouvrez une
[issue](https://github.com/Hrobitaillie/scopenod-releases/issues/new) sur ce dépôt pour demander le
dernier build disponible — le support macOS complet (signature, notarisation, auto-update) est prévu
mais pas encore livré.

## Premier lancement

Depuis l'écran d'accueil de ScopeNod :

- **Nouveau projet** — crée un fichier `.scopenod` vide, prêt à être cadré (souvent avec l'aide de
  Claude — voir [Cadrer un projet avec ScopeNod](cadrer-un-projet.html)).
- **Ouvrir un fichier…** — ouvre un `.scopenod` existant. Les anciens formats `.cadrage`,
  `.graph.json` et `.flooow.json` restent ouvrables (le fichier est migré automatiquement au format
  courant à l'ouverture, sans rien casser).
- **Glisser-déposer** un fichier projet n'importe où sur l'écran d'accueil.

Les projets créés depuis l'app sont rangés dans un dossier de stockage interne et réapparaissent dans
**Projets récents**. Si vous comptez connecter un agent IA depuis un terminal externe (VS Code, etc.),
notez le **chemin** du fichier — il servira à la connexion (voir
[Développer avec ScopeNod + l'agent IA](developper-avec-agent.html)).

## Étape suivante

Une fois ScopeNod installé et un projet ouvert :

- pour **cadrer** un nouveau projet (avec ou sans agent IA) → [Cadrer un projet avec
  ScopeNod](cadrer-un-projet.html) ;
- pour **développer** une fonctionnalité déjà cadrée avec l'aide de Claude Code → [Développer avec
  ScopeNod + l'agent IA](developper-avec-agent.html).
