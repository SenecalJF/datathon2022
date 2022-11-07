# Datathon PolyFinances 2022

## Data preprocessing

- Dans le fichier `/utils/agglomerate.ipynb` nous avons rassembler les différents fichiers de données importnat en un seul fichier csv qui se retrouve dans `/data/agglomerate`. Il y a aussi un algo pour split les données depuis les 6 derniers mois.
- Dans le fichier `/utils/calculateStrengthIndex.ipynb` nous avons calculer le RSI sur les données FXUSDCAD.
- Dans le fichier `/utils/MovingAverage.ipynb` nous avons calculer le Moving Average sur les données.

## Visualization

- Les visualisations de données sont dans le fichier `/jupyternotebooks/visualization.ipynb`.

## Modèle

- Les modèles que nous avons créé sont dans le dossier `/jupyternotebooks`
- Pour reproduire le meilleur résultats, il faut rouler le fichier `pierre.ipynb`. C'est le modèle du Long Short term memory qui se situe vers la fin du fichier qui a le mieux fonctionner.
- Dans le dossier `/src` sont des analyses et traitement sur les données.
