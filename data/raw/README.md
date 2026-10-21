# Raw Data Storage (`data/raw/`)

This directory temporarily holds raw FITS products and uncompressed cutouts retrieved from the NASA/IPAC IRSA archive during development and testing.

### Rules & Exclusions
* **Git Exclusion**: All contents of this directory (except this `README.md`) are excluded from version control via `.gitignore`.
* **Never Commit Real FITS Files**: Large raw astronomical FITS data must never be committed to Git.
* **Non-Authoritative**: This directory serves only as a local scratch buffer. The authoritative source remains NASA/IPAC IRSA.
