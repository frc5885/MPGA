# Sensors and Input Devices

## Sensor Types Hierarchy
```
FRC Sensors
├── Digital Sensors
│   ├── Encoders
│   ├── Limit Switches
│   └── Digital I/O
├── Analog Sensors
│   ├── Potentiometers
│   ├── Ultrasonic
│   └── IR Distance
├── Complex Sensors
│   ├── Gyroscopes
│   ├── Accelerometers
│   └── Vision Cameras
└── Network Tables
    ├── Limelight
    └── PhotonVision
```

## Common Sensor Types

| Sensor Type | Interface | Resolution | Common Use |
|-------------|-----------|------------|------------|
| Quadrature Encoder | DIO | Variable PPR | Position/Velocity |
| Absolute Encoder | CAN/SPI | 12-bit | Absolute Position |
| Limit Switch | DIO | Binary | End Stops |
| Gyro | SPI/I2C | 0.1° | Robot Orientation |
| Ultrasonic | Analog/DIO | ~2mm | Distance |

## Examples

1. Encoder Implementation
```java
public class ArmSubsystem extends SubsystemBase {
    private final TalonFX motor = new TalonFX(1);
    private final DutyCycleEncoder absEncoder = 
        new DutyCycleEncoder(0);
    
    public ArmSubsystem() {
        absEncoder.setDistancePerRotation(360.0);
        absEncoder.setPositionOffset(90.0);
    }
    
    public double getAngle() {
        return absEncoder.getAbsolutePosition();
    }
    
    @Override
    public void periodic() {
        SmartDashboard.putNumber("Arm Angle", getAngle());
    }
}
```

2. Limit Switch Safety
```java
public class ElevatorSubsystem extends SubsystemBase {
    private final CANSparkMax motor = new CANSparkMax(1, MotorType.kBrushless);
    private final DigitalInput bottomLimit = new DigitalInput(0);
    private final DigitalInput topLimit = new DigitalInput(1);
    
    public void setSpeed(double speed) {
        // Stop at limits
        if ((speed < 0 && bottomLimit.get()) || 
            (speed > 0 && topLimit.get())) {
            motor.set(0);
            return;
        }
        motor.set(speed);
    }
}
```

3. Gyro-Based Turning
```java
public class DriveSubsystem extends SubsystemBase {
    private final AHRS gyro = new AHRS(SPI.Port.kMXP);
    
    public void turnToAngle(double targetAngle) {
        double currentAngle = gyro.getAngle();
        double error = targetAngle - currentAngle;
        
        // Simple P controller
        double turnPower = error * 0.01;
        turnPower = Math.min(Math.max(turnPower, -0.5), 0.5);
        
        arcadeDrive(0, turnPower);
    }
}
```

## Sensor Configuration

### Filtering and Smoothing
```java
public class SensorFilter {
    private final int windowSize = 5;
    private final Queue<Double> window = new LinkedList<>();
    
    public double filter(double input) {
        window.add(input);
        if (window.size() > windowSize) {
            window.remove();
        }
        
        return window.stream()
            .mapToDouble(Double::doubleValue)
            .average()
            .orElse(input);
    }
}
```

## Best Practices

### Sensor Management
1. Initialize sensors in robotInit
2. Check sensor connectivity
3. Implement timeout handling
4. Use sensor fusion when possible

### Data Processing
1. Filter noisy signals
2. Implement deadbands
3. Handle sensor failures gracefully
4. Log sensor data for debugging

### Safety Considerations
1. Use redundant sensors for critical systems
2. Implement soft limits before hard limits
3. Regular sensor calibration
4. Failsafe default values

## Further Reading
- [WPILib Sensors](https://docs.wpilib.org/en/stable/docs/software/hardware-apis/sensors/)
- [NavX Documentation](https://pdocs.kauailabs.com/navx-mxp/)
- [REV Through Bore Encoder](https://docs.revrobotics.com/through-bore-encoder/)
