# 2018
## Sim-to-Real: Learning Agile Locomotion for Quadruped Robots
> Link: https://arxiv.org/pdf/1804.10332

DRL is used to leverage the extensive expertise required to manually tune systems for agile locomotion. Learn from simple reward signals. Open loop control reference can be provided to facilitate the learning process when more control over the learned gait is needed. Control policies: Phy Sim -> Real. The gap is reduced by improving the physics sim and learning robust control policies. Sim is improved by using system identification, accurate actuator model and simulating latency. Learning robust control policies is done by randomizing env, adding perturbations (disturbances to behaviors), and compact obs space. Evaluation using 2 gaits: Trotting and Galloping. 

- Simulation: Minitaur in Bullet Physics library ([https://github.com/bulletphysics/bullet3/tree/master/examples/pybullet/gym/pybullet_envs/minitaur/envs](https://github.com/bulletphysics/bullet3/tree/master/examples/pybullet/gym/pybullet_envs/minitaur/envs))
- Policy and Value function - NN with two hidden layers. Data collection - 25 roll outs (episodes) and 1000 steps each and total of 7M timesteps
- With no actuator model and latency, the system learnt slow walking gait in sim. Why? Might be due to the default constraint-based actuator model in Bullet, which seems to be overdamped and limits the agility.
- When compared with real robots the authors claim that their trained policy consume significantly less power (35% for galloping and 23% trotting)
- Reality gap cannot be narrowed by the binary success rate (defined by the the percentage of controllers that can balance in real world for the entire episode which is of about 1000 steps and abou 6 seconds), hence the authors have adopted techniques from ([Crossing the reality gap in evolutionary robotics by promoting transferable controllers](https://sci-hub.st/10.1145/1830483.1830505)). They say that the difference of expected value of their reward function between sim and real as the gap!
- To conclude results, the authors trained 100 controllers with different parameters and seed values, took top 3 (based on returns in the sim), and ran each of them thrice. The expected return is the average of the nine runs.
- The three groups of controllers trained each corresponds to one run with the baseline sim without any actuator model or latency handling, one with baseline sim & random perturbations and the last run was the author's proposed method. This is using 4D dimensional space using IMU readings with roll, pitch and angular velocity of the base along the two axes.
- The authors claim that if the model discrepancy is too big, even a robust controller trained with random perturbations cannot overcome the reality gap.
- The authors have also found that both the accurate actuator model and latency simulation are important without which the learned controllers do not work on the real robot.
- As pointed out in Lerrel Pinto's work in the paper Robust Adversarial Reinforcement Learning (arXiv:1703.02702, 2017), uncertainties in physical params can be viewed as extra torque/force acting on the system. 
- Irrespective of the observation space, the authors have shown that the returns of the controllers trained using randomization have a lower mean(indicated suboptimal performance) and a lower standard deviation(Robustness).
- In the simulation, larger observation space (12D), the performance of the learned controllers is higher. However, these policies when they are deployed in real robot, the controllers with large observation space perform worse in the real world and the reality gap is wider. 
- When the space is large, the observations encountered in training become relatively sparse. Encountering similar observations in the real world is less likely, which can cause the robot to fall. In contrast, when the observation space is small, the observation distributions in training and testing are more likely to overlap, which narrows the reality gap.
- To learn transferable policies, reward function and environment were simple. Reward function was to maximise running speed on a flat ground. 
- Proposed changes  
    1) Dynamically change the speed and direction  
    2) Extend this to handle complex terrain

# 2019
## Learning to Walk via Deep Reinforcement Learning
> Link: https://arxiv.org/pdf/1812.11103

Even though DRL could enable learning locomotion skills with minimal engineering and without an explicit model of the robot dynamics, applying DRL to real-world robot is extremely difficult due to poor sample complexity and sensitivity to hyper parameters. Tuning is expensive on a physical robot and using extensive trial-and-error methods can damage them. To solve this, the authors have proposed a sample efficient deep RL algo based on maximum entropy RL (SAC) that requires minimum-task tuning and comparatively low number of trials to learn neural network policies. Without a model or simulation the robot learns to walk in the real world within two hours. 

- Poor Sample Complexity? DRL generally requires large number of interactions (samples) with env to learn effective policies
- The deep RL algorithm developed is sample efficient and also is robust to the choice of hyperparameters. Usually maximum entropy RL algorithms are sensitive to the choice of the temperature parameter (T). But the authors have proposed a **gradient descent based optimisation of T**. 
- This method controls the expected entropy over the states, while allowing per-state entropy to vary. (If you do not understand this, just imagine that the policy will act deterministic when it is about to fall, and can explore when in stable and open conditions while the overall entropy is the expected average).
-  The optimisation problem here is both the expected return and the entropy of the policy. The Lagrangian relaxation optimisation by including the entropy is proposed as the solution.
-  The practical algorithm contains function approximators where in each iteration there are three steps happening sequenctially:
	1) Data collection phase for each environment step
	2) Optimisation phase for each gradient step
	3) Re-parameterization (you can back propagate through a sampling operation, like GANs topic)
