# FEDENE DataViz

Une application Svelte conçue pour la FEDENE, intégrant des fonctionnalités de scrollytelling et une composante cartographique.

## Création du projet

Pour créer un projet Svelte, suivez ces étapes :

```bash
# Créez un nouveau projet dans le répertoire courant
npm create svelte@latest

# Créez un nouveau projet dans le répertoire 'fedene-dataviz'
npm create svelte@latest fedene-dataviz


Développement
Une fois le projet créé et les dépendances installées, vous pouvez démarrer un serveur de développement :

bash
Copier le code
npm install
npm run dev

# Ou démarrez le serveur et ouvrez l'application dans un nouvel onglet de navigateur
npm run dev -- --open
L'application sera disponible à l'adresse http://localhost:5173 par défaut.

Fonctionnalités
Scrollytelling : Présente des données interactives avec des animations au défilement de la page.
Cartographie : Intègre des cartes interactives pour visualiser les données géographiques.
Construction
Pour créer une version optimisée de l'application pour la production :

bash
Copier le code
npm run build
Vous pouvez prévisualiser la version de production avec :

bash
Copier le code
npm run preview
Déploiement
Pour déployer l'application, vous pourriez avoir besoin d'un adaptateur en fonction de votre environnement cible.

Structure du projet
src/ : Contient tous les fichiers sources de l'application Svelte.
styles/ : Feuilles de style SCSS et CSS.
routes/ : Définition des pages et des routes de l'application.
```
