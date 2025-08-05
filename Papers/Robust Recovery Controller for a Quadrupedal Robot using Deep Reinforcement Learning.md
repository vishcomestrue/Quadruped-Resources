# 📄 Paper Summary
**Authors:** Joonho Lee, Jemin Hwangbo, and Marco Hutter
**Lab/Organization:** RSL, ETH Zurich
**Conference/Journal:** [Venue, Year]  
**Link:** https://www.arxiv.org/abs/1901.07517 
**Code:** [GitHub or other link, if available]

---

## 🔧 1. Experimental Setup
- **Simulator:** Custom, Raisim
- **Robot Model:** ANYmal
- **Hardware Deployment:** Yes
- **Robot Configuration:** 12 DoF, sensors: IMU, joint encoders, TSIF state estimators

---

## 🧠 2. Learning Algorithm
- **RL Algorithm:** TRPO+GAE
- **Model Type:** Model-free
- **Policy Type:** Feedforward
- **Special Techniques:** Domain randomization, sim2real
- **Online or Offline Learning:** Online

---

## 👁️ 3. Observation Space
- **State Variables Used:**
  - Joint positions: Yes
  - Joint velocities: Yes
  - Base pose/vel: Yes
  - Terrain info: No
  - Contact sensors: No
- **History/Memory:** History buffer 
- **Privileged Observations:** None

---

## 🎮 4. Action Space
- **Action Type:** Joint position targets
- **Action Frequency:** Different freqs. self-righting: 20Hz; standing up: 100Hz; locomotion: 200Hz; selector: 50Hz
- **Actuator Constraints:** Data driven SEA model (NN torque predictors)

---

## 🏆 5. Reward Function
- **Reward Components:**
  - Velocity tracking: Yes
  - Energy minimization: Yes
  - Base stability: Yes
  - Smooth motion: Yes
  - Other: Impulse/slippage penalties, self-collision avoidance, foot clearance/slip
- **Reward Shaping:** Heavy, Curriculum
---

## 🧱 6. Network Architecture
- **Policy Network:** 2-layer MLP, self-righting/standing-up: 128-128; locomotion: 128-256; selector: 128-128, softmax output, tanh activation
- **Critic Network:** Separate value function
- **Encoders:** None
- **Special Modules:** Height estimator - 2-layer MLP with 128 softsign units

---

## 🌍 7. Generalization & Transfer
- **Domain Randomization:** Yes
- **Zero/Few-shot Adaptation:** No
- **Cross-Robot Generalization:** No
- **Sim2Real Transfer:** Yes – highly successful; behaviors match sim closely, 97% recovery rate on real hardware

---

## 📈 8. Results & Evaluation
- **Key Metrics:** Recovery success rate (97% over 100+ trials), time (<5s), qualitative sim-real similarity (e.g., behavior switches, motions
- **Baselines Compared:** [Which other methods?]
- **Ablations Conducted:** With/without height estimator (fails without, causing bad switches); learned selector vs FSM (FSM unsmooth, misses corners
- **Hardware Results:** 100% success in 50 arbitrary fall configs; handled disturbances (e.g., kicks) while walking; seamless behavior transitions

---

## 💡 9. Insights & Takeaways
- **Core Contribution:** Hierarchical model-free RL for robust, heuristic-free fall recovery in quadrupeds via pre-trained behaviors and learned selector
- **What Worked Well:** High robustness (97% success), natural/dynamic motions, efficient sim-to-real (no real tuning, data-driven actuators key)
- **What Could Be Improved:** Flat ground only; fails on inclines/rough terrain; assumes no joint over-rotation (>2π); extend to more behaviors/environments
- **Relevance to My Work:** Adapt hierarchical decomposition and sim randomization for multi-skill RL in legged robots, especially recovery tasks

---

## 🧠 10. Questions & Reflections
- What assumptions do I disagree with or want to test further?
- Could this work on different robot morphologies?
- Any failure modes I could investigate?
- What would I do differently in a follow-up?

---
