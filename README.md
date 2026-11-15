#  App Insights Unlocked: Google Play Store EDA

An end-to-end exploratory data analysis (EDA) of Google Play Store apps, built in Python for **Google Colab**. The project looks at what drives app success (ratings, installs, reviews) and turns the findings into practical recommendations for app development, product, and marketing teams.



---

##  Project Overview

You are a data analyst at a mobile-app company that has acquired Google Play Store data. The goal is to find patterns that guide app development strategy, improve user experience, and grow downloads and revenue.

The notebook covers:
- Data cleaning and preprocessing
- Success-metric development
- 25 analytical questions (10 basic, 10 medium, 5 advanced)
- Visualisations, written insights, and stakeholder recommendations

##  Business Problem

1. What key factors contribute to an app's success on the Play Store?
2. How can the company use these factors to improve its own apps?
3. What can we learn about user preferences and behaviour?

**Stakeholders:** App developers, product managers, marketing, senior management (internal); app users, advertisers, partners (external).

##  Dataset

| Item | Detail |
|---|---|
| Source | [Kaggle: Google Play Store Apps](https://www.kaggle.com/datasets/lava18/google-play-store-apps) |
| File | `googleplaystore.csv` |
| Raw size | 10,841 rows × 13 columns |
| After cleaning | **9,659 unique apps** |
| Snapshot date | Latest update in data: 8 Aug 2018 |

**Columns:** `App`, `Category`, `Rating`, `Reviews`, `Size`, `Installs`, `Type`, `Price`, `Content Rating`, `Genres`, `Last Updated`, `Current Ver`, `Android Ver`

> The optional `googleplaystore_user_reviews.csv` from the same Kaggle page is only needed for the sentiment-analysis question (Advanced Q4).

##  Repository Structure

```
.
├── App_Insights_EDA.ipynb          # Main analysis notebook (run in Colab)
├── googleplaystore.csv             # Dataset (download from Kaggle)
├── googleplaystore_user_reviews.csv  # Optional, for sentiment analysis
└── README.md
```

##  How to Run

### Option 1: Google Colab (recommended)
1. Go to [colab.research.google.com](https://colab.research.google.com) and choose **File → Upload notebook**.
2. Upload `App_Insights_EDA.ipynb`.
3. Choose **Runtime → Run all**.
4. When the upload prompt appears, select `googleplaystore.csv`.
5. (Optional) For Advanced Q4, upload `googleplaystore_user_reviews.csv` when prompted, or cancel to skip.

**Using Google Drive instead:** uncomment the Drive block in the loading cell and set `FILE_PATH = '/content/drive/MyDrive/googleplaystore.csv'`.

### Option 2: Local Jupyter
```bash
pip install pandas numpy matplotlib seaborn jupyter
jupyter notebook App_Insights_EDA.ipynb
```
Place `googleplaystore.csv` in the same folder as the notebook.

##  Methodology

### 1. Data cleaning
| Step | Action |
|---|---|
| Corrupted row | Removed 1 row with shifted columns (Category = `1.9`) |
| Duplicates | Removed 483 exact duplicates and 698 repeated app names (kept the record with the most reviews) |
| Data types | `Installs` ("10,000+") → int; `Price` ("$4.99") → float; `Size` ("19M", "512k") → MB; `Reviews` → int |
| Dates | `Last Updated` converted to datetime; year and month extracted |
| Missing values | `Type` filled using price; minor text gaps set to "Unknown"; **`Rating` and `Size` left as NaN** instead of imputing |
| Text standardisation | Categories title-cased with underscores removed; primary genre extracted from `Genres` |

### 2. Success metrics
- **Average rating:** user satisfaction
- **Installs:** reach (bucket lower bound, since installs are given as ranges)
- **Reviews:** engagement
- **Success Score (0–100):** mean percentile rank of rating, installs, and reviews

### 3. Analysis
Descriptive statistics, group-bys, correlations (Pearson and Spearman), binned analyses, and visualisations (histograms, boxplots, scatter plots, heatmaps, bar charts).

##  Questions Answered

<details>
<summary><b>Basic (10)</b></summary>

1. Average app rating
2. Number of unique categories
3. Distribution of app sizes
4. Free vs paid apps
5. Most common content rating
6. Top 5 most-installed apps
7. Apps rated 4.0 and above
8. Average reviews: free vs paid
9. Average app size per category
10. Apps last updated in 2018
</details>

<details>
<summary><b>Medium (10)</b></summary>

1. Correlation between installs and rating
2. Categories with the highest average rating
3. Effect of price on rating (paid apps)
4. Rating distribution across content ratings
5. Genres with the most apps over 1M installs
6. Update frequency
7. Impact of app size on installs
8. Highest-reviewed apps and their ratings
9. Content rating mix: free vs paid
10. Top 5 categories by installs
</details>

<details>
<summary><b>Advanced (5)</b></summary>

1. Top 10 highest-rated apps compared with their reviews and installs
2. Update trends and seasonality
3. Average rating by install bins
4. Sentiment analysis (needs the optional reviews file)
5. Genre vs rating (mean and median)
</details>

Plus a **bonus** comparison of high-rated (4.5+) apps against the rest.

##  Key Findings

| # | Finding |
|---|---|
| 1 | Mean rating is **4.17** (median 4.30). **77%** of rated apps score 4.0 or higher, so 4.0 doesn't differentiate an app. |
| 2 | Installs and rating are **very weakly correlated** (≈ 0.03–0.08). Ratings are **U-shaped** across install buckets: inflated for tiny apps, a trough of about 4.0 at 1K–50K installs, and about 4.3–4.4 at 10M+. |
| 3 | **Game, Communication, Tools, Family and Productivity** hold the most installs. **Photography, Racing, Action and Arcade** have the highest share of apps passing 1M installs (about 43–44%). |
| 4 | **92% of apps are free**, and they average far more reviews (234K vs 8.7K). Paid apps are rated slightly higher (4.26 vs 4.17), but ratings dip above $10. |
| 5 | Apps updated in the last 3 months have a **median of 1M installs vs 10K** for apps not updated in over a year. Highly rated apps (4.5+) were updated about twice as recently (56 vs 103 days). |
| 6 | Size is not a barrier: larger apps have higher median installs (Spearman ≈ 0.31). Games average about 42 MB and Tools about 9 MB. |
| 7 | **Events, Art & Design, Education, Books & Reference** and Personalization rate highest; **Dating, Maps & Navigation and Tools** rate lowest. |
| 8 | Reviews correlate almost perfectly with installs (≈ 0.97), so they work as an engagement and reach proxy. |

##  Recommendations

- **Product / management:** target *Everyone*-rated apps in high-demand categories, and consider less crowded, high-satisfaction niches (Education, Art & Design, Books & Reference).
- **Developers:** ship updates at least every ~3 months; don't cut features just to save megabytes.
- **Monetisation:** default to free with ads or in-app purchases; if charging, keep prices modest and justify the value.
- **Marketing:** protect ratings during the 1K–50K install phase with strong onboarding, well-timed rating prompts, and review responses. Use review counts as an engagement KPI.

> These are associations, not proven causes.

##  Limitations

- **Installs are bucketed** (e.g. `1,000,000+`), so real install counts are unknown.
- **Single snapshot:** each app has one "Last Updated" date, so true update frequency and seasonality can't be measured. "Days since last update" is used as a proxy.
- **Ratings are self-selected**, missing for about 15% of apps, and unreliable for apps with few reviews.
- **No revenue data**, so monetisation can't be measured directly.
- **No review text** in the main file, so sentiment analysis needs the companion dataset.

##  Tech Stack

- Python 3
- pandas, NumPy
- Matplotlib, Seaborn
- Google Colab / Jupyter


