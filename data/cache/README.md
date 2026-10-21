# Local Metadata & File Cache (`data/cache/`)

This directory houses the local SQLite database (`timelens_metadata.db`) and short-term cached FITS cutouts managed by the storage service.

### Policy & Configuration
* **Configurable Defaults**:
  * **Size Limit**: 5 GiB (`CACHE_MAX_BYTES=5368709120`).
  * **Retention Period**: 7 days (`CACHE_RETENTION_DAYS=7`).
* **LRU Eviction**: Unreferenced cutouts exceeding the retention period or size threshold are automatically purged.
* **Git Exclusion**: All databases (`*.sqlite`, `*.db`) and cached files are excluded from Git via `.gitignore`.
* **Disposability**: Deleting this directory will not destroy any user code or permanent science records; it simply resets the local cache.
