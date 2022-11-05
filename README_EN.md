# Datathon 2022 - Central bank edging

## Table of Contents

1. Installation
2. Data Understanding
3. Code Description
4. Process idea
5. Licensing, Authors, Acknowledgements

## 1. Installation

`Some python files are provided, but you are free to use any technology`

#### Libraries

Required libraries are described in requirements.txt. The code should run with no issues using Python versions 3.7+.
Create a virtual environment of your choice.

```
python -m venv venv
source venv/bin/activate        # On linux/Mac
venv\Scripts\activate.bat       # On Windows
pip install -r requirements.txt
```

#### To Run Notebook (Local)

1. Go to top directory
2. Run the jupyter notebooks
   `jupyter notebook`
3. Open and run notebooks `Exemple_to_get_started.ipynb`

#### To Run Notebook (Google Colab)

The notebook can be executed on Google Colab.

1. Upload notebooks to your Google Colab
2. Upload data to your Google Drive (Colab Data dir)
3. Execute notebook (Note: You need to authorize the access to your Google Drive when asked to input the code)

## 2. Data Understanding

/!\ Data used for each prediction are only those available before the date.

#### Text Data - Fed U.S.

- FOMC/fomc_calendar.csv - all FOMC calendar dates
- FOMC/statement.csv - Statement text along with basic attributes such as dates, speaker, title. Each text is also available in the directory with the same name. Statements are available post press conference for almost all meetings, which include rate decision and target rate. From 2008, target rate became a range instead of a single value.
- FOMC/minutes.csv - Minutes text along with basic attributes such as dates, speaker, title. Each text is also available
  in the directory with the same name. Minutes are summary of FOMC Meeting and contents are structured in sections and paragraphs, most of which were updated in 2011 and 2012. The minutes of regularly scheduled meetings are released three weeks after the date of the policy decision.
- FOMC/presconf_script.csv - Press conference scripts text along with basic attributes such as dates, speaker, title. Each text is also available in the directory with the same name. This is available from 2011. Starting with the speaker name, so extract those spoken by the chairperson because the other person's words are more likely to be questions and not FOMC's view. It is in pdf form, so download pdf and then process the text.
- FOMC/meeting_script.csv - Meeting scripts text along with basic attributes such as dates, speaker, title. Each text is also available in the directory with the same name. FOMC decided to publish this five years after each meeting. It contains all the words spoken during the meeting. It will contain some insight about FOMC discussions and how the consensus about monetary policy is built, but **cannot be used in prediction as this is not published for five years**.
- FOMC/speech.csv - Speech text along with basic attributes such as dates, speaker, title. Each text is also available in the directory with the same name. There are many speeches published but some of them are not related to monetary policies but various topics such as regulations and governance. Some speeches may contain indication of FOMC policy, so use only those by the chairperson.
- FOMC/testimony.csv - Testimony text along with basic attributes such as dates, speaker, title. Each text is also available in the directory with the same name. Like speeches, testimony is not necessarily related to monetary policy. There are semi-annual testimony in the congress, which can be a good inputs of FOMC's view by chairperson.

#### Text Data - Bank of Canada

- MPR.csv - A quarterly report of the Bank of Canada’s Governing Council, presenting the Bank’s base-case projection for inflation and growth in the Canadian economy, and its assessment of risks. (Publish at the same time of a interest rate change) **Will need some data cleanning**
  [Here is where you can find these documents to explore their contents](https://www.bankofcanada.ca/publications/mpr/)

#### CanBankValetApi

- FXUSDCAD.csv - Exchange rate USD/CAD
- CA.-interest_rate.csv - Canada: Interest rate that commercial banks charge their most creditworthy customers. Set by the Bank of Canada
- U.S.-Prime_Rate_Chargedc_By_Bank.csv - U.S.: Interest rate that commercial banks charge their most creditworthy customers. Set by the Federal Reserve (FOMC)

#### Market Data → Example of complementary data

Data pulled from the Nasdaq API

Get data from Nasdaq data link's aps

1. Create an account: [Docs here](https://docs.data.nasdaq.com/docs).
2. Get the data: python utils/NasdaqGetData.py [your API Key] 1980-01-01

- FED Rate
  - FRED_DFEDTAR.csv - Target FED Rate till 2008, Daily
  - FRED_DFEDTARU.csv - Target Upper FED Rate from 2008, Daily
  - FRED_DFEDTARL.csv - Target Lower FED Rate from 2008, Daily
  - FRED_DFF.csv - Effective FED Rate, Daily
- GDP
  - FRED_GDPC1.csv - Real GDP, Quarterly
  - FRED_GDPPOT.csv - Real potential GDP, Quarterly
- CPI
  - FRED_PCEPILFE.csv - Core PCE excluding Food and Energy, Monthly
  - FRED_CPIAUCSL.csv - Consumer Price Index for All Urban Consumers: All Items in U.S. City Average
- Employment
  - FRED_UNRATE.csv - Unemployment Rate, Monthly
  - FRED_PAYEMS.csv - Employment, Monthly
- Sales
  - FRED_RRSFS.csv - Advance Real Retail and Food Services Sales, monthly
  - FRED_HSN1F.csv - New Home Sales, monthly
- ISM
  - ISM_MAN_PMI.csv - ISM Purchasing Managers Index
  - ISM_NONMAN_NMI.csv - ISM Non-manufacturing Index
- Treasury
  - USTREASURY_YIELD.csv

#### Loughran-McDonald Dictionary

- LoughranMcDonald/LoughranMcDonald_SentimentWordLists_2018.csv - This is used in preliminary analysis.

## 3. Code Description

### Files

- NasdaqGetData.py - Get market data from NasdaqApi.
- BankOfCanandaValet.py - Basic script to get data from [Valet Api](https://www.bankofcanada.ca/valet/docs)

## 4. Process idea

Start with a the preliminary analysis of the data, some idea:

1. Analyze sentiment of the statement text using Loughran and McDonald Sentiment Word Lists
2. Plot sentiment (count of positive words with negation, negative words and net over time series, normalized by the number of words
3. Load FED Rate, map the rate and decision to statement
4. Plot the moving average of the sentiment along with FED rate and recession period

## 5. Licensing, Authors, Acknowledgements

Data attributes to the source (FRED, ISM, US Treasury and Nasdaq). Loughran McDonald dictionary attributes to https://sraf.nd.edu/textual-analysis/resources/ in University of Notre Dame.
