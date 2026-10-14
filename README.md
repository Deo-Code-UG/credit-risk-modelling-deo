# credit-risk-modelling-deo
Independent credit-risk modelling project covering Probability of Default (PD), Loss Given Default (LGD), Exposure at Default (EAD), expected loss estimation and portfolio stability monitoring using Python.

# Credit Risk Modelling: PD, LGD, EAD & Portfolio Stability Monitoring

## 1. Project Overview

This repository documents an independent credit-risk modelling project using historical LendingClub loan data. The project explores the development, evaluation and monitoring of models covering Probability of Default (PD), Loss Given Default (LGD), Exposure at Default (EAD), Expected Loss (EL) and Population Stability Index (PSI).

The objective is to understand how historical loan characteristics can be used to estimate credit risk, investigate potential losses and monitor changes in the characteristics of a loan portfolio.

The project brings together statistical modelling, Python-based data analysis, model diagnostics and portfolio monitoring in a single workflow.

## 2. Business Problem

Lenders need to assess the likelihood that borrowers will default, estimate potential losses if defaults occur, and understand the exposure associated with those losses.

They also need to monitor whether the characteristics of the portfolio and the behaviour of their models change over time.

This project explores these challenges through three complementary credit-risk components:

- **Probability of Default (PD):** estimating the likelihood of a borrower defaulting over a defined prediction horizon.
- **Loss Given Default (LGD):** estimating the proportion of exposure that is lost after accounting for recoveries.
- **Exposure at Default (EAD):** estimating the exposure outstanding when default occurs.

The three components can be combined to estimate Expected Loss:

**Expected Loss = PD × LGD × EAD**

The interpretation of the resulting estimate depends on the target definitions, time horizon, exposure units and assumptions used by each component.

## 3. Dataset

The project uses historical LendingClub loan data covering the 2007–2014 period, with approximately 466,000 loan records in the development dataset.

The data contains loan and borrower characteristics that can be used to investigate default risk, repayment behaviour and recovery outcomes.

The modelling workflow involves preparing the data, handling missing values, transforming categorical variables, constructing model inputs and aligning the features used during training and prediction.

The LGD analysis focuses on defaulted loans and recovery-related outcomes. The EAD/CCF analysis examines exposure-related quantities derived from loan amounts and repayment information.

Raw data files and local training/testing artifacts are not included in this repository.

## 4. Project Components

### 4.1 Probability of Default (PD)

The PD component explores logistic regression-based credit-risk modelling and scorecard-style feature preparation.

The workflow includes:

- Preparing loan-level predictor variables.
- Transforming categorical variables into model-ready representations.
- Preparing training and testing inputs.
- Estimating default-related probabilities.
- Examining model discrimination and probability calibration.
- Investigating score distributions and the interpretation of predicted probabilities.

The distinction between the probability of default and the probability of a good loan is important. Predicted probabilities must be mapped to the correct target class before they are used in downstream credit-risk calculations.

### 4.2 Loss Given Default (LGD)

The LGD component investigates recovery behaviour and loss severity among defaulted loans.

A two-stage modelling approach is explored:

- **Stage 1 — Recovery classification:** modelling the recovery-related outcome used to determine the relevant LGD modelling pathway.
- **Stage 2 — Recovery-rate estimation:** estimating recovery severity for the subset of observations selected by the Stage 2 target definition.

The analysis also examines data preparation, feature selection, multicollinearity and regression diagnostics.

LGD estimates must be interpreted in relation to the exact recovery-rate definition, the exposure denominator, the population being modelled and the assumptions used to convert recoveries into loss severity.

### 4.3 Exposure at Default (EAD) and Credit Conversion Factor (CCF)

The EAD component explores exposure estimation using loan amounts and repayment-related information.

The Credit Conversion Factor (CCF) provides a way to express exposure relative to an appropriate reference amount. The project examines a CCF-style calculation based on funded loan amounts and total recovered principal.

The purpose is to investigate how exposure-related quantities can support expected-loss estimation.

The definition and economic interpretation of the CCF must be checked against the actual implementation and the type of credit exposure being modelled. A formula developed for a particular loan dataset should not automatically be treated as a validated regulatory or production EAD model.

### 4.4 Expected Loss (EL)

The project brings the three credit-risk components together through the relationship:

**EL = PD × LGD × EAD**

For a meaningful calculation, the PD must represent the probability of default over the relevant horizon, LGD must represent the appropriate loss proportion, and EAD must be expressed in compatible monetary units.

The component estimates also need to be aligned to the same loan or exposure and the same prediction point.

The integrated workflow therefore requires careful alignment of model inputs, loan identifiers, prediction outputs and target definitions. These checks are important to avoid mismatched observations, incorrect probability classes and information leakage.

## 5. Model Evaluation and Diagnostics

The project includes model evaluation and diagnostic workflows intended to examine different aspects of model quality.

### PD evaluation

The evaluation methods explored include:

