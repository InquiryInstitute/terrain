# Castalia Automata

## Embodied AI Robotics Sandbox

**An open research platform for embodied intelligence.**

Automata is a physical robotics environment where autonomous construction vehicles learn to interact with real terrain.

Inspired by robotics research labs and construction automation systems, Automata brings embodied AI experimentation into homes, schools, and research environments.

The platform combines:

- **AI**
- **robotics**
- **simulation**
- **perception**
- **real terrain interaction**

to create a closed-loop learning system between simulation and reality.

---

## Vision

Most AI today is **disembodied**.

Large language models reason in text but have no physical understanding of the world.

Yet intelligence evolved through interaction with matter.

*Digging. Moving. Building. Shaping terrain.*

Automata explores a fundamental question:

**What happens when AI learns by shaping the physical world?**

---

## Core Concept

A **4×8 ft** robotics terrain bed containing soil, sand, or gravel where autonomous miniature construction vehicles operate.

Robots perform tasks such as:

- digging
- grading
- trenching
- hauling
- terrain leveling

while AI models learn physics-aware planning.

---

## System Overview

Automata integrates five layers:

1. **Physical Terrain Environment**
2. **Autonomous Robots**
3. **Perception System**
4. **AI Planning Stack**
5. **Simulation (Isaac Sim)**

---

## Physical Environment

### Terrain Bed

**Standard size: 4 ft × 8 ft**

This matches common greenhouse and garden bed sizes.

| parameter | value    |
|-----------|----------|
| length    | 8 ft     |
| width     | 4 ft     |
| depth     | 12–18 in |
| material  | wood or steel frame |
| fill      | soil / sand / gravel |

### Terrain Materials

The system can operate with:

- construction sand
- soil
- gravel
- compost
- mixed substrates

Each material produces different AI learning conditions.

### Bed Features

The bed includes:

- vehicle ramp entry
- soil storage zone
- fiducial markers
- calibration grid

**Optional:**

- irrigation system
- moisture sensors
- plant growth experiments

---

## Robots

The platform uses modified RC construction vehicles.

**Example base models:**

- Huina 1550 excavator
- Huina 1580 excavator
- RC dump truck
- RC bulldozer

### Robot Dimensions

Example excavator (Huina 1550):

| parameter | value   |
|-----------|---------|
| length    | 25 cm   |
| width     | 16 cm   |
| height    | 16 cm   |
| scale     | 1:14    |

This size works well inside a 4×8 terrain bed. The bed length fits approximately **10 excavator lengths**.

### Robot Hardware Architecture

Each robot is upgraded to become an AI robotics node.

**Core compute: ESP32-S3**

Functions:

- motor control
- sensor interface
- wireless communication

### Robot Electronics

| component    | details |
|-------------|---------|
| **Core board** | ESP32-S3 — WiFi, BLE, vector instructions, sufficient compute for edge control |
| **Motor drivers** | H-bridge drivers, PWM control |

### Sensors

Each robot may include:

| sensor       | purpose           |
|-------------|-------------------|
| IMU         | orientation       |
| encoders    | motion tracking   |
| depth camera| terrain perception|
| ToF sensor  | obstacle detection|

### Depth Perception

**Example module:** Sipeed MaixSense A010 RGBD ToF camera  
**Approx cost:** $30  

Features:

- depth sensing
- onboard RISC-V processor
- USB / UART interface
- ROS compatibility

### Communication

Robots communicate with the main system via **WiFi**.

**Protocol:** ROS2 DDS  

Edge nodes run **micro-ROS**.

---

## ROS2 Architecture

The system uses ROS2 for coordination.

```
Automata Server (ROS2)
 ├ perception
 ├ terrain mapping
 ├ planning
 ├ robot coordination
 └ simulation bridge
      │
      │ DDS
      │
 Robots (micro-ROS)
```

### Central Compute

Automata requires a central robotics server.

