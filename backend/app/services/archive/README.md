# Archive Service (NASA/IPAC IRSA)

This service manages external communication with the NASA/IPAC Infrared Science Archive:
- `client.py`: Async HTTP client for IVOA SIAv2 and TAP interfaces.
- `queries.py`: Spatial cone search and metadata query formulations.
- `download.py`: Safe FITS cutout retrieval with bandwidth and size guards.
- `validation.py`: Verification that data originates from verified SPHEREx collections.
