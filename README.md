# ProjetsOpenCV

Piano virtuel simple / Dessin virtuel simple / Piano Tiles automatique 

# I-	Piano virtuel

Piano simplifié de sept touches DO RE MI FA SOL LA SI 

*Fonctionnalités* :

•	Possibilité de jouer avec les deux mains

•	Arrêt des notes si la main n’est plus sur le clavier

•	Pour une même main, si elle change de touche, la note s’arrête et passe à la suivante

•	Pour une deuxième main, la note jouée par la 1ère se superpose à la note jouée par la 2ème 

*Problèmes *:

•	Latence de la caméra

•	Saturation du son

On cherche tout d’abord à ce que le programme sache détecter une main : on utilise la bibliothèque *mediapipe*.

•	On convertit l’image en RGB

•	On utilise la fonction *process()* qui récupère les informations sur les mains. Si cette fonction renvoie une information non nulle, alors il y a une ou plusieurs mains à l’écran

•	Pour chaque main détectée, on affiche les coordonnées des 21 marqueurs qui la compose

•	On récupère une liste contenant chaque coordonnée : si deux mains sont à l’écran, la liste contiendra 41 éléments que l’on divisera en deux listes pour contrôler chaque main

•	On utilise la fonction *draw_landmarks()* pour la dessiner à l’écran avec ses connections

On télécharge ensuite 7 notes de musique DO RE MI FA SOL LA SI que l’on fera jouer grâce à la bibliothèque *SimpleAudio*, et on dessine en haut de l’écran 7 rectangles collés les uns aux autres représentant les touches.

Pour chaque touche voici le procédé utilisé : 

•	On regarde si l’un des index des deux mains est au niveau des coordonnées de la touche DO

•	Si c’est la main n°1, alors on indique qu’elle ne se trouve plus sur les autres touches. Idem si c’est la main n°2.

•	Si la main n°1 et la main n°2 n’étaient pas sur DO avant, alors on joue le son. On indique ensuite que la main n°1 ou la main n°2 est sur cette touche. On évite ainsi que le son se joue en permanence quand on maintient appuyé ou si les deux mains viennent sur la même touche.

•	Juste avant avoir joué le son, on coupe la note de piano qui était en train de joué avant si nécessaire : Si la main n°1 avait appuyé sur une touche puis avait changé de touche.Pour cela, on note la touche sur laquelle est passée est la main n°1 et on éteindra cette note au prochain passage sur autre touche.

•	Si la main n°2 appuie sur une autre touche que la main n°1, ayant des variables différentes que celle-ci, elle n’aura pas sa note coupée. Il y aura superposition des notes.




# II-	Dessin virtuel

Zone de dessin 

*Fonctionnalités* :

•	Choix de trois couleurs de crayon

•	Choix de trois épaisseurs du crayon

•	Dessin sur tableau blanc ou sur caméra

*Problème* :

•	Latence de la caméra

Même procédé pour détecter les mains à l’écran que pour AirPiano. 

Pour dessiner à l’écran :

•	On charge l’image de notre caméra ainsi qu’une image de tableau blanc

•	Pour chacune d’entre elles, on rajoute un bandeau à son sommet où sont dessinées les différentes couleurs et tailles de pinceau

Si le majeur et l’index sont levés ( utilisation de *math.dist* pour voir la distance entre eux) :

•	On ne dessine alors pas à l’écran mais l’on regarde les cordonnées de l’index

•	Si celles-ci sont proches de l’un des pinceaux de différentes couleurs et tailles du bandeau au sommet de l’image, alors, notre variable de couleur et de tailles sont modifiées.

Si le majeur est baissé et l’index levé :

•	On change alors la couleur des pixels autour de l’index sur le tableau blanc, de la couleur et de la taille choisis préalablement. 

•	On utilise la fonction *bitwise_and()* entre le tableau blanc et l’image de notre caméra pour que le dessin apparaisse également dessus

Remarque : L’image de la caméra s’actualise en permanence et la couleur dessinée par notre main s’effacerait à chaque fois. Le tableau blanc est une image chargée qui conserve le dessin et qui peut le calquer sur l’image de la caméra à chaque fois.

![Picture1](https://user-images.githubusercontent.com/92324336/175810349-7c424c4a-4a51-460c-ac88-2507f6c8427d.png)
![Picture2](https://user-images.githubusercontent.com/92324336/175810351-03e61aa8-e653-4bba-ac8d-a8f9e00da785.png)

 

# III-	Piano Tiles automatique

Jeu Piano Tiles joué automatiquement pour battre son record.

Pour capturer l’écran de son ordinateur, on utilise la bibliothèque *PIL* et *numpy* : pip install pillow numpy

•	On capture l’image que l’on met dans un tableau
*img = ImageGrab.grab()*

*img_np = np.array(img)*

•	On modifie le type de l’image en HSV pour pouvoir lui appliquer un filtre de couleur avec la fonction *inRange((0,0,0,0)(180,155,30,0))*. On cherche à isoler le noir qui correspond aux tuiles du jeu.

•	On récupère les contours du masque qui correspondent uniquement aux tuiles, on vérifie que le résultat est non nul puis on récupère les coordonnées du contour principal détecté 

•	Dès qu’un contour de tuiles est détecté, la souris se déplace au niveau de ses cordonnées (que l’on adapte à notre écran) puis clique avec *Pynput*.

 ![ezgif com-gif-maker (3)](https://user-images.githubusercontent.com/92324336/175808652-d8dba42f-1a4d-4e04-b1b6-a62775e833d1.gif)

