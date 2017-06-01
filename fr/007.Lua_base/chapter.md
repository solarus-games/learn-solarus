# Chapitre 7 - Les bases des scripts Lua #

Ca y est, on commence la partie intéressante. Dans ce chapitre, nous verrons les bases du lien entre le moteur Solarus et les scripts Lua de votre quête.

Ces scripts seront d'une grande importance, car ce seront eux qui vous permettront de gérer la majeure partie des entités présentes dans votre quête.

## I) Les scripts principaux ##

A la création d'une quête, le moteur crée par défaut plusieurs scripts, qui sont, pour la majeure partie d'entre eux, nécessaires à l'exécution de votre quête.

Le fichier `main.lua` contient par exemple le code qui permettra à Solarus de créer une fenêtre adaptée à votre quête, ainsi que de gérer toutes les informations envoyées par le joueur. 

Le fichier `game_manager.lua`, lui, contient toutes les données pour lancer l'exécution de votre quête dans la fenêtre précédemment créée.

Le fichier `initial_game.lua` sert à initialiser toutes les données nécessaires pour créer les données lors de la création d'une nouvelle partie. Pour rappel, nous avons déjà eu l'occasion de modifier un peu ce fichier lors de la création de notre première map, afin de faire apparaitre notre héros dans cette dernière plutôt que dans la map de base.

## II) Les bases du Lua ##

Bien, passons aux choses sérieuses. Pour commencer, vous aurez besoin de la documentation de Solarus, qui est accessible en allant dans l'onglet aide/documentation de l'éditeur, ou en appuyant sur la touche F1.

### 1) Les fonctions ###
Nous allons commencer de manière simple, en examinant de plus près la ligne que nous avons modifiée : `game:set_starting_location("first_map", nil)  -- Starting location.`

Cette ligne peut se diviser en 3 parties :

- `game:set_starting_location` : il s'agit du nom de la fonction utilisée. Celle-ci permet de modifier la destination de base.
- `("first_map", nil)` : il s'agit des paramètres de la fonction (en d'autres termes, les données qu'on lui envoie et dont elle a besoin pour fonctionner). Ici, on spécifie qu'on veut avoir pour map de départ la map avec l'ID "first_map", avec comme position de départ la première entité de type "destination" rencontrée.
-  `--Starting location.` : Il s'agit là d'un commentaire destiné à faire comprendre le fonctionnement du code aux autres personnes participant à la création du jeu.

Lorsque vous voudrez créer une nouvelle fonction, vous devrez utiliser le mot clé "function" suivi du nom de votre fonction, puis entre parenthèses ses paramètres, séparés par des virgules.

**exemple :**

`function ma_nouvelle_fonction(game)` crée une fonction nommée "ma_nouvelle_fonction", qui prendra en paramètre un argument, qui sera nommé "game" dans la fonction.

`function do_nothing()` crée une fonction nommée "do_nothing", qui ne prend pas d'arguments.

### 2) Les variables ? Kézako ? ###

Les variables, pour les décrire en termes techniques, ce sont des zones mémoire bien précises qui vont contenir les données que vous leur enverrez.

Pour faire plus simple, pour vous, une variable s'apparentera à un nom (par exemple "porte_maison", qui fera référence à une valeur ("ouvert" ou "fermé"). C'est Solarus qui s'occupera du reste. 

Dans l'exemple de la fonction "set\_starting\_location, qu'on a vu dans la section précédente, le résultat de la fonction est stocké dans la variable nommée "game". En réalité, set\_starting\_location est ce que l'on appelle une méthode de la variable game, c'est à dire une fonction qui ne pourra être utilisée que par la variable game (c'est à ça que sert le caractère `:`).

Pour finir sur ce sujet, je vous apprendrai juste qu'il existe 2 grands types de variables : les variables dites "locales" et les variables dites "globales" 

### 3) Locales ? Globales ?###

Oui, ce n'est peut-être pas très clair, donc on va essayer d'approfondir un peu le sujet.

#### a) Les variables locales ####

Dans l'exemple précédent, `game` est une variable dite "locale", c'est à dire qu'elle n'existe que dans la fonction qui appelle set\_starting\_location (donc que dans initialize\_new\_savegame, dans cet exemple). Ces variables sont créées en utilisant le mot clé `local` suivi du nom de la variable.

**exemples :**

/!\ Tous les noms de variables utilisés dans les exemples sont des noms choisis par mes soins. Vous pouvez bien évidemment choisir vos propres noms pour vos variables

`local porte_maison_ouverte` pourra prendre deux valeurs (true ou false), et nous servira à afficher la porte de notre maison comme étant ouverte (true) ou fermée (false).

`local nombre_cles` pourra prendre des valeurs entières positives ou nulles (de 0 à l'infini), et correspondra au nombre de clés possédées par le héros.

#### b) Les variables globales ####

/!\ Je préfère le préciser tout de suite : l'utilisation de variables globales est très déconseillée, car elles risquent de rendre votre code beaucoup moins lisible pour les autres utilisateurs.

L'initialisation des variables globales se fait sensiblement de la même manière que pour les locales, on retire juste le mot "local". Ces variables seront ensuite accessibles depuis l'intégralité des fichiers de votre quête.

**exemples :**

`vie_heros` correspondra à la quantité de PV de votre héros. Dans un jeu Zelda, cette valeur sera de 12 au départ (3 coeurs, chacun divisé en 4 parties).

`pourcentage` indiquera l'avancée de la quête.

Dans solarus, il existe par défaut une seule variable globale : il s'agit de la variable `sol`. Ainsi, on peut accéder à toutes les fonctions dont le nom commence par "sol." dans la documentation depuis l'intégralité de nos fichiers.

## III) Les événements ##

Si vous allez jeter un oeil dans le fichier `main.lua`, vous pourrez remarquer la création d'une fonction nommée `sol.main:on_key_pressed`. Cette fonction servira à gérer les événements que le joueur enverra grâce à son clavier (la gestion des touches, si vous préférez) et modifiera les données du jeu en conséquence. Par exemple, dans cette fonction, une des conditions nous indique que si le joueur appuye sur les touches alt et F4 simultanément, on quittera le jeu.

### Exercice : Une touche, un son ###

Pour finir, je vais vous proposer un petit exercice assez simple : essayez, par exemple, de faire en sorte que l'on entende le son "secret" se déclencher lorsqu'on appuye sur la touche G. Toutes les données dont vous avez besoin sont dans le sous-menu "audio" de la documentation et dans la fonction on\_key\_pressed.

Solution : 

Juste avant le "end" dans la fonction `on_key_pressed`, ajoutez les lignes suivantes :

elseif key == "g" then
	sol.audio.play_sound("secret")
	handled = true