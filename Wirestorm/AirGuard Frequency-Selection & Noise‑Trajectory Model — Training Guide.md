
---

## 1  Purpose

This document aggregates **all telemetry, spatial, and policy metrics** plus a **distributed noise‑trajectory workflow** to guide data‑engineering and ML‑model design for AirGuard’s self‑healing, frequency‑optimising mesh network.

---

## 2  Comprehensive Metric Catalogue

### 2.1  Local Radio Metrics (per AP)

|Metric|Description|
|---|---|
|RSSI|Received Signal Strength Indicator from nearby APs|
|SNR|Signal‑to‑Noise Ratio on current & candidate channels|
|Noise Floor|Background RF noise baseline|
|Channel Utilisation %|Airtime consumption on each channel|
|Beacon / Data‑Packet Rate|Load estimate of co‑channel APs|
|Retry Rate / BER|Packet retransmissions / bit errors|
|Client Count per Channel|Current load & density|
|Connected‑Client RSSI Spread|Coverage quality indicator|
|Channel History / Occupancy Trend|Long‑term congestion stability|

### 2.2  Geospatial & Orientation Metrics

| Metric                       | Description                                     |
| ---------------------------- | ----------------------------------------------- |
| GPS (lat, lon, alt)          | Physical device position                        |
| Gyroscope (pitch, roll, yaw) | Antenna/device orientation                      |
| Relative Bearing to Peers    | Used for directional LoS & interference vectors |
| Antenna Height AGL           | Fresnel‑zone clearance & obstruction checks     |

### 2.3  Remote‑Node (LoRaWAN) Metrics

| Metric                   | Description                           |
| ------------------------ | ------------------------------------- |
| Channel Currently Used   | Active frequency at remote node       |
| Remote Noise Level       | Detected interference strength        |
| Remote GPS + Orientation | Inputs for 3‑D noise‑vector modelling |
| Remote Device Health     | Validity flag for remote data         |
| Last Report Timestamp    | Staleness filter for incoming metrics |

### 2.4  Elevation & Line‑of‑Sight Metrics

|Metric|Description|
|---|---|
|Elevation Profile (AP‑to‑AP)|Terrain samples for LoS vector|
|Obstruction Score|% of path obstructed by terrain|
|Line‑of‑Sight Validity|Clear vs blocked indicator|
|Fresnel Zone Clearance|Diffraction‑risk assessment|

### 2.5  Regulatory & Policy Constraints

|Metric|Description|
|---|---|
|Country/Region Code (GPS‑derived)|Determines legal channel set|
|Allowed 5 GHz Channel List|Region‑specific plan|
|DFS Requirement|Radar‑vacate timer applicability|
|User Opt‑Out Flag|Compliance override (logged)|

### 2.6  System Intelligence & Historical Metrics

|Metric|Description|
|---|---|
|Last Frequency‑Change Impact|ΔPerformance after last switch|
|Switch Frequency Per Channel|Anti‑flap back‑off counter|
|Event‑Correlation Confidence|P(noise source = remote)|
|Predicted Load Growth|Forecast of client demand|

### 2.7  Application & QoS Aware (Optional)

|Metric|Description|
|---|---|
|App Type (Stream, VoIP, …)|Latency‑critical classification|
|QoS Violations|Dropped/lag incidents per channel|

---

## 3  Distributed Noise‑Trajectory Analysis Workflow

1. **Noise Vector Generation**  
    • Source AirGuard detects interference → emits `{channel, origin, heading, intensity}`.
    
2. **Trajectory Projection** (receivers)  
    • Compute great‑circle path ±θ using Haversine + heading.  
    • Estimate path‑loss vs distance & antenna angle.
    
3. **LoS & Elevation Validation**  
    • Pull DEM/elevation samples (Google Elevation API or offline SRTM).  
    • Calculate obstruction & Fresnel clearance.
    
4. **Impact Decision**  
    • If receiver lies inside cone **and** path‑loss < threshold → apply noise penalty.  
    • Else noise excluded from channel scoring.
    
5. **Feedback Loop**  
    • Store event for Bayesian update of Event‑Correlation Confidence.
    

---

## 4  Model‑Training Guidance

- **Feature Engineering**: Combine raw metrics into higher‑level features (e.g., _ChannelStress_ = Utilisation·ClientCount / SNR).
    
- **Label Strategy**: Use `best_channel` decisions (post‑event success) as supervised labels; OR train RL agent with reward = throughput gain − switching cost.
    
- **Spatial Features**: Encode GPS as geohash; encode bearing as sine/cosine pairs to retain cyclicity.
    
- **Regulatory Mask**: Provide legal‑channel bitmap as static feature to model so compliance is built‑in.
    
- **Temporal Features**: Rolling averages & time‑of‑day indicators to capture diurnal interference patterns.
    
- **Elevation‑Aware Features**: Include Obstruction Score & LoS validity for each remote‑noise vector.
    

---

## 5  Additional Ideas to Boost Accuracy & Efficacy

1. **Graph‑Neural‑Network Layer**  
    Model mesh APs as nodes; edges carry interference probability → GNN predicts optimal global channel plan.
    
2. **Confidence‑Weighted Ensemble**  
    Blend rule‑based heuristics with ML predictions; weight by Event‑Correlation Confidence.
    
3. **Online Learning Loop**  
    Continuously retrain lightweight model on sliding window of recent events to adapt to evolving RF environment.
    
4. **Bayesian Noise Heatmap**  
    Maintain probabilistic spatial map of interference; feed as prior to model for faster convergence.
    
5. **Multi‑Objective Optimisation**  
    Optimise not just throughput but also **energy consumption** (Tx‑power) and **regulatory penalties**; use Pareto frontier.
    
6. **Explainability Module**  
    Log SHAP or feature attribution for each switch‑decision to build operator trust and accelerate debugging.
    

---

**End of Document**