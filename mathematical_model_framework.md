# Mathematical Model Framework: UAV Swarming in Degraded Communication Using Agentic AI

## 1. System Mathematical Architecture

The overall system is modeled as a **Decentralized Partially Observable Markov Decision Process under Communication Constraints** ($\text{Dec-POMDP-Comms}$), extended with an **Agentic Belief & Intent Engine**:

$$\mathcal{M} = \left\langle \mathcal{N}, \mathcal{S}, \{\mathcal{A}_i\}, \mathcal{P}, \{\Omega_i\}, \mathcal{O}, \mathcal{G}(t), \mathcal{B}_i, \mathcal{R} \right\rangle$$

Where:
* $\mathcal{N} = \{1, 2, \dots, N\}$ is the set of $N$ UAV agents in the swarm.
* $\mathcal{S}$ is the global environmental state space.
* $\mathcal{A}_i$ is the action space for UAV $i$.
* $\mathcal{G}(t) = (\mathcal{V}, \mathcal{E}(t))$ is the time-varying communication graph topology.
* $\mathcal{B}_i$ is the local agentic belief/state space maintained by Agentic AI (LLM/SLM context + local memory).

---

## 2. Core Mathematical Components

### A. UAV Physical & Kinematic State
The physical continuous state of UAV $i$ at time $t$ is denoted as $x_i(t) \in \mathbb{R}^{12}$:

$$x_i(t) = \begin{bmatrix} p_i(t) \\ v_i(t) \\ R_i(t) \\ \omega_i(t) \end{bmatrix} \in \mathbb{R}^{12}$$

* $p_i(t) = [x, y, z]^T \in \mathbb{R}^3$: 3D Position in World Frame.
* $v_i(t) = [\dot{x}, \dot{y}, \dot{z}]^T \in \mathbb{R}^3$: Linear Velocity.
* $R_i(t) \in \text{SO}(3)$: Attitude (Rotation matrix / Euler angles $[\phi, \theta, \psi]^T$).
* $\omega_i(t) \in \mathbb{R}^3$: Angular Velocity.

The discrete-time motion update governed by onboard low-level controllers is:
$$x_i(t+1) = f_{\text{dyn}}(x_i(t), u_i(t)) + w_i(t)$$
where $u_i(t)$ is the control input (thrust/torque) and $w_i(t) \sim \mathcal{N}(0, Q_i)$ is process noise.

---

### B. Time-Varying Communication Graph & Degradation Model
Communication between UAVs is captured by a dynamic directed graph $\mathcal{G}(t) = (\mathcal{V}, \mathcal{E}(t))$, where edge $(i, j) \in \mathcal{E}(t)$ exists if UAV $i$ can successfully send a packet to UAV $j$ at time $t$.

#### 1. Signal-to-Interference-plus-Noise Ratio (SINR)
The link between UAV $i$ and UAV $j$ depends on distance $d_{ij}(t) = \Vert{}p_i(t) - p_j(t)\Vert{}$, transmit power $P_i$, and RF degradation:

$$\text{SINR}_{ij}(t) = \frac{P_i \cdot h_{ij}(t) \cdot d_{ij}(t)^{-\alpha}}{\sigma_0^2 + J_j(t)}$$

* $h_{ij}(t) \sim \text{Rayleigh/Rician}$: Small-scale multipath fading parameter.
* $\alpha \in [2, 4]$: Path loss exponent.
* $\sigma_0^2$: Thermal noise power.
* $J_j(t)$: External jamming signal power at receiver $j$.

#### 2. Communication Degradation Matrix (Packet Loss & Latency)
Define edge weight $w_{ij}(t) \in [0, 1]$ representing the **Link Reliability / Success Probability**:

$$w_{ij}(t) = \mathbb{P}(\text{Packet Received}_{ij}) = \exp \left( -\gamma \cdot \frac{1}{\text{SINR}_{ij}(t)} \right) \cdot (1 - \text{BER}(\text{SINR}_{ij}))^{L}$$

Where $L$ is packet length in bits, and $\text{BER}$ is Bit Error Rate.

* **Packet Drop Probability:** $P_{\text{loss}, ij}(t) = 1 - w_{ij}(t)$
* **Communication Latency:** $\tau_{ij}(t) = \tau_{\text{prop}} + \tau_{\text{proc}} + \frac{L}{B_{ij}(t)}$
  where $B_{ij}(t)$ is the available bandwidth.

---

### C. Agentic AI State & Belief Formulation
Traditional MARL models represent agent state as fixed feature vectors. In this formulation, **Agentic AI introduces unstructured and structured contextual states**.

Define the local Agentic Context $\mathcal{C}_i(t)$ for UAV $i$:

$$\mathcal{C}_i(t) = \left\langle \mathcal{H}_i(t), \mathcal{M}_i(t), \mathcal{I}_i(t), \hat{\mathcal{S}}_{-i}(t) \right\rangle$$

