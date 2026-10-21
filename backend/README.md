# SPHEREx TimeLens — Backend Service

> FastAPI REST API service for NASA/IPAC IRSA archive discovery, astronomical FITS validation, WCS-aware image differencing, and candidate transient ranking.

---

## 1. Responsibilities & Boundaries

The backend service acts as the scientific and computational core of SPHEREx TimeLens. Its primary duties include:
- Exposing versioned REST endpoints (`/api/v1/`) validated via Pydantic v2 schemas.
- Discovering public SPHEREx observations via IVOA SIAv2/TAP interfaces at NASA/IPAC IRSA.
- Validating astronomical FITS headers, coordinate systems (WCS), exposure epochs, and photometric units.
- Executing WCS reprojection, background leveling, bad-pixel masking, and error-propagated image subtraction.
- Generating pre-stretched image previews (WebP/PNG) for fast browser visualization.
- Extracting candidate temporal features and evaluating them using scientific criteria and a baseline scikit-learn classifier.
- Maintaining a local SQLite cache for metadata and cutouts with a 5 GiB limit and 7-day retention period.

### Architectural Rules
- Route handlers (`app/api/routes/`) must strictly handle HTTP request validation and response delegation. They must never contain low-level FITS file handling or numerical array math.
- Scientific processing (`app/services/processing/`) must remain modular and independently testable without launching the FastAPI server.
- The backend must never treat the local SQLite cache as the authoritative ground truth for archive data.

---

## 2. Current Status

> **STATUS: SCAFFOLD ONLY**<br />
> No application code, endpoint handlers, or dependencies have been installed yet. Backend implementation begins in **Phase 2** (Member 6), establishing the FastAPI skeleton, Pydantic schemas, and local storage layer.

---

## 3. Directory Layout

* `app/api/routes/`: FastAPI API route controllers (`health.py`, `observations.py`, `comparisons.py`, `candidates.py`).
* `app/core/`: Application settings, environment configuration (`pydantic-settings`), logging, and error handlers.
* `app/schemas/`: Strongly-typed Pydantic request and response data models.
* `app/services/archive/`: IRSA client, SIAv2 query builders, and safe FITS cutout retrieval.
* `app/services/processing/`: Astropy WCS alignment, reprojection, differencing, and preview generation.
* `app/services/candidates/`: Candidate feature extraction, artifact screening, and scikit-learn scoring.
* `app/storage/`: SQLite database setup, cache index tracking, and LRU eviction logic.
* `tests/`: Pytest unit and functional tests for backend components.

---

## 4. Intended Technology Stack
- **Framework**: FastAPI, Uvicorn
- **Validation**: Pydantic v2
- **Astronomical Processing**: Astropy, NumPy, SciPy, Reproject
- **Machine Learning**: scikit-learn
- **Database / Cache**: SQLite, SQLAlchemy / aiosqlite
