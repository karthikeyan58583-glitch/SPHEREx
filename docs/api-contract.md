# SPHEREx TimeLens — API Contract Specification

> **STATUS: PROPOSED — NOT IMPLEMENTED**<br />
> This document specifies the proposed v1 REST API schemas and interaction patterns for **SPHEREx TimeLens**. These endpoints and data structures are architectural proposals for Phase 2 and are not yet implemented in code. Archive-specific fields remain provisional until actual NASA/IPAC IRSA responses are inspected in Phase 3.

---

## 1. General Principles & Versioning

* **Base URL**: `/api/v1`
* **Protocol**: HTTP/1.1 or HTTP/2 over HTTPS (HTTP allowed for local development).
* **Payload Format**: `application/json` for request and response bodies.
* **Error Handling**: Standard HTTP status codes accompanied by structured RFC 7807 problem details or an application error envelope.

### Error Envelope Schema
```json
{
  "error": {
    "code": "INVALID_COORDINATES",
    "message": "Right Ascension must be between 0 and 360 degrees.",
    "details": [
      {
        "field": "ra",
        "issue": "Value 412.5 exceeds maximum allowable value of 360.0."
      }
    ]
  }
}
```

---

## 2. Endpoint Specifications

### 2.1 System Health
* **Endpoint**: `GET /api/v1/health`
* **Description**: Returns the operational status of the backend service and local storage connectivity.

#### Response (200 OK)
```json
{
  "status": "healthy",
  "version": "0.1.0-alpha",
  "environment": "development",
  "storage": {
    "database": "connected",
    "cache_dir_accessible": true,
    "cache_used_bytes": 104857600,
    "cache_max_bytes": 5368709120
  }
}
```

---

### 2.2 Observation Discovery
* **Endpoint**: `GET /api/v1/observations`
* **Description**: Queries available public SPHEREx observations covering a specified celestial coordinate or sky region.
* **Query Parameters**:
  * `ra` (float, required): Right Ascension in decimal degrees (J2000, $0.0 \le \text{ra} < 360.0$).
  * `dec` (float, required): Declination in decimal degrees (J2000, $-90.0 \le \text{dec} \le +90.0$).
  * `radius_arcmin` (float, optional, default: `5.0`): Search cone radius in arcminutes ($0.1 \le \text{radius} \le 60.0$).
  * `band_id` (integer, optional): SPHEREx spectral band filter (provisional range: 1–6).
  * `min_obs_time` (string, optional): ISO-8601 UTC timestamp filter.
  * `max_obs_time` (string, optional): ISO-8601 UTC timestamp filter.

#### Response (200 OK)
```json
{
  "query": {
    "ra": 180.0,
    "dec": 45.0,
    "radius_arcmin": 5.0,
    "total_found": 2
  },
  "observations": [
    {
      "observation_id": "SPHEREX_OBS_20250412_019283",
      "archive_product_id": "IRSA_SPHEREX_L2_20250412T143201_B1",
      "release_version": "QR2",
      "observation_time_utc": "2025-04-12T14:32:01.201Z",
      "mjd": 60777.6055,
      "exposure_time_seconds": 120.0,
      "spatial_footprint": {
        "center_ra": 180.012,
        "center_dec": 45.008,
        "corner_coords": [
          [179.95, 44.95],
          [180.07, 44.95],
          [180.07, 45.07],
          [179.95, 45.07]
        ]
      },
      "spectral": {
        "band_id": 1,
        "wavelength_min_um": 0.75,
        "wavelength_max_um": 1.12,
        "channels_available": 16
      },
      "calibration": {
        "flux_unit": "MJy/sr",
        "pixel_scale_arcsec": 6.2
      },
      "data_quality": {
        "quality_flag": "GOOD",
        "has_masks": true,
        "usable_pixel_fraction": 0.994
      },
      "provenance": {
        "archive_source": "NASA/IPAC IRSA",
        "retrieved_at": "2026-10-22T00:00:00Z"
      }
    },
    {
      "observation_id": "SPHEREX_OBS_20251015_088192",
      "archive_product_id": "IRSA_SPHEREX_L2_20251015T091522_B1",
      "release_version": "QR3",
      "observation_time_utc": "2025-10-15T09:15:22.842Z",
      "mjd": 60963.3856,
      "exposure_time_seconds": 120.0,
      "spatial_footprint": {
        "center_ra": 180.010,
        "center_dec": 45.011,
        "corner_coords": [
          [179.95, 44.95],
          [180.07, 44.95],
          [180.07, 45.07],
          [179.95, 45.07]
        ]
      },
      "spectral": {
        "band_id": 1,
        "wavelength_min_um": 0.75,
        "wavelength_max_um": 1.12,
        "channels_available": 16
      },
      "calibration": {
        "flux_unit": "MJy/sr",
        "pixel_scale_arcsec": 6.2
      },
      "data_quality": {
        "quality_flag": "GOOD",
        "has_masks": true,
        "usable_pixel_fraction": 0.998
      },
      "provenance": {
        "archive_source": "NASA/IPAC IRSA",
        "retrieved_at": "2026-10-22T00:00:00Z"
      }
    }
  ]
}
```

