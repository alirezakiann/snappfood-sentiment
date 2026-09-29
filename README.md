# Persian Sentiment Analysis on Snappfood Reviews

| Notebook | Open |
|---|---|
| 01 - EDA | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/alirezakiann/snappfood-sentiment/blob/main/notebooks/01_eda.ipynb) |
| 02 - Preprocessing | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/alirezakiann/snappfood-sentiment/blob/main/notebooks/02_preprocessing.ipynb) |
| 03 - Baseline models | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/alirezakiann/snappfood-sentiment/blob/main/notebooks/03_baseline_models.ipynb) |
| 04 - LSTM | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/alirezakiann/snappfood-sentiment/blob/main/notebooks/04_lstm.ipynb) |

Classifying ~70,000 Persian food-delivery reviews from Snappfood as positive or negative,
and finding out *what* customers complain about.

## Results so far
Test set: 6,910 reviews, never used for training or tuning.

| Model | Accuracy | Macro F1 |
|---|---|---|
| Always guess one class | 0.500 | - |
| TF-IDF (word 1-2 grams) + Linear SVM | 0.864 | 0.864 |
| BiLSTM (PyTorch, trained from scratch) | 0.860 | 0.859 |

## Key insights
- **The stopword trap:** hazm's default Persian stopword list contains sentiment words like «عالی», «خوب» and «نبود». Using it as-is erases the strongest signals, so a custom keep-list was added.
- **Negation matters:** «بد» (bad) has weight -6.4 but «بد نبود» (not bad) has +7.4. Word pairs (bigrams) improved every model.
- **Label noise:** many of the model's most confident "errors" are actually mislabeled reviews (e.g. «خوشمزه بود و خیلی سریع رسید» labeled negative), so true accuracy is likely higher than measured.
- **Main weakness of TF-IDF:** it ignores word order, so mixed reviews like «کیفیت خوب بود ولی خیلی دیر رسید» are hard for it.
- **A BiLSTM did not beat TF-IDF:** reading word order did not help overall (0.860 vs 0.864), and was not better on «ولی» (but) reviews either. About 80% of the errors are shared by both models, which points to a data ceiling (label noise, genuinely mixed reviews) rather than a model problem.

## Dataset
[Snappfood - Persian Sentiment Analysis](https://www.kaggle.com/datasets/soheiltehranipour/snappfood-persian-sentiment-analysis) on Kaggle (CC0 license).
The data is not stored in this repo. Download it from Kaggle to run the notebooks.

## Project roadmap
| Step | Notebook | Status |
|---|---|---|
| 1. Exploratory data analysis | `notebooks/01_eda.ipynb` | Done |
| 2. Persian text preprocessing | `notebooks/02_preprocessing.ipynb` | Done |
| 3. Baseline models (TF-IDF + Scikit-Learn) | `notebooks/03_baseline_models.ipynb` | Done |
| 4. LSTM model (PyTorch) | `notebooks/04_lstm.ipynb` | Done |
| 5. Error analysis & complaint clustering | `notebooks/05_error_analysis.ipynb` | Planned |
| 6. Power BI dashboard | `dashboard/` | Planned |

## How to run
Open a notebook in Google Colab with the badge above, put the CSV file in your Google Drive
at `MyDrive/snappfood-sentiment/`, and run the cells from top to bottom.