- The authors do not transfer any simulated policy to the real world. All real world experiments only use real-world training. 
- Apart from the number of steps, even the number of episodes can be an important parameter as they decide the amount of times the robot is reset which might be costly and time consuming physically and could sometimes also require human intervention. 
- The authors also suspect that the robustness emerging from the trained policy is automatically from SAC method due to entropy maximisation at training time.  
- In the MDP formulation, the observation includes 8 motor angles (2 in each leg for Minitaur), roll and pitch angles, and their angular velocities. Yaw has been discarded as they seem to drift pretty fast. The action space includes the swing angle and extension of each leg. 
- Since the latencies and partial observation makes the system non-Markovian, they augment an observation space to include the history of the last five observations and actions which results in a 112 dimensional observation space(5 past observations {12x5}, 5 past actions {(2x4)x5}, 1 current observation {12x1} = 60 + 40 + 12 = 112 ).
- The reward is designed to encourage longer walking distance(using MCS), penalises large joint accelerations (computed via finite differences using the last three actions) , penalises large roll angle off the base and the joint angles when the front legs are folder under the robot. These are also proven to be common failure cases. 
- The feed-forward neural network has two hidden layers and 256 neurons per layer which are randomly initialised. For preventing too jerky motions at the early stage, we smoothed out actions for the first 50 episodes. 
- One interesting point to observe from the Results is that the learned gait is periodic and synchronised even though there is no explicit trajectory generator, symmetry constraint, or periodicity constraint is encoded into the system.  
- Due to SAC's robust policies, policy was able to readily generalise to terrains and perturbations without any additional tweaking. 
- Two of the most critical remaining challenges of the current system are 
	- Heavy dependency on manual resets between episodes
	- Lack of a safety layer that would enable learning on bigger robots

## Learning Agile and Dynamic Motor Skills for Legged Robots
> Link: https://arxiv.org/pdf/1901.08652 

Imitation of dynamic and agile movement of animals using existing methods crafted by humans are impossible. RL research is mainly limited to simulation and only a few and comparably simple examples have been deployed on real systems. Primary reason for the same is that such methods are complicated and expensive. The paper reports a new method for training a neural network policy in simulation and transferring it to ANYMal leveraging fast, automated, and cost-effective data generation schemes. 

- The freedom of legged robotics to choose contact points with the env enables them to overcome obstacles comparable to their leg length.
- From control perspective, these systems are high-dimensional and non-smooth systems with many physical constraints. Contact points change over time depending on the maneuver and therefore cannot be prespecified. 
- Analytical models are inaccurate and complex uncertainties in the dynamics. Complex sensor suite will add on noise and delays the information transfer. Conventional control theories are often insufficient to deal with these problems. Specialized control methods developed for such problems often require lengthy design process and arduous parameter tuning. 
- Most popular approach to control physical legged systems is by using modular controller design. It breaks the control problems into submodules which are decoupled and easier to manage. Each module is based on template dynamics, or heuristics, and generates reference values for the next module.
	- Template Dynamics - Simplified dynamics models - SLIP, Inverted Pendulum, Cart-Pole system
	- Heuristics - Manually designed rules based on intuitions or observations - "If foot is slipping, reduce step length"
- A Modular pipeline will look the following:
		High level planner (Template Dynamics) -> Footstep planner -> Trajectory Generator -> PD/MPC Controller
