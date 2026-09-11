# ScopeNod — distribution & documentation

**ScopeNod** est un logiciel de **cadrage de projet assisté par IA** (application desktop, *local-first*).
Ce dépôt public héberge deux choses :

- 📥 les **artefacts de mise à jour** (updater Tauri) : `…-setup.exe` signés + `latest.json`, attachés aux
  [**Releases**](https://github.com/Hrobitaillie/pbones-updates/releases) — générés par `pnpm release`,
  à ne pas éditer à la main ;
- 📖 la **documentation utilisateur**, publiée via GitHub Pages.

## 📖 Documentation

👉 **[Lire le guide utilisateur](https://hrobitaillie.github.io/pbones-updates/)**

On y trouve : installation, connexion de Claude à un projet (terminal intégré ou n'importe quel terminal
via le bouton « Connecter Claude »), installation du skill, et un **tutoriel « projet de zéro »** pas à pas.

## 📥 Installer

1. Ouvrez la [dernière release](https://github.com/Hrobitaillie/pbones-updates/releases/latest).
2. Téléchargez le `…-setup.exe` et lancez-le.
3. Les mises à jour suivantes s'installent automatiquement depuis l'app.

## À propos

Le code source de ScopeNod est privé ; seuls les artefacts distribués et la documentation sont publics.
La doc est éditable dans le dossier [`docs/`](docs/) (Markdown, thème Jekyll — GitHub Pages depuis
`main` / `/docs`).
