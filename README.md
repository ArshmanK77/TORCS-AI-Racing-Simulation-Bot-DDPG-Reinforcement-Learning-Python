# TORCS-AI-Racing-Simulation-Bot-DDPG-Reinforcement-Learning-Python

Overview
This project implements an autonomous racing controller for the TORCS simulator using a Deep Deterministic Policy Gradient (DDPG) algorithm in Python. The controller navigates complex tracks, avoids collisions with opponents, and recovers from stuck situations using a custom Gym environment and a tailored reward function. Key features include telemetry processing (track sensors, speed, opponent proximity), track curvature integration, and reverse gear logic for robust performance.
Features

DDPG Algorithm: Actor-critic model with batch normalization for stable learning in a continuous action space (steering, acceleration, braking).
Custom Gym Environment: Processes TORCS telemetry, including track curvature and opponent proximity, for enhanced decision-making.
Reward Function: Optimizes speed, track adherence, collision avoidance, and recovery (reverse gear when stuck).
Training Pipeline: Utilizes replay buffer and Ornstein-Uhlenbeck noise for efficient exploration and policy learning.

Prerequisites

TORCS: Install TORCS simulator (see installation guide).
Python: Version 3.8+.
Dependencies:pip install numpy tensorflow gym psutil


Operating System: Tested on Linux (Ubuntu recommended).

Installation

Clone the Repository:git clone https://github.com/your-username/torcs-autonomous-racing.git
cd torcs-autonomous-racing


Install Dependencies:pip install -r requirements.txt


Install TORCS:
Follow the TORCS installation guide.
Ensure the TORCS server is configured to use the scr_server module.


Configure Track:
Place track configuration files (e.g., standard.xml) in the track_configs/ directory.



Project Structure
torcs-autonomous-racing/
├── ddpg.py                 # DDPG algorithm implementation
├── torcs_env.py            # Custom Gym environment for TORCS
├── snakeoil3_gym.py        # TORCS server communication interface
├── track_configs/          # Track configuration files (e.g., standard.xml)
├── requirements.txt        # Python dependencies
└── README.md               # This file

Usage

Start TORCS Server:torcs -nofuel -nolaptime -nodamage


Run the Training:python ddpg.py


Training logs display actions, states, rewards, and recovery attempts.
Models are saved as actormodel.h5 and criticmodel.h5.


Monitor Performance:
Enable rendering in torcs_env.py (enable_render=True) to visualize the car’s behavior.
Check logs for metrics like speedX, trackPos, opponent_min, and reward.



Key Implementation Details

State Space: 30D vector including normalized track sensors, speed, angle, RPM, and track curvature.
Action Space: 3D continuous (steering [-1, 1], acceleration [0, 1], braking [0, 1]).
Reward Function:
Progress: speedX * cos(angle)
Penalties: -20 (off-track), -10 (collision), -5 * |trackPos| (straying)
Bonuses: +10 (reversing when stuck), +5 * cos(angle) (track alignment)


Recovery Logic: Activates reverse gear (gear=-1) and random steering when stuck (speedX < 5, off-track, or opponent proximity < 10).
Training: 2000 episodes, batch size 32, actor LR 0.0003, critic LR 0.002, with action smoothing (α=0.5).

Results

Speed: Maintains 50–150 km/h with smooth gear transitions.
Track Adherence: Keeps trackPos near 0, with effective recovery from off-track events.
Collision Avoidance: Maintains opponent distance >10 units in most scenarios.
Training: Achieves stable rewards (200–300 per episode) after 2000 episodes.

Challenges and Future Work

Challenges:
Initial instability in training (mitigated with batch normalization).
Delayed turns in sharp corners (partially addressed with curvature feature).


Future Improvements:
Integrate vision-based inputs for richer state information.
Explore multi-agent training for better opponent handling.
Experiment with PPO or SAC for stabler learning.



Contributing
Contributions are welcome! To contribute:

Fork the repository.
Create a feature branch (git checkout -b feature-name).
Commit changes (git commit -m "Add feature").
Push to the branch (git push origin feature-name).
Open a pull request.

License
This project is licensed under the MIT License. See the LICENSE file for details.
Acknowledgments

TORCS for the racing simulator.
OpenAI Gym for the environment framework.
Inspired by reinforcement learning resources and TORCS tutorials.
