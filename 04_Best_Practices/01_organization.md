# Code Organization

## Project Structure
```
robot/
├── Constants.java
├── Main.java
├── Robot.java
├── RobotContainer.java
├── commands/
│   ├── autonomous/
│   └── teleop/
├── subsystems/
└── utils/
```

## Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Classes | PascalCase | ShooterSubsystem |
| Methods | camelCase | setTargetVelocity |
| Constants | UPPER_SNAKE_CASE | SHOOTER_PORT |
| Variables | camelCase | currentSpeed |

## Constants Management
```java
public final class Constants {
    public static final class DriveConstants {
        public static final int LEFT_MASTER_PORT = 1;
        public static final int LEFT_FOLLOWER_PORT = 2;
        public static final double kP = 0.1;
    }
    
    public static final class ShooterConstants {
        public static final int MOTOR_PORT = 5;
        public static final double TARGET_RPM = 4000;
    }
}
```

## Documentation Standards
```java
/**
 * Controls the robot's shooting mechanism.
 * Handles velocity control and feed wheel timing.
 */
public class ShooterSubsystem extends SubsystemBase {
    /**
     * Sets the shooter wheel to a specific velocity
     * @param rpm Target velocity in rotations per minute
     * @return true if at target velocity
     */
    public boolean setVelocity(double rpm) {
        // Implementation
    }
}
```

## Code Reviews Checklist
1. Code Style
   - Consistent formatting
   - Clear naming
   - Proper documentation

2. Architecture
   - Single responsibility
   - Dependency management
   - Error handling

3. Safety
   - Motor safety
   - Current limits
   - Failsafe defaults

## Version Control Best Practices
1. Commit Messages
   - Clear descriptions
   - Reference issues
   - Tag subsystems

2. Branching Strategy
   - feature/
   - bugfix/
   - comp/

3. Code Review Process
   - Review checklist
   - Test requirements
   - Documentation updates
