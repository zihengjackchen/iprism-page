+++
author = "Ziheng (Jack) Chen"
title = "iPrism: Characterize and Mitigate Risk by Quantifying Change in Escape Routes"
date = "2024-04-11"
description = "Accepted DSN 2024"

series = ["Projects"]
toc = true

+++

<h4 style="text-align: center;">
  <a href="https://github.com/zihengjackchen/iPrism">Paper (in proceeding)</a> |
  <a href="https://github.com/zihengjackchen/iPrism">GitHub (WIP)</a> |
  <a href="https://zenodo.org/doi/10.5281/zenodo.10279653">Artifacts</a>
</h4>


> (Site Under Construction)  
> Things to add: more visualization, human intuition

## Problem Statement
In complex and dynamic real-world situations involving multiple actors, ensuring safety is a significant challenge. This complexity often leads to severe accidents. The current techniques for mitigating safety hazards are not effective because they do not guarantee accessible escape routes and do not specifically address actors contributing to hazards. As a result, these techniques may not provide timely responses. 

To overcome these limitations, we propose a new measure called the safety-threat indicator (STI). This metric helps identify crucial actors by incorporating defensive driving techniques through counterfactual reasoning. We utilize STI to analyze real-world datasets, revealing inherent biases towards safe scenarios. Additionally, we employ it to develop a hazard mitigation policy using reinforcement learning. 

Our approach demonstrates a substantial reduction in the accident rate for advanced autonomous vehicle agents in rare hazardous scenarios—up to a 77% improvement over current state-of-the-art methods. 


## Quantifying Risk
![risk](/iPrism/intro.jpg)


### Algorithm
{{< highlight python >}}
# Reach: Compute escape routes using reach-tubes
# Inputs: M (model), X(t:t+k) (state over time), 
#         x^ego_t (ego state at time t)
# Control Constants: [a_min, a_max], [phi_min, phi_max]
# Constants: Δt (time increment), k (time horizon), 
#            N (number of samples)
# Output: T(t:t+k) (trajectory over time)

# Initialization
initCondDict = {}
initCondDict[t] = {x^ego_{t+Δt}}

# Compute reach-tube over the time horizon from t to t+k
for Δt in range(t, t+k):
    for x^ego_t in initCondDict[t]:
        sample_count = 0
        while sample_count < N:
            # Uniformly sample control inputs 
            # (acceleration a and steering angle phi)
            a, phi = sample_uniformly([a_min, a_max], 
                        [phi_min, phi_max])

            # Compute next state using the Bicycle Model
            x^ego_{t+Δt} = BicycleModel(a, phi, Δt)

            # Check for collision and boundary conditions
            if no_collision(x^ego_{t+Δt}, X_{t:t+Δt}) \
                and within_boundaries(x^ego_{t+Δt}, M):
                # Add to initial conditions if valid
                initCondDict[t+Δt].add(x^ego_{t+Δt})

            sample_count += 1

    t += Δt

# Generate bounded reach tube
T(t:t+k) = BoundedReachTube(initCondDict)

return T(t:t+k)
{{< /highlight >}}


### Results
### Comparison with other Risk Metrics
**Table:** Comparative analysis of Lead-Time-for-Mitigating-Accident (LTFMA) in seconds across various risk metrics. PKL-All: trained on all scenarios. PKL-Holdout: trained on all scenarios except the *ghost cut-in* and the *lead cut-in* scenarios.

| **Metric**        | **Ghost Cut-In**   | **Lead Cut-In**    | **Lead Slowdown**      | **Rear-End**         | **All Scenarios** |
| ----------------- | ------------------ | ------------------ | ---------------------- | -------------------- | ----------------- |
| TTC               | 0.00 (0.00)        | 0.00 (0.00)        | 3.30 (0.89)            | 0.02 (0.17)          | 0.83              |
| Dist. CIPA        | 0.00 (0.00)        | 0.00 (0.00)        | **5.50 (0.89)**        | 0.02 (0.17)          | 1.38              |
| PKL-All           | 0.75 (0.30)        | 1.01 (0.76)        | 1.22 (0.62)            | 0.01 (0.12)          | 0.75              |
| PKL-Holdout       | 0.14 (0.21)        | 3.36 (4.18)        | 1.23 (0.69)            | 0.01 (0.12)          | 1.19              |
| **STI (ours)**    | **2.94 (0.33)**    | **8.37 (0.70)**    | 2.22 (0.23)            | **1.23 (0.11)**      | **3.69**          |

**Notes:**
- We used *LBC agent* as the ADS to control the ego actor to obtain these results.
- In the front accident scenario, the ego actor's ADS (*LBC agent*) avoided the accident, resulting in no LTFMA metric to report.
- *SD* stands for *standard deviation*.


## Mitigating Risk
**Table:** Comparative analysis of agents' accident prevention rates across scenarios.

The agents are compared for the following reasons:
- **LBC+controller w/ STI (LBC+system):** To show improvement over baseline agent.
- **LBC+controller w/o STI:** To show that score is important (ablation study).
- **LBC+TTC-based (ACA):** To show improvement with respect to ACA techniques.
- **RIP+controller w/ STI (RIP+system):** To show generalization with other agents.

| **Scenario**        | **Metric**          | **LBC+SMC w/ STI** | **LBC+SMC w/o STI** | **LBC+SMC w/ACA** | **RIP+SMC w/ STI** |
|---------------------|---------------------|---------------------------|----------------------------|-------------------------|---------------------------|
| **Ghost Cut-In**    | CA↑ (%)             | 49%                       | 1%                         | 0%                       | 86%                       |
|                     | TCR↓ (%)            | 26.7%                     | 51.6%                      | 51.9%                    | 6.5%                      |
|                     | CA↑ (#)             | 252                       | 3                          | 0                        | 413                       |
|                     | TAS (#)             | 519                       | 519                        | 519                      | 478                       |
| **Lead Cut-In**     | CA↑ (%)             | 98%                       | 2%                         | 0%                       | 61%                       |
|                     | TCR↓ (%)            | 0.3%                      | 16.7%                      | 17.0%                    | 26.5%                     |
|                     | CA↑ (#)             | 167                       | 3                          | 0                        | 406                       |
|                     | TAS (#)             | 170                       | 170                        | 170                      | 671                       |
| **Lead Slowdown**   | CA↑ (%)             | 87%                       | 86%                        | 92%                      | 71%                       |
|                     | TCR↓ (%)            | 1.5%                      | 1.6%                       | 1.0%                     | 12.9%                     |
|                     | CA↑ (#)             | 103                       | 102                        | 108                      | 311                       |
|                     | TAS (#)             | 118                       | 118                        | 118                      | 440                       |

**Notes:**
- **CA#** stands for collision avoided (higher is better ↑); 
- **CA%** stands for the percentage of accident scenarios prevented by the mitigation strategy, calculated as `CA (%) = (CA (#) / TAS (#)) × 100`.
- **TCR** stands for total collision rate (lower is better ↓); **ACA** stands for automatic collision avoidance.
- 1000 scenario instances were executed for each scenario and for each baseline agent.
- **TAS** represents the total number of accident scenarios, capturing the accidents experienced by the agents.
- **TCR** is calculated as `TCR (%) = ((TAS(#) - CA(#)) / 1000) × 100`.



### Implementation
Please refer to [GitHub (WIP)](https://github.com/zihengjackchen/iPrism).



## Affiliations 
- Shengkun Cui
- Saurabh Jha
- Ziheng Chen
- DEPEND