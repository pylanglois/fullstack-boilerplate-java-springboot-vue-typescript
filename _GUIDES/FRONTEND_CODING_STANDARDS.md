# Frontend Coding Standards

Ces normes s’appliquent à `app-frontend/` (Vue 3 + TypeScript + Vite). Elles visent un code lisible, modulaire, simple à itérer dans un environnement agile.

## Principes directeurs

- **Single Responsibility** : un composant = un rôle; externaliser les fonctions transverses dans des composables/services.
- **DRY** : factoriser les hooks, services d’API, constantes, styles partagés.
- **YAGNI / KISS** : éviter l’over-engineering; implémenter le strict nécessaire avec une structure claire.
- **Lisibilité > astuce** : privilégier la verbosité explicite (noms descriptifs, typage) pour faciliter les revues rapides quotidiennes.

## Organisation recommandée

```
src/
├── components/          # UI réutilisable (boutons, cards, etc.)
├── views/               # Pages complètes (utilisées par le router)
├── composables/         # Hooks réactifs (`useXyz`) partagés
├── services/            # Accès API / logique métier côté client
├── store/               # Pinia ou autre state management (si besoin)
├── styles/              # Styles globaux ou modules CSS
├── router/              # Configuration vue-router
└── assets/              # Images, icônes importées
```

`App.vue` se limite à la mise en page principale (layout + `<router-view>`), `main.ts` instancie les plugins (router, store, i18n…).

## Components & Views

- Utiliser `<script setup lang="ts">` pour les SFC.
- Noms descriptifs en PascalCase (`UserCard`, `DashboardView`).
- Props typées via `defineProps<{ ... }>()`, emits via `defineEmits`.
- Préférer `defineExpose` pour les APIs explicites plutôt que de se reposer sur les refs parentales.
- Ajouter des tests unitaires (Vitest) pour les composants critiques si le temps le permet.

## Composables & services

- Créer des hooks (`src/composables/useFoo.ts`) pour factoriser la logique réactive (state local, watchers, computed). Retourner uniquement les propriétés nécessaires.
- Centraliser les appels réseau dans `src/services/` avec des fonctions typées. Exemple :

```ts
// src/services/userService.ts
import type { User } from '@/types/user';

export async function fetchUsers(): Promise<User[]> {
  const res = await fetch(`${import.meta.env.VITE_API_URL}/users`);
  if (!res.ok) throw new Error('Failed to fetch users');
  return res.json();
}
```

- Gérer les erreurs au plus près du point d’appel, avec un feedback UX clair (notifications, toasts, etc.).

## State management

- Utiliser Pinia seulement si nécessaire (state partagé ou logique complexe). Sinon, privilégier les composables locales.
- Respecter le découplage : un store par domaine avec getters/actions spécialisés.

## Styles & design system

- `src/style.css` contient les styles globaux. Ajoutez des fichiers dédiés (SCSS/PostCSS) dans `src/styles/`.
- Utiliser les `CSS variables` ou un design system pour garder les couleurs/typos cohérentes.
- Préférer `<style scoped>` dans les composants; pour les styles partagés, importez-les explicitement.

## Conventions TypeScript

- Activer/maintenir `strict` et `noImplicitAny`. Typage obligatoire des API, props, retours de composables et services.
- Définir les types dans `src/types/` ou via des `interfaces/records`.
- Préférer les enums `as const`/`type unions` pour représenter les états limités.

## Tests et qualité

- Ajoutez Vitest + Testing Library pour les tests de composant.
- Linting : ESLint + Prettier (si besoin) pour vérifier le style avant commit.
- Revues rapides mais systématiques; privilégier des PRs petites (< 400 lignes) pour maintenir la cadence quotidienne.

## Perf & DX

- Lazy-load des vues via `defineAsyncComponent` ou `router` (`component: () => import('./views/...')`).
- Utiliser `env` Vite (`import.meta.env.VITE_*`) pour configurer les endpoints, feature flags.
- Documenter les conventions dans README/guide pour que Copilot exploite les patterns (noms de dossiers, hooks).

Ces standards offrent un cadre professionnel léger qui maximise la clarté du code front, facilite les revues et soutient un rythme de livraison quotidien.
