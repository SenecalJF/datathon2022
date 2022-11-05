# Datathon 2022 - L'impact des banques centrales

## Table des matières

1. Installation
2. Compréhension des données
3. Description du code
4. Idée de processus
5. Licences, auteurs, remerciements

## 1. Installation

`Certains fichiers python sont fournis, mais vous êtes libre d'utiliser n'importe quelle technologie.`

#### Librairies

Les bibliothèques requises sont décrites dans le fichier requirements.txt. Le code devrait fonctionner sans problème avec Python version 3.7+.
Créez un environnement virtuel de votre choix.

```
python -m venv venv
source venv/bin/activate        # linux/Mac
venv\Scripts\activate.bat       # Windows
pip install -r requirements.txt
```

#### Pour exécuter un ordinateur portable (local)

1. Allez dans le répertoire principal
2. Lancez les notebooks jupyter
   `jupyter notebook`
3. Ouvrez et exécutez les notebooks `Exemple_to_get_started.ipynb`.

#### Pour exécuter un notebook (Google Colab)

Le notebook peuvent être exécutés sur Google Colab.

1. Téléchargez le notebook sur votre Google Colab
2. Téléchargez les données sur votre Google Drive (Colab Data dir)
3. Exécuter le notebook (Note : Vous devez autoriser l'accès à votre Google Drive lorsqu'il vous est demandé de saisir le code)

## 2. Compréhension des données

/!\ Les données utilisées pour chaque prédiction sont uniquement celles disponibles avant la date.

#### Données textuelles - Fed U.S.

- FOMC/fomc_calendar.csv - toutes les dates du calendrier FOMC
- FOMC/statement.csv - Texte des déclarations avec les attributs de base tels que dates, orateur, titre. Chaque texte est également disponible dans le répertoire portant le même nom. Les déclarations sont disponibles après la conférence de presse pour presque toutes les réunions, qui incluent la décision de taux et le taux cible. Depuis 2008, le taux cible est devenu une fourchette au lieu d'une valeur unique.
- FOMC/minutes.csv - Texte des minutes avec les attributs de base tels que les dates, l'orateur, le titre. Chaque texte est également disponible
  dans le répertoire portant le même nom. Les procès-verbaux sont des résumés des réunions du FOMC et leur contenu est structuré en sections et paragraphes, dont la plupart ont été mis à jour en 2011 et 2012. Les procès-verbaux des réunions régulières sont publiés trois semaines après la date de la décision de politique monétaire.
- FOMC/presconf_script.csv - Texte des scripts des conférences de presse avec les attributs de base tels que les dates, l'orateur, le titre. Chaque texte est également disponible dans le répertoire du même nom. Ceci est disponible depuis 2011. En commençant par le nom de l'orateur, donc extraire ceux parlés par le président parce que les mots de l'autre personne sont plus susceptibles d'être des questions et non le point de vue du FOMC.
- FOMC/meeting_script.csv - Texte des scripts de réunion avec les attributs de base tels que les dates, l'orateur, le titre. Chaque texte est également disponible dans le répertoire portant le même nom. **Le FOMC a décidé de publier ce document cinq ans après chaque réunion**. Il contient tous les mots prononcés pendant la réunion. Il donne un aperçu des discussions du FOMC et de la manière dont se construit le consensus sur la politique monétaire, mais **il ne peut pas être utilisé pour faire des prévisions car il n'est pas publié avant cinq ans**.
- FOMC/speech.csv - Texte des discours avec les attributs de base tels que les dates, le locuteur, le titre. Chaque texte est également disponible dans le répertoire portant le même nom. De nombreux discours sont publiés mais certains d'entre eux ne sont pas liés aux politiques monétaires mais à divers sujets tels que la réglementation et la gouvernance. Certains discours peuvent contenir des indications sur la politique du FOMC, utilisez donc uniquement ceux du président.
- FOMC/testimony.csv - Texte des témoignages avec les attributs de base tels que dates, orateur, titre. Chaque texte est également disponible dans le répertoire portant le même nom. Comme les discours, les témoignages ne sont pas nécessairement liés à la politique monétaire. Il y a des témoignages semestriels au congrès, qui peuvent être une bonne source d'information sur le point de vue du FOMC par président.

