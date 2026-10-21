# SPHEREx TimeLens

> Exploring multi-epoch astronomical observations from the NASA SPHEREx mission to detect and inspect candidate temporal changes with rigorous scientific safeguards.

---

## 1. Project Mission

**SPHEREx TimeLens** is an open-source astronomical web application designed to explore public observations from the NASA SPHEREx (Spectro-Photometer for the History of the Universe, Epoch of Reionization and Ices Explorer) mission. Its primary goal is to allow astronomers, researchers, and students to discover compatible observations of the same sky footprint across distinct exposure epochs, perform rigorous WCS-aware image alignment and differencing, and evaluate candidate transient or variable phenomena.

### The Core Scientific Principle
**An apparent difference or high machine-learning score is a candidate change, not a discovery.**<br />
SPHEREx TimeLens enforces explicit checks to distinguish genuine astronomical variability from instrumental artifacts, point-spread-function (PSF) mismatches, coordinate registration offsets, cosmic rays, and background variation. Every candidate is delivered with quantified uncertainty and end-to-end data provenance.

---

## 2. Project Status

> **CURRENT STATUS: INITIAL MONOREPO SCAFFOLD (v0.1.0-alpha)**<br />
> This repository is currently an architectural scaffold establishing directory structure, design documents, data policies, API contracts, and engineering boundaries. **Functional archive retrieval, scientific image processing, machine learning ranking, and frontend visualization have not yet been implemented.**

Implementation proceeds strictly module-by-module in accordance with the seven-phase roadmap documented in [docs/module-roadmap.md](file:///d:/SphereX/docs/module-roadmap.md).

---

## 3. High-Level Architecture & Workflow

The system enforces a unidirectional pipeline with strict boundary separation:

```
[ Frontend: React / TS / Vite ]
           │  (REST API calls: sky search, comparison requests)
           ▼
[ Backend: FastAPI / Pydantic ]
           │  (Programmatic query via IVOA SIAv2 / TAP)
           ▼
[ Archive: NASA/IPAC IRSA ]
           │  (Public SPHEREx FITS cutouts & metadata)
           ▼
[ Scientific Preprocessing: Astropy / WCS / SciPy ]
           │  (Alignment, reprojection, bad-pixel masking, normalization)
           ▼
[ Observation Differencing & Error Propagation ]
           │  (Difference array, signal-to-noise map, uncertainty map)
           ▼
[ Candidate Extraction & ML Ranking: scikit-learn ]
           │  (Features, rule-based screening, candidate scores)
           ▼
[ Typed API Response: Pydantic Schemas ]
           │  (Pre-rendered display previews, candidates, provenance)
           ▼
[ Frontend Visualization & Inspection ]
```

* **Frontend Responsibility**: Exploration, coordinate search, epoch filtering, side-by-side inspection, and candidate browsing. **The frontend does not perform astronomical calculations or parse raw FITS files.**
* **Backend Responsibility**: Coordinate transformations, archive communication, FITS header validation, WCS reprojection, differencing, preview generation, and ML candidate evaluation.

For full architectural details, see [docs/architecture.md](file:///d:/SphereX/docs/architecture.md).

---

## 4. Planned Capabilities

1. **Target & Footprint Exploration**: Cone-search and celestial coordinate lookup (Equatorial J2000 and Galactic frames).
2. **Archive Discovery**: Querying verified public SPHEREx data products through official NASA/IPAC IRSA interfaces.
3. **Compatibility Validation**: Verifying spatial overlap, observation timestamp distinction, compatible spectral bands, and matching calibration units before allowing comparison.
4. **WCS-Aware Differencing**: Astropy- and SciPy-powered reprojection, background leveling, and invalid-pixel masking.
5. **Candidate Ranking**: Transparent scientific baseline metrics followed by classical machine-learning candidate ranking (scikit-learn).
6. **Provenance & Inspection**: Complete audit trails capturing archive identifiers, processing versions, timestamps, and known limitations.

---

## 5. Technology Stack

| Layer | Component | Technologies |
| :--- | :--- | :--- |
| **Frontend** | User Interface | React 18+, TypeScript, Vite, npm |
| **Backend** | REST API & Service | Python 3.10+, FastAPI, Pydantic v2 |
| **Astro Processing** | Scientific Engine | Astropy (FITS & WCS), NumPy, SciPy, Reproject |
| **Machine Learning** | Candidate Ranking | scikit-learn (baseline); PyTorch only if justified |
| **Metadata & Cache** | Local Fast Storage | SQLite (metadata cache), local filesystem cache |
| **Primary Archive** | Data Source | NASA/IPAC Infrared Science Archive (IRSA) |
| **Environment** | Dependency Isolation | Python virtual environment with lockfiles, npm |

---

## 6. Scientific Limitations & Rules

1. **Observation Time vs. Product Release**: Release labels such as `QR2` or `QR3` represent data processing iterations from the project pipeline, **never** distinct observation epochs. Observation timestamps (`DATE-OBS` / `MJD`) are strictly used for temporal ordering.
2. **Authoritative Archive**: NASA/IPAC IRSA is the authoritative source. SQLite is strictly a local convenience cache and never the authoritative record of archive data.
3. **No Silent Fallback**: If SPHEREx observations are not available for a given sky region, the system explicitly reports unavailable data. It will never silently substitute data from other surveys (such as WISE or NEOWISE).
4. **Synthetic Data Policy**: Synthetic fixtures are used strictly for automated tests and interface validation. They must be clearly labeled and never displayed as genuine astronomical observations.
5. **No Speculative Discovery**: Candidate detections are ranked anomalies requiring independent astrophysical follow-up and verification.

For the comprehensive data governance policy, see [docs/data-policy.md](file:///d:/SphereX/docs/data-policy.md).

---

## 7. Prerequisites & Development Setup

### System Prerequisites
- **Operating System**: Windows (CPU-first, 16 GB RAM laptop compatible), Linux, or macOS.
- **Node.js**: v18.0.0 or higher (LTS recommended) and `npm`.
- **Python**: v3.10 or higher.
- **Git**: v2.30 or higher.

### Quick Start (Scaffold Inspection)
```bash
# Clone the repository
git clone https://github.com/karthikeyan58583-glitch/SPHEREx.git
cd SPHEREx

# Copy the configuration template
copy .env.example .env
```

For complete instructions on virtual environments, toolchains, testing, and collaboration guidelines, see [docs/development-setup.md](file:///d:/SphereX/docs/development-setup.md).

---

## 8. Project Documentation Index

- [Architecture & Data Flow](file:///d:/SphereX/docs/architecture.md) — Detailed system design and boundaries.
- [Data Policy & Scientific Standards](file:///d:/SphereX/docs/data-policy.md) — Archive authority, caching, and synthetic data rules.
- [Architectural Decision Records](file:///d:/SphereX/docs/decisions.md) — Rationale for technical decisions and baseline assumptions.
- [Module Roadmap & Phases](file:///d:/SphereX/docs/module-roadmap.md) — Seven-phase execution plan and integration gates.
- [API Contract (Proposed)](file:///d:/SphereX/docs/api-contract.md) — Draft schemas for REST endpoints.
- [Development Setup Guide](file:///d:/SphereX/docs/development-setup.md) — Local development workflow and guidelines.
- [Project Rules (AGENTS.md)](file:///d:/SphereX/AGENTS.md) — Core behavioral guidelines and team rules.