- The main disadvantage to this is that limited details in modelling constraints the model's accuracy of the system. This can be mitigated by limiting the operational state to a small domain where the approximations are valid. But this comes at the cost of significant compromise in performance and designing these modular controller are extremely labourous.
- Trajectory optimization methods were introduced to mitigate the above issues. It consisted of one planner and tracker. Planner uses rigid body dynamics and numerical optimization to compute an optimal path. The tracking then follows the path.
- |Aspect|Low-Impedance Position Control|Torque Control|
|---|---|---|
|**What the policy outputs**|Desired joint angles|Raw joint torques|
|**Control loop**|PD controller interprets the desired angle|Policy must do all stability work|
|**Stability**|More stable due to closed-loop tracking|Harder to learn, can be unstable|
|**Sample efficiency**|Better, because local stabilization is handled by the PD loop|Worse — must learn both action and stabilization|
|**Training difficulty**|Lower, easier to explore|High — noisy torques can destabilize robot|
|**Risk of damage (real robot)**|Lower — internal safety from impedance|Higher — can apply extreme torques directly|
- |Concept|Meaning|
|---|---|
|**Low-impedance joint position command**|The policy says "move to this position", and a **soft PD controller** tracks it gently.|
|**Why it's better than torque control**|It gives **local stability**, **smooth exploration**, and **faster learning** — the policy doesn't need to learn raw physics stabilization from scratch.|

## Robust Recovery Controller for a Quadrupedal Robot using Deep Reinforcement Learning
> Link: https://arxiv.org/pdf/1901.07517

Learning to recover from a fall is essential for a quadruped robot. Works till now have only built upon a pre-existing trajectory whose behaviour is unnatural. The paper proposes model-free DRL to control recovery maneuvers of quadrupeds using a hierarchical behavior-based controller. The controller consists of four neural network policies out of which three accounts to behaviors and one as behavior selector to coordinate them. The authors have deployed it in ANYmal and recovery is being achieved in less than 5s for any arbitrary fall configuration and a success rate of 97%.

- Self-righting -> Standing up -> Locomotion, the three behaviors.
- Self-righting behavior fails when a joint position >= 180deg. Due to the fact that during the training phase, the policy hardly experiences such cases. 
- Simple FSM, could not capture edge cases. Transition between behaviors, unsmooth.
- Height estimator is learnt, crucial for reliable maneuvers. 
- Low-impedance PD controllers on the joints individually. 
- The policies are two layered feed-forward neural network with tanh units. The self-righting and standing up policies have 128 units in each hidden layer and the locomotion policy has 128 and 256 units respectively.
- Tested only on flat surfaces. Authors propose to mitigate the issue faced through randomization of the environment and by estimating its properties. 

# 2020
## Learning to walk in the real world with Minimal Human Effort
> Link: https://arxiv.org/pdf/2002.08550

DRL has emerged as a promising method for developing control policies which are challenging for legged robots. The key difficulties for on-robot learning systems are automatic data collection and safety. These are overcome by developing a multi-task learning procedure and a safety-constrained RL framework. This paper presents a framework that enables on-robot learning of quadruped walking with minimal human intervention, demonstrated on Minitaur. The core contributions include automatic reset policies, safety constraints, and minimal reward shaping, allowing learning on real hardware in only ~2 hours.

- The core algorithm used is Soft Actor-Critic (SAC) with entropy regularization. A multi-task SAC setup is used to train on walking forward, backward, and turning tasks jointly.
- The policy outputs desired motor positions which are tracked using low-level PD controllers on Minitaur. The policy operates in action space defined by swing angle and leg extension for each of the 4 legs.
- The observation includes motor angles, base orientation (roll and pitch), angular velocities, and a history of recent observations and actions to handle partial observability. This results in a high-dimensional input (~112D).
- The action space is 8D (2 DoF per leg): one for swing angle and one for leg extension.
- The training is done entirely in the real world. Around 160k steps (~2 hours) of robot interaction are sufficient for learning robust gaits.
- A learned reset controller is used to autonomously bring the robot back to standing, enabling continuous training without human intervention.
- A fallback safe policy is used during early training phases to ensure safety and avoid hardware damage.
- The reward is sparse and minimal, focusing on encouraging desired velocity, penalizing excessive accelerations and large body roll. Gait patterns like pacing and bounding emerge naturally.
- The actor and critic networks are MLPs with two hidden layers of 256 units and ReLU activations.
- The final policy generalizes well to small obstacles, slopes, and pushes despite being trained only on flat terrain. The key idea is to eliminate the sim-to-real gap entirely through real-world data, safety-aware design, and autonomous resets.