#### Données textuelles - Banque du Canada

- MPR.csv - Rapport trimestriel du Conseil de direction de la Banque du Canada, présentant le scénario de référence de la Banque en matière d'inflation et de croissance de l'économie canadienne, ainsi que son évaluation des risques. (Publié en même temps qu'un changement de taux d'intérêt) **Nécessite un nettoyage des données**.
  [Voici ou vous pouvez retrouver ces documents pour explorer leur contenue](https://www.bankofcanada.ca/publications/mpr/)

#### CanBankValetApi

- FXUSDCAD.csv - Taux de change USD/CAD
- CA.-interest_rate.csv - Canada : Taux d'intérêt que les banques commerciales appliquent à leurs clients les plus solvables. Fixé par la Banque du Canada
- U.S.-Prime_Rate_Chargedc_By_Bank.csv - États-Unis : Taux d'intérêt que les banques commerciales appliquent à leurs clients les plus solvables. Fixé par la Réserve fédérale (FOMC).

#### Données de marché - Example de données complémentaires que vous pouvez aller chercher

Données tirées de l'API du Nasdaq

Obtenir des données à partir de l'aps du Nasdaq data link

1. Créez un compte : [Docs ici] (https://docs.data.nasdaq.com/docs).
2. Obtenez les données : python utils/NasdaqGetData.py [votre clé API] 1980-01-01

- Taux FED
  - FRED_DFEDTAR.csv - Taux FED cible jusqu'en 2008, Quotidiennement
  - FRED_DFEDTARU.csv - Taux FED supérieur cible à partir de 2008, Quotidiennement
  - FRED_DFEDTARL.csv - Taux cible inférieur de la FED à partir de 2008, Quotidiennement
  - FRED_DFF.csv - Taux FED effectif, Quotidien
- PIB
  - FRED_GDPC1.csv - PIB réel, trimestriel
  - FRED_GDPPOT.csv - PIB potentiel réel, trimestriel
- IPC
  - FRED_PCEPILFE.csv - IPC de base hors alimentation et énergie, Mensuel
  - FRED_CPIAUCSL.csv - Indice des prix à la consommation pour tous les consommateurs urbains : Tous les articles en moyenne dans les villes américaines
- Emploi
  - FRED_UNRATE.csv - Taux de chômage, mensuel
  - FRED_PAYEMS.csv - Emploi, Mensuel
- Ventes
  - FRED_RRSFS.csv - Ventes réelles anticipées du commerce de détail et des services de restauration, mensuel
  - FRED_HSN1F.csv - Ventes de logements neufs, mensuel
- ISM
  - ISM_MAN_PMI.csv - Indice ISM des directeurs d'achat
  - ISM_NONMAN_NMI.csv - Indice ISM non-manufacturier
- Trésorerie
  - USTREASURY_YIELD.csv

#### Dictionnaire Loughran-McDonald

- LoughranMcDonald/LoughranMcDonald_SentimentWordLists_2018.csv - Ce document est utilisé pour l'analyse préliminaire.

## 3. Description du code

### Fichiers

- NasdaqGetData.py - Récupère les données du marché depuis NasdaqApi.
- BankOfCanandaValet.py - Script de base pour obtenir des données de [Valet Api] (https://www.bankofcanada.ca/valet/docs)

## 4. Idée de processus pour vous aider à commencer

Commencez par une analyse préliminaire des données, une idée :

1. Analyser le sentiment du texte de l'énoncé en utilisant les listes de mots de sentiments de Loughran et McDonald.
2. Tracer le sentiment (nombre de mots positifs avec négation, mots négatifs et net sur la série temporelle, normalisé par le nombre de mots).
3. Charger le taux de la FED, faire correspondre le taux et la décision à la déclaration.
4. Tracer la moyenne mobile du sentiment avec le taux de la FED et la période de récession.

## 5. Licences, auteurs, remerciements

Attributs de données à la source (FRED, ISM, US Treasury et Nasdaq). Attributs du dictionnaire Loughran McDonald à https://sraf.nd.edu/textual-analysis/resources/ à l'Université de Notre Dame.
