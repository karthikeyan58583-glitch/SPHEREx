# SPHEREx TimeLens — Test Suite

This directory contains repository-wide automated test suites, integration tests, and shared test fixtures.

---

## 1. Directory Structure

* `integration/`: Multi-service integration tests validating interactions between the FastAPI backend, local cache, archive client, and image processing pipeline.
* `fixtures/`: Explicitly labeled synthetic test data, mock FITS cutouts, and mock IRSA response files.

---

## 2. Testing Guidelines

* **Fast Execution**: Unit tests and lightweight integration tests should execute rapidly on standard developer hardware.
* **Hermetic & Offline by Default**: Automated CI test runs must not require active Internet connectivity to IRSA; mock clients and synthetic fixtures must be used for routine testing.
* **End-to-End Live Tests**: Live archive connectivity tests must be isolated into dedicated test suites marked with `@pytest.mark.live_archive`.
