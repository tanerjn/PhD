# PhD Research Roadmap: UAV Swarming in Degraded Communication Using Agentic AI

---

## 1. High-Level PhD Vision & Core Objectives

```
                        ┌──────────────────────────────────────────────┐
                        │              CORE RESEARCH GOAL             │
                        │ Multi-UAV Swarm Autonomy in Communication-   │
                        │ Denied / Degraded Environments via Agentic AI │
                        └──────────────────────┬───────────────────────┘
                                               │
             ┌─────────────────────────────────┼────────────────────────────────┐
             ▼                                 ▼                                ▼
┌──────────────────────────┐     ┌──────────────────────────┐     ┌──────────────────────────┐
│   Communication Loss     │     │ Dynamic Graph Topology   │     │  Agentic Reasoning & LLM │
│ Robustness (Jamming, RF) │     │ Consensus & Dec. Control │     │ Context-Aware Adaptation │
└──────────────────────────┘     └──────────────────────────┘     └──────────────────────────┘
```

When inter-UAV RF links suffer from **jamming, bandwidth dropouts, latency spikes, or total degradation**, traditional centralized controllers fail. Your thesis bridges **Agentic AI** (LLMs/SLMs, dynamic tool-use, symbolic reasoning, memory graphs) and **Distributed UAV Swarm Robotics**.

---

## 2. Comprehensive PhD Research Roadmap (4-Year Horizon)

### Year 1: Systematic Literature Review (PRISMA) & Problem Formulation
* **Q1–Q2:** Execute the **PRISMA Protocol** to map existing research at the intersection of Agentic AI, Multi-Agent Systems (MAS), and Communication-Degraded UAV Swarms.
* **Q3:** Define the mathematical model for communication degradation (Packet Loss Rate $P_{loss}$, Signal-to-Interference-plus-Noise Ratio $SINR$, Bandwidth constraints $B(t)$, and Time-Varying Adjacency Matrix $G(t)=(V, E(t))$).
* **Q4:** Formulate the Multi-Agent Markov Decision Process under Partial Observability and Communication Constraints (**Dec-POMDP-Comms**).

### Year 2: Simulation Environment Setup & Baseline Agentic Framework
* **Q1–Q2:** Construct the dual-layer simulation architecture (Physical Dynamics in ROS 2 / Gazebo / Isaac Sim paired with Network Emulation in EMANE / NS-3).
* **Q3:** Implement baseline swarm algorithms (e.g., Consensus-Based Bundle Algorithm [CBBA], Centralized Training with Decentralized Execution [CTDE-MARL], Dynamic Leader-Follower).
* **Q4:** Develop the single-node Agentic AI pipeline (Small Language Models [SLM] or Fine-Tuned Llama-3/Phi-3 onboard with local memory state and tool-calling capabilities).

### Year 3: Agentic Swarm Orchestration under Degraded Communication
* **Q1–Q2:** Design **Degraded-State Fallback Topologies** (e.g., transitioning from global LLM consensus to local dynamic graph execution via specialized edge agents).
* **Q3:** Introduce **Bandwidth-Aware Agent Communication Protocols** (e.g., state embedding passing rather than natural language tokens during low-bandwidth phases).
* **Q4:** Test resilience against active jamming, RF non-line-of-sight (NLOS) in urban canyons, and node loss.

### Year 4: Validation, Real-World Flight Tests, & Thesis Defense
* **Q1–Q2:** Real-world hardware-in-the-loop (HIL) validation using physical micro-UAVs (e.g., PX4/ArduPilot running on NVIDIA Jetson Orin Nano).
* **Q3:** Benchmark comparisons: Agentic AI Swarm vs. Traditional MARL vs. Fixed Rule-Based Swarm.
* **Q4:** Thesis writing, journal publications (IEEE Transactions / Autonomous Robots), and final defense.

---

## 3. Systematic Literature Review Strategy (PRISMA Method)

