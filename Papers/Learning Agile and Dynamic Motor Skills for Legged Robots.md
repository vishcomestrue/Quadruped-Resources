# 📄 Paper Summary
**Authors:** JEMIN HWANGBO1*, JOONHO LEE1, ALEXEY DOSOVITSKIY2, DARIO BELLICOSO1, JOONHO LEE1, VASSILIOS TSOUNIS1, VLADLEN KOLTUN2, AND MARCO HUTTER1
**Lab/Organization:** RSL, ETH Zurich
**Conference/Journal:** Science Robotics 4.26 (2019)
**Link:** https://www.arxiv.org/abs/1901.08652
**Code:** [GitHub or other link, if available]

---

## 🔧 1. Experimental Setup
- **Simulator:** Custom
- **Robot Model:** ANYmal quadruped
- **Hardware Deployment:** Yes, no real world fine tuning
- **Robot Configuration:** 12 DoF, sensors: IMU, joint encoders, torque measurement, Kalman-based height estimator

---

## 🧠 2. Learning Algorithm
- **RL Algorithm:** TRPO
- **Model Type:** Model-free
- **Policy Type:** Feedforward
- **Special Techniques:** Domain randomization (inertial params, obs noise), curriculum learning (cost modulation), learned actuator nets for sim-to-real.
- **Online or Offline Learning:** Online

---

## 👁️ 3. Observation Space
- **State Variables Used:**
  - Joint positions: Yes
  - Joint velocities: Yes
  - Base pose/vel: Yes
  - Terrain info: No
  - Contact sensors: No
- **History/Memory:** Finite joint state history (last 2 data, specifically t-0.01, t-0.02)
- **Privileged Observations:** No

---

## 🎮 4. Action Space
- **Action Type:** Joint position targets (low impedence, PD converted to torque)
- **Action Frequency:** 20-200Hz. 200Hz locomotion, 100Hz recovery
- **Actuator Constraints:** Joint vel/torque limits

---

## 🏆 5. Reward Function
- **Reward Components:**
  - Velocity tracking: Yes
  - Energy minimization: Yes
  - Base stability: Yes
  - Smooth motion: Yes
  - Other: Joint speed/accel, foot clearance/slip, contact impulse/internal collision (recovery)
- **Reward Shaping:** Curriculum

---

## 🧱 6. Network Architecture
- **Policy Network:** 2-layer MLP, 256, 128 units, tanh activations, maps obs/history to joint positions
- **Critic Network:** Separate
- **Encoders:** None
- **Special Modules:** ActuatorNet: 3 layer MLP (32 units each); softsign activations for action-to-torque modeling

---

## 🌍 7. Generalization & Transfer
- **Domain Randomization:** Yes, picked from Uniform distribution
- **Zero/Few-shot Adaptation:** Yes, zero-shot
- **Cross-Robot Generalization:** No
- **Sim2Real Transfer:** Successful

---

## 📈 8. Results & Evaluation
- **Key Metrics:** Vel error (lin 0.143 m/s, yaw 0.174 rad/s), energy (torque 8.23 Nm, power 78.1 W), speed (1.5 m/s real, +25% prior), recovery (100% success from 9 poses in <3s)
- **Baselines Compared:** ideal/analytical actuator models (fail on hardware).
- **Ablations Conducted:** Ideal/analytical actuators (violent shaking, falls); activation funcs (tanh more robust than ReLU); history configs (tuned for perf/validation error)
- **Hardware Results:** Robust command following (5 min, random cmds/pushes), high-speed running (1.5 m/s, exploits hardware limits), recovery from complex falls (e.g., upside-down, on own legs)

---

## 💡 9. Insights & Takeaways
- **Core Contribution:** Learned actuator nets + fast hybrid sim enable zero-shot sim-to-real RL for agile legged skills outperforming manual controllers
- **What Worked Well:** Automated training (4-11 hrs), superior accuracy/efficiency/speed, robust to noise/wear (3+ months), dynamic recovery feats
- **What Could Be Improved:** Single-task policies (no hierarchy/multi-task); manual cost/initial dist tuning (2-7 days); assumes flat terrain/independent actuators
- **Relevance to My Work:** Adapt learned actuator modeling and curriculum cost modulation for sim-to-real in dynamic systems with complex actuators

---

## 🧠 10. Questions & Reflections
- What assumptions do I disagree with or want to test further?
- Could this work on different robot morphologies?
- Any failure modes I could investigate?
- What would I do differently in a follow-up?

---
