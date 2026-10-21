# Storage & Metadata Cache Service

This service manages local caching and metadata persistence:
- `database.py`: SQLite connection engine and session factory.
- `models.py`: Database table definitions for cached observations, comparisons, and candidates.
- `cache_manager.py`: Filesystem cache management enforcing size limits (default 5 GiB) and LRU eviction (default 7 days).
