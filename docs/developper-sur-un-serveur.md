---
layout: default
title: Développer directement sur un serveur
parent: Développer avec ScopeNod + l'agent IA
nav_order: 2
description: "Installer un serveur MCP scopenod autonome (aucun Node.js requis) sur votre propre serveur, pour développer avec Claude Code en SSH directement là où vit le fichier projet."
---

# Développer directement sur un serveur
{: .no_toc }

## Sommaire
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Le [flux habituel](developper-avec-agent.html) suppose que l'agent tourne **sur votre poste**, à côté de
l'app ScopeNod ouverte. Si vous développez plutôt **directement sur un serveur** — terminal SSH, VS Code
Remote-SSH, un conteneur distant — Claude y tourne physiquement, hors de portée de l'app locale : il lui
faut son propre petit serveur MCP, installé sur **cette machine**, pointé sur un fichier `.scopenod` qui y
vit lui aussi.

## Ce que c'est — et ce que ce n'est pas

{: .important }
> Ce serveur MCP autonome n'est **jamais hébergé par ScopeNod**. C'est un exécutable que vous téléchargez
> et faites tourner sur **votre propre** serveur, avec vos propres identifiants. Le contenu de votre
> projet ne transite jamais par une infrastructure ScopeNod.

C'est un **exécutable compilé** — aucun Node.js à installer sur le serveur cible — une variante « Single
Executable Application » du même serveur MCP `scopenod` que l'app desktop embarque.

## Installation (Linux x86_64)

1. Sur le serveur, téléchargez le binaire de la dernière version et rendez-le exécutable :

   ```bash
   curl -fsSL https://github.com/Hrobitaillie/scopenod-releases/releases/latest/download/scopenod-mcp-linux-x64 \
     -o /usr/local/bin/scopenod-mcp
   chmod +x /usr/local/bin/scopenod-mcp
   ```

2. Enregistrez-le auprès de Claude Code, en pointant sur le chemin **exact** de votre fichier `.scopenod`
   sur ce serveur :

   ```bash
   claude mcp add scopenod -s user -- /usr/local/bin/scopenod-mcp /chemin/vers/votre-projet.scopenod
   ```

3. Lancez `claude` : l'outil `scopenod` est connecté directement à ce fichier, sans passer par
   `list_projects`/`select_project` (ces outils listent aussi les projets du dossier de l'app desktop —
   un serveur nu n'en a pas). Au premier lancement, le binaire installe aussi les **skills ScopeNod**
   (`~/.claude/skills` et `~/.agents/skills`) — le même guide de cadrage que l'app desktop pose sur votre
   poste, rien à faire de plus.

{: .tip }
> Pour développer sur **plusieurs** projets du même serveur, répétez l'étape 2 avec un chemin différent :
> chaque enregistrement cible un fichier précis.

## D'où vient le fichier `.scopenod` sur le serveur ?

Ce binaire ne fait qu'ouvrir un fichier déjà présent — il n'en crée pas. Le plus simple : envoyez-le
depuis votre poste (`scp mon-projet.scopenod utilisateur@serveur:/chemin/`), ou créez-le directement là
si vous démarrez un projet neuf depuis ce serveur.

## Limite connue : pas de verrou entre les deux mondes

{: .warning }
> Rien n'empêche aujourd'hui d'ouvrir le **même** fichier depuis l'app desktop (sur votre poste) et depuis
> ce serveur (via ce binaire) en même temps. Le dernier qui enregistre écrase l'autre, sans avertissement.
> Tant que ce garde-fou n'existe pas, évitez d'éditer le même projet des deux côtés à la fois.

## Prochaine étape

[Référence des outils MCP](reference-mcp.html) — le détail de chaque outil exposé par le serveur
`scopenod`, valable identiquement que vous soyez connecté en local ou via ce binaire autonome.
