# 📄 Paper Summary:
**Authors:** Tuomas Haarnoja∗,1,2, Sehoon Ha∗,1, Aurick Zhou2, Jie Tan1, George Tucker1 and Sergey Levine1,2
**Lab/Organization:** UC Berkley
**Conference/Journal:** RSS 2019
**Link:** https://www.arxiv.org/abs/1812.11103
**Code:** [GitHub or other link, if available]

---

## 🔧 1. Experimental Setup
- **Simulator:** No sim direct real robot. The method was tested in OpenAI Gym env
- **Robot Model:** Minitaur
- **Hardware Deployment:** Yes, Minitaur
- **Robot Configuration:** 8-DoF

---

## 🧠 2. Learning Algorithm
- **RL Algorithm:** SAC with automatic entropy adjustment using gradient optimization
- **Model Type:** Model-free
- **Policy Type:** Feedforward
- **Special Techniques:** Automatic temperature adjustment to constrain expected entropy. History augmentation to handle non-Markovian dyamics
- **Online or Offline Learning:** Online

---

## 👁️ 3. Observation Space
- **State Variables Used:**
  - Joint positions: Yes
  - Joint velocities: No
  - Base pose/vel: Yes
  - Terrain info: No
  - Contact sensors: No
- **History/Memory:** Last 5 observation put into as history
- **Privileged Observations:** No

---

## 🎮 4. Action Space
- **Action Type:** Joint Position
- **Action Frequency:** 50 Hz
- **Actuator Constraints:** Low PD gains for compliance, actions are bounded

---

## 🏆 5. Reward Function
- **Reward Components:**
  - Velocity tracking: Yes
  - Energy minimization: No
  - Base stability: Yes
  - Smooth motion: Yes
  - Other: Front leg folding penalty
- **Reward Shaping:** Heavy

---

## 🧱 6. Network Architecture
- **Policy Network:** 2-layer MLP, 256 units/layer, ReLU
- **Critic Network:** Separate
- **Encoders:** None
- **Special Modules:** None

---

## 🌍 7. Generalization & Transfer
- **Domain Randomization:** None
- **Zero/Few-shot Adaptation:** No
- **Cross-Robot Generalization:** No
- **Sim2Real Transfer:** No, direct real world learning

---

## 📈 8. Results & Evaluation
- **Key Metrics:** Average return, walking speed (0.32m/s), perturbation robustness (220N in sim)
- **Baselines Compared:** SAC (fixed temp), DDPG, PPO, TD3 (Not for the work but for the algo)
- **Ablations Conducted:** Hyperparam sensitivity; entropy/temp evolution
- **Hardware Results:** 2-hour training; generalizes to slopes/blocks/steps; recovers from pushes

---

## 💡 9. Insights & Takeaways
- **Core Contribution:** Auto entropy adjustment in SAC for sample-efficient real-world legged RL without sim/modeling
- **What Worked Well:** Robust gaits from scratch; fixed hyperparams across tasks; terrain generalization
- **What Could Be Improved:** Manual resets; no safety layer; motion capture dependency
- **Relevance to My Work:** Auto-tuning for RL hyperparams in real robotics; max entropy for robustness

---

## 🧠 10. Questions & Reflections
- What assumptions do I disagree with or want to test further?
- Could this work on different robot morphologies?
- Any failure modes I could investigate?
- What would I do differently in a follow-up?

---
