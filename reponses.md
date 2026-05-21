# Réponses ouvertes — Évaluation DevOpsGPT

## Exercice 1 — Conception logicielle

### 1. Diagramme de contexte

Le système DevOpsGPT est composé de trois éléments principaux autour de l'application :

- L'utilisateur : il envoie une question et reçoit une réponse.
- L'API GPT-4 : elle génère une réponse à partir du message envoyé.
- La base de données : elle sauvegarde l'historique des messages et des réponses.

Le schéma est disponible dans le dossier `docs/`.

### 2. Flowchart du traitement d'un message

Lorsqu'un utilisateur envoie un message, le système suit les étapes suivantes :

1. Réception du message de l'utilisateur.
2. Vérification du contenu du message avec un filtre de modération.
3. Si le message contient des insultes, le système bloque le message et affiche une erreur.
4. Sinon, le message est envoyé à l'API GPT-4.
5. L'API GPT-4 renvoie une réponse.
6. La réponse est sauvegardée dans la base de données.
7. La réponse est affichée à l'utilisateur.

Le schéma est disponible dans le dossier `docs/`.

### 3. Dictionnaire de données

| Nom de la donnée  | Type          | Description                                      |
| ----------------- | ------------- | ------------------------------------------------ |
| id                | String / UUID | Identifiant unique du message                    |
| userMessage       | String        | Message envoyé par l'utilisateur                 |
| assistantResponse | String        | Réponse générée par l'API GPT-4                  |
| createdAt         | DateTime      | Date et heure de création du message             |
| status            | String        | Statut du traitement : success, blocked ou error |
| userId            | String        | Identifiant de l'utilisateur associé au message  |

---

## Exercice 2 — Git et Docker

### 1. Méthodologie et Git

#### Question A — User Story

En tant qu'utilisateur de DevOpsGPT, je veux pouvoir souscrire à un abonnement Premium afin d'accéder à des fonctionnalités avancées comme un nombre plus élevé de messages, un historique étendu et une meilleure priorité de traitement.

#### Question B — Commandes Git

Créer une nouvelle branche de fonctionnalité :

```bash
git checkout -b feature-premium-subscription
```

Ajouter les fichiers modifiés :

```bash
git add .
```

Créer un commit :

```bash
git commit -m "Add premium subscription feature"
```

Revenir sur la branche principale :

```bash
git checkout main
```

Fusionner la branche de fonctionnalité dans `main` :

```bash
git merge feature-premium-subscription
```

Créer un tag de version nommé `v1.0.0` :

```bash
git tag v1.0.0
```

Pousser le tag sur GitHub :

```bash
git push origin v1.0.0
```

---

### 2. Dockerisation

Deux fichiers Dockerfile ont été créés :

- `backend/Dockerfile`
- `frontend/Dockerfile`

Le fichier `backend/Dockerfile` utilise l'image `node:22-alpine`, définit le dossier de travail `/app`, installe les dépendances, expose le port `3000` et lance l'API avec la commande :

```bash
node index.js
```

Le fichier `frontend/Dockerfile` utilise également l'image `node:22-alpine`, installe les dépendances, expose le port `5173` et lance l'interface frontend avec :

```bash
npm run dev
```

---

### 3. Orchestration avec Docker Compose

Un fichier `docker-compose.yml` a été créé à la racine du projet.

Il lance trois services :

| Service  | Description                     | Port |
| -------- | ------------------------------- | ---- |
| chat-api | API backend Node.js             | 3000 |
| chat-ui  | Interface frontend              | 5173 |
| cache    | Service Redis avec redis:7-alpine | 6379 |

Les services sont reliés dans un même réseau Docker afin que l'API, l'interface et Redis puissent communiquer entre eux.

Commande utilisée pour lancer le projet :

```bash
docker compose up --build
```

Commande utilisée pour arrêter les services :

```bash
docker compose down
```

---

## Exercice 3 — CI/CD avec GitHub Actions

### 1. Le Workflow CI/CD

Le workflow GitHub Actions a été créé dans le fichier suivant :

```text
.github/workflows/main.yml
```

Ce workflow se déclenche :

- lors d'un push sur la branche `main`
- lors de la création d'un tag commençant par `v`, par exemple `v1.0.0`

Il contient un job nommé :

```text
test-and-deploy
```

Ce job s'exécute sur :

```text
ubuntu-latest
```

Les étapes réalisées sont :

1. Récupération du code avec `actions/checkout@v4`.
2. Installation de Node.js version 22 avec `actions/setup-node@v4`.
3. Installation des dépendances du backend.
4. Exécution des tests du backend.
5. Installation des dépendances du frontend.
6. Exécution des tests du frontend.
7. Simulation du déploiement uniquement lorsqu'un tag de version est publié.

La condition utilisée pour lancer le déploiement uniquement sur un tag est :

```yaml
if: startsWith(github.ref, 'refs/tags/v')
```

La commande de simulation du déploiement est :

```bash
echo "Déploiement en cours..."
```

---

### 2. Sécurité et Secrets

#### Question A — Où enregistrer le secret OPENAI_API_KEY ?

Pour enregistrer la clé secrète sur GitHub, il faut aller dans le dépôt GitHub du projet, puis cliquer sur :

```text
Settings > Secrets and variables > Actions > New repository secret
```

Ensuite, il faut renseigner :

| Champ  | Valeur |
| ------ | ------ |
| Name   | OPENAI_API_KEY |
| Secret | valeur réelle de la clé API |

Enfin, il faut cliquer sur :

```text
Add secret
```

Cette méthode permet d'utiliser une clé secrète sans l'écrire directement dans le code source. Cela évite d'exposer des informations sensibles dans le dépôt GitHub.

#### Question B — Syntaxe pour injecter le secret dans main.yml

Dans le fichier `.github/workflows/main.yml`, le secret peut être injecté comme variable d'environnement avec la syntaxe suivante :

```yaml
env:
  OPENAI_API_KEY: "${{ secrets.OPENAI_API_KEY }}"
```

Exemple dans l'étape de déploiement :

```yaml
- name: Simuler le déploiement
  if: startsWith(github.ref, 'refs/tags/v')
  env:
    OPENAI_API_KEY: "${{ secrets.OPENAI_API_KEY }}"
  run: echo "Déploiement en cours..."
```

---

## Captures d'écran

Les captures d'écran justificatives sont disponibles dans le dossier `docs/captures/`.

| Capture | Description |
| ------- | ----------- |
| `actions-green.png` | Workflow GitHub Actions exécuté avec succès |
| `docker-compose-running.png` | Services Docker Compose lancés en local |
| `localhost-5173.png` | Interface frontend accessible en local |
| `github-secret.png` | Secret `OPENAI_API_KEY` configuré dans GitHub Actions |

---

## Conclusion

Le projet DevOpsGPT a été configuré avec une démarche DevOps complète :

- gestion du code source avec Git et GitHub ;
- création d'une branche de fonctionnalité et d'un tag de version ;
- dockerisation du frontend et du backend ;
- orchestration avec Docker Compose ;
- ajout d'un service Redis ;
- configuration d'un workflow CI/CD avec GitHub Actions ;
- utilisation sécurisée des secrets GitHub pour protéger la clé `OPENAI_API_KEY`.

Le dépôt GitHub contient donc les fichiers nécessaires pour le rendu de l'évaluation.
