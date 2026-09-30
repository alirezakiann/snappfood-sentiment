# Persian Sentiment Analysis & Complaint Mining on Snappfood Reviews

End-to-end NLP project on **69,100 Persian food-delivery reviews**: text preprocessing, classic and deep-learning sentiment models, a hand-labeled audit of dataset quality, complaint clustering, and a Power BI dashboard.

![Dashboard overview](dashboard/dashboard_overview.png)

## Highlights
- Built a Persian text pipeline with **hazm** and a **sentiment-aware stopword list**. The default list would have deleted words like «عالی» (excellent) and «نبود» (wasn't).
- Best model: **TF-IDF (word 1-2 grams) + Linear SVM, 86.4% test accuracy**. A **BiLSTM** trained from scratch in PyTorch reached 86.0% and did not beat it.
- A **blind hand-labeling audit** showed that **71%** of the errors made by both models are actually **wrong dataset labels**. Estimated accuracy on true labels: **~89%**.
- **KMeans clustering** of 34,506 negative reviews: about **half of complaints concern the food itself**, and about **a quarter concern delivery and order accuracy**.

## Results
Test set: 6,910 reviews, used once at the very end. Nothing was tuned on it.

| Model | Accuracy | Macro F1 |
|---|---|---|
| Always guess one class | 0.500 | - |
| BiLSTM (PyTorch, trained from scratch) | 0.860 | 0.859 |
| **TF-IDF (word 1-2 grams) + Linear SVM** | **0.864** | **0.864** |
| TF-IDF + SVM, estimated on true labels | ~0.89 | - |

The best model catches **92%** of negative reviews but labels **19%** of positive reviews as negative. Many positive reviews contain small complaints.

## Key insights

### Text processing
- **The stopword trap.** hazm's default Persian stopword list is built for news text and includes sentiment words such as «عالی», «خوب», «نبود» and «متاسفانه». A custom keep-list of 47 negation, contrast, intensity and evaluation words was added back.
- **Hidden duplicates.** There were no exact duplicates in the raw data, but 749 appeared after normalization. Removing them prevents train/test leakage (70,000 → 69,100 reviews).

### Models
- **Negation is learned through word pairs.** «بد» (bad) has weight -6.4 while «بد نبود» (not bad) has +7.4. Bigrams improved every model, by up to 1.6 F1 points.
- **Regularization matters.** With weak regularization, train F1 reaches 0.99 while validation drops to 0.84. 5-fold cross-validation gives 0.860 ± 0.002.
- **Word order did not help here.** The BiLSTM was slightly worse overall and also worse on mixed «... ولی ...» (but) reviews (78.6% vs. 80.5%). Reviews are short (median 13 words) and the LSTM learns word vectors from only 55k reviews.
- **Both models fail on the same reviews.** 750 test reviews are misclassified by both, which is 77-80% of each model's errors. This points to the data rather than the models.

### Label quality
150 test reviews were re-labeled by hand **without seeing the dataset label**: 100 that both models got wrong and 50 that both got right, shuffled together.

| Group | Dataset label correct | Dataset label wrong | Mixed / unclear |
|---|---|---|---|
| Both models wrong (n=100) | 22% | **71%** | 7% |
| Both models right (n=50) | 92% | 6% | 2% |

- Example of a wrong label: «سیب‌زمینی‌ها غرق در روغن، مونده و سیاه بودن» (fries soaked in oil, stale and black) is labeled **positive**.
- **At least ~7% of test labels are wrong.** This uses the low end of the 95% interval (~61%) on the 750 shared errors alone.
- **Estimated true accuracy is ~89%.** This is a rough estimate: it also depends on the noise rate among correct predictions, which was measured on only 50 reviews.
- **Real model failures include sarcasm**, e.g. «رکورد تاخیرو با ۱۶۰ دقیقه تاخیر زدید، خسته نباشید خدا قوت» (you set a record with a 160-minute delay, well done).

### What customers complain about
| Complaint type | Share of negative reviews |
|---|---|
| Bad taste | 27.8% |
| Bakery, groceries & packaging | 22.7% |
| Late & cold delivery | 12.5% |
| Low quality | 11.3% |
| Very bad / small portion | 10.3% |
| Fries & sides | 5.3% |
| Wrong item sent | 5.2% |
| Missing items | 4.9% |

- **Food quality ~49%, delivery and order accuracy ~23%.** Late & cold delivery, wrong items and missing items are the part a delivery platform can fix directly.
- **Longer reviews are more negative.** 39% of 1-5 word reviews are negative, rising to 66% for reviews over 40 words.
- **Lateness is often forgiven, cold food is not.** 42% of reviews using «دیر» (late) are still positive, usually softened («کمی دیر رسید»), and 32% when «تاخیر» (delay) is included. In negative reviews, lateness usually comes together with cold food.
- **Pizza is not worse, just more common.** Every food has a higher complaint rate than the overall 50%, because unhappy customers name the dish while happy ones write short reviews. Pizza (60.6%) sits in the middle. Eggs (70.9%, mostly broken) and chicken (65.5%) rank highest.
- **Outlier detection** surfaced English-language reviews (~0.5%) and a recurring issue hidden inside the big clusters: **missing cutlery**.

## Dashboard
Built in Power BI from the CSV files produced by notebook 06. Clicking a complaint type filters the example reviews.

| Complaints | Model & data quality |
|---|---|
| ![Complaints](dashboard/dashboard_complaints.png) | ![Model and data quality](dashboard/dashboard_model.png) |

## Notebooks
| # | Notebook | What it does | Open |
|---|---|---|---|
| 01 | `01_eda.ipynb` | Data size, label balance, review length, frequent words | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/alirezakiann/snappfood-sentiment/blob/main/notebooks/01_eda.ipynb) |
| 02 | `02_preprocessing.ipynb` | Normalization, custom stopwords, deduplication, stratified train/val/test split | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/alirezakiann/snappfood-sentiment/blob/main/notebooks/02_preprocessing.ipynb) |
| 03 | `03_baseline_models.ipynb` | TF-IDF × 3 feature types × 3 models, regularization tuning, cross-validation, feature weights | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/alirezakiann/snappfood-sentiment/blob/main/notebooks/03_baseline_models.ipynb) |
| 04 | `04_lstm.ipynb` | BiLSTM in PyTorch, early stopping, comparison by review type | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/alirezakiann/snappfood-sentiment/blob/main/notebooks/04_lstm.ipynb) |
| 05 | `05_error_analysis.ipynb` | Blind label audit, complaint clustering (TF-IDF + SVD + KMeans), outlier detection | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/alirezakiann/snappfood-sentiment/blob/main/notebooks/05_error_analysis.ipynb) |
| 06 | `06_dashboard_data.ipynb` | Tidy CSV files for the Power BI dashboard | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/alirezakiann/snappfood-sentiment/blob/main/notebooks/06_dashboard_data.ipynb) |

