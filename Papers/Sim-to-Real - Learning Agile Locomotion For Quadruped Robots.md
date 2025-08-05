# 📄 Paper Summary:
**Authors:** Jie Tan1, Tingnan Zhang1, Erwin Coumans1, Atil Iscen1, Yunfei Bai2, Danijar Hafner1, Steven Bohez3, and Vincent Vanhoucke1
**Conference/Journal:** RSS, 2018 
**Link:** https://www.arxiv.org/abs/1804.10332 
**Code:** Not official - https://github.com/chillybird/minitaur_sim2real_ppo/tree/master

---

## 🔧 1. Experimental Setup
- **Simulator:** PyBullet
- **Robot Model:** Minitaur
- **Hardware Deployment:** Yes
- **Robot Configuration:** 8 DoF, motor encoders and IMU

---

## 🧠 2. Learning Algorithm
- **RL Algorithm:** PPO
- **Model Type:** Model-free
- **Policy Type:** Feedforward
- **Special Techniques:** Dynamics randomization, random perturbations, system ID, actuator modelling, latency simulation
- **Online or Offline Learning:** Online PPO

---

## 👁️ 3. Observation Space
- **State Variables Used:**
  - Joint positions: Yes, 8 joint motor angles
  - Joint velocities: No
  - Base pose/vel: Yes (IMU: roll, pitch, angular velocities)
  - Terrain info: No
  - Contact sensors: No
- **History/Memory:** None
- **Privileged Observations:** No

---

## 🎮 4. Action Space
- **Action Type:** Joint Position
- **Action Frequency:** 150-200Hz
- **Actuator Constraints:** Torque saturation, PWM-based model, latency, friction, voltage variation

---

## 🏆 5. Reward Function
- **Reward Components:**
  - Velocity tracking: Yes
  - Energy minimization: Yes
  - Base stability: Yes, indirectly, episode terminates on tilt
  - Smooth motion: No
  - Other: No
- **Reward Shaping:** Dense
---

## 🧱 6. Network Architecture
- **Policy Network:** 2-layer MLP, 125, 89 for trotting and 125, 95 for gallopting
- **Critic Network:** Separate
- **Encoders:** None
- **Special Modules:** Hybrid policy (open-loop reference + feedback component)

---

## 🌍 7. Generalization & Transfer
- **Domain Randomization:** Yes, they have provided a table, its more of plusminus 20%
- **Zero/Few-shot Adaptation:** No
- **Cross-Robot Generalization:** No
- **Sim2Real Transfer:** Successful

---

## 📈 8. Results & Evaluation
- **Key Metrics:** Forward speed (m/s), energy consumption (watts), expected return (progress minus energy), reality gap (sim vs real return diff)
- **Baselines Compared:** Handcrafted gaits (Ghost Robotics); baseline sim (no actuator/latency models)
- **Ablations Conducted:** Sim improvements (actuator model, latency); randomization/perturbations; observation space size (4D vs 12D)
- **Hardware Results:** Trotting: 0.60 m/s, 71.78W (23% less than baseline); galloping: 1.18 m/s, 188.79W (35% less); stable >3m runs, no falls

---

## 💡 9. Insights & Takeaways
- **Core Contribution:** System for learning agile quadruped gaits from scratch with direct sim-to-real transfer via sim enhancements and robust policies
- **What Worked Well:** Emergent gaits (galloping/trotting); energy efficiency over handcrafted; hybrid control for user guidance; reality gap narrowing
- **What Could Be Improved:** Flat terrain only; no vision/speed control; potential suboptimality from randomization
- **Relevance to My Work:** Hybrid policies for guided learning and sim fidelity techniques (actuator modeling, latency) adaptable for robust RL in robotics

---

## 🧠 10. Questions & Reflections
- What assumptions do I disagree with or want to test further?
- Could this work on different robot morphologies?
- Any failure modes I could investigate?
- What would I do differently in a follow-up?

---