# 2022
## A Walk in the Park: Learning to Walk in 20 minutes with model-free reinforcement learning
> Link: https://arxiv.org/pdf/2208.07860

DRL is a promising approach to learn policies in uncontrolled domain env that do not require domain knowledge but due to sample inefficiency, simulation alternatives were sought after. The paper presented a careful implementation of one of the several existing algorithmic frameworks combined with modern optimised deep learning packages and a number of careful design decisions for the MDP formulation of the locomotion task.

- The algorithm is a standard Q-function actor-critic method, DroQ, which extends the SAC with Dropout and Layer Normalization.
- Usual SAC and DDPG does one actor and one critic updates for each env step. Here they do nearly 20 critic update and one actor update. The algorithm allows to take significantly more gradient steps on the critic after each environment steps which in turn leads to more sample-efficient learning.
- They use low level action space and only proprioceptive information. The parameterized the policy to directly output joint targets rather than rely on pre-defined gaits or trajectory generators. 
- The state $s_{t}$, contains root orientation (roll and pitch), root angular velocity (roll, pitch and yaw), root linear velocity, joint angles, joint velocities, binary foot contacts, and the previous action.
- Actions $a_{t}$ are PD position targets for each ofthe 12 joints and applied at a frequency of 20 Hz. They define the action space for every leg as $[p-o, p+o]$. 
- The robot trains continuously terminating only when the robot's roll or pitch exceeds 30 degrees. 
- To reset the robot, they use the open-source reset policy (No idea about this gotta study)
- Most important of them all, to facilitate real-time computation, they use JAX.

