# Brugada Syndrome Detection from ECG

A comparative study of **feature engineering** and **deep learning** for Brugada syndrome screening using the **Brugada-HUCA** 12-lead ECG dataset.

This project focuses on a clinically meaningful question: **how can we reduce missed high-risk Brugada cases from ECG recordings?** Instead of positioning the model as a final diagnostic system, the workflow is designed as a **screening-support pipeline** to help surface suspicious ECGs earlier for closer clinical review.

## Project Overview

Brugada syndrome is a cardiac electrical disorder associated with ventricular arrhythmias and sudden cardiac death. Its ECG pattern may be subtle, variable, and occasionally transient, which makes detection difficult in routine clinical settings because missed positive cases can have serious consequences, this project prioritizes **recall-oriented screening** rather than accuracy alone.

The notebook compares three representation strategies:
- **Traditional features**: clinically informed handcrafted features extracted from leads V1-V3
- **DL Embedding**: BiGRU-based learned representations with attention
- **Hybrid**: combination of traditional and deep representations

These feature sets are evaluated across multiple classifiers using:
- **5-fold cross-validation on the training set**
- **Held-out test set evaluation** for final model comparison
- **Blind inference on uncertain patients (label = 2)** using a top-3 ensemble

## Main Findings

- Cross-validation favored several deep-learning-based combinations, especially **DL Embedding + ExtraTrees**.
- However, on the **held-out test set**, the best-performing model was **Traditional Features + Random Forest**.
- This model achieved the **highest recall** and the **lowest false negatives** among the final candidates.
- The results suggest that for a **small medical dataset**, **clinically grounded and interpretable features** may generalize better than more complex deep representations.
- In the uncertain group, the top-3 ensemble flagged **6 of 7 patients** at the safety-first threshold of **0.30**, including one borderline case that would have been missed at the default threshold of 0.50.

## Dataset

This notebook uses the **Brugada-HUCA** dataset.

A total of 363 standard 12-lead ECG recordings were collected from individuals evaluated for suspected Brugada syndrome at the Cardiology Department of the Hospital Universitario Central de Asturias (HUCA). Each recording has a duration of 12 seconds and a sampling frequency of 100 Hz.

## Important Path Note

The current notebook configuration still points to a **Kaggle-style absolute path**. If you run the notebook locally or from a GitHub clone, update the configuration cell accordingly.

Replace the dataset paths with something like this:

```python
from pathlib import Path

BASE_DIR = Path('.')
FILES_DIR = BASE_DIR / 'data' / 'files'
META_CSV  = BASE_DIR / 'data' / 'metadata.csv'
```

## Notebook Workflow

The notebook is organized into the following stages:

1. **Environment setup**
2. **Library imports and experiment configuration**
3. **ECG loading and signal preprocessing**
4. **Metadata cleaning and held-out train/test split**
5. **Traditional feature extraction**
6. **BiGRU training and deep embedding extraction**
7. **Construction of three feature sets**
8. **Brute-force model comparison with 5-fold CV**
9. **Retraining on the full training set**
10. **Final evaluation on the held-out test set**
11. **Feature importance and generalization analysis**
12. **Blind inference on uncertain patients (label = 2)**
13. **Export of final results to CSV**

## Citation

Costa Cortez, N., & Garcia Iglesias, D. (2026). Brugada-HUCA: 12-Lead ECG Recordings for the Study of Brugada Syndrome (version 1.0.0). PhysioNet. RRID:SCR_007345. https://doi.org/10.13026/0m2w-dy83

Goldberger, A., Amaral, L., Glass, L., Hausdorff, J., Ivanov, P. C., Mark, R., ... & Stanley, H. E. (2000). PhysioBank, PhysioToolkit, and PhysioNet: Components of a new research resource for complex physiologic signals. Circulation [Online]. 101 (23), pp. e215–e220. RRID:SCR_007345.