- ROC curve and Area Under the ROC Curve (ROC-AUC).
- Kolmogorov–Smirnov (KS) discrimination assessment.
- Gini coefficient.
- Probability calibration assessment.
- Score and probability distribution analysis.

Discrimination measures how well a model separates observations belonging to different outcome classes. Calibration examines whether predicted probabilities correspond to observed outcome frequencies.

A model can rank borrowers effectively without producing well-calibrated probabilities, so these dimensions should be assessed separately.

### LGD and EAD diagnostics

The modelling work investigates issues relevant to regression and exposure estimation, including:

- Multicollinearity and Variance Inflation Factor (VIF) diagnostics.
- Constant and highly correlated predictors.
- Missing and invalid values.
- Target definitions and predictor selection.
- Potential target leakage.
- The interpretation of regression evaluation results.

Diagnostic warnings and weak evaluation results require investigation rather than automatic removal of variables or assumptions that the model is valid.

### Validation considerations

A rigorous credit-risk assessment should distinguish training performance from validation performance and final testing.

Random train/test splits can be useful during development, but they do not by themselves demonstrate that a model will perform reliably on later loan vintages.

Chronological validation, out-of-time testing, leakage checks and stability analysis are important considerations for assessing performance under changing portfolio conditions.

## 6. Population Stability Index (PSI) and Portfolio Monitoring

The PSI component explores changes in the distributions of model inputs, scores and portfolio characteristics between a reference population and a comparison population.

PSI can help identify population shifts that warrant further investigation.

The commonly used interpretive bands explored in this project are:

- **PSI below 0.10:** generally treated as relatively stable.
- **PSI from 0.10 to below 0.25:** a potential warning that warrants monitoring.
- **PSI of 0.25 or greater:** a potentially material distribution shift requiring investigation.

These thresholds are practical rules of thumb, not universal regulatory standards or proof that a model has failed.

The monitoring workflow is intended to support:

- Identification of changes in population and feature distributions.
- Comparison of score distributions across populations or periods.
- Investigation of variables contributing to observed shifts.
- Assessment of potential data drift and changes in portfolio composition.
- Identification of cases requiring further model-performance or calibration checks.

A PSI result alone does not establish why a distribution has changed or prove that predictive performance has deteriorated. It should be interpreted alongside business context, outcome data and other model monitoring evidence.

## 7. Technology and Tools

The project uses Python and associated data analysis and modelling tools, including:

- Python
- pandas for data preparation and manipulation
- NumPy for numerical operations
- scikit-learn for machine learning and model evaluation
- statsmodels and SciPy for statistical analysis and diagnostics, where used
- Matplotlib and Seaborn for visualisation, where used
- Jupyter Notebook for interactive analysis and documentation

The final dependency list should be checked against the imports and package versions required by the published notebooks.

## 8. Repository Structure

The repository contains the project notebooks and supporting documentation for the credit-risk modelling workflow.

The intended organisation is:

- `notebooks/` — PD development, LGD/EAD modelling and PSI monitoring notebooks.
- `src/` — reusable Python functions extracted from the notebooks, where applicable.
- `reports/` — selected evaluation tables, charts and monitoring findings.
- `README.md` — project description, modelling approach and limitations.
- `requirements.txt` — Python dependencies required to reproduce the analysis.
- `.gitignore` — rules for excluding temporary files, local artifacts and data files.

Folder names and contents should reflect the files actually present in the repository.

## 9. Limitations and Responsible Interpretation

This is an independent modelling project using historical loan data. It should not be interpreted as evidence that the models have been deployed in a live lending institution or approved for regulatory use.

Model estimates depend on data quality, target definitions, feature availability, modelling assumptions and validation design.

The project requires particular attention to:

- Consistency between training and prediction features.
- Correct mapping of model probability classes.
- Potential leakage from recovery, repayment or post-default information.
- Appropriate temporal validation and out-of-time testing.
- Calibration and stability under changing portfolio conditions.
- The compatibility of PD, LGD and EAD definitions when calculating expected loss.

Performance claims should be based on verified evaluation results rather than assumed from the modelling technique used.

## 10. Reproducibility

The notebooks document the analysis and modelling workflows.

To reproduce the work, a user will need a compatible Python environment, the required packages, access to the appropriate source dataset and any additional files explicitly required by the notebooks.

Some notebooks may currently depend on local data files, saved model objects or training/testing artifacts that are not included in the repository. These dependencies must be configured before the entire workflow can be reproduced on another computer.

Package versions, data-access instructions and execution order should be documented after the notebooks have been checked in a clean environment.

## 11. Project Status

The repository presents an independent credit-risk modelling and monitoring project. Its notebooks provide the basis for reviewing the implementation, diagnostic work and evaluation evidence.

The completeness and reproducibility of each component, as well as the strength of its validation evidence, should be assessed from the corresponding code and results.

## Author

**Deogratius Kizito**

Independent Credit Risk Modelling | Data Analytics | Statistical Modelling | Portfolio Monitoring
