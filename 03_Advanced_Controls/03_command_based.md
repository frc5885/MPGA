# Command-Based Programming

## Overview
Command-based programming is a design pattern that separates robot functionality into subsystems and commands.

## Basic Structure

### Subsystem Example
```java
public class ArmSubsystem extends SubsystemBase {
    private final TalonFX motor;
    private final PIDController pid;
    
    public ArmSubsystem() {
        motor = new TalonFX(1);
        pid = new PIDController(1.0, 0.0, 0.1);
    }
    
    public void setPosition(double position) {
        double output = pid.calculate(getPosition(), position);
        motor.set(TalonFXControlMode.PercentOutput, output);
    }
    
    public double getPosition() {
        return motor.getSelectedSensorPosition();
    }
}
```

### Command Example
```java
public class SetArmPositionCommand extends CommandBase {
    private final ArmSubsystem arm;
    private final double targetPosition;
    
    public SetArmPositionCommand(ArmSubsystem arm, double position) {
        this.arm = arm;
        this.targetPosition = position;
        addRequirements(arm);
    }
    
    @Override
    public void execute() {
        arm.setPosition(targetPosition);
    }
    
    @Override
    public boolean isFinished() {
        return Math.abs(arm.getPosition() - targetPosition) < 0.1;
    }
}
```

## Command Groups

### Sequential Commands
```java
new SequentialCommandGroup(
    new SetArmPositionCommand(arm, 90),
    new WaitCommand(1.0),
    new IntakeCommand(intake).withTimeout(2.0)
);
```

### Parallel Commands
```java
new ParallelCommandGroup(
    new DriveDistanceCommand(drive, 2.0),
    new PrepareShooterCommand(shooter)
);
```

## Trigger Bindings
```java
public class RobotContainer {
    private final XboxController controller = new XboxController(0);
    
    public void configureButtonBindings() {
        new JoystickButton(controller, Button.kA.value)
            .whenPressed(new SetArmPositionCommand(arm, 90));
            
        new Trigger(() -> controller.getRightTriggerAxis() > 0.5)
            .whileTrue(new ShootCommand(shooter));
    }
}
```

## Best Practices

1. Command Organization
   - One action per command
   - Clear naming conventions
   - Document requirements

2. Subsystem Design
   - Single responsibility
   - Encapsulated state
   - Default commands when needed

3. Error Handling
   - Timeout safety
   - Interruption handling
   - Graceful failure modes
