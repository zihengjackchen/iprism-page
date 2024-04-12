+++
author = "Hugo Authors"
title = "iPrism: Characterize and Mitigate Risk by Quantifying Change in Escape Routes"
date = "2024-04-11"
description = "Accepted DSN 2024"

series = ["Projects"]
toc = true

+++

<h4 style="text-align: center;">
  <a href="https://github.com/zihengjackchen/iPrism">Paper (in proceeding)</a> |
  <a href="https://github.com/zihengjackchen/iPrism">GitHub</a> |
  <a href="https://zenodo.org/doi/10.5281/zenodo.10279653">Artifacts</a>
</h4>


> (Site Under Construction)

## Problem Statement
In complex and dynamic real-world situations involving multiple actors, ensuring safety is a significant challenge. This complexity often leads to severe accidents. The current techniques for mitigating safety hazards are not effective because they do not guarantee accessible escape routes and do not specifically address actors contributing to hazards. As a result, these techniques may not provide timely responses. 

To overcome these limitations, we propose a new measure called the safety-threat indicator (STI). This metric helps identify crucial actors by incorporating defensive driving techniques through counterfactual reasoning. We utilize STI to analyze real-world datasets, revealing inherent biases towards safe scenarios. Additionally, we employ it to develop a hazard mitigation policy using reinforcement learning. 

Our approach demonstrates a substantial reduction in the accident rate for advanced autonomous vehicle agents in rare hazardous scenarios—up to a 77% improvement over current state-of-the-art methods. 


## Quantifying Risk

<object data="/iPrism/intro.pdf" type="application/pdf" width="700px" height="700px">
    <embed src="/iPrism/intro.pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="./iPrism/intro.pdf">Download PDF</a>.</p>
    </embed>
</object>


### Algorithm
{{< highlight python >}}
def compute_escape_routes(M, X, x_ego_t, constants, delta_t, k, N):
    """
    Compute escape routes using reach-tubes.

    :param M: The boundaries of the model
    :param X: The state of other entities from time t to t+k
    :param x_ego_t: The initial state of the ego
    :param constants: Control constants [a_min, a_max, phi_min, phi_max]
    :param delta_t: Time step
    :param k: Total number of time steps
    :param N: Number of samples
    :return: Reach tube from time t to t+k
    """
    a_min, a_max, phi_min, phi_max = constants
    init_cond_dict = {t: [x_ego_t]}

    # Simulation over time
    t = 0
    while t < k:
        next_cond_set = []
        for x_ego in init_cond_dict[t]:
            count = 0
            while count < N:
                # Uniformly sample control inputs within given bounds
                a = uniform(a_min, a_max)
                phi = uniform(phi_min, phi_max)

                # Compute the next state using the Bicycle Model
                x_ego_next = bicycle_model(x_ego, a, phi, delta_t)

                # Check collision and model boundaries
                if is_within_boundaries(x_ego_next, M) and not_collide(x_ego_next, X):
                    next_cond_set.append(x_ego_next)
                count += 1

        init_cond_dict[t + delta_t] = next_cond_set
        t += delta_t

    # Generate the bounded reach tube from the simulation results
    T = bounded_reach_tube(init_cond_dict)
    return T

{{< /highlight >}}


### Results
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

### Implementation

### Results
**Table:** Comparative analysis of agents' accident prevention rates across scenarios.

**Explanation of Agent Comparison:**
- **LBC+controller w/ STI (LBC+system):** To show improvement over baseline agent.
- **LBC+controller w/o STI:** To show that score is important (ablation study).
- **LBC+TTC-based (ACA):** To show improvement w.r.t. ACA techniques.
- **RIP+controller w/ STI (RIP+system):** To show generalization with other agents.

| **Agent**                              | **Ghost cut-in**                |          |           |         | **Lead cut-in**                 |          |           |         | **Lead slowdown**               |          |           |         |
|----------------------------------------|---------------------------------|----------|-----------|---------|---------------------------------|----------|-----------|---------|---------------------------------|----------|-----------|---------|
|                                        | **CA↑ (%)**                      | **TCR↓ (%)** | **CA↑ (#)**| **TAS** | **CA↑ (%)**                      | **TCR↓ (%)** | **CA↑ (#)**| **TAS** | **CA↑ (%)**                      | **TCR↓ (%))** | **CA↑ (#)**| **TAS** |
| **LBC+controller w/ STI (LBC+system)** | 49%                             | 26.7%    | 252       | 519     | 98%                             | 0.3%     | 167       | 170     | 87%                             | 1.5%     | 103       | 118     |
| LBC+controller w/o STI                 | 1%                              | 51.6%    | 3         | 519     | 2%                              | 16.7%    | 3         | 170     | 86%                             | 1.6%     | 102       | 118     |
| LBC+TTC-based (ACA)                    | 0%                              | 51.9%    | 0         | 519     | 0%                              | 17.0%    | 0         | 170     | 92%                             | 1.0%     | 108       | 118     |
| **RIP+controller w/ STI (RIP+system)** | 86%                             | 6.5%     | 413       | 478     | 61%                             | 26.5%    | 406       | 671     | 71%                             | 12.9%    | 311       | 440     |

**Notes:**
- **CA#** stands for collision avoided (higher is better ↑); 
- **CA%** stands for the percentage of accident scenarios prevented by the mitigation strategy, calculated as `CA (%) = (CA (#) / TAS (#)) × 100`.
- **TCR** stands for total collision rate (lower is better ↓); **ACA** stands for automatic collision avoidance.
- 1000 scenario instances were executed for each scenario and for each baseline agent.
- **TAS** represents the total number of accident scenarios, capturing the accidents experienced by the agents.
- **TCR** is calculated as `TCR (%) = ((TAS(#) - CA(#)) / 1000) × 100`.




## Authors: 
- Shengkun Cui
- Saurabh Jha
- Ziheng Chen
- DEPEND



## Other Elements — abbr, sub, sup, kbd, mark

<abbr title="Graphics Interchange Format">GIF</abbr> is a bitmap image format.

H<sub>2</sub>O

X<sup>n</sup> + Y<sup>n</sup> = Z<sup>n</sup>

Press <kbd>CTRL+ALT+Delete</kbd> to end the session.

Most <mark>salamanders</mark> are nocturnal, and hunt for insects, worms, and other small creatures.


