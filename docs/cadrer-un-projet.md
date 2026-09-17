---
layout: default
title: Cadrer un projet avec ScopeNod
nav_order: 3
description: "Le processus concret de cadrage dans ScopeNod : graphe de modules et fonctionnalités, arborescence écran, chiffrage, documentation, commentaires."
---

# Cadrer un projet avec ScopeNod
{: .no_toc }

## Sommaire
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Cadrer un projet dans ScopeNod, c'est construire progressivement un **graphe** — visible et manipulable
sur un canvas — plutôt que rédiger un document qui se désynchronise du produit réel. Vous pouvez le
faire entièrement à la main dans l'app, entièrement via un agent IA connecté en MCP, ou (le cas le plus
courant) les deux en alternance : l'IA pose un premier jet, vous réarrangez et commentez, l'IA retraite
vos commentaires.

## Les quatre couches d'un projet

Un fichier `.scopenod` réunit quatre couches :

| Couche | Contenu | L'atome |
|---|---|---|
| **Graphe structurel** | pages & blocs — l'arborescence écran, le *où* | la **page** / le **bloc** |
| **Graphe fonctionnel** | modules & fonctionnalités, services externes — le *quoi* | la **fonctionnalité** |
| **Documentation** | pages markdown classées par catégorie (Contexte, Specs, Technique…) | le **document** |
| **Commentaires** | fils ancrés sur une page, un bloc, ou un passage de texte précis | le **fil** |

Le chiffrage n'est pas une couche séparée : c'est un **attribut de la fonctionnalité** (voir plus bas).

## Le graphe fonctionnel : modules et fonctionnalités

La **fonctionnalité** est l'atome du cadrage — l'unité chiffrable, celle sur laquelle tout le reste
s'accroche. Chaque fonctionnalité porte :

- un **code** court (ex. `AUTH-01`, `RESA-02`) qui sert de poignée stable dans tout le projet — y
  compris pour l'agent IA en phase de développement (voir plus bas) ;
