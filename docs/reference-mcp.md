---
layout: default
title: Référence des outils MCP
parent: Développer avec ScopeNod + l'agent IA
nav_order: 1
description: "Détail des outils exposés par le serveur MCP scopenod : lecture (project_summary, entity_card, feature_refs…) et écriture (apply_ops)."
---

# Référence des outils MCP
{: .no_toc }

## Sommaire
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Le serveur MCP `scopenod` expose un petit nombre d'outils, pensés pour une **économie de contexte** :
un agent ne charge jamais tout le document d'un coup, il navigue de proche en proche par poignées
courtes. Vous n'avez normalement pas à appeler ces outils vous-même — Claude s'en sert — mais les
connaître aide à comprendre ce que l'agent fait, et pourquoi.

## Lecture

| Outil | Rôle |
|---|---|
| `guide` | rappelle le process de travail recommandé (l'agent le consulte au besoin) |
| `project_summary` | l'inventaire complet du projet ouvert — une ligne par entité, avec sa poignée courte. Aucun contenu. Point d'entrée systématique. |
| `entity_card` | la fiche d'un élément précis (page, bloc, module, fonctionnalité, service), désigné par poignée : métadonnées, liens, et pour une fonctionnalité, son état de déblocage. Un paramètre permet d'inclure aussi le contenu riche (description complète). |
| `find_entities` | recherche par nom, code ou route — renvoie des poignées, jamais de contenu. |
| `docs_summary` | le sommaire de la documentation projet : documents groupés par catégorie. |
| `doc_card` | le markdown brut d'un document entier (poignée = id court ou titre exact). |
| `feature_refs` | ce que la documentation dit d'**une** fonctionnalité précise : cherche son code et son id dans tous les documents, renvoie de courts extraits (fenêtre étroite) autour de chaque occurrence, avec un repère (document + position) pour aller plus loin. |
| `doc_excerpt` | une tranche d'un document à une position donnée — sert à élargir un extrait trouvé par `feature_refs` sans charger le document entier, ou à lire un document par petits blocs successifs. |
| `read_comments` | les fils de commentaires — par défaut, uniquement les fils « pour Claude » non résolus. |
| `read_tests` | les tests manuels (Vue Test) qui attendent un traitement de l'agent : échecs à corriger, ou retours humains sans réponse. |

## Écriture — un seul point d'entrée, atomique

| Outil | Rôle |
|---|---|
| `apply_ops` | applique un **lot d'opérations** en une fois. Le lot est validé (schéma strict + cohérence référentielle) avant d'être écrit sur le disque : si une seule opération du lot est invalide, **rien** n'est appliqué. |

`apply_ops` couvre, entre autres : créer une page/un bloc/un module/une fonctionnalité/un service,
créer ou modifier de la documentation, créer un commentaire ou répondre à un fil et le résoudre, relier
deux entités (dépendance, réalisation, navigation) ou retirer un lien, ajouter/cocher des étapes de
suivi, poser des questions de cadrage, consigner ou mettre à jour un test manuel, renommer le projet.
Les cibles se désignent toujours par **poignée courte** (jamais d'identifiant technique complet), et
aucune opération ne prend de coordonnées : le placement sur le canvas reste entièrement du ressort de
l'humain.

## Présence (curseur d'agent)

Un outil `presence` permet à l'agent d'afficher un **curseur visible dans l'app** pendant qu'il
travaille — utile en particulier quand plusieurs agents interviennent en parallèle sur le même projet
(une équipe d'agents, un par module) : chaque agent peut se donner une identité (nom + couleur) pour
être distingué des autres à l'écran.

## Bon à savoir

- **Format de sortie économe.** La plupart des outils de lecture renvoient des lignes compactes plutôt
  que des blobs JSON, pensées pour être lisibles par l'agent avec un minimum de tokens.
- **Résolution de cible dynamique.** Le serveur relit à chaque appel le pointeur vers « le projet
  actuellement ouvert » — si vous changez de projet dans l'app ou renommez le fichier, l'agent suit,
  sans qu'il soit nécessaire de reconnecter le serveur MCP.
- **Traçabilité.** Chaque appel d'outil est journalisé à côté du fichier projet ; l'app en affiche un
  aperçu (panneau latéral, section MCP) pour que l'humain voie ce que l'agent a fait, en direct.
