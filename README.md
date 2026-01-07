# dropout_prediction
This repository provides the reproducible implementation for the study:

**“A Portable, Generalizable Machine Learning Framework for Long-Term Student Dropout Prediction”**
(IEEE Access, 2026 — third-round revision)

## Data Availability & Privacy

**Note on Real Data:**
The original student-level administrative records used in the study are owned by Sun Moon University and cannot be publicly released due to strict national privacy regulations and institutional policies.

**Synthetic Dataset for Reproducibility:**
To ensure the reproducibility of our code and pipeline, we provide a **synthetic dataset** (`datasets/synthetic_aca_21-23.csv`) that mimics the structure and feature distribution of the original data.
*   This file allows reviewers and researchers to run the full pipeline (from Phase 2 onwards).
*   **Usage:** You can load this dataset directly in `2_sample_construction.ipynb` to test the sample generation logic.

## Note on Reusability & Configuration

This codebase is designed as a **methodological skeleton**. While it includes specific configurations used for the case study, the core logic is portable.

To adapt this framework to your institution, please locate and update the **institution-specific variables** defined in the preprocessing notebooks:

**1. Input Data Loading (`1_data_preparation.ipynb`):**
*   **File Paths & Loading Logic:** The current code loads six semester-level snapshots from an Excel file (`datasets/academic_data_21-23.xlsx`). You must update these lines to load your institution's raw data files (e.g., CSVs or SQL query results) and concatenate them into a single DataFrame `df`.

**2. Data Schema & Mappings (`1_data_preparation.ipynb`):**
*   **Column Names:** Update the `col_names` list (Cell 2) to match your raw data schema.
*   **Status Codes:** Update the `state_code` dictionary to map your institution's status labels (e.g., 'Enrolled', 'Graduated') to the framework's standard integers (0, 1, etc.).
*   **Demographics & Admission:** Update `nation_code` (nationality groups) and `in_capa` (in-quota/out-of-quota) mappings to reflect your student body composition and admission policies.
*   **Organizational Codes:** Update `college_code` and `adm_unit_code` dictionaries to reflect your institution's college/department structure and admission types.

**2. Horizon Parameters (`2_sample_construction.ipynb`):**
*   **Target Definitions:** In the `prepare_dataset` function, the logic defining the prediction horizon (e.g., variables like `no_next` checking `df['year'] == 2023`) is hardcoded to the study's timeframe (2021-2023). You must update these year/semester conditions to match the final snapshot of your own dataset.

*These sections are conceptually marked as configuration points requiring local adaptation.*

### Data Dictionary (Registry Features)
The framework uses standard registry variables:
*   `semester`(snapshot): Spring (0) or Fall (1).
*   `sex`: Male or Female.
*   `nation`: Nationality (6 groups: Korea, Japan, Vietnam, . . . ).
*   `adm_unit`(adm_type): Type of admission (5 types).
*   `inquota`: In-quota or out-of-quota admission.
*   `college`: Affiliated college (6 colleges).
*   `grade`(year): Academic year (1–5).
*   `years_since`: Calendar years since initial admission (including leave periods).
*   `gpa_last`: GPA in the last semester (0–4.5).
*   `credits_last`: Credits earned in the last semester.
*   `credits_tot`: Cumulative credits earned.
*   `n_semesters`: Total semesters enrolled.
*   `state` (status): Academic status in the snapshot (enrolled (0) / graduated (1) / dropped out (2) / on leave (3)).

## Usage Pipeline

The notebooks are numbered to guide you through the pipeline.

### Phase 1: Data Preparation
*   **`1_data_preparation.ipynb`**: Integrates raw semester snapshots.
    *   *Note: This step requires raw semester files. For reproducibility using synthetic data, please skip to Phase 2.*

### Phase 2: Sample Construction
*   **`2_sample_construction.ipynb`**: Handles the rolling window logic. It processes leave-of-absence periods (military leave, etc.) and generates targets ($Y_t$) for horizons $t=1, 2, 3$.

### Phase 3: Base Modeling & Tuning
*   **`3_hp_tune_cat_t_1_2.ipynb`**: Demonstrates hyperparameter tuning (Grid Search) for CatBoost.
*   **`3_test_base_t_1_{n}.ipynb`**: Reproduces **Table 6** for SVM, MLP, LightGBM, XGBoost, and CatBoost.
*   **`3_test_stack_t_1_{n}.ipynb`**: Reproduces Stacking results in **Table 6**.
*   **`3_robust_tempo_datasets.ipynb`**: Builds temporal splits of the dataset for the robustness test.
*   **`3_robust_base_t_1_2.ipynb`**: Reproduces **Table 7** for SVM, MLP, LightGBM, XGBoost, and CatBoost.
*   **`3_robust_stacking_t_1_2.ipynb`**: Reproduces Stacking results in **Table 7**.

### Phase 4: Class Imbalance Handling
Evaluates resampling techniques (SMOTE, ADASYN, CNN, ENN, Tomek-links, SMOTE-ENN) for $t=2$.

*   **`4_imbal_base_no_opt_t_1_2.ipynb`**: Reproduces the results of SVM, MLP, LightGBM, XGBoost, and CatBoost in **Table 9** (Default Threshold).
*   **`4_imbal_base_opt_t_1_2.ipynb`**: Reproduces the results of SVM, MLP, LightGBM, XGBoost, and CatBoost in **Table 9** (Optimized Threshold).
*   **`4_imbal_stack_no_opt_t_1_2.ipynb`**: Reproduces Stacking results in **Table 9** (Default Threshold).
*   **`4_imbal_stack_opt_t_1_2.ipynb`**: Reproduces Stacking results in **Table 9** (Optimized Threshold).

> *Files ending in `_no_opt` show performance without threshold tuning for comparison.*

### Phase 5: Calibration & Explanation
*   **`5_calibrate_t_1_2.ipynb`**: Reproduces **Figure 2**. Applies Platt Scaling and visualizes reliability diagrams.
*   **`5_shap_t_1_2.ipynb`**: Reproduces **Figure 3**. Generates SHAP summary plots to explain model decisions.

## 📝 Citation

If you use this code or framework, please cite our paper:

> J. B. Kwon, "A Portable, Generalizable Machine Learning Framework for Long-Term Student Dropout Prediction," *IEEE Access*, under review, 2025.

---
For any questions regarding the code or data, please contact the corresponding author at [jbkwon@sunmoon.ac.kr](mailto:jbkwon@sunmoon.ac.kr).
