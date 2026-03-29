# Introduction

Qui n’a jamais débattu avec ses amis pour savoir quel jeu vidéo est le meilleur de tous les temps ? Entre les fans de LOL *(quelle idée…)*, les nostalgiques de Mario et les adeptes de Minecraft, les opinions sont souvent… très tranchées.  

Mais le jeu vidéo ne se résume pas uniquement à ses ventes : derrière chaque succès commercial se cache aussi une communauté de joueurs, parfois très engagée. Certains terminent un jeu… d’autres cherchent à le finir le plus vite possible.

Dans ce projet, nous avons choisi de croiser deux aspects complémentaires du jeu vidéo :

- les **performances commerciales** avec le dataset **Video Game Sales** ;
- l’**activité compétitive et communautaire** avec le dataset **Speedrun.com Data**, qui recense les records, les joueurs et les catégories de speedrun.

L’objectif est de ne pas se limiter à une vision économique du jeu vidéo, mais d’explorer également l’engagement des joueurs. En combinant ces deux sources, nous cherchons à comprendre si les jeux les plus vendus sont aussi ceux qui génèrent le plus d’activité dans la communauté, ou si, au contraire, certains jeux deviennent cultes indépendamment de leur succès commercial.

---

# Données

Dans le cadre de ce projet, nous utilisons deux jeux de données complémentaires afin d’analyser à la fois le **succès commercial** des jeux vidéo et leur **engagement au sein de la communauté**.

## Dataset 1 : Video Game Sales (Rush Kirubi)

