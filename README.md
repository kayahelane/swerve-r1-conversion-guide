# swerve-r1-conversion-guide
# FRC Swerve R1 Conversion & Validation Guide

## Overview

This repository documents the complete process for converting an FRC swerve drivetrain to **R1 gearing** and validating that the drivetrain performs correctly afterward.

Changing drivetrain gearing affects multiple subsystems including:

* Speed calculations
* Encoder conversion factors
* Autonomous path planning
* Odometry accuracy
* Feedforward characterization
* Driver control responsiveness

This guide provides a **step-by-step workflow**, validation procedures, and troubleshooting guidance to ensure a reliable drivetrain after mechanical changes.

The procedures in this repository assume the robot is programmed using **WPILib** and autonomous paths are created with **PathPlanner**.

---

# Repository Structure

```
swerve-r1-conversion-guide
│
├── README.md
├── docs
│   ├── 01-gear-ratio-update.md
│   ├── 02-max-speed-configuration.md
│   ├── 03-pathplanner-configuration.md
│   ├── 04-module-alignment.md
│   ├── 05-odometry-validation.md
│   ├── 06-sysid-characterization.md
│   ├── 07-rotation-pid-tuning.md
│   ├── 08-teleop-drive-adjustments.md
│   └── 09-autonomous-validation.md
│
├── checklists
│   ├── drivetrain-validation.md
│   └── pit-checklist.md
│
└── examples
    ├── constants-example.java
    └── encoder-conversion-formula.md
```

Each document contains detailed instructions for completing a specific step in the drivetrain conversion process.

---

# Conversion Workflow

When switching to **R1 swerve gearing**, follow the steps below in order:

1. Update drivetrain gear ratio in robot code
2. Update maximum drivetrain speed constant
3. Update robot configuration in PathPlanner
4. Verify swerve module alignment
5. Validate odometry accuracy
6. Recharacterize the drivetrain using SysID
7. Retune drivetrain rotation PID
8. Adjust teleop drive limits if necessary
9. Revalidate autonomous routines

Performing these steps sequentially ensures that software configuration remains consistent with the mechanical drivetrain.

---

# Key Configuration Changes

## Gear Ratio Update

Update the drivetrain gear ratio constant used for encoder conversions and speed calculations.

Example:

```java
public static final double DRIVE_GEAR_RATIO = 6.12;
```

After updating the ratio, confirm that the following values remain accurate:

* Wheel diameter
* Encoder conversion factors
* Maximum drivetrain speed

---

## Maximum Speed Configuration

The robot’s theoretical maximum speed must be updated to reflect the new gearing.

Example:

```java
public static final double MAX_SPEED = 5.2; // meters per second
```

This value must match:

* Robot code constants
* PathPlanner robot configuration
* Actual measured drivetrain performance

---

## Encoder Conversion Factors

Encoder values must convert motor rotations into **meters traveled**.

Example formula:

```
distancePerRotation =
(wheelDiameter × π) / gearRatio
```

Incorrect conversion factors will cause odometry drift and autonomous path errors.

---

# Drivetrain Validation Tests

After completing configuration changes, run the following validation tests.

## 1. Straight-Line Odometry Test

Purpose: verify encoder conversions and drivetrain calculations.

Procedure:

1. Command the robot to drive forward exactly **2 meters**
2. Measure the actual distance traveled

Expected result:

```
Commanded: 2.0 m
Measured: ~1.95–2.05 m
```

If the error exceeds this range, verify:

* Gear ratio constant
* Wheel diameter
* Encoder conversion factor

---

## 2. Module Alignment Test

Purpose: confirm that absolute encoder offsets are correct.

Procedure:

1. Raise robot on blocks
2. Command a slow forward drive
3. Observe wheel orientation

Expected behavior:

```
All four modules point forward
```

Misaligned modules indicate incorrect encoder offsets.

---

## 3. Rotation Control Test

Purpose: validate rotation PID performance.

Procedure:

Rotate the robot through several angles:

```
0° → 90° → 180° → 0°
```

Expected behavior:

* Smooth rotation
* Minimal overshoot
* Stable final heading

---

## 4. Maximum Speed Test

Purpose: confirm drivetrain performance matches configuration.

Procedure:

1. Drive robot at full speed
2. Log measured velocity using **Shuffleboard** or **AdvantageScope**

Measured speed should closely match the configured maximum.

---

## 5. Autonomous Path Validation

Run a simple autonomous test path:

```
Forward 2m
Turn 90°
Forward 2m
```

Observe:

* Smooth motion
* Accurate stopping
* Correct orientation

If this path executes reliably, most autonomous routines should behave correctly.

---

# Common Issues After Gearing Changes

## Odometry Drift

Possible causes:

* Incorrect encoder conversion factors
* Wheel diameter mismatch
* Incorrect gear ratio

---

## Diagonal Driving

Possible causes:

* Incorrect module offsets
* Misaligned swerve modules

---

## Unstable Autonomous Motion

Possible causes:

* Outdated feedforward values
* Incorrect maximum speed configuration
* Improper path planner limits

Running drivetrain characterization with **SysId** usually resolves these issues.

---

# Pit Crew Drivetrain Checklist

Before each match, perform a quick drivetrain check:

```
☐ Wheels visually aligned
☐ Gyro calibrated
☐ Odometry reset
☐ Modules respond correctly
☐ Autonomous routine selected
```

This check helps identify drivetrain issues before the robot enters the field.

---

# Contributing

Team members are encouraged to contribute improvements to this repository.

Suggested additions include:

* Updated drivetrain constants
* Additional troubleshooting steps
* Test procedures for new drivetrain configurations
* Diagrams and validation examples

---

# Purpose of This Guide

Drivetrain changes are one of the most common causes of autonomous issues during competitions.
Maintaining clear documentation ensures that future programmers and drive team members can quickly verify drivetrain performance after mechanical modifications.

This repository serves as a **living technical reference** for drivetrain configuration and validation.
