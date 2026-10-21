# Backend Unit & Integration Tests

This directory will contain the Pytest suite for backend components:
- `test_routes/`: API endpoint status codes and schema validation tests.
- `test_archive/`: Mocked IRSA client response and timeout handling tests.
- `test_processing/`: Reprojection, differencing, and preview generation tests.
- `test_storage/`: SQLite session lifecycle and cache eviction limit tests.
