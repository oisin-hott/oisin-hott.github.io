---
title: "Embedded Control & ROS2 Hardware Interface"
permalink: /projects/robotic-arm/control/
excerpt: "Closed-loop PID on a Teensy 4.1 at kHz rates, with ROS2 sending joint targets through a custom C++ hardware interface."
header:
  overlay_image: /assets/images/robotic-arm/control-hero.jpg
  overlay_filter: 0.5
  teaser: /assets/images/robotic-arm/control-teaser.jpg
toc: true
toc_sticky: true
---

[← Back to the robotic arm overview](/projects/robotic-arm/)

## The problem

ROS2 is excellent for motion planning, but a desktop operating system can't guarantee when a control loop
will run. If joint control ran on the PC, communication delays and scheduling jitter would feed straight
into the arm's motion.

## The architecture decision

I split the system into two layers, each doing the job it's best suited to:

| Layer | Runs on | Responsibility |
|---|---|---|
| Planning | PC (ROS2 / MoveIt2) | Decides *where* the arm should go, sends joint targets in radians |
| Control | Teensy 4.1 | Gets each joint *there*: reads encoders, runs PID, commands the drivers |

Because the PID loop runs on the microcontroller at kHz rates, its timing is deterministic and independent
of the PC. The PC only needs to send targets fast enough for smooth trajectories.

<!-- TODO: block diagram, PC → USB → Teensy → drivers and encoders -->
![Control architecture](/assets/images/robotic-arm/control-architecture.png)

## Firmware

<!-- TODO: fill in -->
- Control loop rate: TODO kHz
- Encoder reading: TODO (interface, e.g. SSI/I2C/ABZ)
- PID tuning approach: TODO
- Safety: TODO (e.g. joint limits, watchdog if the PC stops sending targets)

## ROS2 hardware interface

A custom `ros2_control` hardware interface, written in C++, connects the ROS2 controllers to the Teensy.
It sends joint position targets and reads back joint states, so MoveIt2 sees the real arm exactly as it
sees the simulated one.

<!-- TODO: message format and rate, how you handle connection loss, a short code snippet -->

```cpp
// TODO: paste a short, representative snippet, e.g. your write() function
```

## Development setup

The physical robot runs from a dedicated Linux laptop connected to the Teensy, while I write code and run
simulation on my main PC and sync through GitHub. I moved to this setup after USB passthrough in WSL proved
unreliable for talking to the microcontroller.

## What went wrong

<!-- TODO: real issues from bring-up, e.g. communication timing, encoder noise, PID tuning -->

## Results

<!-- TODO: step response plot, tracking error, loop timing measurements -->
