# 🏙️ The Effect of NO₂ Emissions on Rental Prices in Dresden

![R](https://img.shields.io/badge/R-276DC3?style=for-the-badge&logo=r&logoColor=white)
![QGIS](https://img.shields.io/badge/QGIS-589632?style=for-the-badge&logo=qgis&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)
![Year](https://img.shields.io/badge/Year-2024-blue?style=for-the-badge)

---

## 📌 Overview

This project investigates whether **NO₂ air pollution levels** have a statistically significant effect on **apartment rental prices** in Dresden, Germany. Using a combination of **geospatial analysis** and **OLS regression models**, the study draws on real estate listings and satellite-based emission data from 2015.

> *Does living in a more polluted area mean paying lower rent — or does it not matter in a low-emission city like Dresden?*

---

## ❓ Research Questions

- Is there a significant relationship between NO₂ emissions and rental prices in Dresden?
- How does **proximity to the main train station (HBF)** affect rental prices?
- What other apartment-level characteristics mediate the effect of air pollution on rent?

### Hypotheses
| | Hypothesis |
|---|---|
| **H₀** | No significant relationship between NO₂ emissions and housing prices due to low emission levels |
| **H₁** | Significant negative relationship between distance to city center and rental prices |

---

## 📦 Data Sources

| Dataset | Source | Description |
|---------|--------|-------------|
| Apartment listings | ImmobilienScout24 (2012–2015) | Rent prices, size, rooms, floor, amenities |
| NO₂ raster data | Global Surface NO₂ Concentrations (Figshare) | Satellite-based emission levels, 1km resolution |
| District boundaries | OpenData Dresden | Vector shapefiles for spatial joins |

**Final dataset:** 7,160 observations after filtering for Dresden, year 2015

**Key variables:**
- `Kaltmiete` — Cold rent price (€)
- `Wohnflaech` — Living space (m²)
- `Zimmer` — Number of rooms
- `Etage` — Floor level
- `X2015_final_1km` — NO₂ emission level (µg/m³)
- Distance to HBF (calculated via spatial computation)

---

## 🗺️ Geospatial Analysis

Spatial processing was conducted in **R** using `sf`, `terra`, and `ggplot2`:

- Loaded NO₂ **raster data** and Dresden district **vector boundaries**
- Performed **spatial joins** to assign emission values to each apartment
- Calculated **distance from each apartment to Dresden HBF** using coordinate geometry
- Generated **thematic maps** overlaying apartments, HBF location, and NO₂ levels

---

## 📐 Econometric Models

### Model 1 — NO₂ as Continuous Variable (log-log)

$$\log(\text{Rent}) = \beta_0 + \beta_1 \log(\text{Distance to HBF}) + \beta_2 \log(\text{Size}) + \beta_3(\text{Kitchen}) + \beta_4(\text{Rooms}) + \beta_5(\text{Floor}) + \beta_6(\text{Garden}) + \beta_7(\text{Barrier-free}) + \beta_8(\text{Basement}) + \beta_9(\text{Balcony}) + \beta_{10}(\text{Elevator}) + \beta_{11} \log(\text{NO}_2) + \epsilon$$

### Model 2 — NO₂ as Categorical Variable (High/Low)

Same specification, but NO₂ split into **"Low"** and **"High"** categories based on column mean.

---

## 📊 Key Results

| Variable | Model 1 | Model 2 |
|----------|---------|---------|
| log(Distance to HBF) | −0.084*** | −0.054*** |
| log(Living Space) | 0.960*** | 0.957*** |
| Built-in Kitchen | 0.113*** | 0.114*** |
| Barrier-free Access | 0.214*** | 0.212*** |
| log(NO₂) | **−0.120***` | — |
| NO₂ High Category | — | 0.013** |
| **R²** | **0.830** | **0.830** |
| Observations | 7,160 | 7,160 |

*Significance: \*p<0.1, \*\*p<0.05, \*\*\*p<0.01*

---

## 💡 Key Findings

- **NO₂ as a continuous variable** has a statistically significant **negative effect** on rent prices (β = −0.120, p<0.01) — higher pollution → lower rent
- **NO₂ as a category** shows a very small and positive effect, suggesting the binary split loses precision
- **Living space** is the strongest predictor of rent (β ≈ 0.96)
- **Distance to HBF** significantly reduces rent — apartments farther from the city center are cheaper
- Dresden's **generally low emission levels** (5.7–16.8 µg/m³) limit the practical magnitude of NO₂ effects

---

## 🛠️ Tools & Packages

```r
# Spatial analysis
library(sf)
library(terra)
library(ggplot2)

# Data manipulation
library(tidyverse)
library(tibble)

# Econometrics
library(fixest)
library(stargazer)
```


---

## 👤 Author

**Elvin Mammadov**
M.Sc. Public & International Economics — TU Dresden
📧 elvin.mammadov@mailbox.tu-dresden.de
🔗 [LinkedIn](https://www.linkedin.com/in/elvin-mammadov-182949397)
🔗 [GitHub](https://github.com/mammadov-elvin)

---

## 📚 References

- Anselin, L. (2001). Spatial econometrics. *A Companion to Theoretical Econometrics*
- Ball, M. J. (1973). Determinants of relative house prices. *Urban Studies*
- OpenData Dresden — Air Pollutants Data
- Global Surface NO₂ Concentrations (1990–2020), Figshare
