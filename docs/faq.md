---
layout: default
title: FAQ & dépannage
nav_order: 6
description: "Questions fréquentes et dépannage : connexion de l'agent IA, changements qui n'apparaissent pas, mises à jour, signalement de bug."
---

# FAQ & dépannage
{: .no_toc }

## Sommaire
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## « Lecture impossible » — Claude ne voit pas le projet

La cible du serveur MCP n'est pas définie, ou le fichier est introuvable.

- Depuis le **terminal intégré** de ScopeNod : vérifiez qu'un projet est bien ouvert dans l'app avant
  de lancer `claude`.
- Depuis un **terminal externe** : recliquez sur **Connecter Claude** dans le panneau latéral (le
  chemin du fichier `.scopenod` copié doit être correct), collez la commande, puis relancez `claude`.

## Mes modifications faites par l'agent n'apparaissent pas dans l'app

L'app recharge le fichier automatiquement après chaque écriture (quelques secondes de latence). Si rien
ne bouge après un délai raisonnable :

- le lot d'opérations a peut-être **échoué en entier** — l'écriture est atomique (tout ou rien), donc
  soit tout passe, soit rien n'est écrit. Demandez à l'agent le message d'erreur renvoyé par l'outil
  d'écriture ;
- vérifiez que le terminal utilisé est bien connecté au **bon** fichier projet (voir la section
  précédente).

## La bannière de mise à jour ne s'affiche pas

C'est normal en dehors de l'application desktop (elle n'existe que dans la fenêtre native, pas dans un
navigateur), et tant qu'aucune version plus récente que la vôtre n'a été publiée. Voir
[Installation & démarrage](installation.html) pour le détail des mises à jour automatiques (Windows) et
des limites actuelles sur macOS.

## Je suis sur un vieux fichier `.cadrage` ou `.flooow.json`, est-ce grave ?

Non. Ces anciennes extensions restent ouvrables : le fichier est **migré automatiquement** au format
courant à l'ouverture, sans rien casser (voir [Concepts clés — format et
versioning](concepts-cles.html#format-de-fichier-et-versioning)).

## Un agent IA peut-il chiffrer le projet à ma place ?

Non, volontairement. L'estimation d'une fonctionnalité est considérée comme un **geste humain** — un
agent IA ne chiffre pas de façon fiable. Les fonctionnalités créées par l'IA restent donc « à estimer »
jusqu'à ce qu'une personne pose la valeur dans l'app (voir [Cadrer un projet — Chiffrage](cadrer-un-projet.html#chiffrage)).

## Comment signaler un bug ou demander une fonctionnalité ?

Le code source de ScopeNod est privé, mais le suivi des retours se fait publiquement sur ce dépôt :
ouvrez une [issue](https://github.com/Hrobitaillie/scopenod-releases/issues/new) — vous n'avez pas
besoin d'accès au code pour cela.

## Où sont documentées les nouveautés de chaque version ?

Sur la page [Changelog](changelog.html), qui renvoie vers les [Releases
GitHub](https://github.com/Hrobitaillie/scopenod-releases/releases) de ce dépôt.
