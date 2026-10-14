# EO-OFE Kenya

Earth-observation cross-check and gap-filling of crop-calendar dates (sowing, topdressing,
harvest) recorded in an on-farm-experiment (OFE) maize database for Kenya, using Sentinel-1
and Sentinel-2 in Google Earth Engine.

## What it does

- Loads farmer-reported field-season records and field polygons, keeping only farmers with a
  near-complete seasonal record.
- Extracts a cloud-masked Sentinel-2 optical time series (GCVI/NDVI/NDRE) and a
  speckle-filtered Sentinel-1 radar time series (VH / VH-VV cross-ratio) per field polygon,
  within a season-specific observation window.
- Smooths each series (Savitzky-Golay) and detects phenological stages (start of season, peak,
  end of season) independently from the optical and radar curves.
- Back-calculates sowing and topdressing dates from the satellite start-of-season using lags
  calibrated against the subset of records with a trustworthy recorded date, validated with
  leave-one-out and a temporal holdout (train on 2021-2023, test on 2024).
- Builds a dual-sensor agreement table, reports accuracy (MAE, RMSE, R²) against recorded dates,
  and benchmarks against an independent TIMESAT-style (Jönsson-Eklundh) phenology detector.
- Adds a naive seasonal-calendar baseline, a leave-one-farmer-out transfer test, and bootstrap
  prediction intervals.

The two recorded yield columns (grain and stover, kg/ha) are used only as a diagnostic
cross-check against the satellite canopy signal (to flag a likely failed/stressed crop) — yield
is never predicted or imputed.

## Data

The farmer-level database and field polygons used to produce the results in this notebook are
not included in this repository. Inputs are configured once near the top of the notebook
(`DB_PATH`, `POLY_PATH`, `TAMSAT_PATH`). A Google Earth Engine project (your own) is required
for the extraction steps.

Farmer identities have been replaced throughout with anonymized codes (`Farmer_A` … `Farmer_H`);
no personally identifying information or cloud credentials are included in this repository.

## Reference

Method and results are described in the accompanying manuscript (citation to be added on
publication).

## License

MIT — see [LICENSE](LICENSE).
