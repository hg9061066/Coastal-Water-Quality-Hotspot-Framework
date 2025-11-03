# Coastal-Water-Quality-Hotspot-Framework
# A Machine Learning Framework for Forecasting Coastal Water Quality Hotspots in India

This repository contains the complete code, data, and analysis for the research paper: *"A Machine Learning Framework for Forecasting Coastal Water Quality Hotspots in India: A Comparative Analysis of Ensemble and Decision Tree Models"*.

This project develops a dual-component analytical framework to (1) classify current water quality "hotspots" using machine learning and (2) forecast future water quality trends to identify high-risk stations.

---

## 🔑 Key Features

* **1. Classification Framework (`master_ml_framework_final.py`):**
    * Compares Decision Tree, Random Forest, and XGBoost classifiers.
    * Employs a **Stratified Group K-Fold** cross-validation strategy (grouped by `station_code`) to prevent spatial data leakage and ensure model generalization.
    * Handles severe class imbalance using **SMOTE** and **class weighting**.
    * Generates key performance metrics (F1, PR-AUC, ROC-AUC) with 95% bootstrap confidence intervals.
    * Provides model interpretability via **SHAP** summary plots and feature importance tables.

* **2. Forecasting System (`forecast_prophet_final.py`):**
    * Uses **Facebook Prophet** to model and forecast long-term trends for `BOD` and `Dissolved Oxygen`.
    * Generates 2025-2026 forecasts with 95% credible intervals for every monitoring station.
    * Automatically identifies and flags **"high-risk" stations** projected to experience water quality degradation.
    * Generates individual PDF forecast plots for each station.

---

## 🔬 Project Structure

```
.
├── 📄 cleaned_water_quality_data.csv   # Main dataset (2020-2023)
├── 🐍 master_ml_framework_final.py     # (SCRIPT 1) Runs the classification (RF, XGB, DT)
├── 🐍 forecast_prophet_final.py        # (SCRIPT 2) Runs the Prophet forecasting
│
├── 📂 outputs/                        # Outputs from the classification script
│   ├── model_metrics_summary.csv      # (RESULT) Overall model performance
│   ├── feature_importances_summary.csv# (RESULT) Feature importance scores
│   ├── DecisionTree_cv_folds.csv      # (RESULT) Fold-by-fold metrics
│   ├── RandomForest_cv_folds.csv      # (RESULT) Fold-by-fold metrics
│   ├── XGBoost_cv_folds.csv           # (RESULT) Fold-by-fold metrics
│   └── 📂 plots/
│       ├── roc_comparison.pdf         # (FIGURE) ROC curve comparison
│       ├── pr_comparison.pdf          # (FIGURE) Precision-Recall curve comparison
│       ├── shap_DecisionTree.png      # (FIGURE) SHAP plot
│       ├── shap_RandomForest.png      # (FIGURE) SHAP plot
│       └── shap_XGBoost.png           # (FIGURE) SHAP plot
│
├── 📂 outputs_forecast/                 # Outputs from the forecasting script
│   ├── forecasts_summary.csv          # (RESULT) All forecast data (2025-2026)
│   ├── high_risk_forecasts.csv        # (RESULT) Stations flagged for degradation
│   └── 📂 plots/
│       └── (Station-specific forecast PDFs are saved here)
│
└── README.md
```

---

## 🚀 How to Run

### 1. Installation

1.  Clone this repository:
    ```bash
    git clone [https://github.com/your-username/Coastal-Water-Quality-Hotspot-Framework.git](https://github.com/your-username/Coastal-Water-Quality-Hotspot-Framework.git)
    cd Coastal-Water-Quality-Hotspot-Framework
    ```

2.  Create and activate a virtual environment:
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows: venv\Scripts\activate
    ```

3.  Install the required packages:
    ```bash
    pip install pandas scikit-learn xgboost prophet matplotlib seaborn shap imbalanced-learn
    ```
    *(Alternatively, you can create a `requirements.txt` file with these packages and run `pip install -r requirements.txt`)*

### 2. Run the Analysis

The analysis is run in two parts. Both scripts will read `cleaned_water_quality_data.csv` and generate their respective `outputs/` folders.

1.  **Run the Classification Framework:**
    ```bash
    python master_ml_framework_final.py
    ```
    *This will generate the `outputs/` directory with all model metrics and plots.*

2.  **Run the Forecasting System:**
    ```bash
    python forecast_prophet_final.py
    ```
    *This will generate the `outputs_forecast/` directory with all forecast data and plots.*

---

## 📈 Key Results

* **Best Classification Model:** **Random Forest** achieved the highest and most robust performance on the geographic holdout set (F1-Score: **0.985**) and in cross-validation (Mean F1-Score: **0.992**, PR-AUC: **~1.000**).

* **Key Hotspot Predictors (from SHAP):**
    1.  **Fecal Coliform** (High values $\rightarrow$ Hotspot)
    2.  **Biochemical Oxygen Demand (BOD)** (High values $\rightarrow$ Hotspot)
    3.  **Dissolved Oxygen (DO)** (Low values $\rightarrow$ Hotspot)

* **Forecasting:** The Prophet model identified numerous high-priority stations (see `high_risk_forecasts.csv`) projected to show increasing BOD and/or decreasing DO levels by 2026.

---
