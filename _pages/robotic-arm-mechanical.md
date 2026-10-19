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

## Joint overview

Each joint has different demands, so each uses a different drive. The reduction at every joint was
chosen from torque calculations (see the [calculations folder](https://github.com/oisin-hott/robot_arm_hardware)
in the hardware repo).

| Joint | Role | Drive | Reduction |
|---|---|---|---|
| J1 | Base rotation | Printed turntable with ball bearings | 5:1 |
| J2 | Shoulder | Cycloidal drive + belt | 16:1 + 3:1 |
| J3 | Elbow | Belt-driven pulleys | 10:1 |
| J4 | Forearm roll | Split-ring planetary gearbox | 37:1 |
| J5 / J6 | Wrist pitch and roll | Belt-driven bevel-gear differential | 3:1 |

## Why a cycloidal drive

The shoulder (J2) carries the greatest load, and it's also where small angular errors matter most:
a small angle at the shoulder becomes a large error at the end effector. I compared three options for it.

| Option | Reduction | Backlash | Size | Printability |
|---|---|---|---|---|
| Planetary | Higher per stage; used at J4 (37:1) for a large reduction in a small space | Greater with printed parts | Compact | Easy |
| Harmonic (strain wave) | Lower than the cycloidal in this design | Minimal | Similar to cycloidal | Harder to print |
| Cycloidal | 16:1 in one stage | Minimal: constant contact between rotors and pins | Compact | Prints well |

### Decision matrix

To make the choice for the shoulder joint, I scored each option against weighted criteria using a Pugh matrix.
The planetary gearbox is the datum (baseline), and each option is scored **+1** (better), **0** (same) or
**-1** (worse) against it. Low backlash carries the highest weight, because a small angle error at the
shoulder becomes a large error at the end effector.

| Criterion | Weight | Planetary (datum) | Harmonic | Cycloidal |
|---|:---:|:---:|:---:|:---:|
| Low backlash | 5 | 0 | +1 | +1 |
| Printability | 4 | 0 | -1 | 0 |
| Reduction per stage | 3 | 0 | 0 | -1 |
| Compactness | 3 | 0 | 0 | 0 |
| Cost | 2 | 0 | 0 | 0 |
| **Weighted total** | | **0** | **+1** | **+2** |

The cycloidal drive scored highest, mainly on backlash and on being far easier to print than a harmonic drive.
The matrix only covers the shoulder: planetary still won at J4, where a large reduction in a small space
mattered more than backlash.

## J2: Cycloidal drive and belt stage

The shoulder uses a **16:1 cycloidal drive followed by a 3:1 belt reduction** (48:1 overall). Because this is
the most heavily loaded joint, it's driven by a beefier **60 N·cm NEMA 17**. The torque calculations show this
is enough to move the arm through its full range of motion at full payload, with a built-in factor of safety.

With torque covered, the likely failure points are the **PLA inside the gearbox** and **belt slippage**. For that
reason the rotors and stator are printed in **PLA+ at 50% infill**.

### Designing the rotor profile

The rotor outline is generated from the cycloid curve using these parametric equations:
```
X = (R*cos(t)) - (Rr*cos(t + arctan(sin((1-N)*t) / ((R/(E*N)) - cos((1-N)*t))))) - (E*cos(N*t))
Y = (-R*sin(t)) + (Rr*sin(t + arctan(sin((1-N)*t) / ((R/(E*N)) - cos((1-N)*t))))) + (E*sin(N*t))
```


<!-- TODO: define the symbols with your values: R (pin circle radius), Rr (pin/roller radius), E (eccentricity), N (number of pins), and how you plotted/imported the curve into CAD (script, spreadsheet, CAD plugin) -->

**Two rotors** are used, **180° out of phase**. This keeps two points of contact at all times and cancels much of
the vibration caused by the eccentric motion. A third rotor, with the three spaced 120° apart, was considered,
but two rotors keep the design simpler and more compact. A third remains an option if problems appear in testing.

![Cycloidal drive CAD](/assets/images/Cycloidal.png)

## J1, J3 and J4

**J1 (5:1 turntable).** A groove is cut into the base and filled with ball bearings, and the turntable slides
over them, acting as one large roller bearing. Very little force is needed because the load acts along the
direction of gravity, so only inertial forces act sideways.

**J3 (10:1 belt drive).** A gearbox was considered, but the calculations showed a belt-driven pulley stage gives
enough reduction. It cuts both complexity and cost (printed parts, bearings).

**J4 (37:1 split-ring planetary).** The motor sits inside the arm and acts along the arm's axis. The motor and
base ring are stationary, and the output shaft is bolted to the final section of the arm, so the gearbox is also
the output shaft. The encoder is mounted on the rear of the stepper (the motor side), and homing uses a limit
switch at 175° to fix the multi-turn count and offset.

## Differential wrist

The wrist (J5 and J6) uses two motors working together to produce both pitch and roll. Driving both motors in
the same direction produces one motion, and driving them in opposite directions produces the other. Software
converts the desired pitch and roll into the two motor commands.

A differential gives a **compact 2-DOF wrist** with pitch and roll on intersecting axes. The alternative, two
independent drives, would have needed concentric shafts, which were too expensive and complicated for MK1.
It's driven by compact pancake NEMA 17s through belts, with a **custom-designed bevel-gear differential**.

The trade-off is extra complexity in homing and control, but I judged it worth it for the added dexterity.

![Differential wrist](/assets/images/robotic-arm/wrist.jpg)

## Bearing retention in printed parts

Printed parts don't hold bearings as reliably as machined ones, since tolerances vary and plastic creeps
under load. All bearings are **bought rather than printed**, for longevity, and are **press-fit with a 0.1 mm
tolerance**. I designed retention features into the parts and looked at bearing adhesives as a backup.

<!-- TODO: which features you used (e.g. crush ribs, clamping covers, shoulders) and what you settled on -->

## What went wrong

- **Belt slipping at the shoulder.** The unloaded joint worked flawlessly, but once the second arm was added,
  the belts started slipping at the maximum-torque position, with the arm fully extended at 90°. Gravity
  creates a large moment there. I added **static springs** anchoring the first arm to the rotating base, which
  provide a counter-moment and stop the slippage at extreme angles.
- **J4 printing tolerances.** Some gearbox parts came out with the wrong tolerances, so they needed small
  adjustments and reprints.
- **Wrist clunkiness.** Early testing showed slight clunkiness in the differential, likely fixable by adjusting
  the tolerances between the bevel gears and their axes.

## Results

<!-- TODO: measured backlash, payload tested, photos of printed parts. Link the calcs (J2 safety factor) if you want numbers on the page -->

- Torque at the shoulder is sufficient for the full range of motion at full payload, with a factor of safety (from calculations).
- Rotors and stator printed in PLA+ at 50% infill.
- Early testing of the differential wrist shows slight backlash to tune out.