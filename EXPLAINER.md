# Datathon PolyFinances 2022

## Data preprocessing

- Dans le fichier `/utils/agglomerate.ipynb` nous avons rassemblé les différents fichiers de données important en un seul fichier csv qui se retrouve dans `/data/agglomerate`. Il y a aussi un algo pour split les données depuis les 6 derniers mois.
- Dans le fichier `/utils/calculateStrengthIndex.ipynb` nous avons calculé le RSI sur les données FXUSDCAD.
- Dans le fichier `/utils/MovingAverage.ipynb` nous avons calculé le Moving Average sur les données.

## Visualization et traitement de données

Dans [visualization.ipynb](jupyternotebooks/visualization.ipynb) vous trouverez le code pour la visualization des données (textes et features de `/data/agglomerate`).

Dans ce ipynb on merge toutes les données entre elle et on fill les nan values avec de différentes façon (en répétant pour chaque features la dernière valeur vue par exemple). La méthode choisie et celle d'un linspace entre la valeur précedant et la valeur suivante.

Dans ce fichier nous retirons aussi les colonnes d'indexation et certaines features trop corrélés pour retirer les redondances.


Ce jupyternotebook permet d'output le fichier [agglomerate_fill_with_range_v2.csv](data/dataset/agglomerate/agglomerate_fill_with_range_v2.csv) 

## Pédiction des senetiments (Texte canadien et US)
Nous avons prédit les sentiments des textes en utilisant Loughran McDonald dictionary attributes to https://sraf.nd.edu/textual-analysis/resources/ in University of Notre Dame. 

Le code pour cela se trouve dans [pierre.ipynb](jupyternotebooks/pierre.ipynb)

Ce ipynb permet d'output un fichier [agglomerate_filled_sentiment.csv](data/dataset/agglomerate/agglomerate_filled_sentiment.csv) 

## Modèle

Finalement dans le fichier [lamia.ipynb](jupyternotebooks/pierre.ipynb) vous trouverez les modèles qu'on a utilisez pour entrainer et prédire nos le FXUSDCAD.

Notre test set contient plus de données que le fichier test original car lorsqu'on a merge les features on a considéré toutes les dates induites par les features.


Pour reproduire le meilleur résultats, il faut rouler le fichier `lamia.ipynb`. C'est le modèle ... qui donne les meilleurs résultats.


## Ressource intéressante
Pour comprendre un peu mieux le porblème nous nous sommes inspirer d'un [github](https://github.com/yuki678/centralbank_analysis) qu'on a copier dans le dossier  [src](src)