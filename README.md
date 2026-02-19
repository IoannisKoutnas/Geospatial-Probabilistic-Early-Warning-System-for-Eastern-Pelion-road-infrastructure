# Eastern Pelion Road Infrastructure Risk Monitor

This repository contains a **Geospatial & Probabilistic Early Warning System** designed to monitor **7 Critical Infrastructure Nodes (CINs)** in **Eastern Pelion, Greece**. Developed in the wake of **Storm Daniel**, this tool fuses live weather telemetry with static terrain data to provide **real-time structural risk assessments**.

## 1. Monitoring Network Configuration

The system monitors 7 strategic locations, each assigned specific parameters based on terrain slope and operational role:

| Location              | Coordinates     | Slope (°) | Role                        |
|-----------------------|-----------------|-----------|-----------------------------|
| Zagora HQ            | 39.44, 23.10   | 12        | Regional Logistics Hub      |
| Chania Pass          | 39.39, 23.08   | 22        | Primary Export Route        |
| Karavoma Junction    | 39.40, 23.10   | 18        | Logistics Choke Point       |
| Horefto Road         | 39.44, 23.11   | 38        | High-risk Shear Zone        |
| Kalokairinos Bridge  | 39.45, 23.10   | 42        | Isolation Point             |
| Makrirrachi Bridge   | 39.42, 23.14   | 28        | Scour Risk (Gully)          |
| Papa Nero Bridge     | 39.41, 23.16   | 4         | Surge Risk (Delta)          |

## 2. The Probabilistic Risk Engine

The model uses a **Logistic Function (Sigmoid)** to calculate a failure probability percentage (0.0% to 100.0%).

### Core Formula
$$
P(\text{Failure}) = \frac{1}{1 + e^{-z}}
$$

The linear predictor ($z$) varies by the physical characteristics of the terrain:

- **Landslide Model** (Slope > 30°):  
  $z = -7.5 + (0.09 \times \text{Rain}) + (0.12 \times \text{Slope})$

- **Scour Model** (Slope < 15°):  
  $z = -6.0 + (0.06 \times \text{Rain})$

- **Standard Saturation Model** (Intermediate):  
  $z = -8.0 + (0.05 \times \text{Rain}) + (0.05 \times \text{Slope})$

Rain is in mm (typically accumulated over a relevant period, e.g., 24h or event total).

## 3. Operational Risk Thresholds

Numerical probabilities are classified into standardized operational alert levels:

- 🔴 **CRITICAL**: Probability ≥ 75.0%  
- 🟡 **WARNING**: Probability between 40.0% and 75.0%  
- 🟢 **STABLE**: Probability < 40.0%

## 4. Historical Backtesting Results

Validation was conducted using recorded data from 2023 disaster events:

| Event         | Rainfall (mm) | Failure Probability                          |
|---------------|---------------|----------------------------------------------|
| Storm Daniel | 432.2        | 100.0% (All Nodes)                           |
| Storm Elias  | 151.9        | 95.7% (HQ), 100.0% (Steep Slopes)            |

## 5. Economic Impact

This tool provides direct logistics optimization by calculating the cost of failure:

- **Avoided Costs**: Prevents truck turnarounds costing approximately **€350** per incident.
- **Fleet Savings**: Preventing a 10-truck fleet from entering high-risk zones saves **€3,500** per day.