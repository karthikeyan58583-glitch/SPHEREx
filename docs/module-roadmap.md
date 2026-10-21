# SPHEREx TimeLens — Module Roadmap & Development Phases

This document details the seven development phases of **SPHEREx TimeLens**, assigned team member roles, critical integration gates, and parallelization workflows.

---

## 1. The Seven Development Phases

```mermaid
flowchart TD
    Phase1[Phase 1: Project Setup & Architecture<br/><b>Member 1</b>]
    Phase2[Phase 2: Backend Skeleton & API Foundation<br/><b>Member 6</b>]
    Gate1{Integration Gate 1:<br/>API Contract & Schema Freeze}

    Phase3[Phase 3: Archive Access & IRSA Client<br/><b>Member 2</b>]
    Phase4[Phase 4: Scientific Image Processing<br/><b>Member 3</b>]
    Phase5[Phase 5: ML Baseline & Candidate Ranking<br/><b>Member 4</b>]
    Phase6[Phase 6: Frontend & Visualization<br/><b>Member 5</b>]

    Gate2{Integration Gate 2:<br/>Archive Feasibility & Real-Data Validation}
    Gate3{Integration Gate 3:<br/>End-to-End Pipeline & Evaluation}

    Phase7[Phase 7: Full Integration, Testing & Deployment<br/><b>Members 1–6 (Coord: Member 6)</b>]

    Phase1 --> Phase2
    Phase2 --> Gate1
    Gate1 --> Phase3
    Gate1 --> Phase4
    Gate1 -. Parallel Dev with Fixtures .-> Phase5
    Gate1 -. Parallel Dev with Mock API .-> Phase6
    Phase3 --> Gate2
    Phase4 --> Gate2
    Gate2 --> Gate3
    Phase5 --> Gate3
    Phase6 --> Gate3
    Gate3 --> Phase7
```

### Phase 1: Project Setup and Architecture (Member 1)
* **Goal**: Establish repository scaffold, coding standards, environment configurations, and documentation foundation.
* **Key Deliverables**: Root configuration files (`.gitignore`, `.editorconfig`, `.env.example`), monorepo structure, and core architectural documentation.
* **Status**: **In Progress (Scaffold Phase)**.

### Phase 2: Backend Skeleton and API Foundation (Member 6)
* **Goal**: Build the FastAPI application structure, configure CORS, establish Pydantic request/response validation models, and initialize local SQLite metadata storage.
* **Key Deliverables**: Functional FastAPI app with `/api/v1/health`, OpenAPI specification export, and typed Pydantic models matching [docs/api-contract.md](file:///d:/SphereX/docs/api-contract.md).

### Phase 3: Astronomy Data and Archive Access (Member 2)
* **Goal**: Connect to NASA/IPAC IRSA using IVOA SIAv2/TAP protocols, query public SPHEREx data collections, and implement safe FITS cutout downloading and caching.
* **Key Deliverables**: Verified IRSA client, observation query service, FITS header validation, and local cache eviction engine (5 GiB limit, 7-day retention).

### Phase 4: Scientific Image Processing and Computer Vision (Member 3)
* **Goal**: Build the scientific image processing engine using Astropy, NumPy, and SciPy to align multi-epoch observations and compute difference images.
* **Key Deliverables**: WCS reprojection module, bad-pixel/cosmic-ray masking, background estimation and normalization, error propagation maps, and display preview generator (asinh/zscale stretched PNG/WebP).

### Phase 5: ML Dataset Preparation, Training, and Evaluation (Member 4)
* **Goal**: Build a reproducible candidate extraction and ranking pipeline, starting with transparent scientific metrics and a classical scikit-learn classifier.
* **Key Deliverables**: Feature extraction pipeline (flux ratio, significance, elongation, PSF residual), baseline classifier, evaluation metrics (precision, recall, false-positive rate), and reproducible training scripts.

### Phase 6: Frontend and Visualization (Member 5)
* **Goal**: Develop the interactive user interface in React, TypeScript, and Vite.
* **Key Deliverables**: Celestial coordinate search, observation timeline and compatibility viewer, synchronized side-by-side epoch comparison viewer, difference image inspector, and candidate tables.

### Phase 7: Full Integration, Validation, Testing, and Deployment (Members 1–6, Coordinated by Member 6)
* **Goal**: Assemble all components into an end-to-end verified application, execute automated test suites, and package for local deployment.
* **Key Deliverables**: End-to-end integration tests, complete user documentation, performance benchmarks on 16 GB RAM laptops, and release tags.

---

## 2. Integration Gates & Early Dependencies

To prevent downstream architectural rework, three formal integration gates must be cleared:

### Integration Gate 1: API Contract & Schema Freeze (Post-Phase 2)
* **Prerequisite for Parallel Development**: Phase 2 must produce validated Pydantic schemas and exported OpenAPI specs for `/api/v1/observations`, `/api/v1/comparisons`, and `/api/v1/candidates`.
* **Parallel Enablement**: Once Gate 1 is locked, **Member 5 (Frontend)** can develop against mock API responses, and **Member 4 (ML)** can develop feature schemas independently of backend archive development.

### Integration Gate 2: Archive Feasibility & Real-Data Validation (Post-Phase 3)
* **Early Scientific Validation Task**: Member 2 must execute an explicit validation task querying official IRSA endpoints to confirm:
  1. The exact collection names, table identifiers, and SIAv2 query parameters for public SPHEREx data.
  2. The availability of multiple observation epochs for given celestial footprints.
  3. The actual spectral bands and cutout delivery formats provided by IRSA.
* **Requirement**: The pipeline cannot claim operational status until tested against real archive records or verified archive structures.

### Integration Gate 3: End-to-End Pipeline & Evaluation (Post-Phase 5 & 6)
* **Verification**: Connect frontend, FastAPI backend, real archive data, image differencing, and ML candidate ranking into a unified workflow.
* **Validation**: Run end-to-end regression tests verifying that synthetic test fixtures behave predictably and that real queries preserve end-to-end provenance without fabricating detections.
