# Backend Coding Standards

Ces normes s’appliquent à l’API Spring Boot (`app-backend/`). Elles visent un code clair, maintenable et aligné sur une culture d’entreprise agile qui déploie quotidiennement.

## Principes directeurs

- **Single Responsibility & Separation of Concerns** : contrôleurs = exposition HTTP, services = logique métier, repositories = accès aux données, DTO = contrat d’échange.
- **DRY** : factoriser les règles partagées (validations, mapping, constantes) dans des classes utilitaires ou des composants Spring dédiés.
- **YAGNI** : ne livrer qu’une fonctionnalité utile + testée; refuser les abstractions prématurées.
- **KISS** : privilégier la solution la plus simple compatible avec les exigences et la scalabilité attendue.
- **Agilité** : micro-itérations + feedback rapide → code structuré pour faciliter la modification et le rollback.

## Organisation du code

```
co.devskills.springbootboilerplate
├── api/            # Contrôleurs REST (pas de logique métier)
├── service/        # Services métiers (stateless si possible)
├── domain/         # Entities, value objects, règles domaine
├── repository/     # Interfaces JpaRepository
├── dto/            # Payloads d’entrée/sortie, mappers
├── config/         # Configuration Spring (CORS, sécurité, frameworks)
└── util/           # Helpers partagés (logging, dates, etc.)
```

- Préfixer les classes par leur rôle (`UserController`, `UserService`, `UserRepository`).
- Alignement package/fichier obligatoire pour garder les imports explicites.

## Contrôleurs (`api`)

- `@RestController` + `@RequestMapping` au niveau classe.
- Exposer uniquement des DTO (jamais les entités persistées).
- Valider les payloads avec `@Valid` et des classes annotées (`javax.validation`).
- Gérer les erreurs via un `@ControllerAdvice` global pour des réponses cohérentes.

## Services (`service`)

- Une classe = un domaine fonctionnel. Exemple : `PaymentService`.
- Injecter les dépendances via constructeur (`@RequiredArgsConstructor` ou Lombok si accepté).
- Pas d’accès direct aux objets Http ou à la couche web.
- Respecter l’idempotence et la transactionnalité (`@Transactional`) si nécessaire.

## Repositories & persistance

- Interfaces `extends JpaRepository<Entity, IdType>`.
- Noms explicites pour les méthodes dérivées (`findByEmailAndActiveTrue`).
- Centraliser les requêtes complexes dans des classes dédiées (Specification, QueryDSL, etc.) si besoin.
- Pas de logique métier dans la couche persistance.

## DTO, mapping, validation

- Créer des `record` ou classes immuables pour les requêtes/réponses.
- Mapper entités ↔ DTO via MapStruct ou services dédiés.
- Validation côté DTO (annotations) + règles supplémentaires dans les services.

## Gestion des erreurs & logging

- Logger au niveau service (INFO pour flux nominal, WARN/ERROR pour anomalies).
- Ne pas logguer d’informations sensibles.
- Utiliser un format cohérent pour les réponses d’erreur (`code`, `message`, `details`).

## Tests

- Chaque service critique possède au moins des tests unitaires.
- Tests d’intégration (`@SpringBootTest`, `@DataJpaTest`) pour les interactions avec la DB.
- Respecter le découplage : les tests de contrôleurs utilisent MockMvc/WebTestClient.

## Qualité & pratiques quotidiennes

- Revues de code systématiques avant merge.
- Feature flags pour les changements risqués.
- Favoriser la documentation légère (JavaDoc sur API publiques, README de module).
- Refactoring continu : détection/élimination des duplications ou dettes techniques dans la même itération.

Ces normes offrent un cadre professionnel sans lourdeur excessive, adaptées à une équipe qui itère vite tout en gardant un code backend propre et facile à maintenir.
