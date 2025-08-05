# 📄 Paper Summary:
**Authors:** Milad Shafiee1, Guillaume Bellegarda1, and Auke Ijspeert1
**Lab/Organization:** BioRobotics Laboratory, Ecole Polytechnique Federale de Lausanne (EPFL).
**Conference/Journal:** ICRA, 2024
**Link:** https://www.arxiv.org/abs/2310.10486  
**Code:** Yet to be released.

---

## 🔧 1. Experimental Setup
- **Simulator:** Issac Gym
- **Robot Model:** 16 different quadrupeds including Unitree A1, Go1, Aliengo, Laikago, B1, Boston Dynamics Spot, ANYbotics ANYmal-B and ANYmal-C, MIT mini-cheetah, Little-Dog, Spot-Micro, Solo, and HYQ, and three customized three-segmented leg quadruped robots
- **Hardware Deployment:** Yes
- **Robot Configuration:** 12 & 16 DoFs, with mixed elbow-up and elbow-down configurations.

---

## 🧠 2. Learning Algorithm
- **RL Algorithm:** PPO
- **Model Type:** Model Free
- **Policy Type:** FeedForward
- **Special Techniques:** CPG+RL where CPG takes care of motor control while RL being a level upper
- **Online or Offline Learning:** Online PPO

---

## 👁️ 3. Observation Space
- **State Variables Used:**
  - Body Orientation
  - Body Linear and Angular Velocity
  - Foot contact Booleans
  - Relative feet positions
  - Previous action selected by the policy
  - CPG states ($r, \dot{r}, \theta, \dot{\theta}$)
- **History/Memory:** None
- **Privileged Observations:** [Used during training only?]

---

## 🎮 4. Action Space
- **Action Type:** 8-dimensional vector describing the amplitudes and frequencies of the four limbs
- **Action Frequency:** 100Hz
- **Actuator Constraints:** amplitude $\in [0.5, 4]$, frequency $\in [0, 5]$

---

## 🏆 5. Reward Function
- **Reward Components:**
  - Velocity tracking: No
  - Energy minimization: Yes. 
  - Base stability: Kind of. Non-zero body orientations are penalized
  - Smooth motion: No
  - Other: Viability in forward progress
- **Reward Shaping:** Heavy

---

## 🧱 6. Network Architecture
- **Policy Network:** 3-layer MLP, 512, 256, 128 nodes, ELU
- **Critic Network:** Separate (PPO)
- **Encoders:** None
- **Special Modules:** CPG RG+PF unit for low level motor control

---

## 🌍 7. Generalization & Transfer
- **Domain Randomization:** None
- **Zero/Few-shot Adaptation:** Zero-shot. Direct sim2real, no fine tuning
- **Cross-Robot Generalization:** Yes. Single policy controls 16 robots with varying mass (2-200kg), height (18-100cm), DoFs (12/16), and 3 morphologies; tested on unseen robots
- **Sim2Real Transfer:** Successful. Tested on Unitree Go1 (uneven grass) and A1 (stable trotting with up to 15kg load, 125% nominal mass).

---

## 📈 8. Results & Evaluation
- **Key Metrics:** Base velocity (up to 1.55 m/s in sim, 1 m/s target in hardware); CPG frequency/amplitude; robustness to loads (up to 125% mass); energy efficiency (via power penalty).
- **Baselines Compared:** GenLoco [29] (motion imitation); graph learning [25-28] (agent-agnostic RL); attention-based policies [24] (Qualitative; highlights faster training and broader generalization)
- **Ablations Conducted:** Limited; trained without 3 extreme robots (HYQ, B1, Dog3) and tested generalization on them
- **Hardware Results:** Go1: smooth trotting on uneven grass/concrete; A1: stable with 10-15kg loads (125% mass), no training disturbances

---

## 💡 9. Insights & Takeaways
- **Core Contribution:** Bio-inspired CPG-RL trains one policy for diverse quadrupeds (varying size/morphology/DoFs) in <2 hours, with robust sim-to-real.
- **What Worked Well:** Fast training; generalization to unseen robots; hardware robustness (e.g., heavy loads, uneven terrain) without randomization.
- **What Could Be Improved:** Only forward motion that too limited to trotting; assumes fixed parameters per robot; future: omni-directional locomotion on uneven terrain with different gaits.
- **Relevance to My Work:** [Any key ideas worth adapting?]

---

## 🧠 10. Questions & Reflections
- What assumptions do I disagree with or want to test further?
- Could this work on different robot morphologies?
- Any failure modes I could investigate?
- What would I do differently in a follow-up?

---
