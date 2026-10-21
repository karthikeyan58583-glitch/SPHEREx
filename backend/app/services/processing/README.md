# Scientific Processing Service

This service executes scientific image preprocessing and differencing using Astropy, NumPy, and SciPy:
- `fits_reader.py`: Memory-mapped FITS parsing, header extraction, and coordinate system validation.
- `alignment.py`: WCS-aware coordinate reprojection onto common reference pixel grids.
- `masking.py`: Detector defect, bad-pixel, and cosmic-ray masking.
- `background.py`: Local and global 2D background estimation and leveling.
- `differencing.py`: Calibrated image subtraction with propagated uncertainty maps.
- `preview.py`: Generating asinh/zscale stretched display previews (WebP/PNG) for the frontend.
