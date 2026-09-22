# Data Dictionary — DSRH 2026 Selection Challenge

Each row is one signal **segment** captured on a radio channel. The task is
binary classification: predict `anomaly` (1 = anomalous segment, 0 = normal).

| Column | Type | Description |
|---|---|---|
| `id` | int | Unique segment identifier. Use this to match predictions in `sample_submission.csv`. |
| `channel` | string | Radio channel the segment was recorded on (9 possible channels, e.g. `CADC0872`). |
| `sampling` | int | Sampling rate/step used to record the segment. |
| `duration` | int | Duration of the segment. |
| `len` | int | Number of samples in the segment. |
| `mean` | float | Mean signal value over the segment. |
| `var` | float | Variance of the signal. |
| `std` | float | Standard deviation of the signal. |
| `kurtosis` | float | Kurtosis (tailedness) of the signal distribution. |
| `skew` | float | Skewness of the signal distribution. |
| `n_peaks` | int | Number of peaks detected in the raw signal. |
| `smooth10_n_peaks` | int | Number of peaks after smoothing with a window of 10. |
| `smooth20_n_peaks` | int | Number of peaks after smoothing with a window of 20. |
| `diff_peaks` | int | Number of peaks in the first difference of the signal. |
| `diff2_peaks` | int | Number of peaks in the second difference of the signal. |
| `diff_var` | float | Variance of the first difference. |
| `diff2_var` | float | Variance of the second difference. |
| `gaps_squared` | int | Squared measure of gaps in the segment. |
| `len_weighted` | int | Length weighted metric. |
| `var_div_duration` | float | Variance divided by duration. |
| `var_div_len` | float | Variance divided by length. |
| `anomaly` | int (0/1) | **Target.** Present only in `train.csv`. 1 = anomalous segment. |

## Files

- `train.csv` — 1698 labeled segments. Use this to build and validate your model.
- `test.csv` — 425 unlabeled segments. Predict `anomaly` for each `id` here.
- `sample_submission.csv` — template showing the exact submission format
  (`id,anomaly`, one row per test segment).

## Notes

- Classes are imbalanced: about 20% of segments are anomalies. A model that
  always predicts 0 already scores ~80% accuracy but 0% recall on anomalies —
  this is why the competition is scored on **F1-score**, not accuracy.
- All 9 channels appear in both `train.csv` and `test.csv`, in roughly the
  same proportions.
