# SPHEREx TimeLens — Project Rules

## 1. Project Mission

SPHEREx TimeLens is a web application for exploring public SPHEREx
astronomical observations and comparing compatible observations of
the same sky region across time.

The application should help users identify candidate changes in
astronomical imagery while distinguishing genuine possible changes
from noise, artifacts, processing differences, and uncertainty.

Do not claim an astronomical discovery merely because an image
difference or machine-learning model produces a candidate.

## 2. Agreed Technology Stack

Frontend:
- React
- TypeScript
- Vite

Backend:
- Python
- FastAPI
- Pydantic for request and response validation

Scientific processing:
- Astropy for FITS files and astronomical coordinate systems
- NumPy and SciPy for numerical processing
- WCS-aware alignment and reprojection tools where appropriate

Machine learning:
- Start with a reproducible scikit-learn baseline.
- Evaluate dataset quality, false positives, precision, and recall.
- Consider PyTorch only when the dataset and evaluation justify it.
- Do not introduce a complex model merely for demonstration.

Metadata and caching:
- SQLite for an initial local metadata database or cache where appropriate.
- Never treat cached data as the authoritative source of archive observations.

Data source:
- Use the official NASA/IPAC Infrared Science Archive (IRSA) and
  verified SPHEREx data products and documented access interfaces.
- Verify actual archive capabilities, product availability, and
  access requirements before implementing assumptions.

Development:
- Git and GitHub for version control and collaboration.
- Antigravity for AI-assisted development.
- Use project-specific dependency environments and lock files
  where supported; do not install application dependencies globally.

## 3. Architectural Boundaries

The intended flow is:

Frontend
  -> FastAPI backend
  -> Archive discovery and data retrieval
  -> FITS validation and scientific preprocessing
  -> Compatible-observation comparison
  -> Candidate feature extraction and optional ML ranking
  -> API response with results, provenance, and limitations
  -> Frontend visualization

Rules:
- The frontend must not duplicate scientific processing implemented
  by the backend.
- Keep archive access, processing, ML, API schemas, and UI components
  modular and testable.
- Keep API request and response schemas explicit and documented.
- Validate external archive responses and user input.
- Do not hard-code unverified archive URLs, response fields, or
  product availability.
- Prefer small, testable modules over giant files.

## 4. Scientific Correctness and Data Provenance

Before comparing observations, verify that they are scientifically
compatible. Relevant checks may include:

- Same or sufficiently overlapping sky region.
- Observation timestamps and product provenance.
- Compatible spectral bands or wavelengths.
- Coordinate systems, WCS information, image dimensions, and
  spatial resolution.
- Units, calibration, quality flags, masks, and background differences.

Use appropriate alignment, normalization, invalid-pixel masking,
and uncertainty handling before producing difference images.

Do not assume every observation contains all 102 SPHEREx bands.
Discover and report the actual bands, products, and observation
coverage available for each queried region.

Preserve useful provenance such as archive identifiers, observation
times, sky coordinates, product identifiers, band information, units,
processing parameters, and source references.

An apparent difference may arise from noise, artifacts, image
registration, background variation, or data quality. Where evidence
is insufficient, report uncertainty rather than inventing an answer.

Machine-learning outputs must be presented as ranked candidate
changes, not verified astronomical discoveries.

Use clearly labelled synthetic fixtures for development and tests.
Never present synthetic data as real SPHEREx observations.

## 5. Data and Security Rules

- Never commit API keys, credentials, access tokens, or personal
  secrets.
- Keep local secrets in an ignored environment file.
- Keep .env.example limited to placeholders and documented settings.
- Do not commit large downloaded FITS files, raw archive collections,
  generated datasets, or model weights unless explicitly justified.
- Store cache and generated output separately from source code.
- Respect source licensing, access rules, and data-use requirements.
- Do not delete or overwrite user data without explicit authorization.

## 6. Frontend Requirements

The UI should make it possible to:
- Search for or select a sky region.
- Explore compatible available observations and timestamps.
- Compare observation images and, where appropriate, difference images.
- Inspect candidate changes and their scores or supporting evidence.
- Understand observation provenance, limitations, and uncertainty.

Provide useful loading, empty-result, error, and invalid-query states.
Do not invent observation dates, sky coordinates, scientific values,
or candidate detections to make the UI appear complete.

Keep components accessible, reusable, and reasonably small.
Use typed API responses rather than scattered untyped data.

## 7. Backend and Processing Requirements

- Use clear API schemas and meaningful HTTP error responses.
- Separate route handlers from archive clients and processing logic.
- Validate FITS headers, image dimensions, WCS, units, and data quality.
- Handle missing metadata and unavailable observations explicitly.
- Avoid silently comparing incompatible products.
- Record processing parameters needed to reproduce a result.
- Add unit tests for numerical processing and API behavior.
- Never report a pipeline as working unless it has been tested with
  suitable input data.

## 8. Machine-Learning Requirements

- Establish a simple baseline before adding neural networks.
- Document the source and quality of every labelled dataset.
- Prevent data leakage between training, validation, and test sets.
- Measure false positives as well as successful detections.
- Select thresholds using validation data and document their rationale.
- Compare model output with non-ML scientific quality checks.
- Save model configuration and evaluation results reproducibly.
- Explain that predictions are candidates requiring validation.

## 9. Git and Team Collaboration

- Work only within the assigned repository and task scope.
- Use focused changes and descriptive commit messages.
- Do not commit directly to main for ordinary feature development.
- Use a separate branch and pull request for substantial team work.
- Do not force-push, rewrite shared history, or discard another
  contributor's changes.
- Coordinate shared API schemas and data contracts with the team.
- Run relevant tests before proposing a change as complete.
- Update documentation when architecture or interfaces change.
- Do not commit or push until the user explicitly requests it.

## 10. Team Phase Plan

Phase 1: Project setup and architecture — Member 1.
Phase 2: Backend skeleton and API foundation — Member 6.
Phase 3: Astronomy data and archive access — Member 2.
Phase 4: Scientific image processing and computer vision — Member 3.
Phase 5: ML dataset preparation, training, and evaluation — Member 4.
Phase 6: Frontend and visualization — Member 5.
Phase 7: Full integration, validation, testing, and deployment —
Members 1 through 6, coordinated by Member 6.

Integrate outputs between phases. Do not assume that individual
modules work together until an end-to-end test confirms it.

## 11. Instructions for the AI Coding Agent

Before making changes:
1. Inspect the relevant existing files and project structure.
2. Check this file and any more-specific directory instructions.
3. Explain the intended changes and affected interfaces.
4. Identify dependencies and integration risks.

While implementing:
- Prefer minimal, coherent changes.
- Follow established conventions and keep dependencies justified.
- Avoid unrelated refactors.
- Do not fabricate test results, source data, endpoints, or facts.
- Do not overwrite existing work simply to match a preferred design.

After implementing:
1. Summarize changed files and architectural decisions.
2. Run relevant available checks and report their actual results.
3. Identify incomplete work, assumptions, and known limitations.
4. Provide reproducible commands for manual verification.

If a scientific or architectural requirement is unclear, make
well-supported progress where possible and explicitly document
any unresolved assumption.
