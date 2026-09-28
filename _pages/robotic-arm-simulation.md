---
title: "Motion Planning & Simulation"
permalink: /projects/robotic-arm/simulation/
excerpt: "A ROS2 simulation stack built from scratch: MoveIt2 with STOMP and TRAC-IK, a tuned Gazebo model, and a four-task motion sequence."
header:
  overlay_image: /assets/images/robotic-arm/sim-hero.jpg
  overlay_filter: 0.5
  teaser: /assets/images/robotic-arm/sim-teaser.jpg
toc: true
toc_sticky: true
---

[← Back to the robotic arm overview](/projects/robotic-arm/)

## The problem

I wanted to develop and test the software before the hardware was finished, and to have a safe place to
try motions before running them on the real arm. That meant building a simulation that behaved closely
enough to the physical robot to be useful.

## The stack

- **Robot description:** URDF of the arm <!-- TODO: generated from CAD or written by hand? -->
- **Motion planning:** MoveIt2
- **Planner:** STOMP
- **Inverse kinematics:** TRAC-IK
- **Physics simulation:** Gazebo
- **Wrist:** a differential mixer converting pitch and roll into two motor commands

<!-- TODO: screenshot or GIF of RViz / Gazebo -->
![MoveIt2 simulation](/assets/images/robotic-arm/moveit-sim.gif)

## Design decisions

**STOMP planner.** <!-- TODO: why STOMP over the default OMPL planners, e.g. smoother trajectories -->

**TRAC-IK.** I migrated from the default IK solver to TRAC-IK because <!-- TODO: e.g. more reliable solutions near joint limits -->.

## What went wrong

- **STOMP configuration:** <!-- TODO: what was wrong in the pipeline config and how you found it -->
- **Gazebo physics:** <!-- TODO: what the arm did wrong (e.g. jitter, drooping, instability) and which parameters fixed it -->
- **C++ segfaults:** <!-- TODO: the cause and how you tracked it down (e.g. gdb, sanitizers) -->

## Results

The simulated arm runs a four-task motion sequence end to end.

<!-- TODO: video of the sequence, planning times, success rate -->
{% include video id="YOUTUBE-VIDEO-ID" provider="youtube" %}

## Next steps

- Validate the simulation against the real arm once MK1 is running
- Use the simulation for vision and learned pick-and-place development