---

### 2.3 Observation Comparison
* **Endpoint**: `POST /api/v1/comparisons`
* **Description**: Requests scientific alignment, reprojection, and image differencing between two compatible observations.
* **Request Body**:
```json
{
  "reference_observation_id": "SPHEREX_OBS_20250412_019283",
  "target_observation_id": "SPHEREX_OBS_20251015_088192",
  "region_cutout": {
    "center_ra": 180.0,
    "center_dec": 45.0,
    "width_arcmin": 4.0,
    "height_arcmin": 4.0
  },
  "differencing_parameters": {
    "reprojection_method": "exact",
    "background_subtraction": "median_filter",
    "mask_bad_pixels": true,
    "stretch_type": "asinh"
  }
}
```

#### Response (201 Created)
```json
{
  "comparison_id": "CMP_20261022_918237",
  "created_at": "2026-10-22T00:30:00Z",
  "compatibility_status": {
    "is_compatible": true,
    "spatial_overlap_percentage": 98.4,
    "epoch_difference_days": 185.78,
    "band_match": true,
    "validation_notes": "Observations possess identical spectral band (Band 1) and valid WCS solutions."
  },
  "scientific_validation": {
    "status": "VALIDATED",
    "reference_bg_subtracted": true,
    "target_bg_subtracted": true,
    "noise_sigma": 0.042
  },
  "previews": {
    "reference_preview_url": "/api/v1/previews/CMP_20261022_918237/reference.webp",
    "target_preview_url": "/api/v1/previews/CMP_20261022_918237/target.webp",
    "difference_preview_url": "/api/v1/previews/CMP_20261022_918237/difference.webp",
    "display_metadata": {
      "stretch": "asinh",
      "vmin": -0.15,
      "vmax": 1.25,
      "pixel_dimensions": [256, 256]
    }
  },
  "candidate_summary": {
    "total_candidates_detected": 1,
    "candidates_endpoint": "/api/v1/candidates?comparison_id=CMP_20261022_918237"
  },
  "provenance": {
    "pipeline_version": "0.1.0-alpha",
    "differencing_algorithm": "wcs_reproject_direct_subtraction",
    "executed_at": "2026-10-22T00:30:05Z"
  }
}
```

---

### 2.4 Candidate Inspection & Scoring
* **Endpoint**: `GET /api/v1/candidates`
* **Description**: Returns extracted candidate temporal changes detected within a comparison session, scored by the scientific baseline and ML model.
* **Query Parameters**:
  * `comparison_id` (string, required): Comparison identifier.
  * `min_score` (float, optional, default: `0.0`): Minimum ML ranking score ($0.0 \le \text{score} \le 1.0$).
  * `quality_flag` (string, optional): Filter by artifact screening status (e.g., `CLEAN`, `SUSPECT_ARTIFACT`).

#### Response (200 OK)
```json
{
  "comparison_id": "CMP_20261022_918237",
  "disclaimer": "Machine-learning outputs are ranked candidate changes, not verified astronomical discoveries. Independent validation is required.",
  "total_candidates": 1,
  "candidates": [
    {
      "candidate_id": "CAND_918237_01",
      "coordinates": {
        "ra": 180.00412,
        "dec": 45.00289,
        "pixel_x": 134.2,
        "pixel_y": 128.7
      },
      "photometry": {
        "flux_reference_mjy": 0.082,
        "flux_target_mjy": 0.245,
        "delta_flux_mjy": 0.163,
        "flux_ratio": 2.98,
        "snr_significance": 7.4
      },
      "morphology": {
        "circularity": 0.88,
        "fwhm_pixels": 2.1,
        "psf_residual_chi2": 1.12
      },
      "ranking": {
        "ml_candidate_score": 0.84,
        "model_version": "baseline_random_forest_v0.1",
        "rank_percentile": 96.5,
        "flag": "CLEAN"
      },
      "limitations": [
        "Provisional calibration; PSF template derived from synthetic model.",
        "Residual background gradient detected within 15 arcseconds."
      ],
      "provenance": {
        "extracted_by": "candidates_service_v0.1",
        "extraction_timestamp": "2026-10-22T00:30:06Z"
      }
    }
  ]
}
```
