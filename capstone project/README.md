
# Dynamic Pricing for Urban Parking Lots

This repository implements a real-time, data-driven pricing engine for urban parking lots using Pathway and Bokeh, as part of the Summer Analytics 2025 capstone project hosted by the Consulting & Analytics Club and Pathway.

## Project Overview

The goal is to dynamically price 14 parking lots based on real-time indicators such as:

- Occupancy rate
- Queue length
- Nearby traffic congestion
- Vehicle type
- Special days (events or holidays)
- Competitor pricing (in the advanced model)

Data is streamed live using Pathway, processed in real time, and visualized using Bokeh dashboards.

## Models Implemented

### Model 1: Baseline Linear Pricing

A simple linear model that increases price proportionally with occupancy rate:

```
Price = BasePrice + α * (Occupancy / Capacity)
```

- Static formula
- Per-lot daily aggregation
- Smooth but limited adaptability

### Model 2: Demand-Based Pricing

Incorporates multiple features to estimate demand. Pricing adjusts according to the demand value computed as:

```
Demand = α * (Occupancy / Capacity)
       + β * QueueLength
       - γ * Traffic
       + δ * IsSpecialDay
       + ε * VehicleTypeWeight
```

Final price is then calculated as:

```
Price = BasePrice * (1 + λ * NormalizedDemand)
```

- Allows smarter pricing based on situational factors
- Demand is normalized per day and bounded between 0.5x and 2x the base price
- Vehicle type is weighted (e.g., cars > bikes)

### Model 3: Competitive Pricing (Final)

This model builds on Model 2 by considering nearby competitors using geospatial proximity:

- Uses Haversine distance between lots to identify competition
- If a lot is full and nearby ones are cheaper, price drops or rerouting is suggested
- If nearby lots are expensive, price is allowed to surge

Outputs include:

- Final dynamic price
- Reroute suggestion (True/False)

Pricing can now scale up to ₹25 in high-demand, competitive zones.

## Real-Time Architecture

- Data ingested using Pathway’s streaming CSV ingestion
- Each row is timestamped and parsed into a Pathway schema
- Pricing logic runs as a live computation graph
- Pathway’s `.plot()` feature is used for real-time interactive dashboards via Bokeh and Panel

## Visualizations

Each parking lot dashboard includes:

1. **Real-Time Price Line Plot**
   - Shows pricing evolution over time

2. **Competitor Comparison (optional)**
   - Own price vs. nearby lots

3. **Occupancy vs Capacity**
   - For visual reference of demand

4. **Reroute Indicator**
   - Timeline of when rerouting might have been triggered

All visualizations are auto-updating and integrated into the notebook environment.

## File Structure

```
📁 parking/                  # Input CSV chunks for streaming
📄 model1_baseline.ipynb     # Model 1: Linear baseline
📄 model2_demand.ipynb       # Model 2: Demand-based pricing
📄 model3_competitive.ipynb  # Model 3: Competitive pricing with rerouting
📄 dashboard.ipynb           # Final dashboard view
📄 Sample_Notebook.ipynb     # Provided starter reference
📄 README.md                 # Project overview
```

## How to Run (Google Colab)

1. Upload streaming CSV files to the `/parking/` directory
2. Open `dashboard.ipynb`
3. Run all cells; the Bokeh dashboard will auto-start
4. Pathway handles live streaming and visualization within the notebook (no `panel serve` required)

## Deliverables

- Three models: baseline, demand-based, competitive
- Real-time pricing engine with bounded prices
- Interactive dashboards for every lot
- Clean Colab notebooks with comments and plots
- Pricing logic and assumptions explained clearly

## Assumptions

- Vehicle type weight reflects the likelihood of higher willingness to pay
- Traffic reduces parking desirability
- Special days increase demand
- Nearby lots within 1 km are considered competitors
- Prices are bounded: minimum ₹5, maximum ₹25

## Credits

- Arushi Madhesiya – Pricing logic, real-time implementation
- C&A Club + Pathway – Dataset, starter notebook

## License

MIT License

## References

- Pathway Docs: https://pathway.com
- Bokeh & Panel Documentation
- Summer Analytics 2025: https://www.caciitg.com/sa/course25
