---
layout: default
title: Concepts clés
nav_order: 5
description: "Le vocabulaire de ScopeNod : fichier projet unique, graphe structurel vs fonctionnel, format et versioning, commentaires ancrés."
---

# Concepts clés
{: .no_toc }

## Sommaire
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Le fichier projet unique

Un projet ScopeNod est **un seul fichier** (extension `.scopenod` ; les anciennes extensions
`.cadrage`, `.graph.json` et `.flooow.json` restent ouvrables). Ce fichier contient tout : le graphe de
cadrage, la documentation, le chiffrage et les commentaires. Il n'y a pas de base de données externe,
pas de compte à créer, pas de synchronisation cloud requise — l'application est *local-first*.

Cette approche « fichier unique » a une conséquence directe sur la façon de collaborer avec un agent
IA : puisque tout l'état du projet tient dans un seul document versionné, un agent peut le lire et
l'écrire de façon fiable via un protocole structuré (MCP), sans dépendre d'une API serveur maison.

## Graphe structurel vs graphe fonctionnel

ScopeNod distingue explicitement deux dimensions d'un projet, reliées mais différentes :

- Le **graphe structurel** (*le où*) : pages et blocs, l'arborescence des écrans de l'application
  cadrée, reliés entre eux par des liens de navigation.
- Le **graphe fonctionnel** (*le quoi*) : modules et fonctionnalités, l'unité de travail chiffrable et
  documentable, reliées entre elles par des dépendances.

Le lien `realizedBy` fait le pont entre les deux : une fonctionnalité est *réalisée par* une page ou un
bloc précis. Cette séparation permet de raisonner soit en termes de **produit** (quelles fonctionnalités
existent, dans quel ordre les développer), soit en termes d'**écrans** (quel parcours utilisateur, quelle
navigation) — et de naviguer de l'un à l'autre.

## Cycle de vie d'une fonctionnalité

Une fonctionnalité progresse le long d'un statut explicite, qui pilote à la fois l'affichage dans l'app
et l'état de déblocage vu par l'agent IA en phase de développement :

```
idée → à qualifier → cadrée → estimée → retenue → à développer → en développement → en recette → validée → en production
```

Deux statuts de sortie de route existent en dehors de cette progression linéaire : **reportée** et
**écartée** (une fonctionnalité qui sort du périmètre retenu, sans être supprimée du graphe).

Chaque fonctionnalité porte aussi des **étapes de suivi** cochables, réparties en deux phases
distinctes — `cadrage` (specs, maquettes, choix techniques, validations) et `dev` (endpoints, écrans,
tests) — qui constituent la feuille de route concrète pour la phase suivante, humaine ou IA.

## Format de fichier et versioning

Le format interne du fichier projet est **versionné** (`meta.formatVersion`). Chaque évolution du
modèle de données s'accompagne d'une **migration automatique** : ouvrir un ancien projet dans une
nouvelle version de ScopeNod le met à jour à la volée, sans intervention manuelle et sans perte de
données. C'est ce qui permet au format de continuer à évoluer (nouveaux types de liens, nouveaux
champs, nouvelles fonctionnalités de l'app) sans jamais casser les projets existants.

En pratique, cela signifie aussi que vous n'avez **jamais à éditer le fichier `.scopenod` à la
main** : toute modification — humaine (dans l'app) ou par un agent IA (via MCP) — passe par des
opérations validées qui connaissent le format courant et ses règles de cohérence (par exemple :
impossible de créer un lien vers une entité qui n'existe pas, ou de supprimer un module qui contient
encore des fonctionnalités sans le signaler explicitement).

## Commentaires ancrés

Les commentaires ne sont pas une liste à part : ils sont **ancrés** sur un élément précis du graphe —
une page, un bloc, ou même une sélection de texte à l'intérieur de la description d'une fonctionnalité
(façon Figma, avec une bulle qui suit la carte au pan/zoom). Un fil peut être marqué **« pour
Claude »** : c'est alors une demande explicite adressée à l'agent IA, que celui-ci retrouve en priorité
en début de session et traite en répondant puis en résolvant le fil.

Cette mécanique fait des commentaires le **canal d'itération** entre l'humain et l'IA : plutôt que de
réexpliquer le contexte à chaque conversation, on annote directement l'endroit concerné du projet, et
l'agent dispose de tout le contexte environnant (le nœud ciblé, ses liens, sa documentation) pour agir
de façon pertinente.

## Attributs personnalisés

Au-delà des champs standards (code, estimation, statut…), un projet peut définir des **familles
d'attributs personnalisés** — par exemple une propriété « Phase de production » à choix multiple —
applicables aux fonctionnalités, aux pages ou aux blocs. C'est un mécanisme d'extension du modèle sans
toucher au format lui-même : utile pour des classifications propres à un projet ou une équipe
(priorité, propriétaire, canal de distribution…), configurables directement dans l'app.

## Voir aussi

- [Cadrer un projet avec ScopeNod](cadrer-un-projet.html) — le processus concret qui s'appuie sur ces
  concepts.
- [Développer avec ScopeNod + l'agent IA](developper-avec-agent.html) — comment l'agent IA lit et écrit
  dans ce format via MCP.
