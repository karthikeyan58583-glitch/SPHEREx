# Observation Manifests & Query Inventories (`data/manifests/`)

This directory stores lightweight observation inventories, archive query results, and validation manifests generated during NASA/IPAC IRSA archive discovery and planning workflows.

---

## 1. Purpose & Contents

This directory holds structured metadata catalogs and query results:
- **Observation Inventories**: Records of discovered SPHEREx observations covering target sky regions.
- **Archive Product Identifiers**: Specific IRSA product identifiers, file URIs, and data release versions (e.g., `QR2`, `QR3`).
- **Temporal Metadata**: Physical observation timestamps (`DATE-OBS`, `MJD`), exposure durations, and visit intervals.
- **Spectral & Wavelength Information**: Available spectral bands, central wavelengths ($\mu m$), wavelength ranges, and channel counts.
- **Data Provenance**: Retrieval timestamps, IRSA query parameters, and pipeline execution context.
- **Scientific Validation Status**: Pairwise compatibility flags (spatial overlap percentage, temporal baseline suitability, calibration consistency).

---

## 2. Storage & Version Control Policy

* **Keep Large Data Out of Git**: Only lightweight, structured metadata records (e.g., JSON, CSV, or YAML manifests) may be stored here.
* **No Raw or Processed Science Files**: Raw FITS images, spectral data cubes, calibrated cutouts, and binary arrays must **never** be placed in this directory; they belong in `data/raw/`, `data/processed/`, or `data/cache/` and are strictly excluded from version control.