# 2023
## Robust High-Speed Running for Quadruped Robots via Deep Reinforcement Learning
> Link: [https://arxiv.org/pdf/2103.06484](https://arxiv.org/pdf/2103.06484)

This paper develops robust and fast quadruped locomotion on the Unitree A1 using deep RL. It avoids predefined gaits or trajectory generators and trains purely in simulation with successful sim-to-real transfer up to 2 m/s and load-bearing capability.

- The algorithm used is PPO (Proximal Policy Optimization), where the policy is trained in task space to output desired Cartesian foot positions. These are tracked via a Cartesian PD controller that produces joint torques.
- The policy only uses proprioceptive input: base orientation, base angular and linear velocities, joint states, and foot contact indicators. No vision or external sensors are used.
- The action is a 12D vector representing target foot positions in Cartesian space for each leg. These are mapped to torques through PD control. Control is run at 100 Hz.
- The observation space includes base orientation (as quaternion), angular velocity, linear velocity, joint positions and velocities, and binary foot contacts.
- Training is done entirely in simulation (PyBullet) using heavy domain randomization on dynamics: mass, inertia, friction, motor latency, and terrain profiles.
- A curriculum is applied on target velocities to gradually increase difficulty.
- The reward design is minimal, focusing on velocity tracking, smooth control, and energy efficiency. Bounding and galloping gaits emerge naturally without hand-designed motion patterns.
- The network is a simple MLP for actor and critic with ReLU activations, likely with 2–3 layers and 256 hidden units per layer.
- The final learned policy can generalize to unseen terrains and external disturbances, and works on the real robot (Unitree A1) without additional fine-tuning.

## Not only Rewards but also Constraints: Applications on Legged Robot Locomotion
> Link: https://arxiv.org/abs/2308.12517

Earlier studies have promising results for controllers designed using a neural network and training it with model-free RL. But again, it requires extensive reward engineering, which is highly time-consuming and extensively laborious. The paper proposes a novel RL framework for training consisting not only of rewards but also of constraints. The researchers have shown that these controllers can be trained with significantly less reward engineering, by tuning only a single reward coefficient. This paper proposes a constraint-based reinforcement learning framework for legged locomotion that reduces reward engineering effort. Instead of relying heavily on reward shaping, it formulates locomotion as a constrained MDP using hard physical constraints like torque limits, body roll, and foot slip.

- The algorithm is Interior-point Policy Optimization (IPO), a constrained policy gradient method that integrates log-barrier functions for enforcing constraints. It adapts the thresholds of constraints during training, allowing scalable constraint inclusion without major overhead.
- Constraints include probabilistic (e.g., bounding joint torque violations to be below a threshold probability) and average constraints (e.g., bounding the average roll angle or slippage). These are expressed in physically meaningful units.
- The reward is minimal and primarily encourages velocity tracking. Only a single reward weight needs to be tuned across tasks and robot morphologies.
- Policy and value functions are standard multi-layer perceptrons with ReLU activations, likely 2–3 layers with 256 units per layer.
- Observations are purely proprioceptive: joint angles, joint velocities, base orientation, base velocities, and command velocities. No external sensors are used.
- The action space is either joint positions or torques depending on the robot type. Training environments are built in RaiSim with procedural terrain generation.
- A teacher-student learning setup is used: the teacher policy uses privileged simulation data and constraints during training, and a student learns to mimic the teacher under limited observations for real-world deployment.
- The framework generalizes well to different robots (with 4 to 18 DoF) and requires minimal tuning. Real robot experiments show successful deployment and stable gaits.
- The key idea is to shift the burden of locomotion design from complex reward tuning to interpretable, structured constraints, enabling efficient and robust policy learning.

## ManyQuadrupeds: Learning a Single Locomotion Policy for Diverse Quadruped Robots
> Link: https://www.arxiv.org/abs/2310.10486
> 

# 2024
## Neural Circuit Architectural Priors for Quadruped Locomotion
> Link: https://arxiv.org/pdf/2410.07174

Usually learning based approaches use FC MLPs. These contain inductive bias - they favour any outcome - and hence techniques such as priors inform of rewards, training curricula, imitation data, or trajectory generators are included. Looking at nature, we find such architectural priors in animals such as horses that can walk asap after birth. The work explored advantages of biologically inspired ANNs for quadruped locomotion based on the neural circuits in limbs and spinal cord of mammals. The architecture also exhibits better generalisation to task variations.

- Uses less data and fewer magnitude orders of parameters and yet achieves comparable results to MLPs. 
- Hand-tuned RG (Rhythm Generator) module and BC(Brainstem command - high level control signals to modulate gait, speed, direction or posture) command 
- Fixed speed locomotion, postural adjustment, turning, righting mechanisms missing. 
- NCAP could be trained with additional reward, task or imitation priors. This can be done since architectural priors are orthogonal to other form of priors.

# 2016
## Terrain-Adaptive Locomotion Skills Using Deep Reinforcement Learning

This paper introduces MACE—a mixture of actor-critic experts—to train terrain-adaptive locomotion in simulated planar characters. It uses high-dimensional states including terrain height maps and parameterized action spaces to generate leaps and steps.

- The algorithm is a mixture of actor-critic experts (MACE). Multiple actor-critic pairs are trained jointly, and at each locomotion cycle the expert with highest Q-value is selected. Boltzmann exploration and initial actor biases encourage specialization.
- Action space is 29D, representing parameters for leaps/steps in an FSM-based low-level controller. The controller operates in locomotion cycle units, each cycle triggered by hind-leg touchdown.
- Observation includes 83D character state + 200D terrain heightfield ahead of the character.
- Low-level controller uses a finite-state machine with phases (leap, flight, touch-down) and PD or Jacobian-transpose forces to execute actions.
- Replay buffers: separate actor and critic buffers are used—actor buffer stores noisy exploratory actions, critic buffer stores deterministic ones—to improve stability.
- Training is in physics-based simulation; each learning iteration samples 32-cycle experiences from 50k buffer, with ~300k iterations needed for mixed terrains.
- Network architecture: shared backbone with multiple heads for each actor and critic; actor and critic networks likely small MLPs.
- Reward design: unspecified in detail but tailored to encourage dynamic, stable traversal across terrain irregularities.
- Key ideas: terrain-adaptive locomotion emerges via expert specialization; MACE converges faster and achieves better performance than single actor-critic. Complexity factors like expert count matter—MACE(2) and MACE(3) perform best. Boltzmann exploration and initial biases break symmetry early.
- Emergent behavior includes specialized motion patterns; experts activate variably depending on terrain (e.g., mixed, slopes, gaps).
- Generalization across terrain types demonstrated on dog and raptor models in planar sim.
- Contributions: direct use of high‑dimensional state/terrain inputs, MACE architecture, separate actor/critic buffers, Boltzmann exploration, actor biases enable efficient learning of terrain-adaptive skills.
- [https://github.com/xbpeng/DeepTerrainRL](https://github.com/xbpeng/DeepTerrainRL)