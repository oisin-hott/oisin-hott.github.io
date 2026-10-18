---
title: "6-DOF Robotic Arm"
permalink: /projects/robotic-arm/
excerpt: "A 6-DOF robotic arm designed and built from scratch: custom PCB, closed-loop motor control on a Teensy 4.1, and a full ROS2 / MoveIt2 software stack."
header:
  overlay_image: /assets/images/robotic-arm/hero.jpg
  overlay_filter: 0.5
  teaser: /assets/images/robotic-arm/teaser.jpg
  actions:
    - label: "View on GitHub"
      url: "https://github.com/oisin-hott/robot_arm_hardware"
toc: true
toc_label: "On this page"
toc_sticky: true
---

## Overview

A six-axis robotic arm designed, built and programmed from scratch as a personal project, started in late 2025. It covers the full mechatronics stack: a 3D-printed mechanical design featuring a cycloidal shoulder drive, a custom KiCad PCB with a Teensy 4.1 and TMC2209 stepper drivers, closed-loop control firmware running on the microcontroller, and a ROS2/MoveIt2 motion planning stack validated in Gazebo simulation. MK1 is in final assembly, with computer-vision pick-and-place planned as the next phase.


![Workbench](/assets/images/Workbench.jpg)

**Status:** MK1 in final assembly, making a few small hardware changes to smooth the motion. Full simulation stack complete.

## At a Glance

|||
|---|---|
| **Degrees of freedom** | 6 + servo gripper |
| **Actuation** | Stepper motors, cycloidal drive, differential wrist |
| **Sensing** | MT6701 magnetic encoders on each joint |
| **Controller** | Teensy 4.1 on a custom 2-layer PCB |
| **Software** | C++, ROS2, MoveIt2, Gazebo |
| **Tools** | KiCad, CAD (SolidWorks), 3D printing |

## System architecture

The key design decision was splitting responsibilities between the PC and the microcontroller:

- **PC (ROS2 / MoveIt2)** handles motion planning and sends joint targets in radians.
- **Teensy 4.1** runs closed-loop PID for every joint at kHz rates, reading the encoders directly.

Running the control loop on the Teensy rather than in ROS2 keeps timing deterministic and
independent of the PC's scheduling and communication latency. ROS2 is responsible for
*where* the arm should go, the embedded layer is responsible for *getting it there*.

<!-- TODO: add a block diagram image, e.g. PC → USB serial → Teensy → drivers/encoders -->
![System architecture](/assets/images/ControlArchitecture.svg)

## Mechanical design

- **Cycloidal drive** for high reduction with low backlash in a compact package.
- **Differential wrist** combining two motors to produce pitch and roll, with the mixing handled in software.
- **3D-printed structure** with designed-in bearing retention features.

<!-- TODO: add CAD renders / photos of the cycloidal drive and wrist -->
![Cycloidal drive](/assets/images/Cycloidal.png)

<!-- TODO: add a sentence or two on a specific mechanical challenge and how you solved it -->

## Electronics

A custom 2-layer PCB designed in KiCad brings the whole control system onto one board:

- Teensy 4.1 microcontroller
- 6 x TMC2209 stepper drivers
- 6 x MT6701 magnetic encoder connectors
- Power protection (reverse polarity) and power-integrity design (decoupling, regulated 5 V rail).
- Reverse polarity protection
- 5 V buck converter supplying the servo gripper

Design work included the power architecture, trace width calculations for motor currents,
copper pour strategy, and resolving zone priority and DRC issues. Components were selected
with an Irish/EU-sourced bill of materials.

<!-- TODO: add schematic and PCB layout screenshots, and a photo of the assembled board -->
![PCB layout](/assets/images/PCBKiCAD.png)

## Software

### Embedded firmware
- Per-joint closed-loop PID running at kHz on the Teensy
- Encoder feedback read directly by the microcontroller
- Receives radian targets from the PC

### ROS2 stack
- Custom **ROS2 hardware interface** written in C++ connecting `ros2_control` to the Teensy
- **MoveIt2** motion planning with the STOMP planner and TRAC-IK kinematics
- **Gazebo** simulation of the full arm with tuned physics, integrated with MoveIt2 and a custom motion sequence.
- Differential wrist mixer and a working multitask-task motion sequence in simulation


<video controls muted playsinline width="100%">
  <source src="/assets/images/Video Project.mp4" type="video/mp4">
</video>

### Challenges and lessons learned

#### 1. Planning that behaved on a real arm
- **Problem:** KDL gave erratic IK solutions and OMPL planned long, unwanted trajectories.
- **Fix:** Switched to TRAC-IK and the STOMP planner.
- **Lesson:** Default planners aren't always right for a physical arm, so test alternatives early.

#### 2. An intermittent startup bug
- **Problem:** Random gripper out-of-bounds errors.
- **Fix:** The motion node was starting before the gripper controller was active, so I chained the controller spawners in order.
- **Lesson:** Intermittent failures are usually ordering problems, so make startup dependencies explicit.

#### 3. Putting the control loop in the right place
- **Problem:** ROS2 isn't real time, but closed-loop stepper control needs a kHz loop.
- **Fix:** PID and unit conversion run on the Teensy, and the PC sends radian targets in a fixed 30-byte checksummed packet.
- **Lesson:** Keep timing-critical loops on the microcontroller and leave planning to the PC.

#### 4. Absolute position through a gearbox
- **Problem:** The MT6701 is single-turn absolute, so geared joints are ambiguous over multiple turns.
- **Fix:** Per-joint limit switches and a base-to-end-effector homing sequence, with motion blocked until all joints are homed.
- **Lesson:** Design sensing around what can go wrong, not just what works on the bench.

#### 5. Scoping the mechanical design
- **Problem:** The shoulder needs the most torque with low backlash, and independent wrist drives were too complex for MK1.
- **Fix:** A printed cycloidal drive at the shoulder and a differential wrist.
- **Lesson:** Match complexity to the version you're building, and save the harder option for MK2.

## Results

<!-- TODO: fill in once MK1 is running. Numbers make this section: repeatability, payload, reach, loop rate, cost -->
- Reach: 0.7m
- Payload: Designed to be 500g. Once the build is complete, this figure will be validated.
- Control loop rate: 1kHz.
- Total build cost: €464.72

## Next steps

- Bring up MK1 hardware and validate the control loop against the simulation
- Add computer vision: a fixed overhead camera with ArUco markers for workspace calibration
- Train a learned pick-and-place policy
- Design and implement a swappable end effector

## Links

- [Source code on GitHub](https://github.com/oisin-hott/robot_arm)
- [PCB files](https://github.com/oisin-hott/robot_arm_hardware)
- [CAD files](https://github.com/oisin-hott/robot_arm_hardware)
