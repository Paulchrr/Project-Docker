# Project Docker


# ©Paul Charrier


## Installation

### Prérecquis

* Installer Docker Desktop https://www.docker.com/products/docker-desktop
* Créer un compte DockerHub https://hub.docker.com/

### Setup de l'application

* Ouvrir un terminal
* Se rendre dans le dossier téléchargé
* Lancer la commande : `docker-compose up -d`
* Se rendre sur : **http://localhost:3000**

### Commandes

* Arreter l'application : `docker-compose stop`
* Stopper l'application : `docker-compose start`
* Supprimer l'application : `docker-compose down-`



## Explication du code

### docker-compose.yml

```
services:
```
> Définitions des services 
```
  mongo:
    container_name: mongodb
    networks:
      - backend
    image: mongo:5.0
    ports:
      - '27017:27017'
    volumes:
      - mongodb_data:/data/db
    restart: always
```
> Définitions d'un premier service "mongo" avec l'attribution du réseau "backend", l'image désiré ainsi que le port du container.
Un volume nominatif a été indiqué, avec l'argument rédemarrarage automatique.

```
  api:
    container_name: api
    networks:
      - frontend
      - backend
    depends_on:
      - mongo
    build: ./backend
    volumes:
      - backend:/usr/src/app/
    ports:
      - '8080:8080
```
> Définition d'un service "API" avec l'attribution du réseau "backend" et "front-end qui dépend du service "mongo". Les éléments telles que l'image, le volume et le port ont également été indiqué.


```
  react:
    container_name: react
    depends_on:
      - api
    networks:
      - frontend
    build: ./frontend
    volumes:
      - frontend:/usr/src/app
    ports:
      - '3000:3000'
    restart: always
```

> Définition d'un service "react" avec l'attribution du réseau "frontend" qui dépend du service "API". Les éléments telles que l'image, le volume et le port ont également été indiqué.

```
networks:
  frontend: {}
  backend: {}
```
>Définition des deux réseaux frontend et backend
```
volumes:
  mongodb_data:
  frontend:
  backend:
```
>Définition des trois volumes utilisés (mongodb_data, frontend, backend)


### dockerfile (Frontend)
```
FROM node:10-alpine

EXPOSE 3000
WORKDIR /app
COPY . .
RUN npm install
CMD ["npm", "start"]
```
* Utilisation de l'image `node:10-alpine`
* Définition du port **3000**
* Définition du repertoire de travail `/app`
* Copier l'ensemble des fichiers et dossiers du repertoire vers la racine du container
* Installation des packages
* Démarrage des packages

### dockerfile Backend)
```
FROM node:10-alpine

EXPOSE 8080
WORKDIR /app
COPY . .
RUN npm install
CMD ["npm", "start"]
```
* Utilisation de l'image `node:10-alpine`
* Définition du port **8080**
* Définition du repertoire de travail `/app`
* Copier l'ensemble des fichiers et dossiers du repertoire vers la racine du container
* Installation des packages
* Démarrage des packages



