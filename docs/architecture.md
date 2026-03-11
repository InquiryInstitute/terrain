# Automata Architecture

System overview and data flow for the Castalia Automata embodied AI platform.

## Five-Layer System

```mermaid
flowchart TB
    subgraph Layer1["1. Physical Terrain Environment"]
        Bed["4×8 ft Terrain Bed"]
        Fill["Soil / Sand / Gravel"]
        Fiducials["Fiducial Markers"]
        Ramp["Vehicle Ramp"]
        Bed --> Fill
        Bed --> Fiducials
        Bed --> Ramp
    end

    subgraph Layer2["2. Autonomous Robots"]
        ESP["ESP32-S3"]
        Motors["Motor Drivers"]
        IMU["IMU / Encoders"]
        ToF["Depth / ToF"]
        ESP --> Motors
        ESP --> IMU
        ESP --> ToF
    end

    subgraph Layer3["3. Perception System"]
        RGB["RGB Camera"]
        Depth["Depth Camera"]
        April["AprilTag"]
        Map["3D Terrain Map"]
        RGB --> Map
        Depth --> Map
        April --> Map
    end

    subgraph Layer4["4. AI Planning Stack"]
        Server["Automata Server"]
        Planning["Planning"]
        Coord["Robot Coordination"]
        Server --> Planning
        Server --> Coord
    end

    subgraph Layer5["5. Simulation"]
        Isaac["NVIDIA Isaac Sim"]
        Policies["Trained Policies"]
        Isaac --> Policies
    end

    Layer1 <-->|"physical interaction"| Layer2
    Layer2 <-->|"DDS / micro-ROS"| Layer4
    Layer3 -->|"ground truth"| Layer4
    Layer4 -->|"deploy"| Layer2
    Layer5 <-->|"sim-to-real"| Layer4
```

## ROS2 / DDS Topology

```mermaid
flowchart LR
    subgraph Automata_Server["Automata Server (Jetson)"]
        Perception["/perception"]
        Terrain["/terrain_mapping"]
        Planning["/planning"]
        Coord["/robot_coordination"]
        SimBridge["/simulation_bridge"]
    end

    subgraph Robots["Robots (micro-ROS)"]
        R1["Excavator"]
        R2["Dump Truck"]
        R3["Bulldozer"]
    end

    Perception --> Terrain
    Terrain --> Planning
    Planning --> Coord
    Coord --> R1
    Coord --> R2
    Coord --> R3
    SimBridge -.->|"policy sync"| Coord

    Automata_Server <-->|"WiFi DDS"| Robots
```

## Sim-to-Real Loop

```mermaid
flowchart TD
    A[Isaac Sim] -->|train policies| B[ROS2 Deployment]
    B -->|real terrain| C[Physical Experiments]
    C -->|telemetry & outcomes| D[Data Feedback]
    D -->|improve models| A
```

## Remote Lab Access

```mermaid
flowchart TD
    User["User / Student"]
    Web["Web Interface"]
    Sched["Cloud Scheduler"]
    ROS["ROS2 Server"]
    Lab["Automata robots"]

    User -->|programs, models, experiments| Web
    Web --> Sched
    Sched --> ROS
    ROS --> Lab
    Lab -->|results| ROS
    ROS --> Web
    Web --> User
```

## Robot Node (ESP32-S3)

```mermaid
flowchart LR
    subgraph ESP32["ESP32-S3"]
        WiFi["WiFi"]
        PWM["PWM"]
        I2C["I2C/SPI"]
    end

    WiFi <-->|"DDS"| Server["Automata Server"]
    PWM --> Motors["H-bridge / Motors"]
    I2C --> IMU["IMU"]
    I2C --> ToF["ToF / Depth"]
```

See [Automata-Design.md](Automata-Design.md) for full specification.
