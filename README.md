# ScopeNod — distribution & documentation

**ScopeNod** est un logiciel de **cadrage de projet assisté par IA** (application desktop, *local-first*).
Ce dépôt public héberge deux choses :

- 📥 les **artefacts de mise à jour** (updater Tauri) : `…-setup.exe` signés + `latest.json`, attachés aux
  [**Releases**](https://github.com/Hrobitaillie/scopenod-releases/releases) — générés par `pnpm release`,
  à ne pas éditer à la main ;
- 📖 la **documentation utilisateur**, publiée via GitHub Pages.

## 📖 Documentation

👉 **[Lire le guide utilisateur](https://hrobitaillie.github.io/scopenod-releases/)**

Site multi-pages avec sommaire, recherche intégrée et navigation par catégories : installation
(Windows + macOS expérimental), comment cadrer un projet (graphe, écrans, chiffrage, documentation,
commentaires), comment développer une fonctionnalité déjà cadrée avec Claude Code connecté en MCP, les
concepts clés du format, et une FAQ.

## 📥 Installer

1. Ouvrez la [dernière release](https://github.com/Hrobitaillie/scopenod-releases/releases/latest).
2. Téléchargez le `…-setup.exe` et lancez-le.
3. Les mises à jour suivantes s'installent automatiquement depuis l'app.

## À propos

Le code source de ScopeNod est privé ; seuls les artefacts distribués et la documentation sont publics.
La doc est éditable dans le dossier [`docs/`](docs/) — site Jekyll multi-pages basé sur le thème
distant [Just the Docs](https://just-the-docs.com/) (`remote_theme`, sans pipeline de build local
requis), publié par GitHub Pages depuis `main` / `/docs`.
