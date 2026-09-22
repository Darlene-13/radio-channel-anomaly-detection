# Radio Channel Anomaly Detection

Binary classification of radio channel signal segments as anomalous or normal, built for the Hack4Dev DSRH 2026 selection challenge.

## Problem

Each row represents one signal segment, summarized by statistical features such as mean, variance, kurtosis, peak counts, and other signal characteristics rather than the raw waveform.

The task is to predict `anomaly` (`0` or `1`) for each segment in the test set.

The primary evaluation metric is **F1 score**, since only approximately 20% of the segments are anomalous.

## Data

- `train.csv`- 1,698 labeled signal segments
- `test.csv` - 425 unlabeled signal segments
- `sample_submission.csv`- required submission format
- 9 radio channels
- Anomaly rates vary significantly across channels

## Approach

1. Performed exploratory data analysis (EDA), including:
   - Class balance
   - Per-channel anomaly rates
   - Feature correlations
   - Redundancy between variance-related features

2. Tested engineered features such as:
   - Log transformations
   - Peak ratios

   These did not improve cross-validation performance over the original feature set, so the raw features were retained.

3. Compared multiple models using stratified cross-validation:
   - Logistic Regression
   - Random Forest
   - LightGBM
   - CatBoost

4. Tuned the classification decision threshold for each model instead of relying on the default `0.5` threshold.

5. After the first leaderboard submission underperformed relative to cross-validation, repeated the evaluation with additional cross-validation runs to obtain a more stable estimate of model performance.

6. The final submission uses a probability blend of:
   - LightGBM
   - CatBoost
   - Random Forest

   The blend was selected for robustness rather than simply choosing the model with the highest individual cross-validation score.

## Results

| Model | CV F1 |
|---|---:|
| Logistic Regression | 0.856 |
| Random Forest | 0.886 |
| CatBoost | 0.918 |
| LightGBM | 0.912–0.922 |
| Blend | 0.899 |

> **Note:** Cross-validation scores and leaderboard performance may differ due to differences between the validation splits and the hidden test distribution.

## Project Structure

```text
.
├── notebooks/
│   ├── 01_eda.py
│   ├── 02_feature_engineering.py
│   ├── 03_modeling.py
│   ├── 04_threshold_tuning.py
│   └── 05_generate_submission.py
│
├── data/
│   └── raw/
│       ├── train.csv
│       ├── test.csv
│       └── sample_submission.csv
│
├── submissions/
│   └── generated prediction files
│
└── README.md

```
### Running the Project

Clone the repository and navigate to the project directory:

cd DSRH-2026-Radio-Anomaly-Detection

Run the pipeline in order:


```angular2html

python3 01_eda.py
python3 02_feature_engineering.py
python3 03_modeling.py
python3 04_threshold_tuning.py
python3 05_generate_submission.py
```

### Key Takeaways
1. Tree-based ensemble models performed substantially better than Logistic Regression on the available features.
2. CatBoost and LightGBM achieved the strongest individual cross-validation results.
3. Threshold optimization was important because the dataset is imbalanced.
4. Feature engineering did not provide a consistent improvement over the original features.
5. Ensemble predictions provided an alternative to relying on a single model.
6. The difference between cross-validation and leaderboard performance highlighted the importance of robust validation and avoiding overfitting to a particular split.