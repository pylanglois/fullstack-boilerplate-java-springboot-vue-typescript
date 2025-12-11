# Copilot Instructions

Utilise ce document comme point d’entrée avant d’écrire du code avec GitHub Copilot. Il renvoie vers les guides structurants du dépôt ; lis-les pour comprendre les conventions avant de générer quoi que ce soit.

## Références clés

- **Structure globale du projet** : `_GUIDES/PROJECT_STRUCTURE.md`
  - Vue d’ensemble du monorepo, des dossiers backend/frontend, de Cypress et des scripts npm communs.
- **Frontend**
  - Structure détaillée : `_GUIDES/FRONTEND_STRUCTURE.md`
  - Normes de code : `_GUIDES/FRONTEND_CODING_STANDARDS.md`
- **Backend**
  - Structure détaillée : `_GUIDES/BACKEND_STRUCTURE.md`
  - Normes de code : `_GUIDES/BACKEND_CODING_STANDARDS.md`

## Comment s’en servir

1. Avant chaque session, rappelle à Copilot la structure cible (frontend ou backend) en pointant vers les fichiers ci-dessus.
2. Pour toute nouvelle fonctionnalité, vérifie si un guide impose un emplacement, un pattern ou un nommage particulier et inclue cette information dans ton prompt.
3. En cas de doute sur l’organisation, commence par `_GUIDES/PROJECT_STRUCTURE.md` pour identifier le module concerné, puis bascule vers le guide spécifique (frontend/backend).

En respectant ces références, Copilot produira un code conforme aux attentes (séparation des responsabilités, DRY/YAGNI/KISS) et parfaitement aligné avec la formation.
