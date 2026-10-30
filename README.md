# ✈️ Flight Delays & Cancellations: Analysis and Machine Learning Prediction (2019–2023)

**An end-to-end data analytics and machine learning project investigating when, where, and why U.S. domestic flights get delayed or cancelled, and whether those disruptions can be predicted before departure.**

📊 **Live Dashboard:** [View the interactive report on Looker Studio](https://datastudio.google.com/reporting/24474319-3819-4958-b069-748e02579274)

---

## 1. Title

**Flight Delay & Cancellation Analysis: Testing the Drivers of Disruption Across Time, Airlines, Seasons, and Airports, and Predicting Them with Machine Learning**

---

## 2. Executive Summary

This project analyzes U.S. domestic flight data (January 2019 – August 2023) in two stages:

1. **Statistical analysis:** four hypotheses were tested to find which factors are most strongly associated with delays and cancellations.
2. **Machine learning:** two classification models were built to test whether delays and cancellations can be predicted using only information known before the flight.

Results were presented in an interactive dashboard.

**Key findings: hypothesis testing**

| Hypothesis | Result | Headline Insight |
|---|---|---|
| **H2 – Intraday chain effect** | ✅ Supported | Delay rates climb from **7.25% (05:00)** to **28.29% (21:00)**, nearly 4x higher. Late-aircraft delays rise from ~0.40% to **15.86%** over the same period. The pattern holds for every airline and every high-volume airport examined. |
| **H3 – Airline differences** | ✅ Supported | The gap between the best and worst airline ranged from **10 to 19 percentage points** per year. Chi-square tests show a statistically significant relationship, but the effect size (Cramér's V) is limited, so airline alone is not a decisive factor. |
| **H4 – Winter & weather** | ⚠️ Partially supported | Winter has the highest weather-related **cancellation** rate (**1.75%** vs. 0.56–0.83% in other seasons), but weather-related **delays** peak in **summer** (1.52%), not winter (1.15%). |
| **H5 – Airport congestion** | ✅ Supported | Delay rate rises from **14.17%** at the lowest-volume airports to **17.89%** at the highest-volume airports (+3.72 pp). |

**Key findings: machine learning**

| Model | Algorithm | Test set | Headline Result |
|---|---|---|---|
| **Delay prediction** (arrival delay > 15 min) | Random Forest | 475,105 flights | **Recall 65.6%**: about two thirds of delayed flights were caught before departure. Precision 30.1% vs. a 22.8% base rate (≈1.3x better than random selection). **Departure hour** and **month** were the strongest predictors. |
| **Cancellation prediction** | Logistic Regression vs. Random Forest | 450,000 flights | **Recall 27.3%**, but precision only 4.35% (about 96% of alerts are false alarms). Cancellations are hard to predict with the current features, so the model is **not yet reliable for operational use**. |

**Bottom line:** Time of day is the strongest and most consistent driver of delays, confirmed by both the statistical tests and the machine learning model. The delay model can support low-cost decision-making, while cancellation prediction needs further work. All findings are **associations, not proof of causation**.

---

## 3. Business Problem

Flight delays and cancellations are costly for airlines (crew and aircraft re-planning, compensation, reputation) and for passengers (missed connections, lost time). Operations and planning teams need to know **where disruption is concentrated** and, ideally, **which flights are at risk before they depart**, so they can allocate buffers, staffing, and schedule changes where they have the most impact.

**Questions this project answers**

1. Do delays accumulate as the day progresses (a "chain effect")?
2. Do airlines differ meaningfully in delay and cancellation performance, and is that difference statistically significant and practically important?
3. Is winter the worst season for weather-related disruption, for both delays and cancellations?
4. Do higher-volume (more congested) airports have higher delay rates?
5. Can a machine learning model flag flights likely to be delayed or cancelled using only pre-flight information, and which features matter most?

**Definitions used**
- **Delayed flight (statistical analysis):** departure delay of 15 minutes or more.
- **Delayed flight (ML target):** arrival delay of more than 15 minutes.
- **Delay rate:** delayed flights ÷ completed flights with a valid delay outcome.
- **Cancellation rate:** cancelled flights ÷ all scheduled flights.

---

## 4. Methodology

1. **Data scoping & cleaning**
   - Excluded cancelled flights from delay-rate calculations (no meaningful departure delay).
   - Excluded 2023 from year-over-year comparisons because the data covers only January–August.
   - Used **rates instead of raw counts** so airlines and airports of different sizes can be compared fairly.
   - Treated very low-volume hours (00:00–04:00) as non-evidence for H2.

2. **Hypothesis framing:** each hypothesis was defined with a null (H0) and alternative (H1) hypothesis before analysis.

3. **Segmented analysis**
   - **H2:** delay rate by scheduled departure hour (`CRS_DEP_TIME`), validated separately by airline and by origin airport to rule out composition effects. Late-aircraft delay used as a supporting indicator of the chain effect.
   - **H3:** delay and cancellation rates by airline per year (2019–2022).
   - **H4:** season-level comparison of weather delays (`DELAY_DUE_WEATHER > 0`) and weather cancellations (`CANCELLATION_CODE = 'B'`), plus a month-level drill-down within winter.
   - **H5:** origin airports grouped into four flight-volume quartiles (Q1–Q4), compared on delay rate.

4. **Statistical testing (H3)**
   - Chi-square test of independence for airline vs. delay status and airline vs. cancellation status (α = 0.05).
   - **Cramér's V** to measure the *strength* of the relationship, since very large datasets can make tiny differences statistically significant.

5. **Machine learning (supervised classification)**
   - **Delay model:** a **Random Forest** classifier predicts whether a flight will arrive more than 15 minutes late, using only information known before departure. Evaluated on a held-out test set of 475,105 flights.
   - **Cancellation model:** **Logistic Regression** and **Random Forest** were trained and compared to predict whether a flight will be cancelled. Evaluated on a held-out test set of 450,000 flights.
   - **Features:** departure hour, month, airline, origin airport, destination airport, distance, and day of week.
   - **Evaluation metrics:** accuracy, precision, recall, F1-score, and ROC-AUC. Because both targets are imbalanced (delays ≈ 22.8% of flights, cancellations far rarer), accuracy alone was treated as misleading, and **recall and precision** were the key metrics.
   - **Interpretability:** **permutation feature importance** was used to rank which features drive the delay model's performance.

6. **Dashboarding:** results published in an interactive Looker Studio dashboard with delay and cancellation charts, season and winter-month views, airport-volume quartiles, and KPI cards for the statistical results.

7. **Interpretation discipline:** every conclusion is stated as an association, with limitations and alternative explanations noted (weather, air traffic, airline mix, schedules). Feature importance indicates predictive value, not causation.

---

## 5. Skills

**Analytical**
- Hypothesis design and testing (H0/H1, significance level, decision rules)
- Chi-square test of independence and effect size (Cramér's V)
- Segmentation and cohort comparison (time of day, airline, season, airport quartile)
- Rate-based normalization and data-quality handling (incomplete periods, cancelled flights)
- Distinguishing correlation from causation

**Machine learning**
- Supervised binary classification (Random Forest, Logistic Regression)
- Model evaluation on imbalanced data: precision, recall, F1-score, ROC-AUC, accuracy trade-offs
- Model comparison and baseline benchmarking (precision vs. base rate)
- Permutation feature importance for model interpretation
- Judging model readiness for operational use and identifying improvement areas (class imbalance, decision-threshold tuning, alternative algorithms)

**Tools** *(edit to match your actual stack)*
- Looker Studio, dashboard design and KPI cards
- Power BI, visual planning and chart design
- SQL / Python (pandas, scikit-learn), data preparation, statistical testing and modeling `[confirm which you used]`

**Business & communication**
- Translating statistical and model results into operational recommendations
- Data storytelling for non-technical stakeholders, including honest communication of model limitations
- Researching external context (e.g., pandemic capacity cuts, airline operating models) to explain trends

---

## 6. Results and Business Recommendations

### Results

**Time of day (H2)**
- Delay rate by scheduled departure hour: 7.25% (05:00) → 17.13% (12:00) → 25.38% (18:00) → 28.29% (21:00).
- Every airline analyzed had a higher evening than morning delay rate. Largest gaps: JetBlue (13.48% → 38.30%) and Southwest (8.91% → 35.43%).
- Every high-volume origin showed the same pattern. Largest gaps: MDW (12.88% → 40.03%) and DAL (10.89% → 38.72%).

**Airlines (H3)**
| Year | Highest delay rate | Lowest delay rate | Gap |
|---|---|---|---|
| 2019 | Frontier, 26.15% | Hawaiian, 8.79% | 17.36 pp |
| 2020 | Allegiant, 15.43% | Hawaiian, 5.26% | 10.17 pp |
| 2021 | Southwest, 27.16% | Endeavor, 7.96% | 19.20 pp |
| 2022 | JetBlue, 33.10% | Endeavor, 14.41% | 18.69 pp |

- **2020:** pandemic-era traffic reductions and sparser schedules improved on-time performance.
- **2021–2022:** returning demand, tight aircraft utilization, and crew-planning problems pushed delays back up, especially at low-cost, high-frequency carriers.
- **Cancellations:** Allegiant had the highest rate in 2020 (14.84%) and 2021 (3.26%), linked to its leisure-heavy network and staffing/capacity constraints. Spirit had the lowest rate in 2020 (2.45%) after cutting capacity quickly.
- The airline-delay relationship is statistically significant, but its strength is limited, so airline should not be treated as the sole determinant.

**Seasons (H4)**
| Season | General delay | Weather delay | Weather cancellation |
|---|---|---|---|
| Winter | 17.85% | 1.15% | **1.75%** |
| Spring | 16.11% | 0.85% | 0.72% |
| Summer | 20.73% | **1.52%** | 0.83% |
| Fall | 14.48% | 0.61% | 0.56% |

- Within winter, February had the highest weather-delay rate (1.32%), followed by December (1.17%) and January (0.99%).

**Airport volume (H5)**
| Volume group | Airports | Flights | Delay rate |
|---|---|---|---|
| Q1 (lowest) | 59 | 47,819 | 14.17% |
| Q2 | 58 | 107,224 | 14.93% |
| Q3 | 58 | 291,846 | 15.07% |
| Q4 (highest) | 58 | 2,053,308 | 17.89% |

**Machine learning: delay prediction (Random Forest, 475,105 test flights)**

| Metric | Value | What it means |
|---|---|---|
| Recall | **65.6%** | About two of every three delayed flights are caught before departure |
| Precision | 30.1% | About 1.3x better than random selection (base rate 22.8%) |
| F1-score | 0.413 | Balance of precision and recall |
| ROC-AUC | 0.645 | Modest ability to separate delayed from on-time flights |
| Accuracy | 57.4% | Lower than the other metrics because the model is tuned to catch delays |

Permutation feature importance (performance loss when a feature is shuffled):

| Feature | Importance |
|---|---|
| **Departure hour** | **0.0700** |
| **Month** | **0.0428** |
| Airline | 0.0294 |
| Origin airport | 0.0099 |
| Distance | 0.0064 |
| Day of week | 0.0056 |
| Destination airport | 0.0030 |

- Delay risk is signaled most by **departure hour** and **month**, which matches the intraday chain effect (H2) and seasonal patterns (H4) found in the statistical analysis.
- About 7 of every 10 delay alerts are false alarms, so the model suits **low-cost decision-support actions** rather than costly interventions.

**Machine learning: cancellation prediction (450,000 test flights)**

| Metric | Value | What it means |
|---|---|---|
| Accuracy | 82.23% | Misleading alone, since most flights are not cancelled |
| Recall | **27.34%** | About one in four cancellations is caught |
| Precision | 4.35% | About 96% of cancellation alerts are false alarms |
| F1-score | 7.51% | Low, reflecting the precision/recall imbalance |
| ROC-AUC | 0.6136 | Weak separation between cancelled and non-cancelled flights |

- With the current model and features, cancellations are **hard to predict** and the model is **not yet reliable enough for operational decisions**.
- Priority improvements: handling **class imbalance**, **decision-threshold optimization**, and trying **different modeling methods**.

### Business Recommendations

1. **Build schedule buffers into evening rotations.** Add turnaround slack or spare aircraft to late-day flights, where delays compound the most, and consider moving the most punctuality-sensitive routes earlier in the day.
2. **Break the delay chain.** Prioritize on-time performance of the first morning departures and set recovery rules (swap aircraft, adjust crew pairings) that stop early delays from carrying into evening flights.
3. **Use the delay model as an early-warning flag.** Flag high-risk flights before departure to support operations planning, but pair it with low-cost actions (proactive passenger messaging, gate and crew readiness checks) because most alerts are false alarms.
4. **Do not rely on the cancellation model yet.** Keep it out of operational decisions until imbalance handling and threshold tuning improve precision.
5. **Plan winter operations around cancellations, not delays.** Pre-position crews and aircraft, de-icing capacity, and rebooking resources before December–February, with February as the peak risk month.
6. **Don't ignore summer.** Weather delays peak in summer, so staffing and ATC-related contingency plans should not be winter-only.
7. **Manage congestion at high-volume airports.** Review slot spacing and peak-hour banking at Q4 airports, where delay rates are highest.
8. **Benchmark carefully.** Compare airlines against peers with similar network models (low-cost vs. regional vs. leisure), since airline effects are significant but modest in strength.

---

## 7. Next Steps

- **Improve the cancellation model:** address class imbalance (class weights or resampling), optimize the decision threshold for the precision/recall trade-off, and test other algorithms such as gradient boosting.
- **Enrich the features:** add weather conditions, the previous flight's delay (to model the chain effect directly), route-level patterns, and airport capacity or air traffic data. The current features give only modest predictive power (ROC-AUC 0.645 for delays).
- **Tune and validate the delay model:** hyperparameter tuning, cross-validation, and threshold selection based on the cost of a false alarm vs. a missed delay.
- **Delay-cause decomposition:** break out carrier, NAS, security, and late-aircraft delay categories to quantify the chain effect more precisely.
- **Direct congestion measures:** incorporate runway capacity and air traffic data to move from association toward causal evidence for H5.
- **Complete 2023 data:** refresh the analysis once a full year is available.
- **Predictive dashboard:** surface the delay-risk score by departure hour, airport, and season in the dashboard.

---

## 📁 Repository Structure *(optional, adjust to your repo)*

```
├── README.md
├── data/            # raw and cleaned datasets (or link to the source)
├── sql/             # queries used for analysis
├── notebooks/       # statistical tests (chi-square, Cramér's V) and ML models
├── reports/         # summary slides (delay and cancellation prediction)
└── dashboard/       # screenshots and link to the Looker Studio report
```

## 👤 Author

**Guldeniz Kendigelen**
[GitHub](https://github.com/GuldenizKendigelen) · [LinkedIn](https://www.linkedin.com/in/g%C3%BCldeniz-kendigelen-38051b352/) 
