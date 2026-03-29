## Dataset 2 : Speedrun.com Dataset
 
Ce dataset recense les **jeux référencés sur Speedrun.com**, la plateforme de référence pour le speedrunning. Il fournit pour chaque jeu des métadonnées générales ainsi que des indicateurs d'activité communautaire (nombre de runs soumises, nombre de joueurs actifs).
 
Le fichier est un **CSV** (*games-data.csv*), de dimension **(44 122 × 9)**.
 
Le dataset couvre des jeux dont les dates de sortie s'étendent de **1971 à 2026** (dates futures correspondant à des jeux annoncés), et dont l'ajout sur la plateforme Speedrun.com s'échelonne de **fin 2014 à début 2025**.
 
---
 
### Dictionnaire de variables
 
| Colonne       | Nature                                      | Type   | % NA  | Uniques | Exemples                                    |
|---------------|---------------------------------------------|--------|-------|---------|---------------------------------------------|
| gameId        | qualitative nominale (identifiant unique)   | string | 0.00% | 43 622  | k6qqkx6g ; 3dx2pk41                         |
| gameName      | qualitative nominale (texte)                | string | 0.00% | 43 602  | Super Mario 64 ; Celeste                    |
| url           | qualitative nominale (slug URL)             | string | 0.00% | 43 622  | sm64 ; celeste                              |
| type          | qualitative nominale (catégorie)            | string | 0.00% | 1       | game                                        |
| releaseDate   | quantitative discrète (timestamp UNIX)      | int64  | 0.00% | 9 953   | 1044144000 (2003) ; 1588032000 (2020)       |
| addedDate     | quantitative discrète (timestamp UNIX)      | int64  | 0.00% | 42 857  | 1464477672 (2016) ; 1718636541 (2024)       |
| runCount      | quantitative discrète (compte)              | int64  | 0.00% | 1 522   | 47 395 ; 10                                 |
| playerCount   | quantitative discrète (compte)              | int64  | 0.00% | 664     | 8 105 ; 4                                   |
| rules         | qualitative nominale (texte libre)          | string | 0.00% | 18 651  | *(règles de soumission des runs)*           |
 
---
 
### Remarques
 
- Les dates (**releaseDate**, **addedDate**) sont stockées en **timestamps UNIX** (secondes depuis le 1er janvier 1970) et devront être converties en dates lisibles lors du nettoyage
- La colonne **type** ne contient qu'une seule valeur (*game*), elle n'apportera donc pas d'information discriminante pour l'analyse
- La colonne **rules** contient le texte brut des règles de speedrun ; elle n'apportera également pas d'information discriminante pour l'analyse
- **1 006 jeux** n'ont aucune run soumise, et **1 005** n'ont aucun joueur associé, ce qui suggère des entrées inactives ou récemment ajoutées
- La distribution des runs est **très asymétrique** : la médiane est de 10 runs par jeu, mais le maximum atteint 62 655 (Seterra), ce qui reflète une forte concentration de l'activité sur quelques jeux populaires
- Les jeux les plus actifs incluent des titres emblématiques du speedrunning : **Super Mario 64**, **Celeste**, **Minecraft: Java Edition**, **Super Mario Odyssey**
 
---
 
### Unité d'observation et sous-groupes
 
L'unité d'observation est le **jeu** : chaque ligne correspond à un titre unique référencé sur Speedrun.com.
 
Les sous-groupes exploitables sont :
 
- **Période de sortie** (par décennie ou par année, via conversion de `releaseDate`)
- **Ancienneté sur la plateforme** (via `addedDate`, pour analyser la dynamique d'adoption)
- **Niveau d'activité** (en regroupant les jeux selon leur `runCount` soumises ou leur `playerCount`, par exemple : faible, moyen, élevé)




### Croisement des datasets et normalisation des noms


Pour exploiter pleinement la complémentarité de nos deux sources, il sera nécessaire de les croiser en faisant correspondre les jeux du dataset de ventes avec ceux du dataset Speedrun.com. Le point de jonction naturel entre les deux tables est le **nom du jeu**.


Cependant, cette jointure ne sera pas immédiate : les noms de jeux peuvent varier d'un dataset à l'autre en raison de différences de formatage, d'abréviations, de suffixes de version ou encore de conventions d'écriture propres à chaque source. Par exemple, un jeu nommé *"Super Mario Bros."* dans le dataset de ventes pourrait apparaître comme *"Super Mario Bros"* (sans point) ou *"Super Mario Brothers"* sur Speedrun.com. Un travail de **normalisation des noms** sera donc indispensable avant toute analyse croisée, afin de maximiser le nombre de correspondances fiables entre les deux tables.


Ce processus de nettoyage conditionnera directement la qualité et la fiabilité des analyses croisant performances commerciales et activité communautaire.
