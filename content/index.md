+++
author = "Ziheng (Jack) Chen"
title = "iPrism: Characterize and Mitigate Risk by Quantifying Change in Escape Routes"
description = "Accepted DSN 2024"
series = ["Projects"]
toc = false

+++

<!-- ## Authors 
<p style="text-align: center;">
  <a href="https://www.linkedin.com/in/shengkun-cui-332353136/">Shengkun Cui</a> |
  <a href="https://saurabhjha.one/">Saurabh Jha</a> |
  <a href="https://zihengjackchen.com">Ziheng Chen</a> |
  <a href="https://ece.illinois.edu/about/directory/faculty/kalbarcz">Zbigniew Kalbarczyk</a> | 
  <a href="https://ece.illinois.edu/about/directory/faculty/rkiyer">Ravishankar Iyer</a>
</p> -->

## Problem Statement
In complex and dynamic real-world situations involving multiple actors, ensuring safety is a significant challenge. This complexity often leads to severe accidents. We introduce a novel risk metric called the Safety-Threat Indicator (STI), which is inspired by the proactive strategies of experienced drivers to circumvent hazards by evaluating changes in available escape routes for the AV. STI outperforms the state-of-the-art heuristic and data-driven techniques by 2.7 ~ 4.9 times. To effectively reduce the risks quantified by STI and prevent accidents, we also developed a reinforcement learning-based Safety-hazard Mitigation Controller (SMC). This controller learns optimal policies for risk reduction and accident avoidance. Our approach demonstrates a substantial reduction in the accident rate for advanced autonomous vehicle agents in rare hazardous scenarios—up to a 77% improvement over current state-of-the-art methods. 

All resources associated with this study, including code, scenarios, and models, are available on [GitHub](https://github.com/zihengjackchen/iPrism).

## Motivating Example
![risk](intro.jpg)   
In a street scenario, an ego vehicle (AV) initially has multiple escape routes (highlighted in orange). However, when another vehicle abruptly cuts in front, these routes are blocked, increasing the risk of an accident. An illustration then shows the failure of the Autonomous Driving System (ADS) to respond, resulting in an accident and the loss of all escape routes. Finally, a depiction of the iPrism method, employing a reinforcement learning-based mitigation controller, demonstrates effective accident avoidance. By proactively braking, the ego vehicle opens new escape routes (in green), successfully circumventing the accident. This example highlights braking as the optimal action since changing lanes would eliminate the remaining escape routes.

