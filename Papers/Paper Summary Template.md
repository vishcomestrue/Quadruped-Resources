# 📄 Paper Summary: [Paper Title]
**Authors:** [Author list]  
**Lab/Organization:** [Lab list]
**Conference/Journal:** [Venue, Year]  
**Link:** [Paper URL or DOI]  
**Code:** [GitHub or other link, if available]

---

## 🔧 1. Experimental Setup
- **Simulator:** [e.g. MuJoCo, PyBullet, Isaac Gym]
- **Robot Model:** [e.g. ANYmal, Unitree A1, Custom URDF]
- **Hardware Deployment:** [Yes/No – details if yes]
- **Robot Configuration:** [12-DoF, joint types, sensor info]

---

## 🧠 2. Learning Algorithm
- **RL Algorithm:** [PPO, SAC, TRPO, etc.]
- **Model Type:** [Model-Free / Model-Based]
- **Policy Type:** [Feedforward / Recurrent / Residual / etc.]
- **Special Techniques:** [Domain randomization, Curriculum Learning, etc.]
- **Online or Offline Learning:** [e.g. Online PPO, offline dataset fine-tuning]

---

## 👁️ 3. Observation Space
- **State Variables Used:**
  - Joint positions: [Yes/No]
  - Joint velocities: [Yes/No]
  - Base pose/vel: [Yes/No]
  - Terrain info: [e.g. heightmap, proprioception]
  - Contact sensors: [Yes/No]
- **History/Memory:** [LSTM / GRU / Trajectory encoder / None]
- **Privileged Observations:** [Used during training only?]

---

## 🎮 4. Action Space
- **Action Type:** [Torque / Joint Position / Cartesian Foot Target]
- **Action Frequency:** [e.g. 50Hz, 100Hz]
- **Actuator Constraints:** [Torque limits, delays, noise models]

---

## 🏆 5. Reward Function
- **Reward Components:**
  - Velocity tracking: [Y/N]
  - Energy minimization: [Y/N]
  - Base stability: [Y/N]
  - Smooth motion: [Y/N]
  - Other: [foot clearance, slip penalty, etc.]
- **Reward Shaping:** [Heavy / Sparse / Curriculum]

---

## 🧱 6. Network Architecture
- **Policy Network:** [e.g. 2-layer MLP, 256 units, ReLU]
- **Critic Network:** [Shared / Separate]
- **Encoders:** [Terrain CNN, RNN history encoder, etc.]
- **Special Modules:** [e.g. system ID encoder in RMA, latent context in UPOSI]

---

## 🌍 7. Generalization & Transfer
- **Domain Randomization:** [What parameters?]
- **Zero/Few-shot Adaptation:** [Yes/No – how is it done?]
- **Cross-Robot Generalization:** [Yes/No – different dynamics, same policy?]
- **Sim2Real Transfer:** [If done, how successful?]

---

## 📈 8. Results & Evaluation
- **Key Metrics:** [Velocity error, foot slip, energy consumption, etc.]
- **Baselines Compared:** [Which other methods?]
- **Ablations Conducted:** [Which components tested separately?]
- **Hardware Results:** [Distance covered, recovery, terrain tests]

---

## 💡 9. Insights & Takeaways
- **Core Contribution:** [One-line summary of novelty]
- **What Worked Well:** [Highlights of success]
- **What Could Be Improved:** [Limitations/assumptions]
- **Relevance to My Work:** [Any key ideas worth adapting?]

---

## 🧠 10. Questions & Reflections
- What assumptions do I disagree with or want to test further?
- Could this work on different robot morphologies?
- Any failure modes I could investigate?
- What would I do differently in a follow-up?

---
