# Frontend Structure & Guidance

Ce guide décrit la structure de `app-frontend/` pour que les agents Copilot (ou autres) comprennent où et comment intervenir. Il ne couvre pas les spécificités métiers, uniquement l’ossature technique.

## Pile technologique

- **Framework** : Vue 3 (`<script setup>`) avec TypeScript strict.
- **Bundler/dev-server** : Vite 7.
- **Types** : `vue-tsc` remplace `tsc` pour la vérification.
- **Gestion des dépendances** : npm (voir `app-frontend/package.json`).

> Script principaux :
> - `npm run dev` : Vite en mode hot reload.
> - `npm run build` : type-check (`vue-tsc`) puis build Vite.
> - `npm run start` : `vite preview` (serveur statique port 5000).

## Arborescence clé

```
app-frontend/
├── index.html           # Point d'entrée Vite (injection du bundle).
├── package.json         # Scripts et dépendances locales.
├── tsconfig*.json       # Config TypeScript (app + outils).
├── vite.config.ts       # Config Vite (plugin Vue, personnaliser ici).
└── src/
    ├── main.ts          # Bootstrap Vue; monte l’app sur #app.
    ├── App.vue          # Composition racine (placeholder Vite).
    ├── style.css        # Styles globaux importés depuis main.ts.
    ├── components/      # Composants SFC réutilisables (HelloWorld.vue).
    └── assets/          # Assets importés via bundler (ex : logos).
```

Les assets statiques non transformés vont dans `public/` (servis tel quel depuis /).

## Chaîne de rendu

1. `index.html` définit le `<div id="app">`.
2. `src/main.ts` importe `createApp`, applique `style.css`, monte `App.vue`.
3. `App.vue` orchestre les composants enfants (`<HelloWorld>` pour l’instant).
4. Chaque composant suit le pattern `<script setup lang="ts">` avec `defineProps`, `ref`, etc. Ajoutez vos modules ici (pages, layouts, etc.).

Pour créer de nouvelles vues/pages, ajoutez-les sous `src/components/` ou créez des sous-dossiers (`src/views/`, `src/modules/...`) selon votre convention, puis référencez-les dans `App.vue` ou via un router (que vous ajouterez en dépendance).

## Typage & config

- `tsconfig.json` active `strict`, `noEmit`, `resolveJsonModule`, etc. Toute nouvelle logique doit se conformer aux règles TypeScript strictes.
- `tsconfig.node.json` est référencé pour les outils (ex : Vite, Vitest). Ajoutez les fichiers de config supplémentaires ici si vous créez des scripts Node.
- `vite-env.d.ts` (généré par Vite) expose les types globaux; étendez-le si vous ajoutez des variables d’environnement personnalisées (`import.meta.env`).
- Ajoutez des alias ou plugins via `vite.config.ts`. Exemple : pour des imports absolus, ajoutez `resolve.alias`.

## Styles

- `src/style.css` contient les styles globaux (reset, layout). Il est importé une seule fois dans `main.ts`.
- Chaque composant peut définir des styles `<style scoped>` pour limiter la portée.
- Pour un design system, créez des fichiers CSS ou PostCSS supplémentaires et importez-les où nécessaire; Vite gère SASS/LESS si les paquets correspondants sont installés.

## Communication avec le backend

- Le backend Spring Boot répond sur `http://localhost:8080` (par défaut). CORS est déjà ouvert côté API (`SpringBootBoilerplateApplication.java`), donc le frontend peut appeler les endpoints directement depuis Vite.
- Créez un dossier `src/services/` ou `src/api/` pour centraliser les appels (via `fetch` ou `axios`). Typiquement :

```ts
// src/services/ping.ts
export async function ping(): Promise<string> {
  const res = await fetch('http://localhost:8080/ping');
  if (!res.ok) throw new Error('Ping failed');
  return res.text();
}
```

- Placez les URL répétées (ex : `API_BASE_URL`) dans `import.meta.env` (définies dans un fichier `.env` via Vite) pour faciliter la configuration durant la formation.

## Tests et intégration

- Le front n’a pas de tests unitaires par défaut. Pour en ajouter, installez `vitest`/`@testing-library/vue` et créez `src/__tests__/`.
- Les tests E2E existants (Cypress) vivent à la racine du repo et peuvent cibler l’application front lorsqu’elle tourne (`npm run dev` ou `npm run start`).

## Conseils pour l’agent

1. **Conserver la modularité** : créez un dossier par fonctionnalité (components + composables + styles).
2. **Script setup** : utilisez `defineProps`, `defineEmits`, `defineExpose`. Typage explicite pour guider Copilot.
3. **Réactivité** : préférez `ref`/`reactive` et `computed` du package `vue`.
4. **Interop avec backend** : créez des wrappers fetch typés; évitez de parseur `any`.
5. **Étendre la configuration** : pour les apps plus grandes, ajoutez `vue-router`, `pinia`, etc. via `package.json` puis configurez dans `main.ts`.
6. **Déploiement** : `npm run build` crée le dossier `dist/`; servez-le via un reverse proxy ou via Spring (copie dans `app-backend/src/main/resources/static` si désiré).

En suivant ce document, l’agent sait où placer les nouveaux composants, comment configurer l’outillage et quelles conventions respecter avant d’inventer des fonctionnalités spécifiques à la formation.
