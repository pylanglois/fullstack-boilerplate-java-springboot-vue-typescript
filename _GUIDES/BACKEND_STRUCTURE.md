# Backend Structure & Guidance

Ce guide aide les agents Copilot à comprendre la base Spring Boot (`app-backend/`) et à savoir où ajouter leurs propres fonctionnalités pendant la formation.

## Stack & build

- **Langage** : Java 21.
- **Framework** : Spring Boot (starter web + data-jpa).
- **Persistance** : SQLite via `org.xerial:sqlite-jdbc`.
- **Build Tool** : Gradle 8.6 (wrapper inclus `gradlew`, `gradlew.bat`).

`build.gradle` désactive `bootJar` et active un `jar` classique avec `Main-Class: co.devskills.springbootboilerplate.SpringBootBoilerplateApplication`. Les scripts racine (`npm run build:backend`, `npm run start:backend`) appellent respectivement `./gradlew build` et exécutent le JAR généré.

Tâches utiles :

```bash
./gradlew bootRun        # démarrage + rechargement à chaud (optionnel)
./gradlew test           # JUnit
./gradlew build          # compile + tests + assemble le jar
```

## Arborescence

```
app-backend/
├── build.gradle
├── settings.gradle
├── gradle/              # wrapper files
├── gradlew / gradlew.bat
├── src/
│   └── main/
│       ├── java/co/devskills/springbootboilerplate/
│       │   ├── SpringBootBoilerplateApplication.java
│       │   └── PingController.java
│       └── resources/
│           └── application.properties
└── sqlitestorage.db     # base SQLite persistée localement
```

Les dossiers `build/` ou `.gradle/` apparaissent après compilation; ils peuvent être ignorés/supprimés si besoin.

## Entry point & configuration

- `SpringBootBoilerplateApplication` : classe annotée `@SpringBootApplication`, expose un bean `WebMvcConfigurer` pour autoriser CORS (`allowedOriginPatterns("*")`). Ajoutez vos propres beans/config dans cette classe ou dans des classes dédiées sous `config/`.
- `application.properties` : définit le dialecte SQLite, `ddl-auto=update` et la chaîne de connexion `jdbc:sqlite:sqlitestorage.db`. Pour d’autres environnements, créez des profils (`application-dev.properties`, etc.) ou basculez vers un autre SGBD.

## Modules existants

- `PingController` : simple endpoint `GET /ping -> "pong"`. Servez-vous en comme health check ou supprimez-le si vous créez votre propre API.

## Conventions recommandées

Créez des packages dédiés pour garder le code lisible :

```
co.devskills.springbootboilerplate
├── config/
├── controller/
├── dto/
├── entity/
├── repository/
└── service/
```

Copilot pourra suggérer du code plus pertinent si ces conventions sont suivies.

### Persistances & entités

- Ajoutez vos entités JPA dans `entity/` avec annotations `@Entity`.
- Déclarez les repositories dans `repository/` avec `extends JpaRepository`.
- SQLite étant fichier, évitez les colonnes spécifiques SQL Server/MySQL; pour des ateliers plus avancés, remplacez la configuration JDBC par Postgres/MySQL.

### Services & contrôleurs

- Services (`@Service`) pour la logique métier.
- Contrôleurs REST (`@RestController`) pour exposer les endpoints, mapping via `@RequestMapping` / `@GetMapping` etc.
- Utilisez `@Validated` + DTO (`record` ou `class`) pour structurer les entrées.

### Tests

- Les tests unitaires se placent dans `src/test/java/...` (non créé pour l’instant). Utilisez `@SpringBootTest` ou `@DataJpaTest` selon le besoin.
- Ajoutez `testImplementation` supplémentaires dans `build.gradle` si des frameworks (MockMvc, AssertJ) sont requis.

## Intégration avec le frontend

- Par défaut, l’API écoute sur `http://localhost:8080`.
- CORS étant permissif, le frontend Vite peut consommer les endpoints directement. Si vous changez les règles, pensez à aligner avec le front.
- Pour servir la SPA depuis Spring en prod, copiez le build Vite (`app-frontend/dist`) dans `src/main/resources/static` ou configurez un proxy.

## Conseils pour l’agent

1. **Toujours préciser les packages** lors de la génération de classes; évite les collisions.
2. **Favoriser les records/DTO** pour exposer des données propres, surtout avec Copilot.
3. **Configurer les migrations** (Flyway/Liquibase) si vous devez manipuler le schéma; sinon, `ddl-auto=update` convient pour des démos rapides.
4. **Séparer configuration & logique** : définir des classes `@Configuration` lorsque vous configurer des beans spécifiques (CORS, datasources, etc.).
5. **Utiliser les profils Spring** pour montrer comment Copilot peut générer des variations (`application-dev.properties`, `@Profile`).

Avec ces repères, l’agent sait comment étendre le backend de manière structurée sans dépendre d’une fonction métier particulière.
