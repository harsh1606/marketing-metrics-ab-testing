# Ad Campaign Causal Inference & A/B Testing

This project analyzes a real **shopping mall paid search campaign** dataset and uses it as a playground to demonstrate **marketing experimentation and causal inference skills**.

The focus is not on ML models, but on **statistical thinking for decision-making**:  
A/B testing, confidence intervals, Difference-in-Differences (DiD), and Bayesian A/B testing.

---

## 🎯 Goals

- Understand how **coupon/discount-oriented** campaigns perform vs **non-coupon** campaigns.
- Evaluate performance using key marketing metrics:
  - CTR (Click-Through Rate)
  - CVR (Conversion Rate)
  - ROAS (Return on Ad Spend)
  - P&L (Profit & Loss)
- Apply **frequentist and Bayesian methods** to answer practical business questions.

---

## 📊 Dataset

- Source: `final_shop_6modata.csv` (Shopping mall paid search campaigns)
- Each row = one **ad group × month**.
- Columns include:
  - `Impressions`, `Clicks`, `CTR`
  - `Conversions`, `Conv Rate`
  - `Cost`, `CPC`, `Revenue`, `Sale Amount`, `P&L`

I engineer additional metrics:

- `ROAS = Revenue / Cost`
- `CPA  = Cost / Conversions`
- `ROI  = (Revenue - Cost) / Cost`
- Segment campaigns into:
  - **Variant A** = coupon/discount-oriented ad groups  
  - **Variant B** = everything else

---

## 🧪 Experiments & Analysis

### 1. Descriptive Analytics

- Monthly trends for:
  - Impressions, Clicks, Conversions
  - Cost, Revenue, P&L
  - CTR, Conversion Rate, ROAS
- Distribution analysis for:
  - Revenue, CTR, Conversion Rate, P&L
- Outlier detection on P&L using:
  - **Z-scores**
  - **Empirical Rule (68–95–99.7)**

---

### 2. Frequentist A/B Tests (Variant A vs B)

**Hypotheses**

- **CTR (clicks / impressions)**  
  - H0: CTR_A = CTR_B  
  - H1: CTR_A ≠ CTR_B  

  → Two-proportion **z-test** + 95% CI for the difference.

- **CVR (conversions / clicks)**  
  - H0: CVR_A = CVR_B  
  - H1: CVR_A ≠ CVR_B  

  → Two-proportion **z-test** + 95% CI.

- **ROAS (row-level)**  
  - H0: mean_ROAS_A = mean_ROAS_B  
  - H1: mean_ROAS_A ≠ mean_ROAS_B  

  → **Welch t-test** on per-row ROAS.

**Business framing**

- Do coupon/discount campaigns just attract more clicks?
- Do they also convert better?
- Are they actually profitable once we factor in revenue vs cost?

---

### 3. Difference-in-Differences (DiD) on ROAS

Set up a simple **quasi-experiment**:

- **Treated group**: Variant A (coupon/discount campaigns)
- **Control group**: Variant B (non-coupon campaigns)
- **Pre-period**: July–September  
- **Post-period**: October–November  

Model:

```text
ROAS ~ treated + post + treated:post
```

- `treated:post` = **DiD estimate** (β₃)

**Interpretation**

- H0: β₃ = 0 → no incremental ROAS change for coupon campaigns over time  
- H1: β₃ ≠ 0 → coupon campaigns’ ROAS trend diverges from non-coupon campaigns

Even though the effect is small / not significant here, it shows the full **causal inference workflow**:
- Define treatment/control
- Define pre/post
- Run DiD
- Interpret the interaction term

---

### 4. Bayesian A/B Test – Conversion Rate

Use a **Beta–Binomial** model to compare conversion rate per impression:

- Variant A: conversions_A / impressions_A  
- Variant B: conversions_B / impressions_B  

With **Beta(1, 1)** priors:

- Sample from posterior:
  - conv_rate_A ~ Beta(α_A, β_A)
  - conv_rate_B ~ Beta(α_B, β_B)
- Compute:
  - `P(conv_rate_B > conv_rate_A)`
  - 95% **credible interval** for uplift `(B - A)`

This lets us say things like:

> “Under a Bayesian model, the probability that non-coupon campaigns have a higher conversion rate than coupon campaigns is ~100%, with a 95% credible interval for uplift between ~1.0 and ~1.1 percentage points.”

---

## 🛠 Tech Stack

- **Language**: Python
- **Libraries**:
  - `pandas`, `numpy`
  - `matplotlib`, `seaborn`
  - `scipy.stats`
  - `statsmodels` (OLS, proportions_ztest)
- **Environment**:
  - Jupyter Notebook / Kaggle Notebook

---

## 🧵 

This notebook demonstrates:

- Design and interpret **A/B tests** for real marketing metrics (CTR, CVR, ROAS).
- Use both **frequentist** and **Bayesian** approaches.
- Apply **Difference-in-Differences** to reason about causal impact over time.
- Translate results into **clear business narratives**:
  - Which campaigns are profitable?
  - Are coupon-heavy strategies actually worth the spend?
  - How should budget be reallocated?



