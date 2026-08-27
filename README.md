# Uber Ride Surge Pricing Optimization

**Using demand, congestion, and time patterns to recommend more responsive ride pricing**

`Python` `Pandas` `K-Means` `Random Forest Regression` `Feature Engineering` `Pricing Analytics` `Business Analytics`

---

## Business Problem

Static ride pricing can under-react during high-demand or high-congestion periods. That can create a mismatch between rider demand, driver effort, and platform economics.

This project explores whether ride-level operational data can be used to:

- identify distinct pickup-zone behavior patterns,
- estimate an appropriate surge component,
- protect a minimum fare threshold,
- and support better driver allocation and revenue planning.

---

## Dataset

The analysis uses a Kaggle Uber ride analytics dataset containing approximately **150,000 rows and 21 columns**.

Key fields include:

- pickup and drop locations
- booking value
- ride distance
- vehicle type
- driver arrival time (`VTAT`)
- trip completion time (`CTAT`)
- booking status
- cancellation information

---

## Feature Engineering

To capture congestion and time effects, I created features including:

- `pickup_delay_ratio`
- `trip_congestion_ratio`
- `total_time_ratio`
- `hour`
- `day_of_week`
- `month`
- `is_weekend`
- `is_festival`

Pickup locations were also aggregated for zone-level behavioral analysis.

---

## Analytical Approach

### 1. Pickup-Zone Segmentation

I used **K-Means clustering** to group pickup zones using ride volume, driver/trip timing ratios, and average fare.

The analysis produced four useful behavioral groups:

| Cluster | Interpretation | Typical Behavior |
|---|---|---|
| 0 | Central business surge zones | High congestion, high demand |
| 1 | Commuter corridors | Moderate congestion, predictable demand |
| 2 | Premium long trips | High booking value, longer distances |
| 3 | Residential routes | Lower congestion, shorter rides |

This segmentation makes the pricing problem easier to interpret because the same pricing logic does not need to be applied blindly across every zone.

---

### 2. Surge Fare Prediction

A **Random Forest Regressor** was used to estimate the surge component of the fare.

The target was defined as:

`surge_gap = booking_value - base_fare`

Model inputs included congestion, pickup delay, hour, weekend status, and festival status.

The recommended fare was then calculated as:

`predicted_fare = base_fare + predicted_surge_gap`

A business rule enforced a minimum recommended fare of **₹414**.

---

## Example Recommendations

| Observed Booking Value | Predicted Price |
|---:|---:|
| ₹159 | ₹414 |
| ₹627 | ₹616 |
| ₹737 | ₹766 |
| ₹523 | ₹579 |

The behavior check shows that low-fare rides are lifted to the minimum profitability threshold while more congested or demanding situations can receive a higher recommended fare.

---

## Business Value

The framework can support several operational decisions:

- **Driver compensation:** reflect congestion and pickup difficulty more consistently
- **Revenue planning:** forecast fare behavior by region and hour
- **Customer experience:** balance surge requirements with affordability
- **Driver allocation:** identify high-potential surge zones for supply placement
- **Pricing strategy:** distinguish commuter, premium, residential, and central-business ride patterns

Example high-potential locations identified in the analysis included Connaught Place, Cyber Hub, and Ashram.

---

## Deployment Concept

A production pricing module could consume:

**Inputs**
- live pickup/drop information
- traffic conditions
- time of day
- weekend/festival indicators

**Outputs**
- recommended surge component
- predicted fare

The model could ultimately be surfaced through a pricing API or an operations-facing dashboard for monitoring and scenario review.

---

## Repository Contents

- `uber_final.ipynb` — complete analysis, feature engineering, clustering, and modeling workflow
- `Analysis Report` — supporting business and modeling notes
- `README.md` — recruiter-friendly project summary

---

## Key Takeaway

This project combines **segmentation, predictive modeling, and business constraints** to turn ride-level operational data into a practical pricing recommendation framework. Rather than treating surge pricing as only a modeling problem, the analysis connects predictions to driver supply, customer affordability, and platform economics.
