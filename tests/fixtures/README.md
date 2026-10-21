# Test Fixtures (`tests/fixtures/`)

This directory houses synthetic test fixtures, mock API payloads, and simulated miniature FITS cutouts for offline automated testing.

---

## 1. Strict Scientific Safeguards for Synthetic Data

1. **Explicit Identification**: Every synthetic FITS file stored in this directory must contain clear FITS header metadata explicitly identifying it as artificial:
   ```
   ORIGIN  = 'SPHEREx-TimeLens-Synthetic-Fixture'
   OBJECT  = 'SYNTHETIC_TEST_TARGET'
   COMMENT = 'This file is a simulated test fixture and does NOT represent real SPHEREx data.'
   ```
2. **Never Presented as Real Data**: Synthetic test fixtures are strictly prohibited from being displayed in the application UI as real astronomical observations.
3. **No Fabricated Discoveries**: Simulated transients or variability injected into synthetic fixtures are solely for testing image differencing algorithms and ML scoring logic.
4. **Minimal File Sizes**: Fixture images must be small cutouts (e.g., $64 \times 64$ or $128 \times 128$ pixels) to avoid bloating git repository history.
