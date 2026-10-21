# Machine Learning Datasets (`ml/datasets/`)

This directory houses dataset manifests, feature tables, split definitions, and dataset-level metadata for the candidate ranking and classification pipeline.

---

## 1. Purpose & Contents

This directory maintains the structured data contracts required for reproducible machine-learning development:
- **Dataset Manifests**: Master catalogs inventorying candidate sample identifiers, parent observation pairings, and spatial coordinates.
- **Candidate Feature Tables**: Tabular extracts capturing photometric properties (flux ratios, SNR, significance), morphological parameters (circularity, elongation, FWHM), and residual PSF fit statistics.
- **Label Provenance**: Detailed audit records explaining the origin of sample labels (e.g., synthetic transient injections, verified optical artifacts, diffraction spike masks) to ensure transparent training data provenance.
- **Reproducible Split Definitions**: Fixed train, validation, and test partition mappings (using spatial/temporal blocking to prevent data leakage across sky regions or observation dates).
- **Dataset Evaluation Metadata**: Benchmark statistics, feature distributions, class balance summaries, and baseline evaluation logs.

---

## 2. Storage & Version Control Policy

* **Exclude Large Data Files by Default**: Generated numerical arrays (`.npy`, `.npz`), binary array containers (`.h5`, `.hdf5`), multi-epoch cutout image stacks, and large feature matrices must **not** be committed to Git.
* **Lightweight Metadata Only**: Only small, human-readable dataset manifests, schema definitions, and partition mapping indices (JSON, CSV, YAML) should be tracked in version control.
* **Reproducibility**: Datasets must be deterministically regenerable using the feature extraction scripts in `ml/src/` from raw or cached observation cutouts.
