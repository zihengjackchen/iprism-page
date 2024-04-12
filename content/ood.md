+++
author = "Hugo Authors"
title = "OOD"
date = "2024-04-11"
description = "Out-of-Distribution Scenarios Testing Framework for Autonomous Vehicles (Generating Out-of-Distribution Autonomous Vehicles Scenarios)"


toc = true

+++

> (Site Under Construction)

<h4 style="text-align: center;">
  <a href="https://github.com/zihengjackchen/GoodAVs">GitHub</a>
</h4>


## Motivation
As autonomous vehicles (AVs) approach widespread adoption, their ability to handle unexpected, out-of-distribution (OOD) scenarios is critical. Most existing datasets focus on typical driving conditions, leaving a significant gap in AV training for rare but safety-critical events. This project introduces a suite of tools to address this gap, enhancing the robustness and dependability of AV systems.

## Contribution
1. OOD Data Generation Pipeline for CARLA: A novel pipeline that generates OOD scenarios by altering parameters in safety-critical scenarios defined by the National Highway Traffic Safety Administration (NHTSA), thus broadening the scope of AV training environments.
2. Publication of an OOD Dataset: We have released a dataset with 5000 traces from five critical scenarios, providing over 5000 minutes of vehicle trajectory data, which is crucial for developing and testing AV systems.
3. Online Testing Pipeline: An accompanying online tool that allows for the testing of collision-avoidance strategies within these generated scenarios, facilitating direct validation of AV technologies.
