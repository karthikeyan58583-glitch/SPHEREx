# Machine Learning Source Code (`ml/src/`)

This directory contains standalone Python modules for dataset preparation, feature engineering, and candidate ranking:
- `features.py`: Extraction of morphological, photometric, and residual statistics from difference arrays.
- `dataset.py`: Labeling, dataset assembly, train/val/test splits, and data leakage prevention guards.
- `train.py`: Reproducible model training scripts for the baseline scikit-learn model.
- `evaluate.py`: Evaluation metrics tracking false positive rates, precision, recall, and ROC-AUC curves.
