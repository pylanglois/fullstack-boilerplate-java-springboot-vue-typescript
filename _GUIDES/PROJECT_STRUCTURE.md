# Project Structure Guide

Ce document présente la structure du boilerplate pour faciliter son utilisation lors des ateliers Copilot. Il se concentre sur l’architecture des dossiers et des fichiers clés plutôt que sur la logique métier (qui est censée varier).

## Vue d’ensemble du dépôt

- `README.md`, `SETUP.md` : guides d’accueil (README pour la vision globale, SETUP pour les prérequis/commandes).
- `package.json` (racine) : scripts orchestrant build/start/test des deux apps; `devDependencies.cypress` supporte les E2E.
- `package-lock.json` et `node_modules/` : install racine (uniquement Cypress).
- `.vscode/` : configuration d’exécution et recommandations d’extensions pour VS Code.
- `.github/` : automatisation (Dependabot + workflow `auto-merge.yml` pouvant servir d’exemple à adapter).
- `.specstory/` : métadonnées internes/outillage narratif (non lié à la prod).
- `_GUIDES/` : espace pour la documentation de formation (ce fichier y réside).
- `cypress.config.js` + dossier `cypress/` : configuration + specs E2E communes BE/FE.
- Sous-applications : `app-backend/` (Spring Boot) et `app-frontend/` (Vue/Vite).

## Backend : `app-backend/`

Structure Gradle standard pour une API Spring Boot.

- `build.gradle`, `settings.gradle`, `gradlew*` : wrapper Gradle 8.6 (src compat Java 21). Le `bootJar` est désactivé pour générer un `jar` classique avec manifeste custom (`SpringBootBoilerplateApplication` en Main-Class).
- `.gradle/`, `bin/`, `build/` (si présent) : sorties générées; à nettoyer si besoin mais utiles pendant les sessions.
- `sqlitestorage.db` : fichier SQLite (persisté localement) référencé par `application.properties`.
- `src/main/java/co/devskills/springbootboilerplate/`
  - `SpringBootBoilerplateApplication.java` : classe de lancement + config CORS globale.
  - `PingController.java` : exemple minimal d’endpoint (`/ping`) pour health-check.
- `src/main/resources/application.properties` : configuration datasource (SQLite + credentials génériques) et Hibernate.

Les stagiaires peuvent ajouter leurs modules sous `src/main/java/...` (contrôleurs, services, etc.) et enrichir les propriétés Spring.

## Frontend : `app-frontend/`

Projet Vue 3 + Typescript propulsé par Vite.

- `package.json` local : scripts `dev` (Vite), `build` (vue-tsc + Vite), `start` (Vite preview sur port 5000).
- `tsconfig.json`, `tsconfig.node.json` : séparation config app vs outils (Vite/Vitest).
- `vite.config.ts` : config minimaliste (import plugin Vue).
- `src/`
  - `main.ts` : bootstrap Vue + montage du composant racine.
  - `App.vue`, `components/HelloWorld.vue`, `style.css`, `assets/` : structure Vite par défaut à personnaliser pour les exercices.
- `public/` : assets statiques (logo Vite).
- `.vscode/extensions.json` (local) : recommandations optionnelles spécifiques au frontend (ESLint + Volar).

L’entraînement peut consister à étendre cette structure (nouvelles routes, stores, etc.) tout en gardant la séparation claire FE/BE.

## Tests & automatisation

- `cypress/`
  - `e2e/test.cy.js` : point d’entrée unique pour les scénarios end-to-end; libre à chacun de dupliquer/renommer selon les besoins.
- `cypress.config.js` : configuration partagée (support file, dossier racine, etc.) pour Cypress 12.
- `package.json` racine → script `npm run test` qui appelle `cypress run`.
- `.github/workflows/auto-merge.yml` : exemple de workflow (peut être remplacé par un pipeline de tests).
- `dependabot.yml` : mise à jour automatique des dépendances GitHub (utile pour démontrer Copilot dans les PR).

## Ressources documentation / formation

- `_GUIDES/` : dossiers ou notes supplémentaires (ajouter ici vos workshops).
- `SETUP.md` : mémo pour installer Java + Node + commandes de base.
- `.specstory/` : fichiers liés aux scripts de démonstration (peuvent être nettoyés ou ignorés selon le contexte).

## Résumé hiérarchique condensé

```
root
├── app-backend/          # API Spring Boot + SQLite
├── app-frontend/         # SPA Vue 3 / Vite / TS
├── cypress/              # E2E tests
├── .vscode/              # IDE configs (launch + extensions)
├── .github/              # CI/CD et dépendances
├── _GUIDES/              # Documentation de formation
├── package.json          # scripts orchestrant build/start/test
├── README.md, SETUP.md   # guides d’accueil
└── cypress.config.js     # configuration globale Cypress
```

Utilise ce guide comme base de discussion pendant la formation, puis adapte ou étends chaque section selon les fonctionnalités abordées avec Copilot.
