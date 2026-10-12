# Catch the Light

Projet d'option Maker de SERAPHIN Bradley : réalisation d'une borne inspirée du jeu d'arcade **Catch the Light**. Le dépôt rassemble le firmware embarqué, les conceptions électroniques, les pièces de fabrication et quelques médias du projet.

![Aperçu du projet Catch the Light](media/CatchTheLight-600x600_GF.jpg)

## Principe du jeu

La borne propose une partie à deux joueurs. Chaque joueur dispose de dix boutons associés à dix voyants. Pendant la partie, des voyants s'allument : il faut appuyer sur le bouton correspondant à un voyant allumé pour marquer un point. Une pression sur un voyant éteint retire un point, sans faire descendre le score sous zéro. À la fin, les voyants du joueur gagnant clignotent ; en cas d'égalité, tous les voyants clignotent.

Le comportement décrit ci-dessus correspond à l'implémentation présente dans le firmware :

- En attente, une animation lumineuse est affichée. Le bouton **START** démarre la partie.
- Une partie dure **15 secondes**.
- Le firmware tente de choisir une nouvelle cible pour chacun des joueurs environ toutes les **300 ms**.
- Les boutons sont scrutés avec un anti-rebond logiciel de **50 ms**.
- Après l'affichage du résultat pendant environ **5 secondes**, le jeu revient en attente.

## Matériel et logiciel

- **Microcontrôleur :** STM32G474RETx (famille STM32G4).
- **Firmware :** C, STM32 HAL et CMSIS, projet préparé pour STM32CubeIDE.
- **Entrées de jeu :** 20 boutons, répartis entre deux joueurs, plus un bouton START.
- **Sorties de jeu :** 20 voyants individuels.
- **Affichage auxiliaire :** le code contient également des fonctions de commande de LEDs RGB de type WS2812B pour des chiffres à sept segments.
- **Conception et fabrication :** schémas et routage KiCad, modèles 3D STL/STEP et fichiers de découpe laser DXF.

Les fichiers de conception comportent plusieurs variantes et sauvegardes. Vérifier la version, les dimensions et les empreintes avant de fabriquer ou de commander des composants.

## Organisation du dépôt

| Dossier ou fichier | Contenu |
| --- | --- |
| [`Software`](Software/CatchTheLight/) | Projet STM32CubeIDE : sources dans `Core/Src` et `Core/Inc`, configuration STM32CubeMX (`CatchTheLight.ioc`) et pilotes STM32 dans `Drivers`. |
| [`PCB/Catch_The_Light/`](PCB/Catch_The_Light/) | Schémas, carte principale KiCad, aperçu 3D, nomenclature et fichiers d'export associés. |
| [`PCB/Effect_Hall`](PCB/Effect_Hall/) | Conception KiCad d'une carte liée à l'effet Hall, avec ses variantes et exports. |
| [`Impression 3D`](Impression_3D/) | Pièces imprimables STL et modèle STEP : boîtier, bouton, supports, afficheur et pièces associées. |
| [`Découpe Laser`](Decoupe_Laser/) | Plans DXF de pièces destinées à la découpe laser. |
| [`Media`](media/) | Images, GIF et vidéo de présentation ou de suivi du projet. |


Des README complémentaires peuvent se trouver dans certains sous-dossiers ; les dossiers matériels contiennent aussi des exports et sauvegardes de conception.

## Ouvrir et compiler le firmware

1. Installer **STM32CubeIDE** et les outils STM32 nécessaires à la carte utilisée.
2. Dans CubeIDE, importer un projet existant depuis le dossier `Software/CatchTheLight/`.
3. Vérifier la configuration de la cible et du débogueur/programmeur pour le matériel disponible.
4. Compiler le projet, puis le flasher et le déboguer avec l'interface adaptée à la carte.

Les sources applicatives sont principalement dans `Software/CatchTheLight/Core/Src/` :

- `main.c` initialise le microcontrôleur et appelle la boucle du jeu.
- `utils.c` contient les états de la partie, le choix des cibles, les scores et la gestion du gagnant.
- `button.c` scanne les boutons et traite les appuis.
- `led.c` commande les voyants et les animations.
- `rgb.c` contient la génération de motifs pour l'afficheur RGB.

Les pilotes HAL/CMSIS sont inclus dans le projet sous `Software/CatchTheLight/Drivers/`. La compilation ou l'exécution nécessite toutefois une installation locale de CubeIDE et le matériel STM32 adapté.

## État du projet

Le dépôt conserve des fichiers de travail et des variantes de conception ; il s'agit d'un projet Maker en développement, pas d'un kit de fabrication dont chaque version serait validée. Le bouton START et les entrées/sorties de la partie principale sont présents dans le firmware ; certaines broches et fonctions matérielles visibles dans les schémas ou la configuration peuvent ne pas être utilisées par la boucle de jeu actuelle. Vérifier le comportement réel sur la carte avant une démonstration ou une fabrication.

## Rétro-planning initial

| Séance | Programme | Objectif |
| --- | --- | --- |
| 1 | Programmation | Réalisation du programme |
| 2 | PCB | Concevoir le PCB |
| 3 | Modélisation | Fabrication du corps avec découpe laser |
| 4 | Mécanique | Assemblage du corps |
| 5 | PCB | Soudure du PCB |
| 6 | Mécanique | Assemblage final |
| 7 | Test | Test du projet |
| 8 | Soutenance | Présentation finale |
