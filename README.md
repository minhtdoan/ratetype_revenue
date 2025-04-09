# Causal Analysis of a certain Rate Type on Hotel Revenue

## Project Overview
This project examines the causal effect of using Corporate Contract Rates (CCR, `IS_CCR_RATE_TYPE`) on hotel revenue (`HOTEL_REVENUE`) using observational data from hotel bookings. The analysis employs two causal inference methods: **Inverse Probability Weighting (IPW)** and **Mahalanobis Distance Matching (MDM)**, with Propensity Score Matching (PSM) referenced in conclusions. The goal is to determine whether employing CCR rates increases or decreases hotel revenue, adjusting for confounding variables such as `HOTEL_CATEGORY`, `SATISFACTION_SCORE`, `DISTANCE_TO_STATION`, and `AVG_DISCOUNT`.

## Business Context
Understanding the impact of CCR rates matters to hotel management — balancing the potential benefits of discounted corporate bookings (e.g., volume and loyalty) against possible revenue losses. This section bridges the technical analysis with practical business implications, aiding stakeholders in interpreting results for pricing policy adjustments, contract negotiations, or market positioning.

## Objective
In the hospitality industry, pricing strategies like Corporate Contract Rates (CCR) are designed to secure bookings from corporate clients by offering negotiated discounts, potentially fostering loyalty and consistent revenue streams. However, the financial impact of such discounts remains unclear — do they drive higher overall revenue through increased volume, or do they erode profits by reducing per-booking income? This analysis addresses this question for hotel management and revenue optimization teams. By quantifying the causal effect of CCR rates on revenue, the project helps stakeholders weigh the trade-offs between discount-driven volume and full-price profitability. The findings (e.g., a €509–558 revenue) suggest potential implications for contract negotiations, pricing policies, and resource allocation, though the practical significance may depend on hotel scale, market segment, and long-term strategic goals.

## Dataset
- **Source**: `raw_ccr_revenue.csv` (not included in the repository; users must provide their own copy).
- **Columns**:
  - `HOTEL_ID`: Unique hotel identifier.
  - `HOTEL_REVENUE`: Continuous outcome variable (revenue in EUR).
  - `IS_CCR_RATE_TYPE`: Binary treatment variable (1 = CCR used, 0 = not used).
  - **Covariates**: 
    - `HOTEL_CATEGORY`: Hotel star rating (e.g., 3, 5).
    - `SATISFACTION_SCORE`: Guest satisfaction score (0–10).
    - `DISTANCE_TO_STATION`: Distance to nearest station (km).
    - `AVG_DISCOUNT`: Average discount applied (%).

- **Sample**: The dataset includes about 44,000 hotels across the globe, with an aggregate of 4 years of booking and discount data.

## Methods
1. **Inverse Probability Weighting (IPW)**:
   - **Purpose**: Estimates the Average Treatment Effect (ATE) across all hotels by weighting observations based on propensity scores.
   - **Steps**:
     - Fits a logistic regression model to estimate propensity scores using the covariates.
     - Computes unstabilized and stabilized IPW weights.
     - Estimates the treatment effect using weighted least squares (WLS).
   - **Findings**: Using CCR rates decreases revenue by approximately €509 (95% CI: [-533, -486], p < 0.0001).

2. **Mahalanobis Distance Matching (MDM)**:
   - **Purpose**: Estimates the Average Treatment Effect on the Treated (ATT) by matching treated and control units on raw covariates.
   - **Steps**:
     - Standardizes covariates and computes the covariance matrix.
     - Performs 1:1 nearest-neighbor matching using Mahalanobis distance.
     - Estimates the treatment effect using OLS on the matched sample.
   - **Findings**: Using CCR rates decreases revenue by approximately €558 (95% CI: [-611, -505], p < 0.0001).
  
## Diagnostics
1. IPW Weight Distribution: Both unstabilized and stabilized weights are below 10, indicating stability and good propensity score overlap.
2. Sensitivity Analysis: Tests effects of an unmeasured confounder (U) on treatment (log-odds: 0–3) and outcome (€0–300). The negative effect persists, reinforcing robustness.
3. Statistical Checks: P-values < 0.0001 and CIs excluding 0 confirm strong evidence against the null hypothesis.

## Requirements
- **Python Version**: 3.8+ (tested with 3.12.7 in the notebook).
- **Libraries**:
  - `pandas`, `numpy`: Data manipulation.
  - `statsmodels`: Statistical modeling (WLS, OLS).
  - `sklearn`: Logistic regression, standard scaling.
  - `matplotlib`, `seaborn`: Visualization.
  - `scipy`: KS test and distance calculations.
