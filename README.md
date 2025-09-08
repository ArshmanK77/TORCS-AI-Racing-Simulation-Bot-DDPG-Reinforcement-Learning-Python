# TORCS-AI-Racing-Simulation-Bot-DDPG-Reinforcement-Learning-Python

## 📌 Overview
This project implements an **autonomous racing controller** for the [TORCS (The Open Racing Car Simulator)](http://torcs.sourceforge.net/), leveraging **Deep Reinforcement Learning (DRL)**.  
The controller is trained using the **Deep Deterministic Policy Gradient (DDPG)** algorithm to navigate complex tracks, avoid collisions, and optimize racing performance.  

The solution is **non-rule-based** and relies purely on reinforcement learning. It uses a **Python client** that communicates with the TORCS server over **UDP**, processing telemetry data and outputting continuous control signals for steering, acceleration, and braking.

---

## 🚀 Features
- **Deep Reinforcement Learning (DDPG)**
  - Actor-Critic architecture with TensorFlow/Keras.
  - Handles continuous control of steering, acceleration, and braking.
- **Custom TORCS Environment**
  - Built on top of Gym (`race_environment.py`).
  - Normalized state preprocessing for stable learning.
  - Recovery logic with reverse gear when stuck.
- **Reward Function**
  - Encourages speed, track adherence, and safe driving.
  - Penalizes collisions and off-track deviations.
  - Provides bonuses for alignment and recovery.
- **Training Enhancements**
  - Replay buffer (100k capacity).
  - Ornstein-Uhlenbeck noise for exploration.
  - Action smoothing to reduce jitter.

---

## 📂 Project Structure
├── ddpg.py # Implementation of DDPG (actor, critic, replay buffer, training loop)
├── race_environment.py # Custom Gym environment for TORCS, reward shaping, state processing
├── Main_Driver.py # Handles UDP communication with TORCS server (template provided)
└── README.md # Project documentation


---

## ⚙️ Methodology
1. **Environment Setup**
   - Telemetry data from TORCS (track sensors, speed, RPM, opponent distances).
   - State normalization and derived features (track curvature, opponent proximity).
   - Recovery logic via reverse gear.

2. **DDPG Training**
   - Actor network (2 hidden layers, 64 & 32 units).
   - Critic network for Q-value estimation.
   - Batch normalization for stability.
   - Soft target updates (`τ = 0.001`), γ = 0.99.

3. **Reward Design**
   - **+ve** for progress, alignment, recovery.
   - **-ve** for collisions, going off-track, straying too far.

---

## 📊 Results
- Completed laps with **minimal collisions**.
- Maintained stable speeds (50–150 km/h) with smooth gear transitions.
- Successfully avoided opponents (>10 units safe distance).
- Reverse gear improved recovery from stuck states.
- Average reward stabilized around **200–300 per episode** after ~2000 episodes.

---

## 🔮 Future Improvements
- Incorporate **vision-based inputs** for richer state representation.
- Explore **multi-agent training** for competitive racing.
- Experiment with alternative algorithms like **Proximal Policy Optimization (PPO)**.

---

## 🛠️ Installation & Setup

### Prerequisites
- Python 3.8+
- [TORCS](http://cs.adelaide.edu.au/~optlog/SCR2015/index.html) with SCR patch
- Required Python libraries:
  ```bash
  pip install tensorflow keras gym numpy
Running the Project

Launch TORCS with the server configuration (src_server).

Start the Python client:

python Main_Driver.py


Train the controller:

python ddpg.py


Run races and visualize results.

👨‍💻 Authors

Arshman Khawar (22I-2427)

Course Project for Artificial Intelligence (AI-2002) — May 2025.
