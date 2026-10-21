# SPHEREx TimeLens — Data Policy & Scientific Standards

This document outlines the authoritative data sources, archive access policies, caching mechanisms, FITS handling, synthetic data rules, and scientific standards governing the **SPHEREx TimeLens** system.

---

## 1. Authoritative Data Source

* **Official Archive**: The sole authoritative source for astronomical observations in this project is the **NASA/IPAC Infrared Science Archive (IRSA)**.
* **SPHEREx Products**: SPHEREx TimeLens is engineered exclusively to discover, access, and analyze verified SPHEREx data products (spectral cubes, 2D focal plane images, and calibrated cutouts) hosted at IRSA.
* **No Alternate Survey Fallback**: The application will **never** silently fall back to or substitute data from other infrared surveys (such as WISE, NEOWISE, 2MASS, or Spitzer). If SPHEREx observations are absent or insufficient for a requested coordinate, the system must explicitly inform the user that no SPHEREx data exists.

---

## 2. Programmatic Archive Access & Discovery

* **Standards-Based Discovery**: Archive queries must adhere to documented astronomical protocols, primarily the International Virtual Observatory Alliance (IVOA) **Simple Image Access version 2 (SIAv2)** interface and **Table Access Protocol (TAP)** supported by IRSA.
* **Dynamic Capability Discovery**: The client must dynamically query and discover supported collections, table schemas, and available spectral bands rather than hard-coding speculative table names or assuming that every observation contains all 102 SPHEREx spectral channels.
* **Rate Limits & Fair Use**: Respect IRSA connection limits, concurrent request limits, and user-agent conventions.

---

## 3. Metadata & Product Compatibility Validation

Before any two observations can be paired for comparative analysis, the backend must execute an explicit scientific validation check:

1. **Spatial Overlap**: The target sky coordinates must be encompassed by the physical footprints of both observations.
2. **Observation Timestamp Difference**: The observations must possess distinct exposure times (`DATE-OBS` / `MJD`). Observations differing only by pipeline processing runs (e.g., `QR2` vs `QR3` of the same exposure) cannot be treated as temporal comparison pairs.
3. **Spectral Band Compatibility**: Only observations taken within the same spectral band or matching wavelength windows may be subtracted directly. Cross-band differencing is scientifically invalid for transient detection.
4. **Coordinate Frame & WCS**: Astrometric solutions must be present and parseable into standard FITS WCS representations.
5. **Photometric Calibration**: Flux units (e.g., $MJy/sr$ or microJanskys) must be verified and normalized.

---

## 4. FITS Handling & Targeted Cutouts

* **Laptop-Friendly Footprint**: To maintain responsive performance on standard development environments (e.g., 16 GB RAM laptop with CPU execution), the system must avoid downloading entire full-sky mosaic tiles or multi-gigabyte monolithic cubes.
* **Small, Targeted Cutouts**: Archive requests must specify small, purpose-specific spatial bounding boxes (cutouts) centered on the requested target coordinates.
* **Memory-Mapped I/O**: FITS file inspection and slice extraction must leverage Astropy's memory-mapping capabilities (`memmap=True`) to minimize resident memory allocation.

---

## 5. Local Cache Policy

To prevent redundant downloads and reduce load on IRSA servers, SPHEREx TimeLens maintains a local cache:

* **Non-Authoritative Role**: The local cache is strictly a local performance optimization. It is never an authoritative mirror of IRSA.
* **Proposed Application Defaults**:
  * **Maximum Cache Size**: Configurable default of **5 GiB** (`CACHE_MAX_BYTES=5368709120`).
  * **Retention Period**: Configurable default of **7 days** (`CACHE_RETENTION_DAYS=7`).
  * *Note: These are application-level defaults designed for developer and user convenience, not requirements imposed by IRSA.*
* **Eviction Policy**: Least Recently Used (LRU) eviction when total cache size exceeds the threshold or files exceed the retention window.
* **Metadata Registration**: Cached assets are tracked in a local SQLite database (`timelens_metadata.db`). If the database is deleted or corrupted, the cache directory can be safely cleared without loss of authoritative data.

---

## 6. Synthetic Fixtures & Testing Safeguards

* **Purpose of Synthetic Data**: Synthetic FITS files and mock API payloads are created exclusively for unit tests, continuous integration, and frontend interface scaffolding.
* **Strict Labeling**: Synthetic data fixtures must be stored under `tests/fixtures/` and must include explicit header metadata marking them as artificial (e.g., `OBSERVER = 'SYNTHETIC_TEST_FIXTURE'`).
* **Zero Presentation to End Users**: Synthetic fixtures must **never** be presented in the user interface as real SPHEREx observations, nor may simulated detections be described as real astrophysical discoveries.

---

## 7. Version Control Exclusions

In accordance with project security and storage guidelines:
* **Excluded from Git**: Downloaded raw FITS files, processed cutouts, local SQLite databases, cache directories, model weights, and local environment files (`.env`) are strictly excluded from version control via `.gitignore`.
* **Tracked in Git**: Only code, documentation, configuration templates (`.env.example`), test suites, and small test fixture definitions are tracked in version control.
