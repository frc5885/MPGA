# Introduction to FRC Programming

In this section, we will introduce you to FRC programming concepts, including WPILib, setting up your development environment, and understanding the basic structure of an FRC robot program.

---

## 1. Overview of WPILib

WPILib is the software library used in FRC to control robots. It provides tools and APIs for:
- Motor control
- Sensor integration
- Autonomous and teleoperated modes
- Networking and communication with the Driver Station

### Key Components of WPILib:
- **Subsystems**: Represent physical components of the robot (e.g., drivetrain, arm).
- **Commands**: Define actions for the robot to perform.
- **Scheduler**: Manages the execution of commands.

---

## 2. Setting Up the Development Environment

To start programming your FRC robot, follow these steps:

1. **Install Required Software**:
   - [VS Code](https://code.visualstudio.com/)
   - [WPILib Installer](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/wpilib-setup.html)

2. **Create a New Robot Project**:
   - Open VS Code.
   - Use the WPILib extension to create a new project.
   - Select the appropriate template (e.g., Command-Based Robot).

3. **Deploy Code to the Robot**:
   - Connect your computer to the robot's network.
   - Use the "Deploy Robot Code" option in VS Code.

---

## 3. Basic Robot Code Structure

An FRC robot program typically consists of the following files:

### Robot.java
This is the main entry point of your robot program. It defines the robot's lifecycle methods:
- `robotInit()`: Runs once when the robot starts.
- `autonomousInit()`: Runs once at the start of autonomous mode.
- `teleopInit()`: Runs once at the start of teleoperated mode.
- `robotPeriodic()`: Runs periodically in all modes.

### Example: Basic Robot.java
```java
import edu.wpi.first.wpilibj.TimedRobot;

public class Robot extends TimedRobot {
    @Override
    public void robotInit() {
        System.out.println("Robot Initialized");
    }

    @Override
    public void autonomousInit() {
        System.out.println("Autonomous Mode Started");
    }

    @Override
    public void teleopInit() {
        System.out.println("Teleop Mode Started");
    }

    @Override
    public void robotPeriodic() {
        System.out.println("Robot Periodic Task");
    }
}
```

---

### Subsystems
Subsystems represent physical components of the robot, such as the drivetrain or arm.

### Example: Drivetrain Subsystem
```java
import edu.wpi.first.wpilibj2.command.SubsystemBase;

public class Drivetrain extends SubsystemBase {
    public void drive(double speed, double rotation) {
        System.out.println("Driving with speed: " + speed + " and rotation: " + rotation);
    }
}
```

---

### Commands
Commands define actions for the robot to perform, such as moving forward or turning.

### Example: Drive Command
```java
import edu.wpi.first.wpilibj2.command.CommandBase;

public class DriveCommand extends CommandBase {
    private final Drivetrain drivetrain;

    public DriveCommand(Drivetrain drivetrain) {
        this.drivetrain = drivetrain;
        addRequirements(drivetrain);
    }

    @Override
    public void execute() {
        drivetrain.drive(0.5, 0.0); // Drive forward at half speed
    }
}
```

---

This concludes the **Introduction to FRC Programming** section. Next, we will explore [Advanced FRC Programming](05_Advanced_FRC_Programming.md) concepts.
