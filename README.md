## Gitflow du projet

Ce projet utilise un workflow Git inspiré de Gitflow, avec trois types de branches principaux : `main`, `develop` et les branches de fonctionnalité (`feat/*`).

### Branche `main`

- `main` représente la branche de production.
- Chaque Pull Request mergée vers `main` déclenche automatiquement un déploiement sur Coolify via la CI/CD.
- Le code présent sur `main` doit toujours être stable, testé et prêt à être déployé.

### Branche `develop`

- `develop` est la branche d’intégration principale.
- Toutes les nouvelles fonctionnalités sont fusionnées sur `develop` avant d’être préparées pour la production.
- Quand l’état de `develop` est jugé suffisamment stable, il peut être intégré dans `main` via une Pull Request.

### Branches de fonctionnalité (`feat/*`)

- Chaque nouvelle fonctionnalité ou évolution est développée dans une branche dédiée, créée à partir de `develop`.
- Convention de nommage recommandée :
  - `feat/<nom-fonctionnalite>`
  - Exemple : `feat/authentification-api`
- Une fois la fonctionnalité terminée et validée localement, une Pull Request est ouverte de la branche `feat/*` vers `develop`.
- Après review et validation, la branche `feat/*` est fusionnée dans `develop` et peut ensuite être supprimée.


### CI/CD et sécurité (SAST / scans)

- Une pipeline de CI/CD est exécutée à chaque push et à chaque Pull Request.
- La pipeline exécute notamment des contrôles de sécurité (SAST / scans) avec :
- **TruffleHog** pour détecter la présence de secrets dans le code et l’historique Git.
- **Trivy** pour analyser le code et/ou les artefacts (images, fichiers, configuration) à la recherche de vulnérabilités et de mauvaises configurations.
- En cas de détection de problèmes critiques, la pipeline échoue, ce qui permet de bloquer l’intégration de code non conforme.


### Conteneurisation & Infrastructure Docker

- Le projet est entièrement conteneurisé à l’aide de Docker et orchestré via Docker Compose.
- L’architecture comprend :
    - Base de données PostgreSQL
        - Initialisation automatique via scripts SQL
        - Persistance assurée via un volume dédié
    - Backend Go
        - Compilation dans une image builder optimisée
        - Exécution dans une image Alpine minimaliste
        - Respect des bonnes pratiques : utilisateur non-root, binaire statique, environnement isolé
    - Frontend (Nginx)
        - Build du projet puis déploiement sur un serveur Nginx sécurisé
        - Configuration personnalisée et exécution sous utilisateur non-root
        - Cette structure garantit des images légères, sécurisées et prêtes pour un déploiement automatisé sur Coolify.