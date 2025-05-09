# Subsystems in FRC

Welcome to the world of Java programming for FRC! This guide will help you understand the structure and purpose of the subsystems in your robot code. We'll break down the key files and concepts, using diagrams and tables to make everything clear.

---

## Table of Contents
1. [Introduction to FRC Robot Code](#introduction-to-frc-robot-code)
2. [Subsystem Overview](#subsystem-overview)
3. [File Breakdown](#file-breakdown)
   - [Vision Subsystem](#vision-subsystem)
   - [SuperStructure Subsystem](#superstructure-subsystem)
   - [Feeder Subsystem](#feeder-subsystem)
   - [EndEffector Subsystem](#endeffector-subsystem)
   - [Drive Subsystem](#drive-subsystem)
   - [LED Subsystem](#led-subsystem)
4. [Type Hierarchies](#type-hierarchies)
5. [Relevancy Diagrams](#relevancy-diagrams)
6. [Conclusion](#conclusion)

---

## Introduction to FRC Robot Code

FRC robot code is modular and organized into **subsystems**. Each subsystem represents a physical or logical component of the robot, such as the drivetrain, arm, or vision system. Subsystems interact with each other to achieve complex tasks.

### Key Concepts
- **Subsystems**: Represent individual robot components.
- **Commands**: Define robot actions, often using one or more subsystems.
- **Sensors and Actuators**: Provide input (e.g., cameras, encoders) and output (e.g., motors, LEDs).

---

## Subsystem Overview

The following table summarizes the subsystems in your robot code:

| Subsystem       | Purpose                                      | Key Classes/Files                          |
|------------------|----------------------------------------------|--------------------------------------------|
| Vision          | Processes camera data for navigation         | `Vision.java`                              |
| SuperStructure  | Manages the robot's arm and elevator         | `SuperStructure.java`                      |
| Feeder          | Controls the feeding mechanism              | `Feeder.java`                              |
| EndEffector     | Handles the robot's end effector (e.g., gripper) | `EndEffector.java`                         |
| Drive           | Controls the robot's drivetrain             | `Drive.java`                               |
| LED             | Provides visual feedback for robot states   | `LEDSubsystem.java`                        |

---

## File Breakdown

### Vision Subsystem

The `Vision.java` file processes camera data to help the robot navigate and locate targets.

#### Key Features
- Handles multiple cameras.
- Filters and validates pose observations.
- Integrates with the LED subsystem to indicate camera status.

#### Relevant Code
```java
public Rotation2d getTargetX(int cameraIndex) {
    return m_inputs[cameraIndex].latestTargetObservation.tx();
}

@Override
public void periodic() {
    for (int i = 0; i < m_io.length; i++) {
        m_io[i].updateInputs(m_inputs[i]);
        Logger.processInputs("Vision/Camera" + Integer.toString(i), m_inputs[i]);
    }
    
    // Initialize logging values
    // Loop over cameras
    //     -> Update disconnected alert, tag poses
    //     -> Loop over pose observations
    //         -> Accept or Reject observed poses
    //         -> Calculate standard deviations
    //     -> Log camera data
    // Log summary data
}
```

#### Type Hierarchy
```
Vision
├── VisionIO
│   ├── VisionIOInputsAutoLogged
│   ├── PoseObservation
├── VisionConstants
├── AllianceFlipUtil
├── Logger
```

---

### SuperStructure Subsystem

The `SuperStructure.java` file manages the robot's arm and elevator, coordinating their movements.

#### Key Features
- Uses a **state graph** to transition between predefined states.
- Supports both manual and automated control.
- Includes visualization tools for debugging.

#### Relevant Code
```java
public SequentialCommandGroup setSuperStructureGoal(SuperStructureState state) {
    m_finalGoalState = state;
    Logger.recordOutput("SuperStructure/FinalGoal", m_finalGoalState);
    List<SuperStructureState> states = findShortestPath(getSuperStructureGoal(), m_finalGoalState);
    Logger.recordOutput("SuperStructure/States", states.toString());

    return new SequentialCommandGroup(
        states.stream()
            .filter(desiredState -> desiredState != m_goalState)
            .map(this::setSingleState)
            .toArray(Command[]::new));
}

public List<SuperStructureState> findShortestPath(
      SuperStructureState start, SuperStructureState goal) {
    // Null defense: if start or goal is null, return an empty list.
    if (start == null || goal == null) {
      return Collections.emptyList();
    }

    // If start equals goal, immediately return a singleton list.
    if (start.equals(goal)) {
      return Collections.singletonList(start);
    }

    // Use a queue for breadth-first search (BFS) where each element is a path (list of states).
    Queue<List<SuperStructureState>> queue = new LinkedList<>();
    // Use a set to track visited states to avoid cycles and redundant paths.
    Set<SuperStructureState> visited = new HashSet<>();

    // Start the BFS with a path that only contains the start state.
    List<SuperStructureState> initialPath = new ArrayList<>();
    initialPath.add(start);
    queue.add(initialPath);
    visited.add(start);

    while (!queue.isEmpty()) {
      // Dequeue the next path to explore.
      List<SuperStructureState> path = queue.poll();
      SuperStructureState lastState = path.get(path.size() - 1);

      // If the last state is the goal, we've found the shortest path.
      if (lastState.equals(goal)) {
        return path;
      }

      // Explore each neighbor of the last state.
      for (SuperStructureState neighbor : m_graph.getNeighbors(lastState)) {
        // Only consider neighbors that haven't been visited.
        if (!visited.contains(neighbor)) {
          visited.add(neighbor);
          // Create a new path that extends the current path with the neighbor.
          List<SuperStructureState> newPath = new ArrayList<>(path);
          newPath.add(neighbor);
          queue.add(newPath);
        }
      }
    }

    // If no path is found, return an empty list instead of null.
    return Collections.emptyList();
}

private Command setSingleState(SuperStructureState goal) {
return Commands.runOnce(
        () -> {
            setElevatorGoal(goal.elevatorGoal);
            setArmGoal(goal.armGoal);
        },
        this)
    .andThen(new WaitUntilCommand(this::isGoalAchieved))
    .finallyDo(() -> m_goalState = goal);
}
```

#### Type Hierarchy
```
SuperStructure
├── Elevator
│   ├── ElevatorIO
│       ├── ElevatorConstants
│   ├── ElevatorIOInputsAutoLogged
├── Arm
│   ├── ArmIO
│       ├── ArmConstants
│   ├── ArmIOInputsAutoLogged
├── SuperStructureState
├── StateGraph
├── LoggedMechanism2d
│   ├── LoggedMechanismRoot2d
│   ├── LoggedMechanismLigament2d
├── Logger
```

---

### Feeder Subsystem

The `Feeder.java` file controls the mechanism that feeds game pieces into the robot.

#### Key Features
- Uses a beam break sensor to detect game pieces.
- Includes debouncing to filter noisy sensor data.
- Integrates with the LED subsystem to indicate status.

#### Relevant Code
```java
public boolean isBeamBreakTriggered() {
    return m_beambreakDebouncer.calculate(m_beamBreakInputs.state);
}

public Command startFeederCmd() {
    Command cmd = new Command() {
        private boolean m_wasBeamBreakTriggered = false;
        private boolean m_isFinished = false;

        @Override
        public void initialize() {
            runFeeder(FeederConstants.kFeedSpeed);
            LEDSubsystem.getInstance().setStates(LEDStates.INTAKE_RUNNING);
            m_wasBeamBreakTriggered = false;
            m_isFinished = false;
            m_isHandOffReady = false;
        }

        @Override
        public void execute() {
            if (!m_wasBeamBreakTriggered && isBeamBreakTriggered()) {
                runFeeder(FeederConstants.kFeedSlowSpeed);
                m_wasBeamBreakTriggered = true;
            } else if (m_wasBeamBreakTriggered && !isBeamBreakTriggered()) {
                stop();
                m_isHandOffReady = true;
                m_isFinished = true;
                LEDSubsystem.getInstance().setStates(LEDStates.IDLE);
            }
        }

        @Override
        public boolean isFinished() {
            return m_isFinished || (Constants.kCurrentMode == Mode.SIM && m_isHandOffReady);
        }
    };
    cmd.addRequirements(this);
    return new InstantCommand(() -> cmd.schedule());
}
```

#### Type Hierarchy
```
Feeder
├── FeederIO
│   ├── FeederConstants
├── FeederIOInputsAutoLogged
├── BeamBreakIO
├── BeamBreakIOInputsAutoLogged
├── LEDSubsystem
│   ├── LEDStates
├── GamePieceVisualizer
├── Logger
```

---

### EndEffector Subsystem

The `EndEffector.java` file manages the robot's end effector, such as a gripper or claw.

#### Key Features
- Controls the motor for intake and outtake operations.
- Uses current and velocity filtering to detect when a game piece is held.
- Includes alerts for motor disconnection.

#### Relevant Code
```java
public void runEndEffectorIntake() {
    m_endEffector.setVoltage(12.0);
}

@AutoLogOutput
public boolean isHoldingCoral() {
    return m_filteredCurrent > 10 && m_filteredVelocity > 800;
}
```

#### Type Hierarchy
```
EndEffector
├── EndEffectorIO
│   ├── EndEffectorConstants
├── EndEffectorIOInputsAutoLogged
├── LinearFilter
├── Debouncer
├── Logger
```

---

### Drive Subsystem

The `Drive.java` file controls the robot's drivetrain, enabling movement and navigation.

#### Key Features
- Implements swerve drive for precise movement.
- Integrates with the vision subsystem for autonomous navigation.
- Supports path planning and following.

#### Relevant Code
```java
public void runVelocity(ChassisSpeeds speeds) {
    double linearMagnitude = Math.hypot(speeds.vxMetersPerSecond, speeds.vyMetersPerSecond);
    Rotation2d linearDirection = new Rotation2d(Math.atan2(speeds.vyMetersPerSecond, speeds.vxMetersPerSecond));
    Translation2d adjustmentVector = new Translation2d(
        linearMagnitude * adjustmentBaseFactor.getAsDouble() * adjustmentFactor.getAsDouble(),
        linearDirection);
    speeds = speeds.minus(new ChassisSpeeds(adjustmentVector.getX(), adjustmentVector.getY(), 0.0));

    ChassisSpeeds discreteSpeeds = ChassisSpeeds.discretize(speeds, 0.02);
    SwerveModuleState[] setpointStates = m_kinematics.toSwerveModuleStates(discreteSpeeds);
    SwerveDriveKinematics.desaturateWheelSpeeds(setpointStates, kMaxSpeedMetersPerSec);

    for (int i = 0; i < 4; i++) {
        m_modules[i].runSetpoint(setpointStates[i]);
    }
}
```

#### Type Hierarchy
```
Drive
├── GyroIO
├── GyroIOInputsAutoLogged
├── Module
│   ├── ModuleIO
│       ├── DriveConstants
│       ├── SparkUtil
│   ├── ModuleIOInputsAutoLogged
├── SwerveDriveKinematics
├── PathPlanner
├── Logger
```

---

### LED Subsystem

The `LEDSubsystem.java` file controls the robot's LEDs, providing visual feedback for various robot states.

#### Key Features
- Divides LEDs into two sections: Poseidon and Square.
- Supports multiple LED patterns, including solid colors, gradients, and blinking.
- Integrates with other subsystems to reflect their states (e.g., intake running, scoring).

#### Relevant Code
```java
public void setStates(LEDStates newState) {
    if (!DriverStation.isAutonomous() || newState == LEDStates.AUTO) {
        states = newState;
        if (newState == LEDStates.IDLE && (coralHeld || algaeHeld)) {
            states = LEDStates.HOLDING_PIECE;
        } else {
            states = newState;
        }
    }
}

public void periodic() {
    states.getPattern().applyTo(m_poseidon);
    if (states != LEDStates.AUTO) {
        squareStates = states;
    }
    squareStates.getPattern().applyTo(m_square);
    m_leds.setData(m_buffer);
}
```

#### Type Hierarchy
```
LEDSubsystem
├── AddressableLED
|   ├── AddressableLEDBuffer
├── AddressableLEDBufferView
├── LEDStates
|   ├── LEDPattern
├── Timer
├── DriverStation
```

#### Interaction with Other Subsystems
- **Feeder**: Indicates when the feeder is running or ready for handoff.
- **Vision**: Reflects camera status during alignment.
- **SuperStructure**: Displays scoring or resetting states.

---

## Conclusion

This guide provides an overview of the subsystems in your robot code. By understanding their purpose and interactions, you'll be better equipped to modify and extend the code for your specific needs. Happy coding!
