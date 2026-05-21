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
5. L'API renvoie une réponse.
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



---

## Captures d'écran

Les captures d'écran justificatives sont disponibles dans le dossier `docs/captures/`.

| Capture | Description |
|---|---|
| `actions-green.png` | Workflow GitHub Actions exécuté avec succès |
| `docker-compose-running.png` | Services Docker Compose lancés en local |
| `localhost-5173.png` | Interface frontend accessible en local |
| `github-secret.png` | Secret `OPENAI_API_KEY` configuré dans GitHub Actions |
```
