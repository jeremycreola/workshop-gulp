﻿<p align="center">
  <img src="https://www.alsacreations.com/xmedia/doc/original/gulp-bann.png" alt="Gulp-logo">
</p>

</br>

# GulpJS - Workshop par Dylan, Gary, Jérémy et Meroine

## Avant-propos

Ce workshop a été réalisé en mars 2019 dans le cadre de notre formation [BeCode](https://www.becode.org/index_fr.html). Il s'agissait de faire découvrir aux autres membres de notre promotion l'utilité de Gulp pour un développeur. 

Ce workshop a été réalisé par :

- [Dylan Maghe](https://github.com/Dylan-Maghe);
- [Gary Petrovski](https://github.com/GaryPetrovski);
- [Meroine El Haddadi](https://github.com/Dylan-Maghe);
- Et moi-même.

## GulpJS, qu'est-ce que c'est ?

Gulp, dont la première version a été mise en ligne en 2014, est un outil qui est devenu aujourd'hui indispensable pour les developpeurs front-end. C'est est un task runner, autrement dit un "automatiseur de tâches". Concrètement, il lance des bouts de scripts à notre place, ce qui va nous permettre de gagner énormément de temps.

Les tâches prises en main par Gulp peuvent être très variées :

- Minifier ou concaténer du CSS ou du Javascript;
- Créer ou supprimer des dossiers ou des fichiers (il peut permettre de créer un projet à partir de zéro);
- L'optimisation et/ou la compression d'images;
- Créer un serveur local  afin de tester notre code sur différents périphériques en même temps;
- Simuler des navigateurs fantômes pour parcourir et tester les régressions d'affichage d'une page.

Afin d'avoir une idée du nombre d'actions possibles avec Gulp, il faut savoir que plus de 2.000 plugins uniques sont recensés à l'heure actuelle sur le site officiel, autrement dit autant de tâches exécutables au sein du projet. 

Intégrer Gulp à un projet va donc être significatif d'un énorme gain de temps puisqu'il va nous permettre d'automatiser les tâches répétitives. Il ne restera donc plus qu'à se concentrer sur le principal : produire un code fonctionnel.

## Prérequis et installation
 	
  Pour utiliser Gulp vous aurez besoin de Node.js. Pour l'installer, cliquez le lien suivant:
	
- https://nodejs.org/en/download/

Une fois que l'installation de Node.js est terminée, tapez la commande suivante afin d'installer gulp de manière globale sur votre ordinateur:

```bash
sudo npm install gulp -g
```
Les dépendances que nous allons utiliser durant le livecode :
```bash
npm install gulp --save-dev
npm install gulp-sass --save-dev
npm install gulp-uglifycss --save-dev
npm install gulp-uglify --save-dev
npm install gulp-rename --save-dev
npm install gulp-concat --save-dev
npm install gulp-imagemin --save-dev
npm install gulp-babel --save-dev
npm install --save-dev @babel/core
npm install --save-dev @babel/preset-env
```

## Le fichier Gulpfile.js

Le fichier gulpfile.js est le fichier le plus important puisqu'il va s'occuper de gérer les tâches à réaliser, leurs options, leurs sources et destination. En gros, c'est le fichier qui va gérer tout notre projet.

Une tâche Gulp ressemble à ça :

```javascript
gulp.task('css', () => {
  return gulp.src(ici-ma-source)
    /* ici les plugins Gulp à exécuter */
    .pipe(gulp.dest(ici-ma-destination));
});

```

Dans ce bout de code, voici ce qu'il faut retenir :

- **'css'** : le nom que l'on a donné à la tâche;

- **ici-ma-source** : chemin indiquant l'endroit où se trouve le(s) fichier(s) source à traiter;

- **ici-ma-destination** : chemin vers lequel les fichiers seront créés après l'exécution des tâches Gulp.

Nous pouvons maintentenant déclarer toutes les variables dont nous aurons besoin au début de notre fichier gulpfile.js. Ces variables sont celles des plugins, le dossier source et le dossier de destination. Nous allons faire ça de cette manière :

```javascript
// Requis
const gulp = require('gulp');

// Include plugins
const plugins = require('gulp-load-plugins'); // tous les plugins de package.json

```

## Livecoding

<!-- parler en quelques mots de ce que Gary et Dylan vont présenter -->
Afin d'initier le projet, tapez la commande suivante:

```bash
npm init
```
D'abord nous allons installer Gulp dans les dépendances de développement:

```bash
npm install gulp --save-dev
```
Ensuite installez gulp-sass dans les dépendances de développement, ce plugin permet de compiler le scss en css:

```bash
npm install gulp-sass --save-dev
```
Créez un fichier gulpfile.js où nous allons déclarer les variables dont nous aurons besoin:

```js
const gulp = require('gulp');
const sass = require('gulp-sass'); // Plugins du fichier package.json
```
Modification du lien css dans le html 
```<link rel="stylesheet" href="dist/style.min.css">```

Nous allons maintenant créer une tâche à effectuer :

```js
gulp.task('css', () => { // création de la tâche css
    return gulp.src('assets/scss/style.scss') // source du fichier à modifier 
        .pipe(sass())  // exécute le plugin sass 
        .pipe(gulp.dest('assets/css'))  // création du dossier et du fichier css 
})
```

Pour exécuter cette tâche et compiler le scss, tapez simplement:

```bash
gulp css
```
## Minify 

- https://www.npmjs.com/package/gulp-uglifycss
- https://www.npmjs.com/package/gulp-rename

Installation des dépendances en ligne de commande :
```bash
npm install gulp-uglifycss --save-dev
npm install gulp-rename --save-dev
```

Import des dépendances :
```js
   const uglifycss = require('gulp-uglifycss');
   const rename = require('gulp-rename');
```
Le dossier dist et le fichier style.min.css se génèrent automatiquement à l’exécution du plugin via la fonction : 
```js
gulp.task('css', () => {
    return gulp.src('assets/scss/style.scss') 
        .pipe(sass()) // compilateur SASS
        .pipe(gulp.dest('assets/css'))
        .pipe(rename({
            suffix: '.min'  // pour le minify, on ajoute le suffix au style.css (style.min.css)
          }))
        .pipe(uglifycss({           // exécution du plugin
            "maxLineLen": 80,       // nombre de caractères par ligne
            "uglyComments": true
          }))
        .pipe(gulp.dest('./dist/css'));// génération du dossier dist             
})
```
Pour éviter de taper gulp css à chaque changement, on va automatiser la tâche grâce à la création d'une fonction watch.
```js
  gulp.task('watch', () => {
    gulp.watch('assets/scss/style.scss', gulp.series('css'));
 });
```
### Le fichier gulpfile.js final

```js
const gulp = require('gulp');
const sass = require('gulp-sass');
const uglifycss = require('gulp-uglifycss'); //installation des différents plugins
const uglify = require('gulp-uglify');
const rename = require('gulp-rename');
const concat = require('gulp-concat');
const imagemin = require('gulp-imagemin');
const babel = require('gulp-babel');


gulp.task('css', () => {
    return gulp.src('assets/scss/style.scss') 
        .pipe(sass()) // compilateur SASS
        .pipe(gulp.dest('assets/css'))
        .pipe(rename({ // Renomme le fichier css
            suffix: '.min'
          }))
        .pipe(uglifycss({   //Exécution du plugin qui minifie le css        
            "maxLineLen": 80,
            "uglyComments": true
          }))
        .pipe(gulp.dest('./dist/css'));// génération du dossier dist 
        
})

gulp.task('js', () => {
    return gulp.src('assets/js/*.js')
        .pipe(babel({ // Convertis le javascript en ancien js
          presets: ['@babel/env']
        }))
        .pipe(uglify()) // Minifie tout les fichiers js
        .pipe(concat('global.min.js')) // Permet de concaténer tout les fichiers JS
        .pipe(gulp.dest('./dist/js'))
})

gulp.task('img', () => {
    return gulp.src('assets/img/*.{png,jpg,jpeg,gif,svg}')
        .pipe(imagemin()) // Minifie toutes les images
        .pipe(gulp.dest('./dist/img'))
})


  gulp.task('watch', () => {
    gulp.watch('assets/scss/style.scss', gulp.series('css')); //Surveille toutes les modifications dans le fichier style.scss
    gulp.watch('assets/js/*.js', gulp.series('js')); //Surveille les modifications de tout les fichier js
 });
```


## Ressources utiles

- https://gulpjs.com/
- https://github.com/gulpjs/gulp
- https://www.youtube.com/watch?v=tTrPLQ6nOX8 (tutoriel en anglais et très récent)
- https://www.alsacreations.com/tuto/lire/1685-ebauche-de-workflow-gulp-taches-uncss-includes-critical-css.html