Ce dataset combine des données de ventes issues de [**VGChartz**](https://www.vgchartz.com/) et des scores/avis issus de [**Metacritic**](https://www.metacritic.com/).  

Il est important de noter que le fichier date du **22 décembre 2016**, et ne contient donc pas des données récentes.

Le fichier principal est un **CSV** (*Video_Games_Sales_as_at_22_Dec_2016.csv*), de dimension **(16 719 × 16)**.  

Cependant, le jeu de données présente de nombreuses valeurs manquantes, notamment pour les variables liées aux scores utilisateurs. En effet, la plateforme Metacritic ne couvre pas toutes les plateformes présentes dans le dataset. Ainsi, environ **9 600 observations** disposent de données complètes.

---

### Dictionnaire de variables et qualité attendue

| Colonne           | Nature                                   | Type    | % NA   | Uniques | Exemples                          |
|-------------------|------------------------------------------|---------|--------|---------|-----------------------------------|
| Name              | qualitative nominale (texte)             | object  | 0.02%  | 11 563  | Wii Sports; Super Mario Bros.      |
| Platform          | qualitative nominale (catégorie)         | object  | 0.00%  | 31      | Wii; NES                          |
| Year_of_Release   | quantitative discrète (année)            | float64 | 1.61%  | —       | 2006.0; 1985.0                    |
| Genre             | qualitative nominale (catégorie)         | object  | 0.02%  | 12      | Sports; Platform                  |
| Publisher         | qualitative nominale (catégorie)         | object  | 0.32%  | 582     | Nintendo                          |
| NA_Sales          | quantitative continue (millions)         | float64 | 0.00%  | —       | 41.36; 29.08                      |
| EU_Sales          | quantitative continue (millions)         | float64 | 0.00%  | —       | 28.96; 3.58                       |
| JP_Sales          | quantitative continue (millions)         | float64 | 0.00%  | —       | 3.77; 6.81                        |
| Other_Sales       | quantitative continue (millions)         | float64 | 0.00%  | —       | 8.45; 0.77                        |
| Global_Sales      | quantitative continue (millions)         | float64 | 0.00%  | —       | 82.53; 40.24                      |
| Critic_Score      | quantitative discrète (0–100)            | float64 | 51.33% | —       | 76.0; 82.0                        |
| Critic_Count      | quantitative discrète (compte)           | float64 | 51.33% | —       | 51.0; 73.0                        |
| User_Score        | quantitative continue (0–10 + "tbd")     | object  | 40.10% | 96      | 8; 8.3; tbd                       |
| User_Count        | quantitative discrète (compte)           | float64 | 54.60% | —       | 322.0; 709.0                      |
| Developer         | qualitative nominale (catégorie)         | object  | 39.61% | 1 696   | Nintendo                          |
| Rating            | qualitative (catégorie ESRB)             | object  | 40.49% | 8       | E                                 |

---

### Remarques

- Les scores critiques de Metacritic sont sur une échelle de 0 à 100  
- De nombreuses données liées aux scores sont manquantes, mais le volume restant reste suffisant pour mener des analyses pertinentes
- Les variables **User_Score** et **Year_of_Release** contiennent des valeurs non numériques (*tbd*, *N/A*), nécessitant un nettoyage préalable
- Chaque ligne correspond à un **jeu sur une plateforme donnée**. Ainsi, un même jeu peut apparaître plusieurs fois, ce qui peut biaiser certaines analyses si l’on ne précise pas l’unité d’étude
- Les noms de développeurs, éditeurs ou plateformes peuvent présenter des variations (abréviations, doublons), ce qui nécessite un travail de normalisation, notamment pour le croisement avec d’autres datasets

---

### Unité d’observation et sous-groupes

L’unité d’observation est le couple **(jeu, plateforme)** : un même titre peut apparaître plusieurs fois s’il est sorti sur différentes plateformes.

Cela permet de définir plusieurs sous-groupes :

- **Plateforme** (≈ 31 plateformes)  
- **Genre** (≈ 12 catégories)  
- **Région de vente** (NA, EU, JP, Other)  
- **Rating ESRB** (classification par âge)  

Ces dimensions structurent fortement les analyses possibles.


## Dataset 2 : Speedrun.com Dataset
 
Ce dataset recense les jeux référencés sur [**Speedrun.com**](https://www.speedrun.com/), la plateforme de référence pour le speedrunning. Il fournit pour chaque jeu des métadonnées générales ainsi que des indicateurs d'activité communautaire (nombre de runs soumises, nombre de joueurs actifs).
 
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

# Plan d’analyse

Notre plan d’analyse vise à étudier le jeu vidéo sous deux angles complémentaires : son **succès commercial** et son **engagement communautaire à travers la discipline du speedrunning**.  

L’idée, dans le cadre de ce projet d'analyse, n’est pas seulement d’identifier les jeux qui se vendent le mieux, mais aussi de comprendre quels types de jeux suscitent une forte activité de la part des joueurs, parfois indépendamment de leurs performances économiques.

Nous découperons donc notre analyse en plusieurs axes.

---

## 1. Analyse descriptive du marché du jeu vidéo

Dans un premier temps, nous étudierons uniquement le dataset **Video Game Sales** afin de dégager les grandes tendances du marché du jeu vidéo, en gardant à l’esprit que ces données s’arrêtent au **22 décembre 2016**, date de mise à jour du dataset.

Nous chercherons notamment à répondre aux questions suivantes :

- Quelles **plateformes** ont généré le plus de ventes à l’échelle mondiale ?
- Quels sont les **genres** de jeux vidéo qui génèrent le plus de ventes ?
- Quelle région (**Amérique du Nord**, **Europe**, **Japon**, **autres régions**) contribue le plus aux ventes de jeux vidéo ?
- Observe-t-on des différences régionales dans les préférences de **plateformes** ou de **genres** ?
- Quels jeux sont sortis sur le **plus grand nombre de plateformes** ?

Cette première partie reposera principalement sur les variables `Platform`, `Genre`, `Name`, `NA_Sales`, `EU_Sales`, `JP_Sales`, `Other_Sales` et `Global_Sales`.

L’objectif sera ici de dresser un portrait global du marché à l’aide de classements, d’agrégations et de visualisations simples.

---

## 2. Analyse descriptive de l’activité speedrun

Dans un second temps, nous analyserons uniquement le dataset **Speedrun.com** afin de mieux comprendre la structure de la communauté de speedrunning.

Nous nous poserons notamment les questions suivantes :

- Quels jeux comptent le plus de **runs** ?
- Quels jeux attirent le plus de **joueurs actifs** ?
- Les jeux les plus anciens sont-ils davantage représentés dans la communauté speedrun que les jeux récents ?

Cette partie s’appuiera principalement sur les variables `gameName`, `releaseDate`, `addedDate`, `runCount` et `playerCount`.

Nous chercherons ici à dégager des tendances générales sur la pratique du speedrunning, notamment :

- la **popularité** de certains jeux au sein de la communauté.
- le lien entre **l’ancienneté d’un jeu** et son niveau d’activité dans la communauté.

---

## 3. Croisement entre ventes et activité communautaire

Cette partie constitue le cœur du projet, car elle permettra de relier les deux jeux de données sélectionnés.

Nous chercherons à déterminer si les jeux les plus vendus sont également ceux qui génèrent le plus d’activité sur Speedrun.com, ou si certains jeux deviennent des références dans la communauté sans avoir nécessairement connu un grand succès commercial.

Les principales questions seront les suivantes :

- Les jeux les plus vendus sont-ils aussi ceux qui ont le plus de **runs** ?
- Existe-t-il une relation entre les **ventes globales**  et le nombre de **joueurs impliqués** dans la communauté speedrun ?
- Certains jeux peu vendus deviennent-ils néanmoins très importants dans le speedrunning ? Et à l'inverse ?

Pour cela, il sera nécessaire d’effectuer une **jointure entre les deux datasets** à partir du **nom** des jeux, après un travail de **normalisation** (gestion des différences d’écriture, ponctuation, versions, etc.).

Cette étape permettra d’identifier plusieurs profils de jeux :

- les jeux à **fort succès commercial et forte activité communautaire**.
- les jeux à **fort succès commercial mais faible activité speedrun**.
- les jeux à **faible succès commercial mais forte activité communautaire**, pouvant être considérés comme des jeux “cultes”.
- les jeux à **faible succès commercial et faible activité**, moins marquants à la fois économiquement et communautairement.

---

## 4. Analyse par sous-groupes : genres et plateformes

Une fois les données croisées, nous approfondirons l’analyse en étudiant certains sous-groupes.

Nous chercherons par exemple à répondre aux questions suivantes :

- Quels **genres** de jeux attirent le plus les speedrunners (en termes de run ou joueurs impliqués) ?
- Certaines **plateformes** sont-elles davantage associées à des jeux speedrunnés ?
- Les jeux sortis sur de nombreuses plateformes ont-ils davantage de chances d’être speedrunnés ?
- Certains genres se vendent-ils très bien sans pour autant générer une forte activité communautaire ?

Cette partie permettra de dépasser la simple comparaison “ventes vs runs” et d’identifier des tendances plus fines selon la nature des jeux et leurs caractéristiques.

---

## 5. Ce que nous espérons montrer

À travers ce projet, nous espérons montrer que la popularité d’un jeu vidéo ne se mesure pas uniquement par ses ventes. En effet, un jeu peut être un immense succès commercial sans pour autant marquer durablement une communauté compétitive, tandis qu’un autre peut devenir une référence du speedrunning malgré des ventes plus modestes.

Nous souhaitons ainsi mettre en évidence :

- les jeux qui cumulent **succès économique** et **fort engagement communautaire**.
- les jeux qui doivent leur notoriété davantage à leur **communauté** qu’à leurs ventes.
- les caractéristiques des jeux qui semblent favoriser la pratique du speedrunning.

---

## 6. Difficultés et limites attendues

### Jointure des deux datasets

Le principal défi sera de faire correspondre les jeux entre les deux sources via leurs noms. En effet, de nombreuses différences peuvent exister :

- différences d’écriture.
- présence ou absence de ponctuation.
- abréviations ou noms alternatifs.
- versions ou éditions différentes d’un même jeu.
- distinctions liées aux plateformes.

Un travail de normalisation sera donc indispensable afin d’obtenir un maximum de correspondances fiables entre les deux datasets.

---

### Différence d’unité d’observation

Dans le **dataset de ventes**, une observation correspond à un couple **(jeu, plateforme)**, tandis que dans le **dataset Speedrun.com**, une observation correspond à un **jeu unique**.

Cela peut introduire des biais et nécessitera une harmonisation préalable des données avant toute comparaison.

---

### Données incomplètes ou limitées

Certaines limites sont liées directement aux datasets utilisés :

- le dataset de ventes s’arrête en **2016**, ce qui exclut les jeux récents.
- certaines variables présentent des **valeurs manquantes** (notamment les scores dans le dataset de ventes).

---

### Distribution très déséquilibrée

L’activité de speedrun est fortement concentrée sur un petit nombre de jeux très populaires, tandis que la majorité des jeux ont très peu de runs.

Cela peut rendre certaines analyses globales moins représentatives et nécessitera une interprétation prudente des résultats.

Certaines variables du dataset de ventes contiennent de nombreuses valeurs manquantes, notamment pour les scores critiques et utilisateurs. Cela pourra limiter certaines analyses complémentaires.

---
