````markdown
# 🎨 Interface d'Analyse de Sentiment (Next.js Client)

Ce dépôt contient le code source de l'interface utilisateur (Frontend) pour le microservice d'analyse de sentiment. Développé avec **Next.js (React)**, il offre une interface propre et réactive pour interagir avec notre API Backend sécurisée.

Il permet aux utilisateurs de s'authentifier et de tester le modèle d'IA `nlptown/bert-base-multilingual-uncased-sentiment` en temps réel.

-----

## 🚀 Fonctionnalités Clés

* **Authentification Sécurisée :** Formulaire de connexion communiquant avec l'endpoint `/login` du Backend.
* **Gestion de Session :** Stockage sécurisé du JWT (JSON Web Token) dans le `localStorage` du navigateur.
* **Analyse en Temps Réel :** Envoi de texte à analyser vers l'endpoint sécurisé `/predict`.
* **Visualisation des Résultats :** Affichage clair du sentiment (Positif, Négatif, Neutre) et du score de confiance (1 à 5 étoiles).
* **Gestion des États :** Feedback visuel pour les états de chargement (`loading`), de succès et d'erreur.

-----

## 🛠️ Stack Technique

* **Framework :** [Next.js](https://nextjs.org/) (React)
* **Style :** CSS Modules / Tailwind CSS (selon votre choix)
* **Requêtes HTTP :** Fetch API ou Axios
* **Conteneurisation :** Docker

-----

## 📦 Installation et Démarrage Rapide

### Prérequis
* Node.js (v16 ou supérieur)
* Le Backend API doit être lancé (voir le repo Backend).

### 1. Configuration de l'Environnement
Créez un fichier `.env.local` à la racine du projet pour indiquer l'adresse de votre Backend :

```env
# URL de votre API Backend (ex: FastAPI running on port 8000)
NEXT_PUBLIC_API_URL="http://localhost:8000"
````

### 2\. Installation des dépendances

```bash
npm install
# ou
yarn install
```

### 3\. Lancer le serveur de développement

```bash
npm run dev
# ou
yarn dev
```

L'application sera accessible à l'adresse : `http://localhost:3000`

-----

## 🐳 Démarrage avec Docker

Pour lancer uniquement le conteneur Frontend :

```bash
# Construire l'image
docker build -t sentiment-frontend .

# Lancer le conteneur (port 3000)
docker run -p 3000:3000 sentiment-frontend
```

*Note : Assurez-vous que le conteneur peut communiquer avec le Backend (configuration réseau Docker).*

-----

## 🔄 Workflow Utilisateur

1.  **Page `/login` :** L'utilisateur saisit ses identifiants. En cas de succès, le token est sauvegardé.
2.  **Redirection :** L'utilisateur est redirigé vers la page d'analyse.
3.  **Page `/sentiment` :**
      * L'utilisateur tape un avis client.
      * Le Front envoie la requête avec le header `Authorization: Bearer <token>`.
      * Le résultat de l'IA s'affiche instantanément.

-----

## 🤝 Contribution

Les Pull Requests sont les bienvenues. Pour des changements majeurs, veuillez ouvrir une issue d'abord pour discuter de ce que vous souhaitez changer.

````

---

### 📂 2. Contenu pour le Repo `BACKEND` (FastAPI)
*Ce README est purement technique. Il s'adresse aux développeurs Backend/DevOps et détaille l'API, la sécurité, la configuration Docker et les tests.*

```markdown
# ⚙️ API Microservice d'Analyse de Sentiment (FastAPI)

Ce dépôt héberge le code Backend du projet. C'est une API REST performante construite avec **FastAPI** qui sert de passerelle sécurisée entre le Frontend et le modèle d'IA hébergé sur **Hugging Face**.

Elle gère l'authentification, la validation des données et la logique métier de transformation des scores de sentiment.

-----

## 🎯 Objectifs Techniques

* **Sécurité Avancée :** Implémentation d'un système d'authentification robuste via **JWT (JSON Web Token)**.
* **Intégration IA :** Consommation de l'API Inference Hugging Face (`nlptown/bert-base-multilingual-uncased-sentiment`).
* **Architecture Propre :** Séparation des préoccupations (Routes, Auth, Services).
* **Documentation Auto :** Swagger UI intégré via FastAPI.

-----

## 🛠️ Stack Technique

| Technologie | Usage |
| :--- | :--- |
| **Python 3.9+** | Langage principal. |
| **FastAPI** | Framework Web asynchrone haute performance. |
| **Uvicorn** | Serveur ASGI. |
| **PyJWT** | Gestion de l'encodage/décodage des tokens. |
| **Requests** | Appels HTTP vers Hugging Face. |
| **Docker** | Conteneurisation du service. |
| **Pytest** | Tests unitaires et d'intégration. |

-----

## 🔌 Endpoints de l'API

| Méthode | Endpoint | Accès | Description |
| :--- | :--- | :--- | :--- |
| `POST` | **/login** | 🔓 Public | Authentifie l'utilisateur et délivre un Token JWT. |
| `POST` | **/predict** | 🔒 **Sécurisé** | Analyse le sentiment d'un texte via l'IA. Requiert un Header `Authorization`. |
| `GET` | **/docs** | 🔓 Public | Documentation interactive (Swagger UI). |

### Logique de Transformation (Business Logic)
L'API reçoit un score de 1 à 5 de l'IA et le convertit selon cette règle métier :
* **1-2 étoiles** ➔ Négatif 🔴
* **3 étoiles** ➔ Neutre 🟡
* **4-5 étoiles** ➔ Positif 🟢

-----

## 🔑 Configuration (.env)

**Impératif :** Créez un fichier `.env` à la racine pour stocker vos secrets. Ne commitez jamais ce fichier.

```env
# Clé API Hugging Face (Obtenue sur hf.co/settings/tokens)
HF_API_KEY="hf_xxxxxxxxxxxxxxxxxxxx"

# Sécurité JWT
SECRET_KEY="votre_clé_secrète_super_longue_et_aléatoire"
ALGORITHM="HS256"
ACCESS_TOKEN_EXPIRE_MINUTES=30
````

-----

## 🚀 Installation et Lancement

### A. Via Python (Local)

1.  **Cloner et installer les dépendances :**
    ```bash
    pip install -r requirements.txt
    ```
2.  **Lancer le serveur :**
    ```bash
    uvicorn main:app --reload
    ```
    *L'API est accessible sur `http://localhost:8000`*

### B. Via Docker (Recommandé)

1.  **Construire l'image :**
    ```bash
    docker build -t sentiment-backend .
    ```
2.  **Lancer le conteneur :**
    ```bash
    docker run -p 8000:8000 --env-file .env sentiment-backend
    ```

-----

## 🧪 Tests et Qualité

### Tests Unitaires (Pytest)

Le projet inclut une suite de tests pour valider la sécurité et la logique.

```bash
pytest
```

### Tests Manuels (Postman/cURL)

Exemple de test sécurisé :

```bash
# 1. Login (Récupérer le token)
curl -X POST "http://localhost:8000/login" ...

# 2. Predict (Utiliser le token)
curl -X POST "http://localhost:8000/predict" \
     -H "Authorization: Bearer <VOTRE_TOKEN>" \
     -d '{"text": "Ce produit est incroyable !"}'
```

-----

## ⚠️ Limitations Connues

  * **Dépendance Externe :** La disponibilité du service `/predict` dépend de l'état des serveurs de Hugging Face.
  * **Rate Limiting :** L'API Hugging Face gratuite impose des limites de requêtes par heure.

<!-- end list -->

```
```
