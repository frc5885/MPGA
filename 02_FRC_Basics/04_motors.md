# Motors and Actuators

## Motor Controller Hierarchy
```
Motor Controllers
├── PWM Controllers
│   ├── Spark
│   ├── VictorSP
│   └── PWMSparkMax
├── CAN Controllers
│   ├── TalonFX
│   ├── TalonSRX
│   ├── VictorSPX
│   └── SparkMax
└── Pneumatics
    ├── Solenoid
    └── DoubleSolenoid
```

## Common Motor Types

| Motor Type | Controller | Protocol | Common Use |
|------------|------------|----------|------------|
| NEO        | SparkMax   | CAN      | Drive Train |
| Falcon 500 | TalonFX    | CAN      | High-Power |
| 775pro     | TalonSRX   | CAN      | General    |
| CIM        | VictorSPX  | CAN      | Legacy     |

## Examples

1. Basic Motor Control
```java
public class ShooterSubsystem extends SubsystemBase {
    private final CANSparkMax shooter = new CANSparkMax(1, MotorType.kBrushless);
    private final RelativeEncoder encoder = shooter.getEncoder();
    
    public ShooterSubsystem() {
        shooter.setIdleMode(IdleMode.kCoast);
        shooter.setSmartCurrentLimit(40);
    }
    
    public void setSpeed(double speed) {
        shooter.set(speed);
    }
    
    public double getVelocity() {
        return encoder.getVelocity();
    }
}
```

2. Motor Group Configuration
```java
public class DriveSubsystem extends SubsystemBase {
    private final WPI_TalonFX[] leftMotors = {
        new WPI_TalonFX(1),
        new WPI_TalonFX(2)
    };
    
    public DriveSubsystem() {
        // Configure followers
        leftMotors[1].follow(leftMotors[0]);
        
        // Configure ramping
        for (WPI_TalonFX motor : leftMotors) {
            motor.configOpenloopRamp(0.5);
            motor.configClosedloopRamp(0.2);
        }
    }
}
```

3. Pneumatic Control
```java
public class IntakeSubsystem extends SubsystemBase {
    private final DoubleSolenoid intakePiston = 
        new DoubleSolenoid(PneumaticsModuleType.CTREPCM, 0, 1);
    private final CANSparkMax intakeMotor = 
        new CANSparkMax(3, MotorType.kBrushless);
    
    public void extend() {
        intakePiston.set(DoubleSolenoid.Value.kForward);
    }
    
    public void retract() {
        intakePiston.set(DoubleSolenoid.Value.kReverse);
    }
    
    public void runIntake(double speed) {
        intakeMotor.set(speed);
    }
}
```

## Motor Configuration

### Current Limiting
```java
// TalonFX Example
motorController.configSupplyCurrentLimit(new SupplyCurrentLimitConfiguration(
    true,    // Enable
    40.0,    // Current limit (amps)
    60.0,    // Trigger threshold (amps)
    0.1      // Trigger threshold time (seconds)
));

// SparkMax Example
motorController.setSmartCurrentLimit(40); // Amps
```

### Control Modes
| Mode | Description | Common Use |
|------|-------------|------------|
| PercentOutput | -1.0 to 1.0 | Basic control |
| Velocity | Units/100ms | Shooting |
| Position | Encoder counts | Arms |
| Current | Amps | Force control |

## Best Practices

### Motor Safety
1. Always use current limits
2. Implement soft limits
3. Use ramping for smooth acceleration
4. Monitor temperature

### Code Organization
1. Group related motors in subsystems
2. Use meaningful constants
3. Implement failsafe defaults
4. Document motor IDs

### Configuration Tips
1. Check motor direction
2. Calibrate sensors
3. Test limit switches
4. Verify encoder operation

## Further Reading
- [CTRE Documentation](https://docs.ctre-phoenix.com/)
- [REV Documentation](https://docs.revrobotics.com/)
- [WPILib Motors](https://docs.wpilib.org/en/stable/docs/hardware/hardware-basics/motors.html)
