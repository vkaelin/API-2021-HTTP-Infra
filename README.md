# Labo HTTP Infra

Auteurs: Lazar Pavicevic, Valentin Kaelin

## Etape 1: HTTP serveur statique avec apache httpd

Nous utilisons l'image `php:8-apache-bullseye` afin d'avoir, comme dans la vidéo de présentation, une configuration minimale déjà présente.
Les fichiers de notre site statique doivent donc se trouver dans le chemin suivant à l'intérieur du conteneur: `/var/www/html/`.

Nous construisons l'image Docker avec la commande suivante:
```bash
docker build -t api-2021-http-infra-step1 .
```
Puis, nous pouvons lancer un container et y accèder via [localhost](http://localhost:9090) avec :

```bash
docker run -p 9090:80 api-2021-http-infra-step1
```

## Etape 2: Serveur HTTP dynamique avec Node.js

Pour cette étape, nous utilisons [AdonisJS](https://adonisjs.com/) en remplacement d'Express.js. Il s'agit d'un framework moderne, reprenant une architecture similaire à Laravel et offrant la possibilité de coder en Typescript. Comme cette étape ne demande pas énormément de fonctionnalités, nous utilisons une version slim du framework.

// TODO Partie sur le JSON que l'app va fournir.

Nous profitons des **multi-stage builds** proposés par Docker pour créer notre image Docker. Nous avons donc deux étapes principales, le *build stage* et le *run stage*. Chaque étape utilise l'image de base `node:16-alpine`.

Le build stage transpile les fichiers Typescript en Javascript valide pour le runtime de Node.js et prépare le dossier de production que le run stage va utiliser.

Le run stage, par soucis de sécurité, utilise l'utilisateur non-root `node` mis à disposition par l'image de base.

Nous construisons l'image Docker avec la commande suivante:
```bash
docker build -t api-2021-http-infra-step2 .
```
Puis, nous pouvons lancer un container et y accèder via [localhost](http://localhost:3333) avec :

```bash
docker run -p 3333:3333 api-2021-http-infra-step2