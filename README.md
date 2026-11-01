# Woody Plant Encroachment Modeling

This project explores the use of remote-sensing and ecological data to classify woody versus non-woody vegetation and investigate woody plant encroachment across different geographic locations and years.

The analysis uses data from two study sites—the **Jornada Experimental Range (JORN)** and **Santa Rita Experimental Range (SRER)**—with observations from **2017 and 2021**. A major focus of the project was not only determining whether vegetation could be classified from remotely sensed features, but also exploring whether relationships learned at one site or time period generalized to another.

## Project Goals

The project addressed three main questions:

1. Can remotely sensed vegetation measurements distinguish woody from non-woody vegetation?
2. How do different classification algorithms perform on this task?
3. Do models trained at one geographic site or year generalize to observations from another site or year?

The final analyses also examined whether vegetation classifications could be translated into an ecologically meaningful measure of **fractional woody cover (FWC)**.

## Data and Features

Observations contain a binary vegetation classification (`woody` or `non-woody`) along with remotely sensed predictors including:

- **Canopy Height (CH)** — estimated vegetation height
- **NDVI** — Normalized Difference Vegetation Index
- **EVI** — Enhanced Vegetation Index
- **SAVI** — Soil-Adjusted Vegetation Index
- **ARVI** — Atmospherically Resistant Vegetation Index

Several variables include summary statistics such as **mean, median, and maximum**. These capture different characteristics of the measurements within an observation. For example, the mean represents average conditions, the median provides a measure less sensitive to extremes, and the maximum captures the strongest observed vegetation signal.

## Exploratory Data Analysis

EDA was used to examine:

- Feature distributions
- Missing observations
- Correlations among vegetation indices
- Class balance
- Differences between woody and non-woody vegetation
- Differences across sites and years

The analysis showed substantial overlap between woody and non-woody observations rather than a simple threshold separating the classes. Several vegetation indices were also strongly correlated.

These observations motivated evaluating multivariate classification algorithms capable of combining information across multiple predictors.

## Feature Selection

The project explored feature importance using **Boruta**, a Random-Forest-based feature-selection algorithm.

Boruta creates randomized copies of the original predictors, known as *shadow features*, by independently shuffling their values. It then compares the importance of the real predictors with these randomized alternatives.

A feature that consistently provides more predictive information than the shadow features has evidence of containing useful information about vegetation class.

## Model Comparison

The project explored both classical/non-tree classifiers and tree-based ensemble methods.

### Non-Tree Models

The non-tree model comparison included:

- Logistic Regression
- Linear Discriminant Analysis (LDA)
- Quadratic Discriminant Analysis (QDA)
- K-Nearest Neighbors (KNN)

### Tree-Based Models

Tree-based approaches included:

- Decision Trees
- Bagging
- Random Forest
- AdaBoost
- Gradient Boosting
- Additional boosted-tree implementations explored during model development

Models were evaluated using **5-fold cross-validation** along with classification metrics including accuracy, sensitivity/recall, specificity, precision, F1 score, and confusion matrices.

Tree-based ensemble methods frequently performed strongly, motivating additional investigation of their behavior across different training and testing datasets.

## Cross-Site and Cross-Year Generalization

A major component of the project was exploring whether models generalized beyond observations drawn from the same dataset.

Standard 5-fold cross-validation evaluates a model using held-out observations from the same underlying site/year. To provide a more challenging test of generalization, additional experiments deliberately used different datasets for training and testing.

Experiments included scenarios such as:

- Same site, different year
- Different site, same year
- Different site and different year

For example, a classifier could be trained using **JORN 2017** observations and evaluated using **SRER 2021** observations.

These experiments were exploratory rather than a forecasting exercise. Some combinations therefore included training on a later year and evaluating an earlier year. The purpose was to investigate patterns in the transferability of the learned relationship between remote-sensing features and vegetation class rather than to predict historical vegetation from future observations.

## Geographic Transfer Case Study: JORN 2021 → SRER 2021

A focused experiment trained the four non-tree classifiers on **2021 JORN** and evaluated them on **2021 SRER**.

Keeping the year constant while changing geographic location provided a useful test of geographic transfer.

Among these models, **QDA produced the strongest classification performance, with approximately 72% accuracy**. However, performance differed substantially across vegetation classes, demonstrating why accuracy alone was insufficient for evaluating the models.

The experiment also illustrated the difference between internal cross-validation and external transfer performance. For example, KNN achieved substantially stronger performance during tuning within the JORN data than when subsequently applied to SRER, suggesting geographic distribution shift between the two sites.

## Fractional Woody Cover

The project also translated individual vegetation classifications into **Fractional Woody Cover (FWC)**.

Rather than simply calculating the percentage of observations classified as woody, FWC incorporates the physical area represented by each observation:

```text
FWC = (woody vegetation area / total vegetation area) × 100
```

This provides a landscape-level estimate of woody vegetation coverage.

In the 2021 JORN → 2021 SRER transfer experiment, the actual SRER fractional woody cover was approximately **55%**, while the non-tree models produced estimates of roughly **85–91%**.

This was an important result: moderate observation-level classification accuracy did not necessarily translate into accurate landscape-level estimates.

## Key Findings

- Remote-sensing measurements contained useful information for distinguishing woody and non-woody vegetation, although the classes were not cleanly separable using individual features.
- Several vegetation indices were highly correlated, motivating investigation of feature importance and redundancy.
- Ensemble tree methods provided strong classification performance in several within-dataset comparisons.
- Performance generally became more variable when models were transferred across different sites or years.
- Same-site, cross-year experiments suggested that some learned relationships persisted over time, but temporal generalization differed between study sites.
- Cross-site experiments demonstrated meaningful geographic distribution shift.
- Strong performance under cross-validation within one dataset did not guarantee equivalent performance on a different site's data.
- Moderate classification accuracy could still produce substantial errors in fractional woody cover because classification performance and landscape-level area estimation are different objectives.

## Overall Conclusion

The results suggest that remotely sensed vegetation characteristics provide a meaningful signal for woody/non-woody classification, but that this relationship is not completely stable across geographic locations and years.

Rather than identifying a universally transferable classifier, the exploratory transfer experiments highlighted the importance of **external validation and distribution shift** when applying ecological machine-learning models beyond the environment in which they were trained.

The fractional woody cover analysis further demonstrated that good observation-level classification performance alone is not sufficient to guarantee accurate ecological estimates. Future work could focus on improving cross-site calibration, investigating sources of distribution shift, incorporating additional environmental variables, and directly validating landscape-level woody-cover estimates.

## Repository Structure

The repository contains notebooks covering the major stages of the analysis:

- **EDA** — exploration of vegetation features across JORN and SRER
- **Feature Selection** — Boruta-based investigation of predictor importance
- **Non-Tree Model Comparison** — Logistic Regression, LDA, QDA, and KNN with cross-validation
- **Tree-Based Model Comparison** — Decision Trees and ensemble methods with cross-validation
- **Cross-Dataset Transfer Experiments** — exploratory training/testing across sites and years
- **JORN 2021 → SRER 2021 Validation** — focused geographic-transfer experiment
- **Fractional Woody Cover** — conversion of classifications into landscape-level woody-cover estimates


## Contributors
- Sophie Mao
- Bhoomika Khatri
- Hannah Wang
