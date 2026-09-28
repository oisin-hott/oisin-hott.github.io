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
      url: "https://github.com/YOUR-GITHUB-USERNAME/YOUR-REPO"
toc: true
toc_label: "On this page"
toc_sticky: true
---

## Overview

A six-axis robotic arm designed, built and programmed end to end as a personal project, started in late 2025.
The goal was to work across the full mechatronics stack: mechanical design, custom electronics,
embedded firmware and ROS2 motion planning, and to make each layer do the job it is best suited to.

<video controls muted playsinline width="100%">
  <source src="/assets/images/Video Project.mp4" type="video/mp4">
</video>

**Status:** MK1 in final assembly, waiting on the last electronics. Full simulation stack complete.

| | |
|---|---|
| **Degrees of freedom** | 6 + servo gripper |
| **Actuation** | Stepper motors, cycloidal drive, differential wrist |
| **Sensing** | MT6701 magnetic encoders on each joint |
| **Controller** | Teensy 4.1 on a custom 2-layer PCB |
| **Software** | C++, ROS2, MoveIt2, Gazebo |
| **Tools** | KiCad, CAD (TODO: name your CAD package), 3D printing |

## System architecture

The key design decision was splitting responsibilities between the PC and the microcontroller:

- **PC (ROS2 / MoveIt2)** handles motion planning and sends joint targets in radians.
- **Teensy 4.1** runs closed-loop PID for every joint at kHz rates, reading the encoders directly.

Running the control loop on the Teensy rather than in ROS2 keeps timing deterministic and
independent of the PC's scheduling and communication latency. ROS2 is responsible for
*where* the arm should go, the embedded layer is responsible for *getting it there*.

<!-- TODO: add a block diagram image, e.g. PC → USB serial → Teensy → drivers/encoders -->
![System architecture](/assets/images/robotic-arm/architecture.png)

## Mechanical design

- **Cycloidal drive** for high reduction with low backlash in a compact package.
- **Differential wrist** combining two motors to produce pitch and roll, with the mixing handled in software.
- **3D-printed structure** with designed-in bearing retention features.

<!-- TODO: add CAD renders / photos of the cycloidal drive and wrist -->
![Cycloidal drive](/assets/images/robotic-arm/cycloidal-drive.jpg)

<!-- TODO: add a sentence or two on a specific mechanical challenge and how you solved it -->

## Electronics

A custom 2-layer PCB designed in KiCad brings the whole control system onto one board:

- Teensy 4.1 microcontroller
- TMC2209 stepper drivers
- MT6701 magnetic encoder connectors
- Reverse polarity protection
- 5 V buck converter supplying the servo gripper

Design work included the power architecture, trace width calculations for motor currents,
copper pour strategy, and resolving zone priority and DRC issues. Components were selected
with an Irish/EU-sourced bill of materials.

<!-- TODO: add schematic and PCB layout screenshots, and a photo of the assembled board -->
![PCB layout](/assets/images/robotic-arm/pcb-layout.png)

## Software

### Embedded firmware
- Per-joint closed-loop PID running at kHz on the Teensy
- Encoder feedback read directly by the microcontroller
- Receives radian targets from the PC

### ROS2 stack
- Custom **ROS2 hardware interface** written in C++ connecting `ros2_control` to the Teensy
- **MoveIt2** motion planning with the STOMP planner and TRAC-IK kinematics
- **Gazebo** simulation with tuned physics, built from scratch
- Differential wrist mixer and a working four-task motion sequence in simulation

<!-- TODO: add a Gazebo / RViz screenshot or GIF -->
![MoveIt2 simulation](/assets/images/robotic-arm/moveit-sim.gif)

## Challenges and lessons learned

<!-- TODO: pick 2–3 real problems and write them as problem → approach → result. Examples from this build: -->
- **Planner configuration:** getting the STOMP pipeline working in MoveIt2 and migrating to TRAC-IK.
- **Simulation stability:** tuning Gazebo physics so the simulated arm behaved realistically.
- **C++ debugging:** tracking down segfaults in the ROS2 stack.

## Results

<!-- TODO: fill in once MK1 is running. Numbers make this section: repeatability, payload, reach, loop rate, cost -->
- Reach: TODO
- Payload: TODO
- Control loop rate: TODO kHz
- Total build cost: TODO

## Next steps

- Bring up MK1 hardware and validate the control loop against the simulation
- Add computer vision: a fixed overhead camera with ArUco markers for workspace calibration
- Train a learned pick-and-place policy

## Links

- [Source code on GitHub](https://github.com/YOUR-GITHUB-USERNAME/YOUR-REPO)
- [PCB files](https://github.com/YOUR-GITHUB-USERNAME/YOUR-REPO)
- [CAD files](https://github.com/YOUR-GITHUB-USERNAME/YOUR-REPO)
