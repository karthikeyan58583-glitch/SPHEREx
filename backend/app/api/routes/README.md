# Backend API Routes

This directory contains FastAPI endpoint controllers:
- `health.py`: Service health and storage metrics (`GET /api/v1/health`).
- `observations.py`: Sky search and observation discovery (`GET /api/v1/observations`).
- `comparisons.py`: Multi-epoch differencing requests (`POST /api/v1/comparisons`).
- `candidates.py`: Candidate temporal change inspection (`GET /api/v1/candidates`).
