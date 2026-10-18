# Do Climate Anomalies Improve Short-Term Vegetation Forecasts Beyond Persistence? Pothohar Region, Pakistan

Google Earth Engine + scikit-learn. Districts: Chakwal, Rawalpindi, Jhelum, Attock. Monthly data 2001-2025.

## Question
Do rainfall, soil-moisture and temperature anomalies improve forecasts of NDVI anomalies 1-3 months ahead, beyond a model that uses vegetation history alone?

## Design history (stated openly)
- v1: raw VCI, one 2019-2025 test block: almost no skill (R2 about 0.03-0.06).
- v2: anomalies against a fixed 2001-2017 baseline: test years looked "above normal" because NDVI drifts upward over time, which made skill versus climatology misleading.
- v3 (this notebook): trailing 10-year baseline and rolling-origin evaluation (test years 2015-2025, each predicted from earlier data only). v3 was designed after seeing v1 and v2, so it is not a blind test. Settings were fixed in advance and each result was computed once.

## Methods
Data from Earth Engine (MODIS NDVI, CHIRPS rainfall, ERA5-Land temperature and soil moisture; district means). Ridge, Random Forest and Gradient Boosting were compared with persistence and climatology baselines. The primary comparison is Ridge with climate predictors versus Ridge with NDVI history only; 95% confidence intervals use a bootstrap over test years.

## Results
- A Ridge model using NDVI history beat persistence at all lead times: skill 0.14 (95% CI 0.07-0.20) at 1 month, 0.28 (0.15-0.36) at 2 months and 0.33 (0.16-0.45) at 3 months, better in 8, 9 and 9 of 11 test years.
- Adding climate predictors gave no reliable gain: added skill 0.027 (-0.056 to 0.099), 0.010 (-0.056 to 0.091) and 0.008 (-0.061 to 0.083) at 1, 2 and 3 months. All intervals include zero.
- Random Forest and Gradient Boosting did not outperform Ridge.
- Persistence is a weak baseline at longer leads (a zero-anomaly forecast beat it by 0.155 at 3 months), so skill versus persistence overstates the gain there; skill versus climatology is in metrics_v3.csv.
- Exploratory (four comparisons, no confidence intervals): climate predictors added skill for winter (0.17) and spring (0.25) targets but not for monsoon (-0.07) or autumn (-0.20). This is a hypothesis, not a finding.
- The trailing baseline reduced, but did not remove, the positive NDVI drift (yearly mean anomalies remain mostly positive).

## Limitations
District means hide spatial variation; 1 km MODIS; no cropland mask; four districts; 11 test years; the cause of the NDVI drift was not investigated; v3 design chosen after seeing earlier results.

## Future work
Cropland-masked pixel-level modelling, seasonal models for the winter crop cycle, explanations for the NDVI drift, and additional predictors such as irrigation data.
