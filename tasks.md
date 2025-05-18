### Final Project: End-to-End Planning for Autonomous Driving

---

#### 1. Introduction

End-to-end planning for autonomous driving is a rapidly growing area in intelligent transportation systems. The goal is to design a model that can directly map raw sensor inputs to vehicle control commands. Unlike modular pipelines, this approach simplifies the decision-making stack and has shown great promise in improving robustness and real-time adaptability.

This project invites you to build an end-to-end deep learning-based planner that outputs future trajectories for a self-driving car using sensor data. The development will proceed in three progressive phases, each deepening your understanding and expanding your model's capabilities.

---

#### 2. Dataset Overview

We use a curated subset of the [nuPlan dataset](https://www.nuscenes.org/nuplan) with simulated sensors, the world’s first large-scale planning benchmark for autonomous vehicles.

- **Training Set**: 5,000 samples
- **Validation Set**: 1,000 samples

##### Data Format
Each data point is structured as a Python dictionary with the following keys:

```python
{
    'camera': ndarray (200, 300, 3),               # RGB image simulated at time step 21 
    'depth': ndarray (200, 300, 1),                # Depth image at time step 21
    'driving_command': str,                        # One of ['forward', 'left', 'right']
    'sdc_history_feature': ndarray (21, 3),        # [x, y, heading] from past 21 steps
    'sdc_future_feature': ndarray (60, 3),         # [x, y, heading] for the next 60 steps
    'semantic_label': ndarray (200, 300)           # Semantic segmentation map
}
```

- All coordinates are ego-rotated to the vehicle's local frame at time step 21.
- Only `'camera'`, `'driving_command'`, and `'sdc_history_feature'` are allowed as inputs for **Milestone 1**.

##### Semantic Colormap
```python
semantic_colormap = {
    0: (0, 0, 0),         # UNLABELED
    1: (0, 0, 142),       # CAR
    2: (0, 0, 70),        # TRUCK
    3: (220, 20, 60),     # PEDESTRIAN
    4: (119, 11, 32),     # BIKE
    5: (152, 251, 152),   # TERRAIN
    6: (128, 64, 128),    # ROAD
    7: (244, 35, 232),    # SIDEWALK
    8: (70, 130, 180),    # SKY
    9: (250, 170, 30),    # TRAFFIC_LIGHT
    10: (190, 153, 153),  # FENCE
    11: (220, 220, 0),    # TRAFFIC_SIGN
    12: (255, 255, 255),  # LANE_LINE
    13: (55, 176, 189),   # CROSSWALK
    14: (0, 60, 100)      # BUS
}
```

---

#### 3. Related Work & Baselines

You are encouraged to study the following SOTA papers to inspire your method development:

1. [**End-to-end Autonomous Driving: Challenges and Frontiers**](https://arxiv.org/abs/2306.16927) – A comprehensive survey of challenges and recent trends.
2. [**TransFuser**](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=9863660) – Sensor fusion with Transformers for planning.
3. [**Planning-Oriented Autonomous Driving**](https://arxiv.org/abs/2212.10156) – CVPR 2023 Best Paper, rethinking the role of planning in end-to-end models.

---

#### 4. Evaluation Metrics

We use **Average Displacement Error (ADE)**:

- Defined as the mean Euclidean distance between predicted and ground-truth future trajectories.
- Lower ADE values indicate better performance.

---

#### 5. Logistics

- **Group Size**: Max 2 students per group
- **Compute Resources**: Access to SCITAS (EPFL GPU cluster)

---

### Milestones

---

#### Milestone 1: Basic End-to-End Planner
**Deadline**: 2025-05-02, 11:59 PM

Implement an end-to-end model that predicts future trajectories based on:

- Camera RGB image
- Driving command
- Vehicle’s motion history (sdc_history_feature)

👉 **Reference**: [Colab Starter Code](https://colab.research.google.com/drive/16u0e_gKDLL4cPmCxYaRqdxp9bWKj-Buv?usp=sharing)

##### Deliverables
- [**80%**] Kaggle Submission: Submit to [Kaggle Leaderboard](https://www.kaggle.com/t/338eec1b2cd346eaa3b569340ab2de19)
  - **Full score**: ADE < 2
  - **Zero score**: ADE > 4
  - **Formula**: `Score = 100 * (4 - ADE) / 2`

- [**20%**] Code Submission:
  - Push to GitHub under the **VITA student projects** org
  - Ask your TA to add your GitHub account
  - Include a README that explains structure and how to run/train/infer

✅ **Tips**:
- Use SCITAS if Colab is slow
- Stick strictly to the allowed input modalities

---

#### Milestone 2: Perception-Aware Planning
**Release**: 2025-04-25  
**Deadline**: 2025-05-16, 11:59 PM

Enhance your model by introducing perception-based auxiliary tasks (e.g., semantic segmentation, depth estimation).


👉 **Reference**: [Colab Starter Code](https://colab.research.google.com/drive/1Jpr07VUO_ZjF2YOuxr9lKO3U9jvZ6q2-?usp=sharing)

##### Deliverables & Grading

- [**80%**] Kaggle Submission: Submit to [Kaggle Leaderboard](https://www.kaggle.com/t/a7295af1cfa349eeb57d08538d44cf58)
  - **Full score**: ADE < 1.60
  - **Zero score**: ADE > 2.00
  - **1.5 < ADE < 2.0:** `Score = 250 * (2.0 - ADE) `

[//]: # (  - **Formula**: `Score = 100 * &#40;4 - ADE&#41; / 2`)

[//]: # ()
- [**20%**] Code Submission:

  - Push to GitHub under the **[VITA student projects](https://github.com/vita-student-projects)** (Ask your TA to add your GitHub account)

  - Include a README that explains the structure and how to run/train/infer

---

#### Milestone 3: Sim-to-Real Generalization
**Release**: 2025-05-09  
**Deadline**: 2025-05-23, 11:59 PM

Evaluate your planner in challenging real-world domains. Details can be found here: [Colab Starter Code](https://colab.research.google.com/drive/1apQZtbgvS2lUxp0khlbBTeq_jfyipL-d?usp=sharing). Kaggle submissions will be released soon.

Note: 
1. There is no depth label and semantic label in phase 3.
2. Try to use data augmentation to improve the performance.


- [**80%**] Kaggle Submission: Submit to [Kaggle Leaderboard](https://www.kaggle.com/t/802ad533a6d2477e91fd72c9a030ff15)
  - **Full score**: ADE < 1.8
  - **Zero score**: ADE > 2
  - **Formula**: `Score = 100 * (2 - ade) / 0.2`


- [**20%**] Code Submission:

  - Push to GitHub under the **[VITA student projects](https://github.com/vita-student-projects)** (Ask your TA to add your GitHub account)

  - Include a README that explains the structure and how to run/train/infer
---

#### Bonus: Submission to the [NAVSIM Leaderboard](https://huggingface.co/spaces/AGC2024-P/e2e-driving-navsim)
**Release**: 2025-05-09   
**Deadline**: TBD

**Note**: This is a bonus task and is not mandatory.

Are you confident in your model's performance? Submit to the NAVSIM leaderboard and see how it stacks up against other SOTA models!

Contact Lan for more details (lan.feng@epfl.ch) if you are interested in this task.

---

### Final Grading Breakdown

- **Milestone 1**: 30%
- **Milestone 2**: 30%
- **Milestone 3**: 40%

---

Good luck, and have fun driving into the future! 🚗💨