To build a rigorous foundation, apply the **PRISMA (Preferred Reporting Items for Systematic Reviews and Meta-Analyses)** framework.

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           1. IDENTIFICATION                             │
│   Search IEEE Xplore, ACM DL, ScienceDirect, Scopus, arXiv (2020–2026)  │
│   Query: ("UAV swarm" OR "drone fleet") AND ("degraded communication"   │
│   OR "communication-denied") AND ("agentic AI" OR "LLM multi-agent")     │
└────────────────────────────────────┬────────────────────────────────────┘
                                     │
                                     ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                               2. SCREENING                              │
│   • Remove duplicates                                                   │
│   • Screen titles and abstracts based on inclusion criteria:           │
│     - Multi-agent coordination under RF constraints                     │
│     - AI-driven dynamic decision making                                 │
└────────────────────────────────────┬────────────────────────────────────┘
                                     │
                                     ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                              3. ELIGIBILITY                             │
│   • Full-text screening                                                 │
│   • Exclude non-peer-reviewed blog posts or pure hardware papers        │
└────────────────────────────────────┬────────────────────────────────────┘
                                     │
                                     ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                              4. INCLUSION                               │
│   Synthesize papers into Taxonomy: Algorithms, Simulators, Comms Models  │
└─────────────────────────────────────────────────────────────────────────┘
```

### Key Databases & Search String
* **Databases:** IEEE Xplore, ACM Digital Library, Scopus, ScienceDirect, arXiv, Google Scholar.
* **Search String:**
  `("UAV" OR "drone" OR "unmanned aerial vehicle") AND ("swarm" OR "multi-agent") AND ("degraded communication" OR "bandwidth limited" OR "communication denied" OR "jamming") AND ("agentic AI" OR "large language model" OR "LLM orchestration" OR "autonomous reasoning")`

---

## 4. Evaluation of Simulation Platforms for Degraded Communication

To evaluate degraded communication, a simulator **must support both realistic physics/sensors AND networking emulation**. 

| Simulation Platform | Visual & Physics Realism | Scalability (Drones) | Hardware/Compute Cost | Network Degradation Integration | Best Used For |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Gazebo / Ignition (ROS 2)** | Moderate | High (10–50+) | Low (CPU-heavy) | **Native / High** (Easy integration with NS-3, EMANE, ROS 2 QoS) | **Primary benchmark platform for robotics & RF networking** |
| **Isaac Sim / Pegasus Simulator** | **State of the Art** | **Extreme** (100+) | High (NVIDIA RTX GPU required) | Moderate (Requires custom extension or ROS 2 bridge to EMANE) | **Massive parallel training, vision-based navigation, high-density swarms** |
| **AirSim / Colosseum Fork** | High (Unreal Engine) | Moderate (5–15) | High | Low (Requires external ROS 2 network wrapper) | High-fidelity vision sensors; *Note: AirSim was archived by Microsoft* |
| **CARLA** | High | Low (Urban driving focus) | High | Low | Autonomous driving / ground vehicles; **Not recommended for UAV swarms** |

### Platform Recommendations
1. **Discard CARLA:** CARLA is optimized for ground autonomous vehicles, traffic, and urban street layouts. It lacks native 3D aerial aerodynamics and UAV multi-vehicle networking pipelines.
2. **AirSim Status:** Microsoft discontinued AirSim. If you choose UE5, use the **Colosseum** open-source fork.
3. **Primary Recommendation (Gazebo + ROS 2 + EMANE):** The most flexible setup for network-degraded swarms. You can programmatically alter ROS 2 Quality of Service (QoS) profiles, induce packet loss, latency, and disconnection directly via `EMANE` (Emulated Model Execution Network Environment) or `NS-3`.
4. **Secondary Recommendation (Isaac Sim + Pegasus Simulator):** Best if your agentic AI heavily relies on visual inputs (cameras/LiDAR) or if you plan to simulate 50+ drones simultaneously using Omniverse GPU parallel acceleration.

---

## 5. Architectural Comparison: LangGraph vs. AutoGen vs. Lightweight Frameworks

When deploying Agentic AI across a physical UAV network in degraded conditions, framework selection depends on **topology predictability** and **execution latency**.

```
    LangGraph (State-Graph Driven)              AutoGen (Conversational / Dynamic)
      [UAV 1 Agent] ──(Shared State)──┐            [UAV 1 Agent] ◄──(Chat Tokens)──► [UAV 2 Agent]
            │                         │                  ▲                              ▲
            ▼                         ▼                  │                              │
      [UAV 2 Agent] ───────────► [Action Engine]          └───────────► [LLM Manager] ────┘
  (Predictable, Deterministic Flow, Token Efficient)   (Dynamic multi-turn debate, Token Heavy)
