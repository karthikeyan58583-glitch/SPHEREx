# Machine Learning Models Registry (`ml/models/`)

This directory holds serialized model weights, hyperparameter configuration files, and evaluation benchmark results:
- Serialized model artifacts (`.joblib`, `.pkl`, `.onnx`).
- Benchmark evaluation summaries (`metrics.json`).

### Policy Note
* **Git Exclusion**: Binary model weights and serialized checkpoints are excluded from version control via `.gitignore`.
* **Reproducibility**: Models must be accompanied by training logs or configuration files enabling full retraining from the feature dataset.
