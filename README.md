# UK Power Market Merit-Order Simulator  
**Author:** Lakshana Sridhar
**Language:** Python (pandas • numpy • matplotlib • seaborn)  

---

## Overview  
This model simulates **hourly electricity dispatch and market-clearing prices** for a UK-style power system using the *merit-order principle*.  
Generators are dispatched from lowest to highest marginal cost (including a CO₂ price) until total demand is met.  

Two timeframes are included:
- **1-Day simulation (24 h):** shows clear daily solar-driven patterns.  
- **7-Day simulation (168 h):** captures wind variability, weekend demand changes, and carbon-price effects.

The model is designed for clarity, reproducibility, and interview demonstration purposes — it can later be extended to live ENTSO-E or National Grid ESO data.

---

## Model Inputs  

| Parameter | Value | Rationale |
|------------|--------|-----------|
| **Carbon price** | €80 /t CO₂ | Mid-range UK ETS price |
| **Average demand** | 30 GW | Typical UK weekday load |
| **Demand swing** | ±3 GW | ~10 % daily variation |
| **Wind capacity** | 28 GW | UK 2024 installed base |
| **Solar capacity** | 14 GW | UK 2024 installed base |
| **Gas capacity** | 25 GW | Dominant marginal unit |
| **Coal capacity** | 1 GW | Residual legacy plant |
| **Nuclear capacity** | 5 GW | Typical single-fleet equivalent |

Variable costs (€ / MWh) and CO₂ intensities (t CO₂ / MWh) are drawn from IEA and BEIS reference data.

---

## Dispatch Logic  
1. **Generate hourly profiles** for wind, solar, and demand  
   - Wind varies smoothly over multi-day cycles with random noise.  
   - Solar repeats a daylight sinusoid each day.  
   - Demand follows a daily sine curve with a 5 % weekend reduction.  

2. **Compute marginal cost per technology:**  
   \[
   \text{TotalCost} = \text{VarCost} + \text{CO₂Intensity} \times \text{CarbonPrice}
   \]

3. **Dispatch in merit order** until demand is met.  
   The generator whose cumulative capacity first exceeds demand sets the **clearing price**.

4. **Calculate hourly CO₂ emissions** and aggregate over the period.

---

## Results Summary (Representative 7-Day Run)

| Metric | Result | Realistic Range | Comment |
|---------|:------:|:----------------|---------|
| **Average price** | **€ 61.9 / MWh** | 55 – 70 | Aligned with UK baseload averages |
| **Total CO₂** | **0.33 Mt / week** | 0.3 – 0.8 | Matches mixed gas-renewable system |
| **Renewables share** | **67 %** | 60 – 70 % | Reflects high-wind, moderate-solar week |
| **Price range** | **0 – 83 €/MWh** | 0 – 85 | Gas marginal cost defines upper cap |

---

##  Figures  

### **1. One-Day Simulation**  
Illustrates the classic daily merit-order cycle.  
Solar reduces mid-day prices to near zero; gas ramps up in evening peaks.  

### **2. Seven-Day Simulation**  
Shows multi-day wind variability.  
Periods of low wind drive higher prices and CO₂, while windy days cut both.  

*(Figures are automatically generated when running the notebook.)*

---

## Interpretation  
- **Merit-order behaviour:** zero-cost renewables and nuclear dispatched first; gas and coal only when demand exceeds renewable output.  
- **Carbon pricing:** raises marginal costs of fossil units, setting the market cap at € 83 / MWh (gas = 55 + 0.35×80).  
- **Price dynamics:** lowest during high wind + solar hours; highest during calm evenings.  
- **CO₂ intensity:** ≈ 0.22 t CO₂ / MWh, consistent with real UK grid.  
- **Renewables penetration:** 65 – 70 %, delivering strong decarbonisation and lower price volatility.  

---

## Learning Outcomes  
This model demonstrates:  
- Quantitative energy-market reasoning  
- Carbon-cost impact on dispatch order  
- Understanding of price volatility drivers  
- Ability to code transparent, reproducible simulations  

---

## Possible Extensions  
1. **Live data integration:** connect to ENTSO-E Transparency Platform API.  
2. **Storage & flexibility:** add battery or pumped-hydro dispatch module.  
3. **Policy scenarios:** vary carbon price (0–120 €/t) to test sensitivity.  
4. **Fuel-price shock analysis:** simulate gas/coal cost shifts.  
5. **Investment study:** expand capacities based on profitability.  

---

## Conclusion  
The simulator reproduces realistic UK-style electricity-market behaviour:  
- **Average price ≈ €62 / MWh**  
- **Renewable share ≈ 67 %**  
- **Weekly CO₂ ≈ 0.33 Mt**

It provides a concise yet powerful demonstration of merit-order dispatch and the influence of carbon pricing — ideal for showcasing analytical and quantitative-modelling skills in the energy-consulting sector.
