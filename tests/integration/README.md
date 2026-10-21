# Integration Tests (`tests/integration/`)

This directory will contain integration tests covering multi-module workflows:
- End-to-end comparison flow: archive retrieval -> FITS reprojection -> difference imaging -> candidate ranking -> JSON response.
- Cache lifecycle tests: verifying that subsequent comparison requests utilize cached cutouts without redundant network requests.
- Error handling under archive simulation failures (e.g., simulating HTTP 502/504 responses from IRSA).
