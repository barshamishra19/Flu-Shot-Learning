 # Flu Shot Learning

Machine learning project for the DrivenData competition **Flu Shot Learning: Predict H1N1 and Seasonal Flu Vaccines**. The models predict vaccination probabilities for `h1n1_vaccine` and `seasonal_vaccine`, evaluated using the mean ROC AUC of both targets.

## Data

The competition data is not included in this repository. Download it from the [DrivenData competition page](https://www.drivendata.org/competitions/66/flu-shot-learning/), then place the CSV files in a local `data/raw/` directory.

Expected files:

- `training_set_features.csv`
- `training_set_labels.csv`
- `test_set_features.csv`
- `submission_format.csv`

The `data/` directory is ignored by Git because the dataset should be downloaded separately and may be subject to the competition's terms.

## Setup

```bash
python -m venv .venv
```

Activate the environment and install dependencies:

```bash
pip install -r requirements.txt
```

Open the notebooks in `notebooks/` or `drafts/vsc/` and run them after downloading the data. The ensemble notebook writes `submission_ensemble.csv` to its current working directory.

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
│   
└── submissions/                 # Generated submission files
```

## Reproducibility

The notebooks use fixed random seeds where possible. Local validation scores are estimates; the official score is calculated after submitting predictions to DrivenData.