- un **nom** ;
- une **estimation** (voir [Chiffrage](#chiffrage)) ;
- une **description riche** : objectif, règles métier, cas limites, critères d'acceptation — pas
  seulement un titre ;
- des **étapes de suivi** cochables, réparties en deux phases : `cadrage` (specs à rédiger, maquettes,
  choix techniques, points de sécurité/UX/accessibilité identifiés) et `dev` (endpoints, écrans,
  tests) — la feuille de route que la phase suivante exécutera ;
- un **statut de cycle de vie**, qui progresse typiquement ainsi : *idée → à qualifier → cadrée →
  estimée → retenue* (ou *reportée* / *écartée* si elle sort du périmètre) *→ à développer → en
  développement → en recette → validée → en production* ;
- éventuellement des **questions de cadrage** ouvertes : quand il reste une vraie décision à trancher
  par un humain (choix produit, arbitrage), la fonctionnalité peut porter une liste de questions —
  affichées dans un encart orange « ❓ À cadrer », repérable même en dézoom total sur le canvas.

Les **modules** regroupent les fonctionnalités par domaine fonctionnel (ex. « Authentification »,
« Paiement », « Catalogue »). Les fonctionnalités se relient entre elles par des dépendances
(`dependsOn`) : la carte d'une fonctionnalité affiche alors un **état de déblocage** — bloquée (en
attente d'un prérequis), débloquée, ou terminée — utile aussi bien à un humain qu'à un agent IA qui
s'apprête à développer.

## Le graphe structurel : pages et blocs

Les **pages** et les **blocs** décrivent l'arborescence écran de l'application cadrée — le *où* : quels
écrans existent, comment on navigue de l'un à l'autre (lien `navigatesTo`, page → page), et quel
contenu (blocs) compose chaque page.

Le lien qui fait tenir les deux graphes ensemble est `realizedBy` : une fonctionnalité (le *quoi*) est
**réalisée par** une page ou un bloc (le *où*). Une fonctionnalité de connexion (`AUTH-01`) peut ainsi
pointer vers l'écran de connexion qui l'incarne ; en cliquant sur l'écran, on retrouve les
fonctionnalités qui s'y jouent, et inversement.

Les **services externes** (API tierces, SMTP, paiement…) sont un troisième type de nœud : on y note
l'URL de base, l'authentification, le niveau de risque, et on relie les blocs qui les appellent
(référence d'API : méthode + chemin).

## Catalogue

La vue **Catalogue** projette le graphe fonctionnel en une liste consultable et filtrable de toutes les
fonctionnalités du projet — par code, module, statut, lot — utile pour une revue d'ensemble sans
naviguer le canvas nœud par nœud.

## Chiffrage

Chaque fonctionnalité porte un champ **estimation**, exprimé en jours (convention héritée : 1 jour =
7 heures). Les fonctionnalités peuvent être groupées en **lots** numérotés (par exemple 1, 2, 3…, que
vous nommez comme vous voulez — « Prototype », « V1 », « V2 »…) ; un bloc ou une note héritent
automatiquement du lot de leur parent si aucune valeur n'est posée explicitement dessus, ce qui permet
de faire basculer tout un pan du graphe d'un lot à l'autre en un geste.

{: .important }
> **L'estimation est un geste humain.** Un agent IA peut cadrer, documenter et relier des
> fonctionnalités, mais il **ne doit pas** inventer de chiffrage — l'estimation fiable reste une
> décision humaine. Une fonctionnalité fraîchement créée par l'IA affiche donc « à estimer » tant que
> vous n'avez pas posé de valeur.

## Documentation

Au-delà de la carte d'une fonctionnalité (qui porte déjà l'essentiel : objectif, règles, cas limites,
critères d'acceptation), le projet embarque de vraies **pages de documentation markdown**, classées par
catégorie (Contexte, Specs, Technique…). Une bonne documentation de fonctionnalité va plus loin que la
carte : parcours utilisateur de bout en bout, séquence technique, interactions avec d'autres
fonctionnalités, schéma de données, contrats d'API, décisions et leur justification, scénarios concrets
(nominal + erreurs) — et **cite le code** de la ou des fonctionnalités concernées, ce qui permet à un
agent IA de la retrouver précisément en phase de développement (voir [Développer avec ScopeNod +
l'agent IA](developper-avec-agent.html)).

## Commentaires ancrés

Les commentaires sont le mécanisme d'itération du cadrage : vous annotez, l'IA (ou un⋅e collègue)
traite. Un fil de commentaire peut être ancré :

- sur une **page** ou un **bloc** entier ;
- ou, plus précisément, sur une **sélection de texte** dans la description d'une fonctionnalité (à la
  Figma) : une bulle ronde se pose exactement à l'endroit sélectionné et suit la carte au pan/zoom.

Un fil peut être marqué **« pour Claude »** (fil ✳) : c'est une demande explicitement adressée à
l'agent IA. Le cycle habituel est : vous posez un commentaire ancré → l'agent le lit, effectue les
changements demandés dans le graphe, **répond** dans le fil, puis le **résout**. Vous gardez ainsi, à
même le projet, une trace de ce qui a été demandé et de ce qui a été fait — sans jamais avoir besoin
d'éditer le fichier à la main.

## Cadrer avec l'aide de l'agent IA

Rien n'empêche de cadrer entièrement à la main dans l'app. Mais le flux le plus rapide, pour un premier
jet ou une extension de périmètre, est de décrire l'intention en langage naturel à Claude Code connecté
au projet (voir [Développer avec ScopeNod + l'agent IA](developper-avec-agent.html) pour la
connexion) : l'agent explore l'existant, ne cadre jamais une fonctionnalité isolément — il déplie la
cascade d'implications (sécurité, UX, cas limites, écrans, accessibilité…) — puis écrit le tout en un
lot atomique (tout ou rien) directement dans le fichier projet. L'app se recharge à chaud : vous voyez
le résultat apparaître sur le canvas en quelques secondes, et vous réarrangez librement — les positions
des cartes vous appartiennent, l'agent ne les touche jamais.

## Prochaine étape

Une fois un premier périmètre cadré (quelques modules, des fonctionnalités reliées, le contexte
documenté), place au développement : [Développer avec ScopeNod + l'agent IA](developper-avec-agent.html).
