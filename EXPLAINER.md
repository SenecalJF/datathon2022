# Datathon PolyFinances 2022

## Data preprocessing

- Dans le fichier `/utils/agglomerate.ipynb` nous avons rassemblé les différents fichiers de données important en un seul fichier csv qui se retrouve dans `/data/agglomerate`. Il y a aussi un algo pour split les données depuis les 6 derniers mois.
- Dans le fichier `/utils/calculateStrengthIndex.ipynb` nous avons calculé le RSI sur les données FXUSDCAD.
- Dans le fichier `/utils/MovingAverage.ipynb` nous avons calculé le Moving Average sur les données.

## Visualization et traitement de données

Dans [visualization.ipynb](jupyternotebooks/visualization.ipynb) vous trouverez le code pour la visualization des données (textes et features de `/data/agglomerate`).

Dans ce ipynb on merge toutes les données entre elle et on fill les nan values deux façons différentes (en répétant pour chaque features la dernière valeur vue ou en faisant un smooting). C'est au final cette dernière méthod qui sera retenue, en utilisant un linspace entre la valeur précedant et la valeur suivante. Tous ces différents dataframe sont ensuite enregistrées dans [data/dataset/agglomerate](data/dataset/agglomerate)

Dans ce fichier nous retirons aussi les colonnes d'indexation et certaines features trop corrélés pour retirer les redondances.

Ce jupyternotebook permet d'output le fichier [agglomerate_fill_with_range_v2.csv](data/dataset/agglomerate/agglomerate_fill_with_range_v2.csv) 

## Pédiction des senetiments (Texte canadien et US)
Nous avons prédit les sentiments des textes en utilisant Loughran McDonald dictionary attributes to https://sraf.nd.edu/textual-analysis/resources/ in University of Notre Dame. Le dictionnaire utilisé est celui tel que trouvé dans la ressource mentionnée ci-dessous qui contient plus de mots que celui initialement donné.

Pour ce faire, nous avons, pour chaque type de texte séparément, compté le nombre de mots positifs et négatifs. Le sentiment final est obtenu par (Npositive-Nnegative)/(Npositive/Nnegative), pour obtenir une valeur entre -1 et 1, un interval de valeurs standard. 
Ensuite, pour nous avons défini différentes features qui sont, pour chaque date, le sentiment moyen des documents publiés 1 semaine avant la date, 2 semaines avant la date et 1 mois avant la date.
Cela nous donne un dataframe avec 18 nouvelles features utilisables.
De la même façon que précédemment, nous avons effectué un smoothing des valeurs pour obtenir une distribution sans valeur 0 (qui peuvent être vues comme équivalentes aux valeurs NaN dans ce cas).

Le code pour cela se trouve dans [pierre.ipynb](jupyternotebooks/pierre.ipynb)

Ce ipynb permet d'output un fichier [agglomerate_filled_sentiment.csv](data/dataset/agglomerate/agglomerate_filled_sentiment.csv) 

## Modèle

Finalement dans le fichier [lamia.ipynb](jupyternotebooks/pierre.ipynb) vous trouverez les modèles qu'on a utilisé pour entrainer et prédire nos le FXUSDCAD.

Nous avons employés différentes stratégies pour tester différents modèles. Les modèles considérés ici sont XGboost, RandomForest, régression Linéaire, SVR, un simple LSTM et une régression MLP. Pour choisir les features intéressantes, nous avons utilisés 2 stratégies: partir d'aucune feature et ajouter itérativement les features une à une de manière greedy, et partir de toutes les features et retirer itérativement de manière greedy.

Notre test set contient plus de données que le fichier test original car lorsqu'on a merge les features on a considéré toutes les dates induites par les features.


Pour reproduire le meilleur résultats, il faut rouler le fichier `lamia.ipynb`. C'est le modèle XGBoost qui donne les meilleurs résultats avec seulement 3 features (ce qui nous a un peu étonné): les sentiments sur les statements de la FOMC, les sentiments sur les documents de la banque Canada et les sentiments sur les testimony de la FOMC. Les résultats obtenus sont un mean squared error de 0.000418467031647579 et un r^2 score de 0.622469729311272.


## Ressource intéressante
Pour comprendre un peu mieux le problème nous nous sommes inspiré de ce [site](https://towardsdatascience.com/fedspeak-how-to-build-a-nlp-pipeline-to-predict-central-bank-policy-changes-a2f157ca0434) et de ce [github](https://github.com/yuki678/centralbank_analysis) qu'on a copié dans le dossier  [src](src). La tâche effectuée la dedans est similaire à la nôtre, si ce n'est qu'ils font une classification pour savoir si la valeur augmente, diminue ou reste la même, alors notre tâche est une régression de la valeur exacte.
