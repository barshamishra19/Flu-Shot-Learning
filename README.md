 # Flu Shot Learning

 ![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
 ![Metric](https://img.shields.io/badge/metric-mean%20ROC%20AUC-2ea44f)

 Machine learning experiments for the DrivenData competition **Flu Shot Learning: Predict H1N1 and Seasonal Flu Vaccines**. Each model predicts vaccination probabilities for `h1n1_vaccine` and `seasonal_vaccine`; the competition metric is the mean ROC AUC of those two targets.

 ## Current status

 - Latest submitted leaderboard score: **0.8635**
 - Best recorded local OOF score: **0.86933** in notebook 9
 - Newest experiment: **notebook 11**, an AUC-aware blend that compares probability blending with rank-normalized blending

 Local OOF scores are useful for model selection, but they are not leaderboard scores. Always validate the generated CSV before submitting it to DrivenData.

## Data

The competition data is not included in this repository. Each user must download or attach their own copy of the dataset. You can download it from the [DrivenData competition page](https://www.drivendata.org/competitions/66/flu-shot-learning/) and place the CSV files in a local `data/raw/` directory.

Expected files:

- `training_set_features.csv`
- `training_set_labels.csv`
- `test_set_features.csv`
- `submission_format.csv`

The `data/` directory is ignored by Git because the dataset should be downloaded separately and may be subject to the competition's terms.

### Using a personal Kaggle dataset

When running a notebook on Kaggle, attach your own dataset through **Add Input**. Update the `DATA_DIR` (or the `Path(...)` used by the notebook) to the mounted directory for that dataset. For example:

```python
from pathlib import Path

DATA_DIR = Path("/kaggle/input/datasets/<your-kaggle-username>/<your-dataset-slug>")
```

For this project, the expected Kaggle path is:

```python
Path("/kaggle/input/datasets/barshamishra19/flu-shot-learning-dataset")
```

Replace `barshamishra19/flu-shot-learning-dataset` with your own Kaggle username and dataset slug when using your own uploaded dataset. The directory must contain all four expected CSV files listed above.

## Setup

```bash
python -m venv .venv
```

Activate the environment and install dependencies:

```bash
pip install -r requirements.txt
```

Open a notebook in `notebooks/` and run it after adding your own data. The notebooks write generated submission CSV files to their configured output directory or current working directory.

For local execution, change `DATA_DIR` in the notebook to:

```python
from pathlib import Path
DATA_DIR = Path("data/raw")
```

For Kaggle, attach the dataset and use the mounted path shown in the notebook. Notebook 11 writes `submission_notebook11_auc_rank_blend.csv` and its OOF predictions into an `outputs/` directory.

## Notebook experiments

Scores below are the scores printed by each notebook's validation/OOF cells. They are not guaranteed to match the public leaderboard because the competition uses a hidden test set.

| Notebook | Main approach | Validation / OOF mean ROC AUC | Output |
|---|---|---:|---|
| [1](notebooks/1-flu-shot-learning-predict-h1n1-and-seasonal-flu.ipynb) | Random Forest with imputation, scaling, and one-hot encoding | **0.8410** | `submission.csv` |
| [2](notebooks/2-flu-shot-learning-predict-h1n1-and-seasonal-flu%281%29.ipynb) | CatBoost with categorical survey values and a stratified holdout | **0.86980** | `submission2.csv` |
| [3](notebooks/3-flu-shot-learning-predict-h1n1-and-seasonal-flu.ipynb) | CatBoost, engineered survey aggregates, and five-fold multilabel CV | **0.86840** | `submission3.csv` |
| [4](notebooks/4-flu-shot-learning-predict-h1n1-and-seasonal-flu.ipynb) | Cross-validated CatBoost ensemble with leakage-safe features and blending | **0.86850** | `submission4.csv` |
| [5](notebooks/5-flu-shot-learning-predict-h1n1-and-seasonal-flu.ipynb) | XGBoost with fold-wise target encoding | **0.85780** | `submission5.csv` |
| [6](notebooks/6-flu-shot-learning-predict-h1n1-and-seasonal-flu.ipynb) | LightGBM binary relevance with multilabel stratification | **0.86709** | `submission6.csv` |
| [7](notebooks/7-flu-shot-learning-predict-h1n1-and-seasonal-flu.ipynb) | Tuned Random Forest with engineered ordinal and missingness features | **0.85810** | `submission7.csv` |
| [8](notebooks/8-flu-shot-learning-predict-h1n1-and-seasonal-flu.ipynb) | Regularized logistic regression baseline | **0.85803** | `submission8.csv` |
| [9](notebooks/9-flu-shot-learning-predict-h1n1-and-seasonal-flu.ipynb) | CatBoost + LightGBM + two logistic models with target-specific OOF weights | **0.86933** | submission9.csv |
| [10](notebooks/10-flu-shot-learning-predict-h1n1-and-seasonalflu.ipynb) | Respondent-level CatBoost/LightGBM/linear blend with OOF selection | **0.86881** | `submission10.csv` |
| [11](notebooks/11-flu-shot-learning-predict-h1n1-and-seasonal-flu.ipynb) | Notebook 9 extended with AUC-aware probability vs rank blending | Run notebook | `submission11.csv` |

### Why notebook 11?

The two tree models and the linear models can produce probabilities on different calibration scales. Notebook 11 evaluates both ordinary probability blends and percentile/rank-normalized blends using only out-of-fold predictions. Since ROC AUC depends on ranking rather than probability calibration, this gives the ensemble another principled candidate without using hidden test labels. The selected target-specific blend is then refit through the existing fold ensemble and written in the exact DrivenData submission format.

## Project Structure

```text
Flu Shot Learning/
├── .gitignore
├── LICENSE
├── README.md
├── requirements.txt
├── data/                         # Local competition data; ignored by Git
│   ├── raw/                      # Downloaded CSV files
│   └── processed/                # Optional generated datasets
├── notebooks/                    # Main analysis notebooks
├── drafts/                       # Local experiments; ignored by Git
└── submissions/                 # Generated submission files
```

## Reproducibility

The notebooks use fixed random seeds where possible. Local validation scores are estimates; the official score is calculated after submitting predictions to DrivenData.
