# Project Notes (Quick Reference)

Brief, chronological notes on what was done and found at each step — for quick recall before presenting, or to pick back up the next day without re-reading full notebooks. Not a substitute for the notebooks or `interview_defense.md` — this is the "what," those are the "why."

## Milestones (Stages 1–4)
- **Stage 1:** Dataset chosen — Metro Interstate Traffic Volume (Kaggle/UCI). Regression problem, target `traffic_volume`. Backup considered: hasibullahaman's Traffic Prediction Dataset (classification, not used).
- **Stage 2:** Repo scaffolded (`data/`, `notebooks/`, `powerbi/`, `reports/`, README, requirements.txt, .gitignore) and pushed live: https://github.com/iram-meher-06/traffic_ds-equate
- **Stage 3:** First data pass — 48,204 rows × 9 columns. Found: `temp` min = 0 Kelvin (impossible, sensor error), `rain_1h` max = 9,831.3mm (impossible, bad reading), 17 exact duplicate rows, 7,629 rows sharing a timestamp with another row, `holiday` column 99.9% blank by design (only populated on the actual holiday hour).
- **Stage 4:** Concept-to-stage map completed (which coursework concepts genuinely fit vs. don't). Business questions confirmed (forecasting, weather/holiday impact, peak-pattern risk windows). Deep learning and Great Expectations "why not" reasoning written up in `reports/interview_defense.md`.

## `01_eda.ipynb` — step-by-step

**Shape:** 48,204 rows, 9 columns — confirmed via `df.shape`.

**Checklist step 1 — dtypes:** `date_time` is stored as plain text, not a real datetime type — will need conversion in preprocessing before extracting hour/day-of-week/month features. All other columns matched expectations (ints for counts, floats for weather measurements, strings for categoricals).

**Checklist step 2 — missing values:** only `holiday` has nulls (48,143 of 48,204 rows). Not real missingness — the column is only populated on the actual holiday hour by design. Will convert to a clean `is_holiday` binary flag in preprocessing rather than impute.

**Checklist step 3 — duplicates:** 17 fully identical rows (trivial, drop). More importantly, 7,629 rows share a `date_time` with another row — likely multiple weather conditions logged per hour. Needs a preprocessing decision (keep first record per hour, or aggregate) to avoid near-duplicate rows leaking across train/test splits.
For a regression model where each row should represent one hour's traffic count, this needs a decision in preprocessing (e.g., keep the first record per hour, or aggregate the weather fields) — otherwise a random train/test split could put near-identical rows on both sides, quietly inflating validation scores.

**Checklist step 4 — describe() vs. domain plausibility:** `temp` min = 0 Kelvin (impossible — colder than any temperature ever recorded on Earth, ~184K is the real record). `rain_1h` max = 9,831.3mm (impossible — real-world hourly rainfall records are ~300mm). Both are sensor/logging errors caught only by knowing the real-world unit bounds, not by looking for "big numbers." `traffic_volume`, `snow_1h`, `clouds_all` all check out fine.

**Checklist step 5 — categorical cardinality:** `holiday` (11) and `weather_main` (11) are clean, manageable for one-hot encoding. `weather_description` (38) is more granular and has a casing bug — `'Sky is Clear'` vs `'sky is clear'` are the same thing split into two categories. Decision for preprocessing: normalize casing if we keep `weather_description`, or just use the cleaner `weather_main` and drop it.

**Categorical encoding reference (types, for preprocessing later):**
- **One-hot encoding** — one binary (0/1) column per category (e.g. `weather_main` → `is_Clear`, `is_Rain`, `is_Clouds`, ...). Safe default for nominal (unordered), low-cardinality columns. Plan: use for `weather_main` and `is_holiday`.
- **Label/ordinal encoding** — assigns each category a single integer (`Clear=0, Rain=1, ...`). Risky for nominal data — invents a fake order/distance a linear model would take literally. Not used here.
- **Target/mean encoding** — replaces a category with the average target value for that category (e.g. `'Thunderstorm'` → average `traffic_volume` across thunderstorm rows). Useful for high-cardinality columns like `weather_description` (38 values), but risks **data leakage**: if the average is computed using the same rows used for training/validation, each row's feature value is partly built from its own target value, making validation scores look inflated in a way that won't hold on truly new data. Done correctly, it requires computing the average only within cross-validation folds (train-fold rows compute it, validation-fold rows receive it, never overlapping) — real, but extra setup. Not worth the overhead here since `weather_description` is already leaning toward being dropped (casing bug + redundant with `weather_main`).

**Worked numeric example — how target-encoding leakage actually happens:**

5 rows, all `weather_description = 'heavy snow'`: rows 1–4 (train) have `traffic_volume` = 800, 850, 820, 790; row 5 (validation) = 2000 (an unusual heavy-snow hour).

- *Leaky way* — compute the mean using all 5 rows (row 5 included): (800+850+820+790+2000)/5 = **1052**. That becomes row 5's feature too. Model trained on rows 1–4 learns "feature≈1052 → predict≈1052." Evaluated on row 5: predicted 1052, true 2000, error = **948**.
- *Honest, fold-safe way* — compute the mean using only rows 1–4 (row 5 excluded): (800+850+820+790)/4 = **815**. Apply 815 to row 5 without row 5 ever contributing to it. Predicted 815, true 2000, error = **1185**.

The leaky error (948) is smaller than the honest one (1185) — and that gap is entirely artificial. It's smaller only because row 5's own true value (2000) was one of the 5 numbers averaged into the very feature used to predict row 5 — a fraction of the true answer was folded into the input before the "prediction" ever happened.

**The rule this gives us:** never let a row's own target value participate in computing that row's own feature.

**Checklist step 6 — target variable distribution:** `traffic_volume` is **bimodal** — a sharp spike around 300–500 (overnight low-traffic) and a separate broad hump around 4,000–5,500 (daytime high-traffic), with a valley between. Verified (not assumed) against the actual data: mean traffic by hour is 371–835 for hours 0–4 and 4,100–5,664 for hours 6–17, peaking at hour 16 (5,664, afternoon rush) — confirms the two humps are genuinely day vs. night regimes. Skewness is near zero (-0.089) — mean (3,259.8) is slightly below median (3,380), consistent with a very mild left lean, but skewness and bimodality are different things: the near-zero score doesn't capture the bimodal shape at all, which is the far more important finding. Strongly confirms `hour` will be a critical feature, and supports trying the optional K-Means segmentation in Stage 7. 2 rows at exactly 0 and 57 rows under 50 are plausible quiet-hour readings (no physical law violated, unlike temp=0K or rain=9831mm), not errors — no cleanup needed.

**Checklist step 7 — temporal continuity:** correction to Stage 3's quick pass — the date range reported there ("2013-01-01 to 2017-12-31") was wrong, because it compared `date_time` as raw text strings, which don't sort chronologically for a `DD-MM-YYYY` format. Properly parsed, the real range is **2012-10-02 to 2018-09-30** (~6 years). Of the 52,551 hourly slots that range should contain, only 40,575 unique timestamps actually exist — **11,976 hours (~23%) are missing**, likely sensor outages. Means we cannot treat this as a gapless continuous time series — lag-style features (e.g. "traffic 1 hour ago") need to account for gaps in preprocessing.

**EDA checklist — full reference table (why each step belongs in every DS project, not just this one):**

| # | Step | Why it's standard practice | What it represents / tells you |
|---|---|---|---|
| 1 | Shape & dtypes | Nothing downstream can be trusted until you know what pandas thinks each column *is* — a numeric column silently stored as text breaks calculations silently. | Whether the data matches its expected structure. |
| 2 | Missing values | Missingness isn't automatically a defect, but you must find it before deciding *why* it's missing and how to handle it. | Whether gaps are random noise or a designed signal. |
| 3 | Duplicates | Repeated rows inflate whatever pattern they represent, and can leak near-identical data across train/test splits, making validation scores lie. | Whether each row is a genuinely independent observation. |
| 4 | `.describe()` vs. domain plausibility | Statistics alone don't know what's physically real — a value can look "not extreme" statistically and still be impossible in the real world. | Whether the values themselves are trustworthy, not just present. |
| 5 | Categorical cardinality | Determines encoding strategy (one-hot vs. target encoding vs. drop), and exposes messy/inconsistent labels that silently fragment a category. | How much distinct information a category carries, and how clean it is. |
| 6 | Target variable distribution | You need the shape of what you're predicting before choosing a model or a transformation — a skewed or multi-modal target changes what "good" model behavior looks like. | The underlying structure/behavior of the outcome being predicted. |
| 7 | Temporal continuity | For time-indexed data, the timeline must be confirmed continuous before trusting time-based features (lags, rolling averages) — gaps silently corrupt those. | Whether "the next row" really means "the next hour." |

**The throughline:** EDA's job is to build justified trust (or distrust) in every column before a model sees it — structure (1), completeness (2), independence (3), correctness (4), cleanliness (5), the target's own behavior (6), and time ordering (7).

## `02_preprocessing.ipynb` — step-by-step

**Step 1 — load & parse `date_time`:** parsed with `format='%d-%m-%Y %H:%M'` from the start this time. Confirmed range matches EDA step 7's corrected finding (2012-10-02 to 2018-09-30).

**Step 2 — drop exact duplicates:** 48,204 → 48,187 rows, 17 dropped — matches the EDA finding exactly.

**Step 3 — handle rows sharing a timestamp:** checked first whether `traffic_volume` ever disagrees within a duplicated hour — it never does (0 of 5,430 duplicated hours), confirming these are redundant weather-logging entries, not conflicting readings. Kept the first row per hour, dropped the rest: 48,187 → 40,575 rows, matching EDA's "unique timestamps present" count exactly.

**Step 4 — fix temp=0K:** only 10 rows affected (of 40,575). Replaced with NaN, linearly interpolated from neighboring hours. New min = 243.39K (~-30°C) — cold but physically plausible for Minnesota winter, unlike the impossible 0K before.

**Simplifications & shortcuts taken (flagged honestly — growing list, added to at every step):**
- `method='linear'` (Step 4 interpolation): treats consecutive rows as if evenly spaced in time, ignoring that ~23% of hours are actually missing (EDA step 7) — so two "neighboring" rows could really be several hours apart in real time, and this method wouldn't know. Only affected 10 `temp` readings here, so the error is negligible — but it's a real shortcut, not a fully rigorous one. The more precise choice would be `method='time'` with a real datetime index.
- `limit_direction='both'` (Step 4 interpolation): a backup rule for filling a missing value that has no neighbor on one side (e.g. it's the very first row in the whole dataset). Simple example: if the earliest row (2012-10-02 09:00) had itself been one of the invalid `temp=0` readings, there'd be no earlier row to average with — this setting says "just copy the next hour's temperature instead of leaving it blank," rather than requiring a value on both sides.

**Step 5 — fix rain_1h outlier:** checked first, found exactly 1 row affected (2016-07-11 17:00, matching EDA's finding). Same fix as `temp`: replaced with NaN, linearly interpolated. New max = 55.63mm — realistic, nowhere near the impossible 9,831.3mm before.

**Step 6 — engineer is_holiday:** collapsed the 11-name `holiday` column into a binary `is_holiday` flag (53 holiday hours, 40,522 non-holiday, after dedup). Original `holiday` column dropped — its signal is fully captured in the flag.

**Step 7 — extract hour/day_of_week/month:** pulled three plain number columns out of `date_time` (hour 0-23, day_of_week 0=Monday-6=Sunday, month 1-12). Verified correct against a known date (2012-10-02 = Tuesday → day_of_week=1). These feed directly on the day/night pattern already confirmed in EDA.

**Step 8 — one-hot encode weather_main, drop weather_description:** 11 new True/False columns added (`weather_Clear`, `weather_Rain`, etc.), one per weather type. Original `weather_main` and `weather_description` columns both dropped.

**Step 9 — save cleaned feature matrix:** saved to `data/processed/traffic_features_clean.csv` — 40,575 rows, 21 columns (up from 9 raw columns, thanks to `is_holiday`, `hour`/`day_of_week`/`month`, and 11 one-hot weather columns). This is the file Stage 6 (modeling) loads from, not the raw CSV.

**Preprocessing complete.** Summary of all fixes applied: dropped 17 exact duplicates + 7,612 redundant same-hour rows (verified `traffic_volume` never disagreed within a duplicated hour first); fixed 10 invalid `temp=0K` and 1 invalid `rain_1h=9831mm` reading via interpolation; collapsed `holiday` into a 53-row `is_holiday` flag; extracted `hour`/`day_of_week`/`month`; one-hot encoded `weather_main` (11 columns) and dropped `weather_description`.

*(more steps added here as we go)*
