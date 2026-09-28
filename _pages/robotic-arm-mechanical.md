---
title: "Cycloidal Drive & Mechanical Design"
permalink: /projects/robotic-arm/mechanical/
excerpt: "A compact, high-reduction cycloidal gearbox, a differential wrist, and 3D-printed structures designed around their bearings."
header:
  overlay_image: /assets/images/robotic-arm/cycloidal-hero.jpg
  overlay_filter: 0.5
  teaser: /assets/images/robotic-arm/cycloidal-teaser.jpg
toc: true
toc_sticky: true
---

[← Back to the robotic arm overview](/projects/robotic-arm/)

## The problem

Stepper motors on their own don't have the torque to hold and move an arm at useful speeds, so each
joint needs a gear reduction. For an arm, that reduction has to be compact, stiff and low-backlash,
because any play in the joints shows up as position error at the end effector.

## Why a cycloidal drive

<!-- TODO: fill in your comparison. Suggested table: -->

| Option | Reduction | Backlash | Size | Printability |
|---|---|---|---|---|
| Planetary | TODO | TODO | TODO | TODO |
| Harmonic (strain wave) | TODO | TODO | TODO | TODO |
| Cycloidal | TODO | TODO | TODO | TODO |

I chose a cycloidal drive because <!-- TODO: your reasoning, e.g. high reduction in one stage, many teeth in contact, prints well -->.

## Cycloidal drive design

<!-- TODO: reduction ratio, number of lobes/pins, eccentricity, how you generated the profile (equations, CAD plugin, script) -->

![Cycloidal drive CAD](/assets/images/robotic-arm/cycloidal-cad.jpg)

## Differential wrist

The wrist uses two motors working together to produce both pitch and roll. Driving both motors in the
same direction produces one motion, and driving them in opposite directions produces the other.
Software converts the desired pitch and roll into the two motor commands.

<!-- TODO: diagram or CAD render of the wrist, and why you chose a differential over two independent joints (e.g. keeping motor mass near the base) -->

![Differential wrist](/assets/images/robotic-arm/wrist.jpg)

## Bearing retention in printed parts

Printed parts don't hold bearings as reliably as machined ones, since tolerances vary and plastic creeps
under load. I designed retention features into the parts and looked at bearing adhesives as a backup.

<!-- TODO: which features you used (e.g. crush ribs, clamping covers, shoulders) and what you settled on -->

## What went wrong

<!-- TODO: e.g. tolerance issues, a part that cracked, a print that needed redesigning -->

## Results

<!-- TODO: measured backlash, reduction ratio, torque, print material and settings, photos of printed parts -->
