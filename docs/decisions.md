# SPHEREx TimeLens — Architectural Decision Records (ADRs)

This document formalizes the v0.1 architectural defaults, engineering decisions, and baseline assumptions established for the initial repository scaffold of **SPHEREx TimeLens**.

---

## 1. Architectural Decisions (v0.1 Defaults)

### ADR-001: Monorepo Layout
* **Decision**: Organize the project as a single monorepo containing `frontend/`, `backend/`, `data/`, `ml/`, `scripts/`, `tests/`, and `docs/`.
* **Rationale**: Eliminates multi-repository synchronization overhead for a small, focused team. Ensures API contracts, backend endpoints, and frontend components can be co-versioned, tested, and reviewed atomically.

### ADR-002: IRSA-First Data Access with No Silent Fallback
* **Decision**: Query the NASA/IPAC Infrared Science Archive (IRSA) exclusively via documented IVOA SIAv2/TAP interfaces. Never implement silent fallback to alternate surveys (such as WISE or NEOWISE) when SPHEREx data is absent.
* **Rationale**: Silent substitution destroys scientific integrity. The application is explicitly built to explore SPHEREx observations; if none exist for a given sky region, communicating that absence honestly is required by scientific research standards.

### ADR-003: Versioned REST API with Explicit Pydantic Contracts
* **Decision**: Expose backend functionality under `/api/v1/` using FastAPI, with request and response payloads strictly defined and validated via Pydantic v2 schemas.
* **Rationale**: Decouples client and server development. Provides clear data contracts, automatic OpenAPI/Swagger documentation, and compile-time/runtime type validation. Prevents undocumented fields or loose JSON dictionaries from propagating between layers.

### ADR-004: Backend-Generated Display Previews
* **Decision**: The backend will process raw astronomical FITS arrays and generate normalized, pre-stretched image previews (WebP/PNG) along with optional 1D/2D numerical summaries for frontend display.
* **Rationale**: Raw FITS arrays often require non-linear stretches (e.g., asinh, zscale), bad-pixel masking, and WCS transformations that are computationally expensive and error-prone in the browser. Generating display-ready assets on the backend keeps the frontend fast, lightweight, and hardware-agnostic (runnable on laptops with integrated GPUs).

### ADR-005: React Comparison Viewer with Side-by-Side Comparison
* **Decision**: Implement the user comparison interface starting with a synchronized side-by-side epoch viewer before adding complex alpha-blending or blink-comparator overlays.
* **Rationale**: A side-by-side viewer allows immediate visual inspection of both physical epochs simultaneously, making differences in background, PSF shape, and pixel artifacts intuitively apparent without confusing optical flicker.

### ADR-006: Rule-Based Scientific Checks Prior to Supervised ML
* **Decision**: Enforce deterministic, rule-based scientific quality checks (coordinate matching, exposure difference, band matching, signal-to-noise thresholds, PSF shape metrics) before passing candidates to any machine-learning model.
* **Rationale**: Machine-learning models are susceptible to false positives when confronted with out-of-distribution artifacts (e.g., diffraction spikes, satellite streaks, saturated columns). Filtering known non-astrophysical artifacts with deterministic heuristics ensures that ML ranking focuses exclusively on plausible candidates.

### ADR-007: Cache and Data Storage Boundaries
* **Decision**: Local storage is partitioned into `data/raw/` (temporary downloads), `data/processed/` (reprojected arrays and difference maps), and `data/cache/` (metadata SQLite database and LRU cache). All directories are excluded from Git with configurable size limits (5 GiB default) and retention periods (7 days default).
* **Rationale**: Prevents accidental commits of multi-gigabyte astronomical files while providing fast local iteration for developers. SQLite maintains index references without being treated as the ground truth.

### ADR-008: Dependency Management Strategy
* **Decision**: Use `npm` for frontend dependency management and standard Python virtual environments (`venv`) with locked requirements for the backend and ML subsystems.
* **Rationale**: Maximizes compatibility across Windows, Linux, and macOS without requiring proprietary package managers. Lockfiles ensure deterministic builds across team members.

---

## 2. Scaffold Decisions vs. Assumptions Requiring Early Validation

| Category | Item | Status | Validation Plan |
| :--- | :--- | :--- | :--- |
| **Decision** | Monorepo layout & directory structure | **Adopted** | Scaffold created; ready for Phase 2 implementation. |
| **Decision** | FastAPI + React/Vite stack | **Adopted** | Standardized across team phases. |
| **Decision** | Exclude FITS/data from Git | **Adopted** | Configured in `.gitignore`. |
| **Assumption** | IRSA SPHEREx SIAv2 endpoint schema | **Pending Validation** | Must verify actual public SPHEREx table names and SIAv2 query parameters at IRSA during Phase 3. |
| **Assumption** | Temporal coverage of public SPHEREx data | **Pending Validation** | SPHEREx is a survey mission. We must validate whether public data releases already contain multi-epoch coverage of identical footprints, or if early validation requires targeted calibration fields. |
| **Assumption** | Optimal display stretch algorithm | **Pending Validation** | Test whether `asinh` or `zscale` provides superior contrast across SPHEREx infrared bands during Phase 4. |
| **Assumption** | ML Baseline Feature Set | **Pending Validation** | Extract candidate features on synthetic and real difference images during Phase 5 to determine precision/recall trade-offs. |