**Recommended:** NVIDIA Jetson Orin Nano  

Capabilities:

- GPU acceleration
- ROS2
- Isaac ROS
- CV pipelines

**Alternative:** mini PC.

---

## Perception System

Automata includes an **overhead perception gantry**.

**Purpose:**

- track robots
- map terrain
- generate ground truth data

### Sensors

| sensor           | role                |
|------------------|---------------------|
| RGB camera       | tracking            |
| depth camera     | terrain mapping     |
| LIDAR            | optional            |
| AprilTag detection | robot localization |

### Terrain Mapping

The system constructs **3D terrain height maps**.

Used for:

- planning
- reinforcement learning
- simulation calibration

---

## Simulation

The system integrates with **NVIDIA Isaac Sim**.

This allows:

- simulation training
- policy development
- transfer learning

### Sim-to-Real Loop

```
Isaac Sim
   │
   │ train policies
   │
   ▼
ROS2 deployment
   │
   │ real terrain experiments
   │
   ▼
data feedback
   │
   │ improve policies
   │
   ▼
Isaac Sim
```

---

## Research Applications

Automata enables experiments in:

- **Embodied AI** — How intelligence emerges through physical interaction.
- **Terrain manipulation** — Robots learn to modify terrain structures.
- **Multi-agent coordination** — e.g. excavator loads truck, truck transports soil, dozer levels terrain.
- **Reinforcement learning** — Tasks: digging trenches, leveling surfaces, constructing shapes.
- **Construction robotics** — Similar research: Caterpillar, Built Robotics, MIT CSAIL.

---

## Remote Lab

Automata can be exposed as a **remote robotics lab**.

Students access robots through a **web interface**.

They submit:

- programs
- AI models
- experiments

The lab executes them physically.

### Remote Lab Architecture

```
User
 │
Web interface
 │
Cloud scheduler
 │
ROS2 server
 │
Automata robots
```

---

## Certification Programs

Automata supports certification tracks.

### Automata Level 1 — Robotics Foundations

Topics: robot kinematics, sensors, microcontrollers, ROS basics.

### Automata Level 2 — Embodied AI

Topics: reinforcement learning, terrain perception, planning.

### Automata Level 3 — Autonomous Construction

Topics: multi-robot coordination, excavation planning, terrain modeling.

### Educational Value

Students learn **robotics**, **AI**, **physics**, and **systems engineering** through physical experimentation.

---

## Estimated Costs

### Robots

| component           | cost     |
|--------------------|----------|
| RC excavator base  | $25      |
| ESP32 board        | $5       |
| motor drivers       | $6       |
| sensors            | $20–30   |

**Robot total:** $50–70

### Terrain bed

| item       | cost  |
|------------|-------|
| wood frame | $150  |
| soil       | $50   |
| ramp       | $30   |

**Bed total:** ~$250

### Perception system

| item         | cost  |
|--------------|-------|
| RGB camera   | $50   |
| depth sensor | $150  |
| mount        | $80   |

### Compute

**Jetson Orin Nano:** $500

### Example System Cost

| item           | cost   |
|----------------|--------|
| robots (3)     | $200   |
| bed            | $250   |
| sensors        | $200   |
| compute        | $500   |

**Total ≈ $1,150**

---

## Product Strategy

Two product tiers.

### Automata Home

**Price target:** $599–$899  

Includes: bed kit, one robot, sensors, software.

### Automata Research

**Price target:** $2,000–$4,000  

Includes: multiple robots, perception gantry, ROS2 stack, Isaac Sim integration.

### Open Research Platform

Automata is intended to be:

- open source
- extensible
- modular

Researchers can add new vehicles, new tasks, and new AI models.

---

## Why This Matters

AI must eventually understand **matter**, **physics**, **force**, and **terrain**.

Language alone is insufficient.

Automata allows AI to learn **how the world pushes back**.

---

## Tagline

**Automata: Where AI Learns to Move Earth.**
