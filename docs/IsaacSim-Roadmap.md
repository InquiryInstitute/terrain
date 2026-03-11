# Isaac Sim Integration Roadmap

Automata uses **NVIDIA Isaac Sim** for simulation-based training and sim-to-real transfer. This document outlines the integration roadmap and milestones.

---

## Goals

1. **Train policies in simulation** — digging, grading, hauling, multi-robot coordination.
2. **Export to ROS2** — deploy trained policies to the Automata server and micro-ROS robots.
3. **Close the loop** — use real-world telemetry and terrain data to improve simulation and policies.

---

## Phase 1: Environment and Assets (Months 1–2)

### 1.1 Automata scene in Isaac Sim

- [ ] Create or import a **4×8 ft terrain bed** (USD stage) matching physical dimensions.
- [ ] Define **terrain materials** (soil, sand, gravel) with appropriate physics properties (friction, cohesion, particle behavior if using GPU particles).
- [ ] Add **fiducial markers** (AprilTag) and **calibration grid** in the scene for alignment with real bed.
- [ ] Optional: ramp, soil storage zone, irrigation placeholder.

### 1.2 Robot assets and kinematics

- [ ] **Excavator** (Huina 1550–scale): CAD or simplified URDF/USD with arm, bucket, tracks.
- [ ] **Dump truck**: body, dump bed articulation.
- [ ] **Bulldozer**: blade, tracks.
- [ ] **Joint and drive setup**: match real ESP32 PWM ranges and kinematics where possible.
- [ ] **Sensors**: raycast or simulated depth/ToF; IMU noise model.

### 1.3 ROS2 bridge (Isaac Sim ↔ ROS2)

- [ ] **Isaac Sim ROS2 bridge** (official or Isaac ROS): publish joint states, subscribe to commands.
- [ ] **Topic naming** aligned with Automata server (`/robot_*/cmd`, `/robot_*/state`, `/terrain_map`).
- [ ] **Clock sync** (sim time vs real time) for replay and debugging.

**Deliverable:** Reproducible Isaac Sim scene that loads Automata bed + robots and connects to ROS2.

---

## Phase 2: Terrain and Physics (Months 2–3)

### 2.1 Terrain deformation (optional but high value)

- [ ] Evaluate **Isaac Sim terrain / deformable** options (e.g. heightfield updates, particle-based).
- [ ] Implement **digging / grading** so bucket and blade interactions modify terrain.
- [ ] **Export height maps** for comparison with real overhead perception.

### 2.2 Contact and dynamics

- [ ] Tune **friction and contact** so robot behavior is plausible (no excessive slip, realistic digging resistance).
- [ ] **Soil dynamics**: if using particles, tune density and flow to approximate real soil/sand.

### 2.3 Perception in sim

- [ ] **Synthetic RGB/depth** from overhead camera matching real gantry pose.
- [ ] **AprilTag** detection in sim for robot localization.
- [ ] **Terrain height map** topic published in same format as real Automata server.

**Deliverable:** Simulated terrain that deforms under robot actions and produces perception outputs compatible with the real stack.

---

## Phase 3: Policy Training (Months 3–5)

### 3.1 Single-robot tasks

- [ ] **Reinforcement learning** (e.g. Isaac Gym/Isaac Sim RL) for:
  - Digging a trench.
  - Leveling a surface.
  - Loading a truck (excavator → dump truck).
- [ ] **Reward design**: terrain change, progress, energy, collision penalties.
- [ ] **Observation space**: joint states, terrain patch, goal (e.g. target height or region).

### 3.2 Multi-robot coordination

- [ ] **Excavator + truck + dozer**: shared task (e.g. “move pile from A to B and level”).
- [ ] **Communication**: simulated delays and packet loss to match WiFi/DDS.
- [ ] **Centralized vs decentralized** policies; compatibility with Automata server as coordinator.

### 3.3 Export and deployment

- [ ] **Policy export** to ONNX or TensorRT (or native Isaac format) runnable on Jetson.
- [ ] **ROS2 node** that loads policy, subscribes to Automata topics, publishes commands.
- [ ] **Safety**: velocity limits, workspace bounds, emergency stop — same as real robots.

**Deliverable:** Trained policies that run in ROS2 and can be tested on real Automata hardware.

---

## Phase 4: Sim-to-Real and Data Loop (Months 5–6)

### 4.1 Real-world data ingestion

- [ ] **Telemetry pipeline**: real robot states, terrain maps, and outcomes (e.g. final height map) logged and versioned.
- [ ] **Replay in Isaac Sim**: drive sim with recorded real trajectories to compare sim vs real.

### 4.2 Domain adaptation and calibration

- [ ] **Terrain calibration**: adjust sim terrain params (friction, stiffness) to minimize sim–real gap.
- [ ] **Sensor and actuation noise**: inject real noise profiles into sim for robustness.
- [ ] **Domain randomization** during training (already in Isaac): extend to Automata-specific params.

### 4.3 Continuous improvement

- [ ] **Failure and edge-case logging** from real runs → add to sim scenarios.
- [ ] **Curriculum**: train on increasingly hard sim scenarios informed by real failures.

**Deliverable:** Documented sim-to-real workflow and a pipeline that uses real data to refine simulation and policies.

---

## Dependencies and Tools

| Item | Purpose |
|------|--------|
| NVIDIA Isaac Sim | Scene, physics, sensors, RL (Isaac Gym integration) |
| Isaac ROS | ROS2 bridge, perception, optional GPU acceleration |
| Automata ROS2 stack | Planning, coordination, real robot interface |
| micro-ROS | On-robot (ESP32) nodes; same topic contract as sim |

---

## Success Criteria

- **Research:** Publishable results on embodied AI / terrain manipulation / sim-to-real with Automata.
- **Education:** Certification labs (Level 2–3) include “train in Isaac Sim, run on Automata” exercises.
- **Product:** Automata Research tier ships with an Isaac Sim scene and example policies.

---

## References

- [NVIDIA Isaac Sim](https://developer.nvidia.com/isaac-sim)
- [Isaac ROS](https://nvidia-isaac-ros.github.io/)
- [Automata Design](Automata-Design.md) · [Architecture](architecture.md)
