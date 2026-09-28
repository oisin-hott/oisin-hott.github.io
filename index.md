---
layout: splash
title: "Oisin Hott | Engineering Portfolio"
permalink: /
header:
  overlay_image: /assets/images/home/hero.jpg
  overlay_filter: 0.55
excerpt: "Mechanical engineer building robots across mechanical design, embedded systems and C++/ROS2."

# ---------- Featured project ----------
featured_arm:
  - image_path: /assets/images/robotic-arm/teaser.jpg
    alt: "6-DOF robotic arm"
    title: "6-DOF Robotic Arm"
    excerpt: "A six-axis arm designed and built from scratch: custom PCB, closed-loop control on a Teensy 4.1, and a full ROS2 / MoveIt2 software stack."
    url: "/projects/robotic-arm/"
    btn_label: "View project"
    btn_class: "btn--primary"

# ---------- Robotic arm deep dives ----------
arm_deep_dives:
  - image_path: /assets/images/robotic-arm/pcb-teaser.jpg
    alt: "Custom control PCB"
    title: "Custom Control PCB"
    excerpt: "2-layer KiCad board integrating TMC2209 drivers, MT6701 encoders and a Teensy 4.1."
    url: "/projects/robotic-arm/pcb/"
    btn_label: "Read more"
    btn_class: "btn--inverse"
  - image_path: /assets/images/robotic-arm/cycloidal-teaser.jpg
    alt: "Cycloidal drive"
    title: "Cycloidal Drive & Mechanical Design"
    excerpt: "Compact high-reduction cycloidal gearbox, differential wrist and printed bearing retention."
    url: "/projects/robotic-arm/mechanical/"
    btn_label: "Read more"
    btn_class: "btn--inverse"
  - image_path: /assets/images/robotic-arm/control-teaser.jpg
    alt: "Embedded control"
    title: "Embedded Control & ROS2 Hardware Interface"
    excerpt: "kHz PID on the Teensy with ROS2 sending joint targets, connected through a custom C++ hardware interface."
    url: "/projects/robotic-arm/control/"
    btn_label: "Read more"
    btn_class: "btn--inverse"
  - image_path: /assets/images/robotic-arm/sim-teaser.jpg
    alt: "MoveIt2 simulation"
    title: "Motion Planning & Simulation"
    excerpt: "MoveIt2 with STOMP and TRAC-IK, and a Gazebo simulation built from scratch."
    url: "/projects/robotic-arm/simulation/"
    btn_label: "Read more"
    btn_class: "btn--inverse"

# ---------- Other work ----------
thesis:
  - image_path: /assets/images/thesis/teaser.jpg
    alt: "MSc thesis"
    title: "MSc Thesis: TODO THESIS TITLE"
    excerpt: "TODO: one or two sentences on what you modelled, how you validated it, and the key finding. MSc Mechanical Engineering, UCD."
    url: "/projects/msc-thesis/"
    btn_label: "View project"
    btn_class: "btn--primary"
---

## Featured project

{% include feature_row id="featured_arm" type="left" %}

## Robotic arm deep dives

{% include feature_row id="arm_deep_dives" %}

## Other work

{% include feature_row id="thesis" type="left" %}
