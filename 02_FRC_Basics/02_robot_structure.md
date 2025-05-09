# FRC Java Codebase Tutorial

Welcome to the FRC Java codebase tutorial! This guide will help you understand the structure and functionality of the provided files. By the end, you'll have a solid grasp of how the robot's software is organized and how to extend it.

---

## Table of Contents
1. [Overview](#overview)
2. [File Breakdown](#file-breakdown)
   - [RobotContainer.java](#robotcontainerjava)
   - [Constants.java](#constantsjava)
   - [Robot.java](#robotjava)
3. [Key Concepts](#key-concepts)
   - [Command-Based Programming](#command-based-programming)
   - [Subsystems](#subsystems)
   - [Triggers and Commands](#triggers-and-commands)
4. [Type Hierarchy](#type-hierarchy)
5. [Relevancy Diagram](#relevancy-diagram)
6. [Next Steps](#next-steps)

---

## Overview

This codebase is designed for an FRC robot using the **Command-Based Programming** paradigm. It organizes the robot's behavior into modular components like subsystems, commands, and triggers. Here's a high-level view of the files:

| File                  | Purpose                                                                 |
|-----------------------|-------------------------------------------------------------------------|
| `RobotContainer.java` | Main configuration file for subsystems, commands, and button mappings. |
| `Constants.java`      | Stores global constants for the robot's configuration.                 |
| `Robot.java`          | Entry point for the robot's lifecycle (init, periodic, etc.).          |

---

## File Breakdown

### RobotContainer.java

This file is the heart of the robot's configuration. It initializes subsystems, defines commands, and maps buttons to actions.

#### Key Sections:
1. **Subsystem Initialization**: Creates instances of subsystems like `Drive`, `Vision`, and `SuperStructure`.
2. **Trigger Definitions**: Maps controller buttons to specific robot actions.
3. **Autonomous Command Selection**: Configures autonomous routines using a dashboard chooser.

#### Example:
```java
// Define a trigger for scoring coral
m_driverController.rightTrigger(0.1)
    .whileTrue(new AutoScoreCoralAtBranchCommand(...))
    .onFalse(new ResetSuperStructureCommand(...));
```

| Concept               | Description                                                                 |
|-----------------------|-----------------------------------------------------------------------------|
| Subsystems            | Represent physical components like the drivetrain or arm.                  |
| Commands              | Define specific actions, e.g., "drive forward" or "score coral."           |
| Triggers              | Link controller inputs to commands, enabling user interaction.             |

---

### Constants.java

This file contains global constants that define the robot's runtime mode and other configurations.

#### Key Sections:
1. **Runtime Modes**: Defines whether the robot is running in `REAL`, `SIM`, or `REPLAY` mode.
2. **Tuning Flag**: Enables or disables tuning features.

#### Example:
```java
public static final Mode kCurrentMode = RobotBase.isReal() ? Mode.REAL : kSimMode;
```

| Mode   | Description                              |
|--------|------------------------------------------|
| REAL   | Running on the actual robot hardware.    |
| SIM    | Running in a physics simulator.          |
| REPLAY | Replaying logs for debugging or analysis.|

---

### Robot.java

This file is the entry point for the robot's lifecycle. It manages initialization, periodic updates, and mode transitions.

#### Key Sections:
1. **Initialization**: Sets up logging and the `RobotContainer`.
2. **Periodic Updates**: Runs the scheduler and updates subsystems.
3. **Mode-Specific Behavior**: Handles transitions between disabled, autonomous, teleop, and test modes.

#### Example:
```java
@Override
public void autonomousInit() {
    m_autonomousCommand = m_robotContainer.getAutonomousCommand();
    if (m_autonomousCommand != null) {
        m_autonomousCommand.schedule();
    }
}
```

| Mode       | Purpose                                                                 |
|------------|-------------------------------------------------------------------------|
| Disabled   | Robot is inactive but can still log data or reset states.              |
| Autonomous | Executes pre-programmed routines without user input.                   |
| Teleop     | Allows user control via controllers.                                   |
| Test       | Used for testing individual components or commands.                    |

---

## Key Concepts

### Command-Based Programming

Command-based programming is a design pattern used in FRC to organize robot code. It separates logic into **commands** and **subsystems**.

| Component   | Purpose                                                                 |
|-------------|-------------------------------------------------------------------------|
| Subsystem   | Represents a physical component of the robot (e.g., drivetrain).       |
| Command     | Encapsulates a specific action or behavior (e.g., drive forward).      |
| Trigger     | Links user inputs (e.g., buttons) to commands.                         |

---

### Subsystems

Subsystems are abstractions for hardware components. For example, the `Drive` subsystem controls the robot's drivetrain.

#### Example Subsystems:
- **Drive**: Handles movement and orientation.
- **Vision**: Processes camera data for navigation.
- **SuperStructure**: Manages the robot's arm and elevator.

---

### Triggers and Commands

Triggers map controller inputs to commands. For example, pressing a button might trigger a command to score a game piece.

#### Example:
```java
m_driverController.rightTrigger(0.1)
    .whileTrue(new AutoScoreCoralAtBranchCommand(...));
```

---

## Type Hierarchy

Below is a simplified type hierarchy for the codebase:

```
Robot
├── RobotContainer
│   ├── Subsystems
│   │   ├── Drive
│   │   ├── Vision
│   │   └── SuperStructure
│   ├── Commands
│   │   ├── DriveCommands
│   │   ├── AutoScoreCoralAtBranchCommand
│   │   └── ResetSuperStructureCommand
│   └── Triggers
├── Constants
└── Logger
```

---

## Relevancy Diagram

The following diagram shows how the files interact:

```
[Robot] --> [RobotContainer]
   |              |
   |              |
   |              +--> [Commands] --> [Subsystems]
   |              |                       |
   |              |                       |
   |              +--> [Triggers]         +--> [Drive]
   |                                      +--> [Vision]
   |                                      +--> [SuperStructure]
   +--> [Constants]
   +--> [Logger]
```

---

## Next Steps

1. Explore Subsystems: Look into how subsystems like `Drive` and `Vision` are implemented.
2. Write Commands: Create new commands to add functionality to the robot.
3. Test in Simulation: Use the `SIM` mode to test your code without hardware.
4. Deploy to Robot: Switch to `REAL` mode and deploy your code to the robot.

For more information, refer to the [WPILib Documentation](https://docs.wpilib.org/).
