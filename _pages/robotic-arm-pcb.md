---
title: "Custom Control PCB"
permalink: /projects/robotic-arm/pcb/
excerpt: "A 2-layer KiCad board that brings the arm's motor drivers, encoders, power and microcontroller onto a single PCB."
header:
  overlay_image: /assets/images/robotic-arm/pcb-hero.jpg
  overlay_filter: 0.5
  teaser: /assets/images/robotic-arm/pcb-teaser.jpg
toc: true
toc_sticky: true
---

[← Back to the robotic arm overview](/projects/robotic-arm/)

## The problem

Six stepper motors, six encoders, a servo gripper and a microcontroller can be wired up on a breadboard,
but it quickly becomes unreliable and impossible to debug. I wanted a single board that handled power
distribution, motor drive and sensing cleanly, and that I understood completely.

<!-- TODO: one line on why you chose a custom board over off-the-shelf driver shields -->

## Requirements

- Drive six stepper motors with quiet, configurable drivers
- Read six absolute magnetic encoders
- Supply 5 V for the servo gripper from the main motor supply
- Protect the electronics against a reversed supply connection
- Fit on a 2-layer board, using parts sourceable in Ireland/EU

<!-- TODO: add supply voltage and peak current per motor -->

## Architecture

| Block | Component | Role |
|---|---|---|
| Controller | Teensy 4.1 | Runs the closed-loop control for all joints |
| Motor drivers | TMC2209 modules | Step/dir stepper control |
| Sensing | MT6701 encoders | Absolute joint angle feedback |
| Power | 5 V buck converter | Servo gripper supply |
| Protection | Reverse polarity protection | Guards against a reversed input |

<!-- TODO: add schematic screenshot -->
![Schematic](/assets/images/robotic-arm/pcb-schematic.png)

## Design decisions

**Trace widths.** I sized the motor power traces from the expected current and an acceptable temperature
rise, rather than using default widths. <!-- TODO: current, trace width and the calculator/standard you used (e.g. IPC-2221) -->

**Copper pours and zones.** A ground pour reduces return path impedance and noise. Getting the zone
priorities right between power and ground pours took several iterations, and a few DRC errors taught me
how KiCad resolves overlapping zones. <!-- TODO: what the actual issue was and how you fixed it -->

**Reverse polarity protection.** <!-- TODO: which approach (e.g. P-MOSFET or diode) and why -->

**Sourcing.** I built the bill of materials around Irish/EU suppliers to keep lead times and shipping costs down.

<!-- TODO: add PCB layout screenshot and 3D render -->
![PCB layout](/assets/images/robotic-arm/pcb-layout.png)

## What went wrong

<!-- TODO: one or two honest issues, e.g. a footprint mistake, a DRC problem, a rework on the first board -->

## Results

<!-- TODO: photo of the assembled board, and anything measurable: board size, cost per board, bring-up results -->

## Lessons learned

<!-- TODO: two or three bullet points you'd do differently on revision 2 -->
