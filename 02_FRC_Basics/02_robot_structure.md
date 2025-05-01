# Robot Program Structure

## Program Architecture Types

```
Robot Program Types
├── Iterative Robot
│   ├── robotInit()
│   ├── teleopPeriodic()
│   └── autonomousPeriodic()
└── Command-Based
    ├── RobotContainer
    ├── Subsystems
    └── Commands
```

## Program Flow Diagram
```
Robot Lifecycle
├── Robot Power On
│   └── robotInit()
├── Disabled Mode
│   ├── disabledInit()
│   └── disabledPeriodic()
├── Autonomous Mode
│   ├── autonomousInit()
│   └── autonomousPeriodic()
└── Teleop Mode
    ├── teleopInit()
    └── teleopPeriodic()
```

## Examples

1. Basic Robot Structure
```java
public class Robot extends TimedRobot {
    private DriveSubsystem drive;
    private ShooterSubsystem shooter;
    private XboxController controller;
    
    @Override
    public void robotInit() {
        drive = new DriveSubsystem();
        shooter = new ShooterSubsystem();
        controller = new XboxController(0);
    }
    
    @Override
    public void teleopPeriodic() {
        // Drive with arcade drive
        drive.arcadeDrive(
            controller.getLeftY(),
            controller.getRightX()
        );
        
        // Control shooter
        if (controller.getAButton()) {
            shooter.shoot();
        }
    }
}
```

2. Command-Based Structure
```java
public class RobotContainer {
    private final DriveSubsystem m_drive = new DriveSubsystem();
    private final ShooterSubsystem m_shooter = new ShooterSubsystem();
    private final XboxController m_controller = new XboxController(0);
    
    public RobotContainer() {
        configureButtonBindings();
        
        m_drive.setDefaultCommand(
            new RunCommand(
                () -> m_drive.arcadeDrive(
                    m_controller.getLeftY(),
                    m_controller.getRightX()
                ),
                m_drive
            )
        );
    }
    
    private void configureButtonBindings() {
        new JoystickButton(m_controller, Button.kA.value)
            .whenPressed(new ShootCommand(m_shooter));
    }
}
```

3. Subsystem Example
```java
public class DriveSubsystem extends SubsystemBase {
    private final WPI_TalonFX leftMaster = new WPI_TalonFX(1);
    private final WPI_TalonFX rightMaster = new WPI_TalonFX(2);
    
    public DriveSubsystem() {
        rightMaster.setInverted(true);
    }
    
    public void arcadeDrive(double speed, double rotation) {
        var speeds = DifferentialDrive.arcadeDriveIK(speed, rotation, true);
        leftMaster.set(speeds.left);
        rightMaster.set(speeds.right);
    }
    
    @Override
    public void periodic() {
        // This method is called once per scheduler run
        updateDashboard();
    }
}
```

## Mode-Specific Methods

| Method | Called When | Purpose |
|--------|------------|---------|
| robotInit | Robot starts | One-time initialization |
| disabledInit | Robot enters disabled | Disable outputs |
| autonomousInit | Auto mode starts | Setup auto routine |
| teleopInit | Teleop mode starts | Setup driver control |
| testInit | Test mode starts | Setup test mode |
| *Periodic | Every 20ms | Regular updates |

## Best Practices

### Code Organization
1. One subsystem per major robot function
2. Clear separation of concerns
3. Consistent naming conventions
4. Well-documented interfaces

### Command Guidelines
1. Single responsibility principle
2. Clear start and end conditions
3. Proper use of requirements
4. Timeout implementation

### Safety Considerations
1. Motor safety enabled
2. Limit switch checks
3. Current monitoring
4. Fail-safe defaults

## Further Reading
- [Command-Based Programming](https://docs.wpilib.org/en/stable/docs/software/commandbased/what-is-command-based.html)
- [Robot Program Structure](https://docs.wpilib.org/en/stable/docs/software/roborio-info/program-structure.html)
