# Persian Sentiment Analysis on Snappfood Reviews

| Notebook | Open |
|---|---|
| 01 - EDA | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/alirezakiann/snappfood-sentiment/blob/main/notebooks/01_eda.ipynb) |
| 02 - Preprocessing | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/alirezakiann/snappfood-sentiment/blob/main/notebooks/02_preprocessing.ipynb) |
| 03 - Baseline models | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/alirezakiann/snappfood-sentiment/blob/main/notebooks/03_baseline_models.ipynb) |

Classifying ~70,000 Persian food-delivery reviews from Snappfood as positive or negative,
and finding out *what* customers complain about.

## Dataset
[Snappfood - Persian Sentiment Analysis](https://www.kaggle.com/datasets/soheiltehranipour/snappfood-persian-sentiment-analysis) on Kaggle (CC0 license).
The data is not stored in this repo. Download it from Kaggle to run the notebooks.

## Project roadmap
| Step | Notebook | Status |
|---|---|---|
| 1. Exploratory data analysis | `notebooks/01_eda.ipynb` | Done |
| 2. Persian text preprocessing | `notebooks/02_preprocessing.ipynb` | Done |
| 3. Baseline models (TF-IDF + Scikit-Learn) | `notebooks/03_baseline_models.ipynb` | In progress |
| 4. LSTM model (PyTorch) | `notebooks/04_lstm.ipynb` | Planned |
| 5. Error analysis & complaint clustering | `notebooks/05_error_analysis.ipynb` | Planned |
| 6. Power BI dashboard | `dashboard/` | Planned |

## How to run
Open a notebook in Google Colab with the badge above, put the CSV file in your Google Drive
at `MyDrive/snappfood-sentiment/`, and run the cells from top to bottom.