## Repository structure
```
snappfood-sentiment/
├── notebooks/        # 01-06, run in order
├── dashboard/        # Power BI file and screenshots
├── requirements.txt
└── README.md
```

## Limitations
- Labels probably come from star ratings, so some are wrong (see the audit). The true-accuracy estimate is based on a small hand-labeled sample.
- The dataset is artificially balanced (exactly 50/50); real-world reviews are likely more positive.
- Food categories are detected with keywords, which can miss spelling variants.
- The BiLSTM used default settings and a single run, while the baseline was tuned more carefully.
- Complaint clusters are found automatically; one large cluster ("Bad taste") still mixes several sub-topics.

## Tools
Python · pandas · scikit-learn · PyTorch · hazm · matplotlib / seaborn · Google Colab · Power BI

## How to run
1. Download the [Snappfood - Persian Sentiment Analysis](https://www.kaggle.com/datasets/soheiltehranipour/snappfood-persian-sentiment-analysis) dataset from Kaggle (CC0 license). The data is not stored in this repository.
2. Put `Snappfood - Sentiment Analysis.csv` in your Google Drive at `MyDrive/snappfood-sentiment/`.
3. Open the notebooks in order with the Colab badges above and run all cells. Notebook 04 needs a GPU runtime (Runtime → Change runtime type → T4 GPU).
4. For the dashboard, download the `powerbi` folder created by notebook 06 and open `dashboard/snappfood_dashboard.pbix` in Power BI Desktop.