```

| Feature / Metric | **LangGraph** | **AutoGen** | **Custom ROS 2 Micro-Agents (e.g., Ollama/vLLM + Pydantic)** |
| :--- | :--- | :--- | :--- |
| **Execution Pattern** | Directed Acyclic / Cyclic Graphs | Dynamic Conversational Loops | Event-driven State Machine |
| **Network Sensitivity** | **Low** (Can run localized state machine per node) | **High** (Requires high bandwidth for multi-turn chat loops) | **Minimal** (Uses structured JSON / Protobuf) |
| **Determinism** | **High** (Explicit node transitions) | Low (Emergent, variable conversations) | **Very High** |
| **Token Overhead** | Moderate | Very High | **Low** |
| **Suitability for Swarms** | **Ideal for Onboard Mission Control** | Good for Centralized Strategic Planning | **Best for Flight-Critical Edge Control** |

### Framework Verdict
* **Is AutoGen suitable?** **No for real-time onboard control.** AutoGen relies on verbose conversational exchanges between agents. When communication drops, multi-turn chat loops stall, leading to high latency and system failure.
* **Is LangGraph suitable?** **Yes, for high-level tactical mission orchestration.** LangGraph lets you model state transitions as explicit graph nodes. If inter-UAV communication breaks, a node can immediately execute a local fallback branch without waiting for network responses.
* **Real-World / Edge Deployment Recommendation:**
  * **Top Level (Tactical Agentic Layer):** Use **LangGraph** or **CrewAI** running locally on each UAV's onboard compute (e.g., Jetson Orin) using quantized SLMs (3B–8B parameters).
  * **Bottom Level (Flight Control Layer):** Bypass LLM frameworks completely for flight control loops. Use standard **ROS 2 / MAVROS / MAVSDK** micro-services communicating via low-overhead binary serializations (Protobuf or ROS 2 DDS messages).

---

## 6. Key Sources to Monitor for Latest Developments

To stay up-to-date throughout your PhD:

1. **Top Journals & Conferences:**
   * *IEEE Transactions on Robotics (T-RO)*
   * *IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)*
   * *IEEE International Conference on Robotics and Automation (ICRA)*
   * *Autonomous Robots (Springer)*
   * *AAMAS (Autonomous Agents and Multiagent Systems)*
2. **ArXiv Feeds & Subject Classifications:**
   * `cs.RO` (Robotics)
   * `cs.MA` (Multiagent Systems)
   * `cs.AI` (Artificial Intelligence)
   * `cs.NI` (Networking and Internet Architecture)
3. **Open-Source Repositories & Reputable Benchmarks:**
   * **Pegasus Simulator:** `PegasusSimulator/PegasusSimulator` (Omniverse Isaac Sim framework for UAVs).
   * **PX4 Autopilot & ArduPilot:** Official GitHub repositories for Software-in-the-Loop (SITL).
   * **LangChain / LangGraph Repositories:** For state-machine updates in multi-agent agentic workflows.