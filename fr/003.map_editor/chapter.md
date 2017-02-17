# Chapitre 3 - L'éditeur de maps #

Si vous avez déjà tenté d'éxécuter une quête, vous avez du apercevoir un personnage pouvant se déplacer à l'intérieur d'une salle vide. Cette salle est générée à partir d'un type de ressource nommé les maps. Ce que nous allons faire à présent, à partir des ressources ajoutées au projet dans le chapitre précédent, c'est de créer nous aussi une map, une zone dans laquelle notre personnage pourra se déplacer.

/!\/!\/!\ Ce chapitre et les suivants utiliseront les ressources que nous avons importées dans le chapitre précédent. /!\/!\/!\

## 0) Une map ? Késako ? ##

Une map se résume à un ensemble d'entités de différents types, qui vont être gérées simultanément par le programme.

Les tiles, les coffres ou encore les personnages sont des entités. Il en existe d'autres que nous verrons plus tard.

Chaque entité possède plusieurs données : une couche d'affichage, des coordonnées d'affichage, une taille et un motif. De plus, la plupart des entités (toutes sauf les tiles) possèdent également un nom, ce qui permet de les modifier à partir du code du jeu.

Tout ces points seront vus plus en détail plus tard, concentrons-nous d'abord sur la création d'une map. 

## I) Initialisation d'une map ##

Dans l'arbre de votre quête, vous aurez surement remarqué la présence de nombreuses ressources (sprites, sons, musiques, ...). Nous allons nous intéresser au dossier `maps` visible à la racine du projet.
![](images/maps_file.png)

Faites donc un clic droit dessus, et sélectionnez l'option "nouvelle map".