* $\mathcal{H}_i(t)$: History buffer of past $k$ observations and received messages.
* $\mathcal{M}_i(t)$: Episodic & Semantic Memory graph (symbolic knowledge stored onboard).
* $\mathcal{I}_i(t) \in \mathcal{I}_{\text{swarm}}$: High-level tactical **Intent/Goal** (e.g., "RECON_ZONE_A", "RELAY_SEARCH", "RETURN_HOME").
* $\hat{\mathcal{S}}_{-i}(t)$: Agent $i$'s estimate/belief of neighboring agents' states $x_j(t)$ for $j \neq i$.

#### Consensus & Belief Propagation under Degradation
When UAV $j$ goes silent due to communication loss ($w_{ij}(t) < \epsilon_{\text{threshold}}$), UAV $i$ updates its belief about UAV $j$ using an onboard **Agentic Predictor / Extended Kalman Filter (EKF)**:

$$\hat{x}_j(t+1 \mid t) = f_{\text{dyn}}(\hat{x}_j(t \mid t), \hat{u}_j(t))$$

Where $\hat{u}_j(t) = \text{LLM\_Predict}(\mathcal{C}_i(t), \text{Intent}_j)$ uses the Agentic AI layer to reason about UAV $j$'s most likely objective given its last known intent.

---

### D. Hierarchical Decision-Making & Action Architecture
┌───────────────────────────┐
                      │   Agentic AI Layer (LLM)  │
                      │   Timescale: Δt_high ~ 1s │
                      └─────────────┬─────────────┘
                                    │ High-Level Intent / Target
                                    ▼
                      ┌───────────────────────────┐
                      │  Low-Level Controller     │
                      │  (MPC / PX4 Flight Core)  │
                      │  Timescale: Δt_low ~ 10ms │
                      └─────────────┬─────────────┘

1. **High-Level Agentic Strategy Selection ($\pi_{\text{agent}}$):**
   $$a_i^{\text{high}}(t) = \pi_{\text{agent}}(\mathcal{C}_i(t), w_{ij}(t)) \in \mathcal{A}_{\text{intent}}$$
   *Examples:* `[Form_Relay_Chain, Switch_To_Autonomous_Explore, Compress_Telemetry_Token, Silent_Hover]`

2. **Low-Level Trajectory Generation ($\pi_{\text{control}}$):**
   $$u_i(t) = \text{MPC}\left(x_i(t), a_i^{\text{high}}(t), \hat{x}_{-i}(t)\right)$$

---

### E. Global Multi-Objective Optimization Problem

The framework maximizes swarm task efficiency while minimizing communication cost and collision risk:

$$\max_{\pi_1, \dots, \pi_N} \mathbb{E} \left[ \sum_{t=0}^{T} \sum_{i=1}^{N} \gamma^t \left( R_{\text{task}}(x_i(t), a_i(t)) - \lambda_1 R_{\text{comm}}(B_{ij}(t)) - \lambda_2 R_{\text{uncertainty}}(\hat{\mathcal{S}}_{-i}(t)) \right) \right]$$

Subject to constraints:
1. **Safety Distance:** $\Vert{}p_i(t) - p_j(t)\Vert{} \ge d_{\text{safe}}, \quad \forall i \neq j$
2. **Bandwidth Limit:** $\sum_{j \in \mathcal{N}_i} \text{Bits}(a_{ij}(t)) \le B_{\max}(t)$
3. **Dynamics Constraints:** $u_i(t) \in \mathcal{U}_{\text{admissible}}$

---

## 3. Design Considerations for Model Development

### 1. Scale Heterogeneity & Dual Timescales
LLM-based agentic reasoning operates at $100\text{ms} - 2\text{s}$ per inference pass, whereas UAV flight dynamics require control updates every $10\text{ms} - 20\text{ms}$. The mathematical framework must maintain a strict two-tier execution model separating continuous high-frequency dynamics from discrete low-frequency belief and intent updates.

### 2. Token Overhead vs. RF Bandwidth Limitations
Standard Agentic AI relies on natural language prompts consuming kilobytes of data, which is impractical over jammed or degraded tactical RF links ($1 - 10 \text{ kbps}$). The model requires an adaptive semantic compression operator $\mathcal{K}_{\text{compress}}(\cdot)$ that transitions from symbolic payloads under nominal communication to low-dimensional vector embeddings $\mathbf{e}_i \in \mathbb{R}^d$ or silent deterministic belief propagation under severe degradation.

### 3. Graceful Degradation & Network Bounding
Mathematical proofs for swarm consensus usually assume bounded graph connectivity over time. The system design must maintain stability even during complete communication blackout periods ($\mathcal{E}(t) = \emptyset$), utilizing onboard agentic belief engines to prevent spatial drift and collision.

### 4. Intent Modeling over Direct State Observation
In communication-denied environments, exact real-time telemetry cannot be transmitted continuously. Modeling focuses on predicting high-level agent intent ($\mathcal{I}_j$) rather than direct state coordinates, allowing neighboring nodes to infer dynamics deterministically during silent periods.

### 5. Benchmark Comparison Framework
The mathematical formulation must enable direct comparative evaluation against established baselines:
* **Baseline 1:** Centralized Multi-Agent Reinforcement Learning (MARL) under full communication assumptions.
* **Baseline 2:** Classical Consensus-Based Bundle Algorithm (CBBA) under high packet drop rates.
* **Proposed Approach:** Agentic AI Swarm with Adaptive Context & Fallback Topologies.
