# Interview Defense Notes

Running log of every non-obvious decision in this project and how to defend it. For personal reference before presenting — not part of the public-facing README narrative.

## Why regression, not classification?
`traffic_volume` is a continuous hourly count (0–7,280), not a category. Predicting "how much" traffic rather than "which class of traffic" is both the more useful business signal (a logistics planner needs a number, not a label) and the more technically natural fit for this target.

## Why these models, why not others?
- **Logistic regression / Naive Bayes / LDA / ROC-AUC**: all need a classification target — doesn't apply here.
- **Association rule mining**: needs transactional/basket-style data — nothing here resembles that.
- **PCA**: evaluated and skipped — only ~5 numeric predictors, nothing meaningful to reduce.
- **Genetic algorithms, PAC learning theory, Lagrangian/KKT optimization, Laplace transforms, graph theory**: theoretical foundations that run invisibly underneath libraries (e.g. sklearn's solvers use gradient descent/linear algebra internally) — relevant to "how does it work under the hood" follow-ups, not something hand-coded as a deliverable here.

## Why not Great Expectations / formal data-validation tooling?

Great Expectations (GE) is an open-source Python data validation framework. It lets you write formal "expectations" — assertions about your data, such as "column `temp` should never be below 250 Kelvin," "`traffic_volume` should never be negative," or "`date_time` should have no duplicates" — and it automatically checks these every time new data arrives, generating a report and optionally failing a pipeline if a rule is violated. It's built for **continuous, recurring pipelines** — e.g. a company ingesting fresh sensor data every hour and needing automatic alerts if tomorrow's incoming data breaks yesterday's assumptions.

This project's situation is different: the dataset is a **one-time historical CSV download** (2013–2017, already fully collected and static). There is no live pipeline ingesting new rows on a schedule — no "tomorrow's data" that could silently violate an assumption and need automatic re-checking. Standing up a full validation suite for data that will never change again provides little practical benefit versus a handful of plain pandas assertions run once in the notebook (e.g. `assert df['temp'].min() > 200`) — same real-world effect, a fraction of the setup overhead.

If asked directly — "why didn't you use Great Expectations, real practitioners use it" — the honest, strong answer is: *"I know it and would use it if this were a live production pipeline receiving new data continuously — that's exactly the scenario Great Expectations is built for. Since this is a static historical dataset with no ongoing ingestion, I used direct pandas assertions instead — functionally equivalent for a one-time clean, without the overhead of standing up a full validation suite for data that will never change again."* This demonstrates knowing the tool **and** knowing when it's overkill for the scale of the problem — which is itself a mark of seniority: using the right tool for the actual scale of the problem, not just reaching for the fanciest tool available.

## Why not deep learning?

Deep learning models (neural networks with many layers) have a large number of internal parameters (weights) that need to be learned. To learn those weights reliably without overfitting, they generally need a *lot* of training examples — often hundreds of thousands to millions of rows — and they tend to pay off most when the input itself has rich, complex internal structure for the network to discover automatically on its own. Classic examples: raw pixel values in images (where the network learns to detect edges and shapes on its own), or raw text sequences (where it learns grammar and semantic patterns). In those domains, there's a huge amount of hidden structure between neighboring inputs that a network can exploit — and enough data to learn it without overfitting.

This dataset is ~48,000 rows with only about 8 predictor columns, and those columns are already simple, directly meaningful scalar or categorical values — temperature, rain amount, cloud percentage, a categorical weather label, hour of day, day of week, holiday flag. There is no hidden pixel-like or sequence-like structure here for a neural net to "discover" the way there is in image or text data — the relationships between these features and traffic volume are relatively direct, and are already captured well by tree-based models like Random Forest or Gradient Boosting, which are well known to perform excellently on exactly this kind of small-to-medium sized structured/tabular data.

This isn't just personal opinion — it's a well-established empirical finding in ML research: for tabular data with a modest number of features and rows, gradient-boosted trees (like XGBoost or LightGBM) very consistently match or beat neural networks, and it's only once data reaches into the hundreds of thousands or millions of rows with much higher dimensionality that deep learning starts to pull ahead, if at all — and even then, well-cited research (e.g. "Why do tree-based models still outperform deep learning on tabular data?", 2022) shows tree-based models often still win on tabular data regardless of scale.

**Concretely, here's how this will be proven, not just asserted:** in the Stage 6 model comparison, a simple feed-forward neural network (1–2 hidden layers) will actually be trained and evaluated alongside Random Forest and Gradient Boosting, measuring RMSE/MAE/R² for each, exactly like every other candidate model. The expectation, backed by both the size of this dataset and the research above, is that gradient boosting will match or beat the neural net. If that's what actually happens (very likely), the interview answer becomes evidence-based rather than defensive: *"I tested a neural net — it scored X RMSE, my Random Forest/XGBoost scored Y, which was better — consistent with known research that tree-based models dominate on tabular data at this scale, so I chose the simpler, faster, more interpretable model."* That turns "why not deep learning" from a hand-wave into a demonstrated decision — which is exactly what a 20-year veteran wants to hear: not "deep learning is overhyped" as an opinion, but "I tested it, here is the number, and here is why that number makes sense."

*(This section will be updated with the actual measured RMSE/MAE/R² numbers once Stage 6 is run.)*

## Why not target-encode `weather_description`?

`weather_description` has 38 distinct values (vs. `weather_main`'s clean 11), so target/mean encoding — replacing each category with the average `traffic_volume` for that category — looks tempting as a way to use it without a 38-column one-hot explosion.

It was deliberately not used, for two reasons. First, `weather_description` has its own data-quality problem: inconsistent capitalization (`'Sky is Clear'` vs. `'sky is clear'` both present as separate values), which would need cleaning before any encoding is trustworthy. Second, and more importantly, target encoding carries a real risk of **data leakage** if done naively: say `'heavy snow'` appears in only 5 rows with `traffic_volume` values `[800, 850, 820, 790, 810]`. Target encoding replaces `'heavy snow'` with the average of those five values, 814. If that average is computed using all 5 rows and then applied back as a feature to those same 5 rows, each row's feature value was partly built from its own target value — the model gets a smuggled-in hint of the answer. If one of those rows then lands in a validation split, the model looks unrealistically accurate on it, because part of the true answer leaked into the feature during encoding — an inflated number that won't hold up on genuinely new data (next year's snowstorm), which defeats the entire purpose of validation.

Done correctly, this requires computing the per-category average using only the training fold's rows, then applying it to the validation fold's rows, which never contributed to computing it — real cross-validation plumbing, not just a `.groupby().mean()`. That setup cost isn't worth paying for a column that's already redundant with the cleaner `weather_main` and has its own casing bug — so `weather_main` (one-hot, no leakage risk) is used instead, and `weather_description` is dropped. If asked why not use target encoding here despite it being a legitimate industry technique: *"I know it and would use it if this were a high-cardinality column that carried unique signal `weather_main` didn't already capture — proper cross-validated target encoding is a real, valid technique. Here it wasn't worth the leakage risk and setup cost for a column that mostly duplicated information I already had cleanly."*

## Data quality issues found (and why they matter)
- `temp` minimum of 0 Kelvin — physically impossible (absolute zero), a sensor/logging error, not a real reading.
- `rain_1h` maximum of 9,831.3mm in one hour — far beyond any real-world hourly rainfall record; a known bad reading.
- 17 exact duplicate rows, and 7,629 rows sharing a timestamp with another row (likely duplicate weather-API logs per hour) — left unhandled, these risk leaking near-duplicate rows across train/test splits during random CV splitting.
- `holiday` is 99.9% blank by design (only populated on the actual holiday hour) — engineered into a clean `is_holiday` binary flag rather than treated as a missing-data problem.

## Honest resume-level verdict

This is a solid, well-executed **intermediate/associate-level** Data Scientist portfolio project — not a beginner tutorial-follow, but also not "advanced research." What pushes it above beginner level: real outlier diagnosis found through manual investigation (not hypothetical or tool-flagged), a disciplined multi-model cross-validated comparison rather than a single model, an empirically-tested (not just asserted) deep learning rejection, optional unsupervised segmentation, and a business-framed dual narrative that ends in an actual dashboard artifact rather than just a notebook. What keeps it at "solid associate" rather than "senior/advanced": it uses a clean, well-known benchmark dataset rather than messy real-world data, and there is no deployment/MLOps component — no API, no live serving, no monitoring.

That's not a flaw in the plan — it's actually smart design: this project functions as the **rehearsal** on clean data. The real differentiator is what happens next, when this exact documented methodology gets applied to the mentor's messy, real company dataset in the days immediately following. That's the part that will genuinely read as advanced, because working with messy real-world data — not a polished Kaggle benchmark — is the actual hard part of the job. The honest way to frame this in the talking track: *"This repo is my documented, reusable methodology — I proved it on a clean benchmark first, then applied it live to a real business dataset."*

## Business problem is real, not invented (sources)
- FHWA: ~25% of non-recurring highway delay is due to adverse weather ([source](https://ops.fhwa.dot.gov/weather/overview.htm))
- US DOT: full congestion cost report ([source](https://www.transportation.gov/sites/dot.gov/files/docs/Costs%20of%20Surface%20Transportation%20Congestion.pdf))
- FHWA: 2023 Urban Congestion Trends, most recent published federal data ([source](https://ops.fhwa.dot.gov/publications/fhwahop24027/fhwahop24027.pdf))
- Maryland DOT case study on hourly traffic volume prediction via ML ([source](https://arxiv.org/pdf/1711.00721)) — closest direct precedent to this project's target variable

## A skewed distribution 

It is a dataset that is not symmetrical around its peak, causing the data to cluster more on one side. This creates a long "tail" that extends toward the less frequent values. It indicates an imbalance or asymmetry in the data.

The Two Types of Skewness Distributions are categorized based on the direction in which the long tail points:

Right-Skewed (Positively Skewed): The tail stretches out to the right. This occurs when a dataset contains a few unusually high values (e.g., household wealth or income, where most people make average money, but a few make billions).

Left-Skewed (Negatively Skewed): The tail stretches out to the left. This happens when a dataset has a few unusually low values (e.g., exam scores on an easy test, where most students score high, but a few score very low).Impact on Central TendencyIn a perfectly symmetrical distribution, the mean, median, and mode are all the same. 

In a skewed distribution, extreme values (outliers) pull the average, changing these relationships:
Right-Skewed: The mean is greater than the median. Skew score>0

Left-Skewed: The mean is less than the median. Skew score<0

Skew=0: Perfectly symmetrical distribution (like a standard normal distribution).