Une nouvelle fenêtre va alors s'ouvrir, vous demandant un id pour la map, ainsi qu'une description de celle-ci. Par exemple, si l'on voulait créer la map d'une maison, on pourrait utiliser l'ID `maison1` et la description `maison d'un personnage`, ce qui donnerait ça :
![](images/new_map.png)

Une fois ces données remplies, validez votre sélection. Vous allez avoir un nouvel onglet que va s'ouvrir, vous permettant de modifier à loisir les données de votre nouvelle ressource :
![](images/map_tab.png)

Si vous n'avez pas importé les données du chapitre précédent, vous ne devriez pas avoir d'image au centre de la fenêtre. Importez un tileset pour corriger ça.

Dans cet onglet, on peut distinguer 4 zones distinctes, représentées par les quatres cadres rouges.

- En haut à gauche, les données de la map : sa description, ses dimensions en pixels (320*240 par défaut, ce qui correspond à ce que l'on appelle la "taille de la quête", la taille de la partie de la map affichée dans la fenêtre), le nombre de couches sur lesquelles vous pourrez superposer vos motifs, le tileset utilisé, etc.
- En bas à gauche, l'image du tileset utilisé. Ce tileset est divisé en motifs de tiles, qui seront utilisés pour créer des tiles sur votre map.
- En haut à droite, des entités légèrement différentes, qui vous permettront par exemple de créer des portes ou de disposer des trésors ou des ennemis.
- Enfin, en bas à droite, la zone de dessin de votre map, là où vous allez directement la mettre en forme.

## 2) Création d'un décor ##

Pour le moment, votre map est complètement vierge. Il vas nous falloir y remédier. Pour cela, nous allons utiliser deux des quatre zones citées précédemment : la vue graphique du tileset et la surface de dessin.

Si vous essayez de cliquer dans la zone de dessin d'une map déjà existante, une partie du dessin va s'encadrer en vert. Chacune de ces parties s'appelle un "tile", composé d'une partie du tileset (un "motif"), répétée un certain nombre de fois. Par exemple, dans le tileset, les images des murs à droite et à gauche des salles existantes ne font que 16 pixels de haut, mais en les répétant, on peut créer un mur de 160 pixels de haut.

Attention : certains motifs sont configurés de manière à ne pas pouvoir être répétés. Les coins des maisons en sont un exemple. Cette particularité sera vue plus en détail lors du chapitre sur les tilesets

Dans la suite de ce chapitre, nous allons utiliser le tileset nommé "House", mais vous êtes libre d'utiliser celui que vous voulez.

Pour ajouter une tile à votre map, il vous faudra donc la sélectionner dans la vue du tileset, puis la disposer à votre convenance sur la surface de dessin. Si nous essayons de créer un sol, pour ne plus avoir de surface "vide" (sans dessin dessus), on devrait obtenir quelque chose comme ceci :
![](images/floor_added.png)

Il existe plusieurs motifs de sol, libre à vous de choisir celui que vous voulez.

**astuces**

- Garder le clic enfoncé vous permet de redimensionner votre motif. Le sélectionner et appuyer sur la touche `r` donne le même résultat.
- faire un clic droit pour poser un motif vous permet d'en recréer un autre immédiatement, pratique pour les motifs dispersés dans la salle (des lanternes, par exemple).
- Vous pouvez également déplacer les motifs que vous avez disposé sur votre map en faisant un cliquer-glisser (drag'n drop) sur ce motif, et en sélectionner plusieurs en appuyant sur la touche `ctrl`.
- Faire un clic molette vous permet de vous déplacer dans la zone de dessin ou la vue du tileset plus facilement

### Exercice : créer sa propre map ###

Maintenant que vous savez comment vous y prendre pour disposer les motifs de tiles correctement, essayez de faire en sorte que votre salle soit fermée par quatres murs, de manière à ce qu'on ne voie pas de coupure nette entre eux.
Voici un exemple ou les murs ne sont pas bien reliés :
![](images/bad_walls.png)

Pour corriger celà, il faut penser à utiliser les tiles de "coins", qui vous permettent de corriger ces défauts.

Exemple de corrigé :

![](images/good_walls.png)
Ici, les murs sont bien reliés entre eux, on ne voit plus de cassure aux quatre coins de la map.

## 3) Les entités dynamiques ##

Maintenant que les tiles (entités "statiques") sont placées, nous allons pouvoir parler rapidement des entités  dynamiques, 

/!\ Nous n'allons pas voir en détail tous les types d'entités ici, mais principalement ceux des destinations.

Les entités sont des éléments de maps pouvant être modifiées par le programme, aussi bien au niveau de leur position que de leur apparence.

Par exemple, un item laissé au sol est une entité, qui influera par la suite sur l'équipement possédé. Un personnage est également une entité avec laquelle on pourra interagir.

Comme dit plus haut, les entités que nous verrons ici seront seulement celles des destinations, c'est à dire celles ayant l'image encadrée en rouge ici:
![](images/destination.png)

Une destination correspond à la position qu'aura le héros après un changement de zone ou au lancement du jeu.

Ajoutez donc une nouvelle destination dans votre nouvelle map, pour pouvoir démarrer directement dessus.

Changer la destination d'origine du jeu ne se gère cependant pas seulement en ajoutant cette destination. Ouvrez le fichier  `initial_game.lua`, et ajoutez la ligne suivante à la fin de la fonction `start_game` en remplaçant "maison1" par le nom de la map que vous venez de créer:

`game:set_starting_location("maison1")`

Si tout s'est bien passé, au lancement du jeu, vous devriez arriver sur la map que vous avez créée tout au long de ce chapitre, à l'endroit exact où se situe votre entité de destination.

## 4) Les couches d'affichage ##

Vous l'aurez probablement déjà remarqué dans ce type de jeu, il arrive parfois que le personnage passe derrière certains éléments du décor (une maison, un arbre, et bien plus encore). Cet affichage est géré par le système de couches.

Par défaut, chaque motif de tiles a une couche d'affichage prédéfinie. Cette couche détermine la position des tiles constituées de ce motif en particulier par rapport aux autres entités.

Pour donner un exemple rapide, sur une map comme celle-ci, le héros se situe à la couche 0. Le programme affiche donc toutes les tiles qui sont sur la couche 0, puis le héros, et ensuite les tiles qui sont sur les couches 1 et 2. 

Si vous voulez savoir rapidement quelles tiles sont sur une couche prédéfinie, certains boutons en haut de la fenêtre permettent d'afficher/retirer les tiles de la couche correspondante.
![](images/layers.png)

### Exercice de fin de chapitre ###

Pour finir cette initiation, je vous propose un petit exercice d'application : commencez par créer une nouvelle map, de la taille de votre choix, et sélectionnez le tileset "Light World" dans la liste, ainsi que la musique "Overworld" pour donner un peu plus de gaité à votre quête.

Faites un sol constitué d'herbe, et ajoutez quelques arbres dans cette zone.

Attention, les feuillages des arbres ne sont pas des tiles bloquants. Si vous lancez leu jeu et passez près d'eux, vous pouvez déplacer votre personnage à l' "intérieur" des feuillages des arbres.

En effet, les feuillages sont prévus pour être affichés à la couche 1 de la map, leurs collisions n'ont donc aucun effet sur le héros.

Pour corriger ce problème, un motif noir est fourni dans le tileset. Celui-ci est prévu pour être placé à la couche 0, en dessous des feuillages. Il s'agit du motif suivant (encadré en bleu sombre):

![](images/tree_obstacle.png)

Si l'on regarde la map en supprimant la couche intermédiaire (celle où sont les feuillages), on voit ça :

![](images/tree_no_obstacle.png)

Pour corriger ce problème de collisions, on va ajouter ce motif noir au milieu de chaque arbre, ce qui donne ça :

![](images/tree_obstacle_placed.png)

En réaffichant la couche intermédiaire, vous pouvez vous apercevoir que les rectangles noirs que l'on vient de placer sont totalement invisibles. Pourtant, si on lance le jeu, il n'est plus possible de passer au travers des arbres.

Pour finir, une partie un peu plus technique : Essayez, à partir des motifs fournis, d'assembler une maison ailleurs dans la map.

Voici un exemple du rendu que vous pourriez avoir :

![](images/final_ex_map.png)

Si vous avez un rendu semblable, vous pouvez sans peine passer à l'étape suivante. 