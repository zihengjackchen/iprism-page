+++
author = "Ziheng (Jack) Chen"
title = "OTA"
date = "2024-04-11"
description = "Out-of-Distribution Testing Framework for Autonomous Vehicles"
toc = true

+++

> (Site Under Construction)

<h4 style="text-align: center;">
  <a href="https://github.com/zihengjackchen/OTA">GitHub (WIP)</a>
</h4>

## Introduction
### Problem Statement
In the rapidly advancing field of autonomous vehicle (AV) technology, ensuring the safety and reliability of these systems is paramount. Traditional AV datasets provide a wealth of scenarios for training and testing AV systems under normal conditions, but they fall short when it comes to out-of-distribution (OOD) scenarios—situations that the vehicle might never have encountered during training but could potentially face on the road.

This project introduces a robust OOD Data Generation Pipeline specifically designed for CARLA, an open-source simulator for autonomous driving research. Our pipeline focuses on systematically generating and modifying safety-critical scenarios provided by the National Highway Traffic Safety Administration (NHTSA) to include OOD conditions. This allows us to explore the limits of AV systems in a controlled yet challenging environment.

### Result
### Selected Scenarios
![risk](/OTA/scenarios.jpg)
- scen_front_accident.json
- scen_ghost_cutin.json
- scen_ghost_cutin_curved.json
- scen_lead_cutin.json
- scen_lead_slowdown.json

### Results
Created 519, 170, 118 accidents out of 1000 scenarios in `ghost_cutin`, `lead_cutin`, and `lead_slowdown` scenarios.



## Related Work
