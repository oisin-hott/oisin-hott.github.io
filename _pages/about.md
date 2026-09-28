---
permalink: /about/
title: "About"
---

I'm a mechanical engineer based in Dublin with an MSc in Mechanical Engineering from University College Dublin.
I work where mechanical design meets electronics and software, and I'm most interested in building
physical systems that sense, decide and move.

<!-- TODO: optional one-liner on what you're looking for, e.g. "I'm interested in robotics systems, mechatronics and R&D engineering roles." -->

## Background

**Education.** My MSc thesis focused on vehicular fluid dynamics, where I
<!-- TODO: one sentence on what you modelled or tested, the tools you used (e.g. CFD package), and how you validated it -->.
It taught me how to take an open-ended technical problem, build a model of it, and check that model against real data.

**Formula Student.** As part of UCD's Formula Student team I worked on the design and build of a race car,
<!-- TODO: your area, e.g. aero, chassis, suspension, and one concrete thing you designed or delivered -->.
It was my first experience of engineering to a deadline, as part of a team, where parts had to be designed,
manufactured and made to work together.

**Industry.** I currently work as a Flight Simulation Engineer at Ryanair, maintaining and troubleshooting
full-flight simulators used for pilot training. The work is safety-critical and has made me methodical about
diagnosing faults across mechanical, electrical and software systems, often under time pressure.

<!-- TODO (optional): a sentence on earlier roles, e.g. process engineering or mechanical design / CAD work -->

## Robotics

Robotics is where all of this comes together for me. My main project is a
[6-DOF robotic arm](/projects/robotic-arm/) that I've designed and built from scratch alongside full-time work.

It covers the full mechatronics stack:

- **Mechanical:** a 3D-printed arm with a cycloidal drive and a differential wrist
- **Electronics:** a custom KiCad PCB built around a Teensy 4.1, TMC2209 stepper drivers and MT6701 magnetic encoders
- **Embedded control:** closed-loop PID running at kHz on the microcontroller
- **Software:** a C++ ROS2 hardware interface, with MoveIt2 motion planning and Gazebo simulation

The next stage is adding computer vision and training a learned pick-and-place policy, so the arm can
find and grasp objects on its own.

<!-- TODO (optional): a sentence on what you're learning right now, e.g. robot learning / ML coursework -->

## Contact

<!-- TODO: update links -->
The best way to reach me is by [email](mailto:YOUR-EMAIL@example.com) or on
[LinkedIn](https://www.linkedin.com/in/YOUR-LINKEDIN/). My code is on [GitHub](https://github.com/oisin-hott).
