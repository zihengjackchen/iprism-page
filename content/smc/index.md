+++
author = "Ziheng (Jack) Chen"
title = "iPrism: Characterize and Mitigate Risk by Quantifying Change in Escape Routes"
description = "Accepted DSN 2024"
series = ["Projects"]
toc = false

+++

## Mitigating Risk
### SMC in Action
We constantly assess the risk of the traffic and train an SMC using reinforcement learning. The SMC mitigates the imminent accident by braking based on the scene risk. 

{{< video src="accident" autoplay="true" loop="true">}}
{{< video src="mitigated" autoplay="true" loop="true">}}

In this example, without SMC intervention, an accident occurs when another actor aggressively cuts in. The SMC successfully mitigates the accident by braking when the risk is high. 


The above scenario is the ghost cut-in scenario #225 with parameters `{"distance_same_lane": 12, "distance_lane_change": 8, "speed_lane_change": 14}` and can be reproduced. 


### Results
| Agent                                             | Reasons for Comparison                                               | Ghost cut-in CA% | Lead cut-in CA% | Lead slowdown CA% |
|---------------------------------------------------|----------------------------------------------------------------------|------------------|-----------------|-------------------|
| **LBC+SMC w/ STI (LBC+system)**            | To show improvement over baseline agent.                             | 49%              | 98%             | 87%               |
| LBC+SMC w/o STI                            | To show that score is important (ablation study).                    | 1%               | 2%              | 86%               |
| LBC+TTC-based (ACA)                               | To show improvement w.r.t. ACA techniques.                           | 0%               | 0%              | 92%               |
| **RIP+SMC w/ STI (RIP+system)**            | To show generalization with other agents.                            | 86%              | 61%             | 71%               |

The above table shows the effectiveness of SMC in mitigating accidents comparing to other state-of-the-art methods. **CA%** stands for the percentage of accident scenarios (shown in TAS) prevented by the mitigation strategy. 

SMC significantly enhances safety, reducing accident occurrences by 37% to 98% compared to a baseline Learning-by-Cheating (LBC) agent. Additionally, SMC achieves up to 72.7% accident prevention when compared to state-of-the-art safety hazard mitigation agents.

