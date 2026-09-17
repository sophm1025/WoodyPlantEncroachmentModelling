# EarthScienceSVSM

Machine-learning analyses for predicting **woody-plant encroachment** using ecological and remote-sensing measurements from the **Santa Rita Experimental Range (SRER)** and **Jornada Experimental Range (JORN)** NEON sites. The original repository was developed by the UNCW-SVSM 2022 Earth Science Group.

## Overview

This repository contains exploratory data analysis, feature-selection work, supervised classification experiments, cross-site/cross-year validation, model comparison, and prediction outputs. The notebooks compare multiple machine-learning approaches for distinguishing **woody** and **non-woody** vegetation classes and investigate how model performance transfers between sites and years.

The analysis includes both general model comparisons and targeted experiments in which data from one site/year are used for training and another site/year is used for testing. Several notebooks also incorporate **fractional woody cover (FWC)** analyses.

## Repository Structure

```text
EarthScienceSVSM/
├── README.md
├── LICENSE
├── .gitattributes
├── .gitignore
├── requirements.txt
│
├── data/
│   ├── README.md
│   ├── datasets/
│   │   ├── JORN17_dataset_v1.csv
│   │   ├── JORN17_dataset_v2.csv
│   │   ├── JORN21_dataset_v1.csv
│   │   ├── SRER21_dataset_v1.csv
│   │   ├── SRER_2017_training_bi.csv
│   │   └── precip1922-2022.csv
│   └── predictions/
│       ├── SRER17_pred.csv
│       └── SRER21_pred.csv
│
├── notebooks/
│   ├── eda/
│   ├── models/
│   │   ├── core/
│   │   ├── boosting/
│   │   └── tree_based/
│   └── validation/
│       ├── JORN_as_validation/
│       ├── SRER_as_validation/
│       └── cross_site_transfer/
│
├── analysis/
│   └── feature_selection/
│
├── results/
│   ├── README.md
│   └── figures/
│
└── archive/
    └── experimental_or_duplicate/
```

Generated notebooks checkpoints, macOS metadata, CatBoost training logs, and other automatically generated artifacts are intentionally excluded from the cleaned repository.

## Data

The primary datasets represent SRER and JORN observations from **2017 and 2021**. Variables used throughout the notebooks include vegetation class and canopy-height/remote-sensing-derived measurements such as CH_mean, CH_max, CH_med, ARVI, EVI, NDVI, SAVI, and related summary statistics. The exact feature set varies by notebook.

The repository uses **Git LFS for CSV files**. When cloning the repository, install Git LFS and retrieve the tracked data:

```bash
git lfs install
git lfs pull
```

If a CSV is only a small file containing a `git-lfs.github.com/spec/v1` header, it is an LFS pointer rather than the full dataset.

## Exploratory Data Analysis

The `notebooks/eda/` directory contains analyses of: 

- SRER 2017
- SRER 2021
- JORN 2017
- JORN 2021
- combined site/year datasets
- 2017 versus 2021 comparisons
- woody versus non-woody vegetation distributions
- canopy-height distributions
- correlation heatmaps
- pairplots and boxplots
- fractional woody cover
- precipitation

These notebooks are used to inspect the distributions and relationships among candidate predictors before model training.

## Feature Selection

The `analysis/feature_selection/` directory contains the original **Boruta** analysis materials and related variable-selection work. The feature-selection notebooks/scripts investigate which predictors are most informative for the woody/non-woody classification task.

## Machine-Learning Models

The model notebooks compare a broad set of supervised classification methods, including:

- Logistic Regression
- K-Nearest Neighbors (KNN)
- Linear Discriminant Analysis (LDA)
- Quadratic Discriminant Analysis (QDA)
- Support Vector Classification (SVC)
- Decision Trees
- Bagging
- Random Forest
- Gradient Boosting
- AdaBoost
- XGBoost
- LightGBM
- CatBoost

Several notebooks calculate accuracy, confusion matrices, classification reports, cross-validation scores, and ROC/AUC-related results. Tree-based and boosting analyses receive their own folders because they include multiple targeted experiments and model variants.

## Cross-Site and Cross-Year Validation

A major part of the repository evaluates how models trained on one dataset perform on a different site and/or year. The validation notebooks include experiments such as:

```text
2017 JORN  -> 2021 JORN
2017 SRER  -> 2021 JORN
2021 JORN  -> 2017 JORN
2021 SRER  -> 2021 JORN
2017 JORN  -> 2017 SRER
2017 JORN  -> 2021 SRER
2021 JORN  -> 2021 SRER
2021 SRER  -> 2017 SRER
```

The `JORN_as_validation/` and `SRER_as_validation/` folders preserve these experiment groups while making their purpose easier to navigate. Some experiments use cross-validation, while others perform direct train/test transfer across sites or years.

## Fractional Woody Cover

Fractional woody cover (FWC) is analyzed in dedicated exploratory and model-transfer notebooks. The repository includes comparisons where FWC is incorporated into prediction workflows as an additional variable or used for site/year comparison.

## Model Results

The repository contains model-prediction CSVs and previously generated performance figures. The `results/figures/` directory contains the performance plots that were already part of the original repository.

The notebooks remain the authoritative source for the detailed experimental setup, feature engineering, model parameters, and evaluation logic for each experiment because this project was developed primarily as a notebook-based research workflow.

## Installation

Create a Python environment and install the dependencies:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

On Windows, activate the environment with:

```powershell
.venv\Scripts\activate
```

Then launch Jupyter:

```bash
jupyter lab
```

Some notebooks also contain environment-specific commands or assumptions from the original development environment. Those may need to be adapted for a local machine.

## Reproducing the Analysis

Because the repository contains many experiments rather than a single linear training script, reproduction is organized by analysis family:

1. Retrieve the LFS datasets.
2. Start with the EDA notebooks for the relevant site/year.
3. Review feature-selection results where applicable.
4. Run the core model-comparison notebooks.
5. Run the tree-based/boosting analyses.
6. Run the relevant cross-site/cross-year validation notebook.
7. Compare the resulting evaluation metrics and prediction outputs.

Notebook filenames describe the intended experiment, including training year/site, testing year/site, cross-validation, and whether FWC is included.

## Research Workflow

```text
NEON / ecological datasets
        |
        v
Exploratory Data Analysis
        |
        v
Feature Selection
        |
        v
Train Classification Models
        |
        +--> Linear / Discriminant Models
        |
        +--> SVM / KNN
        |
        +--> Tree-Based Models
        |
        +--> Boosting Models
        |
        v
Cross-Validation / Cross-Site Testing
        |
        v
Metrics + Predictions + Visualizations
```

## Important Notes

- The cleaned repository preserves the original notebooks rather than rewriting them into a new software package, because the notebooks contain the project-specific experimental logic and results.
- Duplicate copies of the same dataset were consolidated into `data/datasets/`; the original repository contained the same Git LFS pointer files in several notebook folders.
- One exact duplicate of `XGB_LGB_roc.ipynb` was removed from the cleaned tree because the same notebook exists elsewhere in the original repository.
- `All Models-Copy1.ipynb` is preserved under `archive/experimental_or_duplicate/` rather than discarded, since it is an alternate notebook rather than an exact byte-for-byte duplicate.
- The original `catboost_info/` directory and Jupyter checkpoint directories are generated artifacts and are not part of the analytical source material.

## Scope

This repository is best understood as a **research analysis and model-comparison project** rather than a production ML package. It documents a substantial set of experiments for predicting woody-plant encroachment across NEON sites and years, with particular emphasis on comparing classical, tree-based, and boosting classifiers and testing model transfer between datasets.
