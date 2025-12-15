# dropout_prediction
This repository provides the reproducible implementation for the study:

**“A Portable, Generalizable Machine Learning Framework for Long-Term Student Dropout Prediction”**  
(IEEE Access, 2025 — second-round revision)

## Data Availability & Privacy

**Note on Real Data:**
The original student-level administrative records used in the study are owned by Sun Moon University and cannot be publicly released due to strict national privacy regulations and institutional policies.

**Synthetic Dataset for Reproducibility:**
To ensure the reproducibility of our code and pipeline, we provide a **synthetic dataset** (`datasets/synthetic_aca_21-23.csv`) that mimics the structure and feature distribution of the original data.
*   This file allows reviewers and researchers to run the full pipeline (from Phase 2 onwards).
*   **Usage:** You can load this dataset directly in `2_sample_construction.ipynb` to test the sample generation logic.

### Data Dictionary (Registry Features)
The framework uses standard registry variables:
*   `sex`, `nation`, `admtype`, `inquota`: Demographic and admission details.
*   `college`: Affiliated college.
*   `year`, `years_since`: Academic timeline information.
*   `gpa_last`, `credits_last`, `credits_tot`: Academic performance metrics.
*   `n_semesters`: Total semesters enrolled.
*   `status`: Current enrollment status (Enrolled, On-leave, etc.).

## Usage Pipeline

The notebooks are numbered to guide you through the pipeline.

### Phase 1: Data Preparation
*   **`1_data_preparation.ipynb`**: Integrates raw semester snapshots.
    *   *Note: This step requires raw semester files. For reproducibility using synthetic data, please skip to Phase 2.*

### Phase 2: Sample Construction
*   **`2_sample_construction.ipynb`**: Handles the rolling window logic. It processes leave-of-absence periods (military leave, etc.) and generates targets ($Y_t$) for horizons $t=1, 2, 3$.

### Phase 3: Base Modeling & Tuning
*   **`3_hp_tune_cat_t_1_2.ipynb`**: Demonstrates hyperparameter tuning (Grid Search) for CatBoost.
*   **`3_test_base_t_1_{n}.ipynb`**: Reproduces **Table 6** (Baseline performance). Evaluates SVM, MLP, LightGBM, XGBoost, and CatBoost.
*   **`3_test_stack_t_1_{n}.ipynb`**: Reproduces Stacking results in **Table 6**.

### Phase 4: Class Imbalance Handling
*   Evaluates resampling techniques (SMOTE, ADASYN, CNN, ENN, Tomek-links, SMOTE-ENN) for $t=2$.
*   **`4_imbal_all_no_opt_t_1_2.ipynb`**: Reproduces the results of SVM, MLP, LightGBM, XGBoost, and CatBoost in **Table 9 (Default Threshold)**.
*   **`4_imbal_all_opt_t_1_2.ipynb`**: Reproduces the results of SVM, MLP, LightGBM, XGBoost, and CatBoost in **Table 9 (Optimized Threshold)**.
*   **`4_imbal_stack_no_opt_t_1_2.ipynb`**: Reproduces Stacking results in **Table 9 (Default Threshold)**.
*   **`4_imbal_stack_opt_1_2.ipynb`**: Reproduces Stacking results in **Table 9 (Optimized Threshold)**.
    *   *Files ending in `_no_opt` show performance without threshold tuning for comparison.*

### Phase 5: Calibration & Explanation
*   **`5_calibrate_t_1_2.ipynb`**: Reproduces **Figure 2**. Applies Platt Scaling and visualizes reliability diagrams.
*   **`5_shap_t_1_2.ipynb`**: Reproduces **Figure 3**. Generates SHAP summary plots to explain model decisions.

## 📝 Citation

If you use this code or framework, please cite our paper:

> J. B. Kwon, "A Portable, Generalizable Machine Learning Framework for Long-Term Student Dropout Prediction," *IEEE Access*, under review, 2025.

---
For any questions regarding the code or data, please contact the corresponding author at [Your Email Address].
