```markdown
# 🎨 Sentiment Analysis Interface (Frontend)

Interface utilisateur moderne et réactive développée avec **Next.js**. Ce client web permet aux utilisateurs de s'authentifier et d'interagir en temps réel avec le microservice d'analyse de sentiment.



## ✨ Fonctionnalités

* **Authentification :** Formulaire de connexion sécurisé.
* **Gestion de Session :** Stockage du JWT (JSON Web Token) dans le navigateur (`localStorage`).
* **Dashboard d'Analyse :** Interface simple pour soumettre des avis clients.
* **Feedback Visuel :** Affichage dynamique des résultats (Score étoiles + Sentiment) et gestion des erreurs.

## 🛠️ Stack Technique

* **Framework :** Next.js (React).
* **Langage :** JavaScript / TypeScript.
* **Styling :** CSS Modules ou Tailwind CSS.
* **Réseau :** Fetch API / Axios.
* **DevOps :** Docker.

## 🚀 Installation & Démarrage

### 1. Configuration (.env.local)
Créez un fichier `.env.local` à la racine du projet pour lier le Frontend à votre API Backend :

```ini
# URL de votre API Backend (ex: localhost:8000)
NEXT_PUBLIC_API_URL="http://localhost:8000"
