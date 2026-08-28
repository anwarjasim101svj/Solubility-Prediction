
# Molecular Solubility Prediction Using Random Forest

A machine learning project for predicting molecular solubility using molecular descriptors calculated from SMILES representations and a Random Forest regression model.

## Project Overview

Molecular solubility is an important molecular property in areas such as drug discovery, environmental chemistry, chemical formulation, and materials research.

In this project, molecular structures represented as **SMILES strings** are converted into numerical molecular descriptors using **RDKit**. These descriptors are then used as input features for a **Random Forest Regression** model to predict experimentally measured log solubility.

The Random Forest hyperparameters are optimized using **GridSearchCV with 5-fold cross-validation**, followed by evaluation on a held-out test set.

## Workflow

```text
SMILES
   │
   ▼
RDKit Molecular Structure
   │
   ▼
Molecular Descriptors
   │
   ├── Molecular Weight
   ├── MolLogP
   ├── H-Bond Donors
   └── H-Bond Acceptors
   │
   ▼
Train/Test Split
   │
   ▼
Random Forest Regression
   │
   ▼
GridSearchCV
5-Fold Cross-Validation
   │
   ▼
Optimized Model
   │
   ▼
Solubility Prediction
   │
   ├── MAE
   ├── MSE
   ├── RMSE
   └── R²
```

## Dataset

This project uses the **Delaney ESOL solubility dataset**, which contains experimentally measured molecular solubility values.

The target variable is:

```text
measured log solubility in mols per litre
```

After removing duplicate rows and entries with missing values, the dataset contained:

* **1,128 molecules**
* **902 training samples**
* **226 test samples**

Molecular structures are represented using SMILES strings.

### Dataset Availability

The dataset file is **not included in this repository** because the copy used during development was obtained through an online course and is not being redistributed.

To run the notebook, obtain the dataset from an **authorized source** and place the CSV file in the location expected by the notebook.

The expected filename is:

```text
delaney-processed.csv
```

## Molecular Descriptors

Four molecular descriptors were calculated using **RDKit**:

| Descriptor      | Description                                   |
| --------------- | --------------------------------------------- |
| `MolWt`         | Molecular weight                              |
| `MolLogP`       | Estimated octanol/water partition coefficient |
| `NumHDonors`    | Number of hydrogen bond donors                |
| `NumHAcceptors` | Number of hydrogen bond acceptors             |

These descriptors provide basic structural and physicochemical information for establishing a molecular structure–solubility relationship.

## Machine Learning Model

### Random Forest Regression

A **Random Forest Regressor** was used to model the relationship between molecular descriptors and experimental log solubility.

Random Forest is an ensemble learning method that combines predictions from multiple decision trees.

The following hyperparameters were optimized:

* `n_estimators` — Number of decision trees
* `max_depth` — Maximum depth of each decision tree
* `min_samples_split` — Minimum number of samples required to split a node

### Hyperparameter Optimization

`GridSearchCV` was used with:

* **5-fold cross-validation**
* **RMSE** as the optimization metric
* **36 hyperparameter combinations**
* **180 total model fits**

The hyperparameter search was performed only on the training data. The test set was reserved for final model evaluation.

### Best Hyperparameters

The optimized Random Forest model used:

```text
n_estimators = 200
max_depth = 10
min_samples_split = 2
```

## Model Performance

The optimized model was evaluated on the **226-molecule held-out test set**.

| Metric |   Test Set |
| ------ | ---------: |
| MAE    | **0.5949** |
| MSE    | **0.7415** |
| RMSE   | **0.8611** |
| R²     | **0.8431** |

The model achieved an **R² of 0.8431**, meaning that approximately 84% of the variance in the experimental log solubility values was explained by the model on the held-out test set.

The model obtained an **RMSE of 0.8611 log solubility units** and an **MAE of 0.5949 log solubility units**.

## Visualizations

The project generates three visualizations to analyze the model:

### 1. Feature Importance

Shows the relative importance of each molecular descriptor in the Random Forest model.

### 2. Actual vs Predicted Solubility

Compares experimentally measured log solubility with model predictions.

Points closer to the diagonal `y = x` line indicate better agreement between experimental and predicted values.

### 3. Residual Distribution

Shows the distribution of prediction errors.

Residuals are calculated as:

```text
Residual = Actual − Predicted
```

The generated figure is saved as:

```text
results/solubility_prediction_results.png
```

## Technologies Used

* **Python**
* **Pandas** — Data handling
* **NumPy** — Numerical calculations
* **RDKit** — Molecular structure processing and descriptor calculation
* **Scikit-learn** — Machine learning, cross-validation, and model evaluation
* **Matplotlib** — Data visualization
* **Jupyter Notebook** — Interactive development

## Project Structure

```text
solubility-prediction-random-forest/
│
├── notebooks/
│   └── solubility_prediction.ipynb
│
├── results/
│   └── solubility_prediction_results.png
│
├── README.md
├── requirements.txt
└── .gitignore
```

The original dataset is intentionally **not included** in the repository.

## Installation

Clone the repository:

```bash
git clone https://github.com/anwarjasim101svj/solubility-prediction-random-forest.git
cd solubility-prediction-random-forest
```

Install the required Python packages:

```bash
pip install -r requirements.txt
```

Launch Jupyter Notebook:

```bash
jupyter notebook
```

Before running the notebook, obtain the dataset from an authorized source and place `delaney-processed.csv` in the expected location.

## Reproducibility

A fixed random seed of `42` is used for the train-test split:

```python
random_state=42
```

The Random Forest model also uses:

```python
random_state=42
```

This allows the experiment to be reproduced under the same software environment and dataset.

## Limitations

This project uses only four molecular descriptors and therefore represents a relatively simple molecular machine learning workflow.

The model could potentially be improved by incorporating:

* Additional molecular descriptors
* Molecular fingerprints
* Feature selection
* Feature engineering
* Alternative regression algorithms
* Larger and more diverse datasets
* Independent external validation

Therefore, this project should be considered a **demonstration of a molecular property prediction workflow**, rather than a production-ready solubility prediction model.

## Future Improvements

Potential extensions include:

* Comparing Random Forest with Gradient Boosting and other regression algorithms
* Incorporating Morgan fingerprints using RDKit
* Performing feature selection
* Applying model interpretation techniques such as SHAP
* Comparing predictions with established ESOL model predictions
* Evaluating the model on an independent external dataset
* Expanding the descriptor set to capture additional molecular properties

## Author

**Anwar Jasim C**

MSc Chemistry | Computational Chemistry | Molecular Simulation | Machine Learning for Chemistry
