# BobApp – Workflow CI/CD

## Sommaire
1. [Étapes du workflow](#étapes-du-workflow)
   1. [Frontend](#frontend)
   2. [Backend](#backend)
2. [KPIs proposés](#kpis-proposés)
3. [Analyse des métriques et des retours utilisateurs](#analyse-des-métriques-et-des-retours-utilisateurs)
4. [Problèmes à traiter en priorité](#problèmes-à-traiter-en-priorité)

---

## Étapes du workflow

La chaîne CI/CD est orchestrée via **GitHub Actions** et se compose de deux workflows distincts :

- **Frontend CI**
- **Backend CI**

Chaque workflow est déclenché :
- lors d’une **pull request** vers la branche `main`
- lors d’un **push** sur la branche `main`
- manuellement via **`workflow_dispatch`**

---

## Frontend

1. **Checkout repository**  
   Récupération du code source à jour avec l’historique complet.

2. **Setup Node**  
   Installation et configuration de Node.js afin de garantir un environnement cohérent avec celui du projet.

3. **Install dependencies**  
   Installation des dépendances depuis le fichier `package.json`.

4. **Run tests + coverage (Karma)**  
   Exécution des tests unitaires et génération du rapport de couverture de code.

5. **Upload coverage report**  
   Publication du rapport de couverture en tant qu’artefact GitHub Actions.

6. **SonarQube Cloud scan**  
   Analyse de la qualité du code avec SonarCloud (coverage, bugs, code smells).

7. **Set up Docker Buildx**  
   Installation et configuration de Docker Buildx dans l’environnement GitHub Actions.

8. **Login to Docker Hub**  
   Authentification sécurisée à Docker Hub à l’aide de secrets GitHub.

9. **Build and push (Docker Hub)**  
   Construction de l’image Docker contenant l’application frontend et publication sur Docker Hub.

---

## Backend

1. **Checkout repository**  
   Récupération du code source à jour avec l’historique complet.

2. **Set up JDK 11**  
   Installation et configuration de Java (JDK 11) et activation du cache Maven pour accélérer les builds.

3. **Run tests + JaCoCo (Maven)**  
   Exécution des tests backend et génération du rapport de couverture JaCoCo.

4. **Upload JaCoCo report**  
   Publication du rapport de couverture en tant qu’artefact GitHub Actions.

5. **SonarQube Cloud scan**  
   Analyse de la qualité du code backend avec SonarCloud.

6. **Set up Docker Buildx**  
   Installation et configuration de Docker Buildx.

7. **Login to Docker Hub**  
   Authentification sécurisée à Docker Hub.

8. **Build and push (Docker Hub)**  
   Construction de l’image Docker contenant l’application backend et publication sur Docker Hub.

---

## KPIs proposés

Afin de mesurer la qualité et la fiabilité du projet, les KPIs suivants ont été définis :

### KPI 1 – Couverture de code
- **Seuil minimal : 70 %**
- Objectif : garantir une fiabilité suffisante du code et limiter les régressions.

### KPI 2 – Quality Gate SonarCloud
- **Critère : PASS**
- Absence de bugs bloquants et de vulnérabilités critiques.
- Dette technique maîtrisée garantissant un code propre et maintenable.

---

## Analyse des métriques et des retours utilisateurs

### Métriques observées

- **Frontend**
  - Couverture de code : **76,92 %**
  - Supérieure au seuil fixé.
  - Quality Gate SonarCloud validé.
  - Image Docker construite et publiée avec succès sur Docker Hub.

- **Backend**
  - Couverture de code : **≈ 32,8 %**
  - Inférieure au seuil fixé.
  - Quality Gate SonarCloud non validé.
  - Présence de plusieurs *code smells*, notamment :
    - Création répétée d’un objet `Random` (impact performance)
    - Usage discutable du pattern Singleton
    - Non-respect de conventions Java
    - Nommage de champs peu explicite (`joke`, `response`)
    - Visibilité inadaptée de certains champs
    - Méthode de test vide non documentée

---

### Retours utilisateurs

Les avis utilisateurs mettent en évidence plusieurs problèmes fonctionnels persistants :

- **Fonctionnalité de suggestion de blagues défaillante**
  - bouton bloqué
  - navigateur qui plante

- **Bug connu non corrigé**
  - signalé depuis plusieurs semaines
  - absence de correction visible

- **Manque de communication**
  - absence de réponse après signalement par email

- **Insatisfaction globale**
  - désinstallation / abandon de l’application par certains utilisateurs

---

## Problèmes à traiter en priorité

1. Augmenter et améliorer les tests unitaires du **backend** afin de détecter les bugs fonctionnels.
2. Renforcer les tests **frontend**, malgré une couverture suffisante, afin de mieux couvrir les cas fonctionnels.
3. Corriger les bugs connus remontés par les utilisateurs.
4. Améliorer la communication et le suivi des retours utilisateurs.
