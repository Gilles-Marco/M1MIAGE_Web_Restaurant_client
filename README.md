# Projet M1 MIAGE WEB

Étudiant : GILLES Marco

Enseignant : BUFFA Michel

MIAGE Sophia Antipolis

# Installation

## Optionnel

Pour avoir accès à plus de fonctionnalités je vous invite à utiliser le serveur qui se trouve sur ce dépôt, par rapport à celui fourni, il permet de modifier tous les champs des restaurants, et d'ajouter un restaurant avec tous ses champs. Pas seulement le nom du restaurant et son type de cuisine

https://github.com/Gilles-Marco/M1_MIAGE_WEB_RESTAURANT_SERVEUR
```
git clone https://github.com/Gilles-Marco/M1_MIAGE_WEB_RESTAURANT_SERVEUR.git
cd M1_MIAGE_WEB_RESTAURANT_SERVEUR
node serverCrudWithMongo.js
```

### Possibilité d'utiliser pm2
```
git clone https://github.com/Gilles-Marco/M1_MIAGE_WEB_RESTAURANT_SERVEUR.git
cd M1_MIAGE_WEB_RESTAURANT_SERVEUR
npm install -g pm2
pm2 start serverCrudWithMongo.js --watch
```

## Installation du client

```
git clone https://github.com/Gilles-Marco/M1MIAGE_Web_Restaurant_client.git
cd M1MIAGE_Web_Restaurant_client
npm install
npm run serve
```

## Attention

Il faut que le serveur soit en premier pour qu'il prenne le port 8080.

Si le client est lancé avant le serveur, le client va prendre le port 8080 et ne trouvera pas le serveur.

Un bon signe que le client et le serveur se soient lancés sur le bon port est si le client est sur le port 8081

# Point remarquable du projet

## Système de réservation

L'utilisateur sélectionne un restaurant, il affiche la carte du restaurant, il sélectionne les plats qu'il souhaite manger. Le bouton "Réservez" devient actif, il lui suffit de renseigner la date et l'heure pour laquelle il souhaite réserver. Puis il clique sur le bouton "Réservez" pour enregistrer sa réversation.

La réservation va être sauvegarder dans le localstorage. Les réservations sont une liste d'objets composés des attributs, date, time, restaurant et command. Les réservations sont ensuite stringigy pour être sauvegarder dans le localStorage.

Et quand on souhaite les récupérer pour l'affichage dans le drawer, on charge les réservations depuis le localStorage, puis on les parse en JSON pour pouvoir itérer sur chaque objet et les afficher dans le drawer à gauche.

## Chargement du menu

Le menu est sauvegardé dans un .json. C'est une liste d'objet composée des attributs suivants:
menu, type, title, price, image.

Le fichier est chargé dans le composant "menuDetail.vue", qui va itérer sur chaque objet du fichier .json et filtrer les objets pour les mettres dans différentes catégories, représentés par les computed suivants:
gastronomiqueEntree, gastronomiquePlat, gastronomiqueDessert, midiEntree, midiPlat, midiDessert.

Les variables computed sont ensuite utilisés dans des v-for pour itérer sur les éléments contenus et les afficher.

## Recherche avancé

Un popin s'ouvre avec les champs possibles de recherche, on renseigne les champs de recherches.

Puis le client va demander au serveur mongoDB le nombre d'éléments "restaurant" qu'ils possèdent, puis ensuite il va faire une requête pour récupérer tous les restaurants dans "une seule page" page=1&pagesize=nb_restaurants. C'est pour ça qu'il fallait récupérer avant le nombre de restaurants.

Puis ensuite on itère sur le nombre de champs qui a été rempli dans le formulaire de recherche. S'il le champs n'a pas été rempli on itère pas dessus.

Puis chaque restaurant est filtré par rapport à ses champs et les champs qui ont été renseigné.

La liste des restaurants filtrés est ensuite stocké dans une variable, et si cette variable possède plus d'un élément alors elle a la priorité sur l'affichage ce qui permet d'afficher les restaurants de la recherche à la place de l'affichage classique des restaurants ou l'affichage via la barre de recherche.

L'affichage des restaurants se fait via une variable computed restaurantToDisplay qui prend la valeur d'une des trois variables suivantes. advancedRestaurant ( là où est stocké les restaurants remplissants les critères de la recherche avancée), filteredRestaurant ( là où est stocké les restaurants remplissants les critères de recherche via la barre de recherche), restaurants ( là où est stocké les restaurants obtenus via la requête pour récupérer des restaurants). En priorité elle prendra la valeur de advancedRestaurant, si celle-ci est vide alors elle prendra la valeur de filteredRestaurant et si celle-ci est encore elle prendra la valeur de restaurants.