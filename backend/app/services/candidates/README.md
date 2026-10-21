# Candidate Extraction & Scoring Service

This service identifies and evaluates candidate temporal changes from difference arrays:
- `extractor.py`: Connected-component anomaly detection and source segmentation.
- `photometry.py`: Aperture photometry, signal-to-noise ratio calculation, and flux ratio estimation.
- `morphology.py`: Shape analysis (fwhm, elongation, circularity, PSF-fit residual $\chi^2$).
- `classifier.py`: Baseline scikit-learn model inference for candidate ranking.
- `screening.py`: Rule-based artifact filters (rejecting optical diffraction spikes and edge defects).
