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
## 2. Comprehensive 3-Year PhD Research Roadmap

### Detailed Year 1: Foundations, System Modeling, & Baselines
* **Quarter 1–2: Systematic Literature Review (PRISMA) & Problem Formulation**
  * **PRISMA Protocol:** Execute a thorough review of IEEE Xplore, ACM, and arXiv across 2020–2026, targeting multi-agent communication constraints and LLM/agentic coordination.
  * **Mathematical Modeling:** Formulate the core problem as a Decentralized Partially Observable Markov Decision Process under Communication Constraints (**Dec-POMDP-Comms**), defining packet loss ($P_{loss}$), Signal-to-Interference-plus-Noise Ratio ($SINR$), and time-varying adjacency matrices $G(t)=(V, E(t))$.
  * **Deliverable:** Foundation survey draft / workshop paper.
* **Quarter 3–4: Dual-Layer Simulation Infrastructure & Classical Baselines**
  * **Simulation Build:** Set up the integrated dual-layer environment using Gazebo / ROS 2 combined with network emulation tools (EMANE or NS-3) to inject realistic network degradation.
  * **Baseline Implementation:** Implement standard reference algorithms (e.g., Consensus-Based Bundle Algorithm [CBBA] or basic leader-follower formations) to create performance benchmarks.
  * **Deliverable:** Functional simulation platform and baseline performance metrics under varying packet drop rates.

### Detailed Year 2: Agentic AI Integration & Resilient Protocols
* **Quarter 1–2: Onboard Agentic Framework Deployment**
  * **Edge Architecture:** Deploy local, quantized Small Language Models (SLMs like Phi-3 or Llama-3-8B) on simulated edge compute nodes.
  * **Framework Selection:** Implement **LangGraph** to manage predictable, deterministic agent state machines locally on each UAV, avoiding token-heavy multi-turn chat loops.
  * **Deliverable:** Single-node agentic reasoning pipeline with local tool-calling capabilities.
* **Quarter 3–4: Bandwidth-Aware Protocols & Fallback Topologies**
  * **Adaptive Communication:** Design protocols where agents dynamically shift from verbose token exchange to compact numerical state embeddings during low-bandwidth phases.
  * **Resilient Fallback Design:** Program local LangGraph fallback paths so that if peer connectivity is lost, individual drones seamlessly transition from global consensus to autonomous local execution.
  * **Deliverable:** Communication-adaptive swarm control architecture.

### Detailed Year 3: Stress Testing, Hardware Validation, & Defense
* **Quarter 1–2: Comprehensive Adversarial Stress Testing & Benchmarking**
  * **Adversarial Scenarios:** Subject the agentic swarm to extreme testing conditions, including active RF jamming, urban canyon non-line-of-sight (NLOS) blockages, and sudden multi-node failures.
  * **Comparative Evaluation:** Benchmark your Agentic AI Swarm against Traditional MARL and Fixed Rule-Based Swarms across key performance indicators (task completion time, resilience, and bandwidth efficiency).
  * **Deliverable:** Comprehensive benchmarking dataset and major journal manuscript submission.
* **Quarter 3–4: Hardware-in-the-Loop (HIL) & Thesis Completion**
  * **HIL Validation:** Deploy code onto physical micro-UAV hardware targets (e.g., NVIDIA Jetson Orin Nano running PX4/ArduPilot) to validate real-time execution constraints.
  * **Writing & Defense:** Finalize dissertation documentation, open-source the modular codebase, and complete the final oral defense.
  * **Deliverable:** Completed PhD dissertation, final defense presentation, and archived open-source repository.
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
