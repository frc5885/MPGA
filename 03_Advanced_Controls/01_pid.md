# PID Control

## Overview
PID (Proportional, Integral, Derivative) control is a feedback mechanism used for precise motor control and robot movements.

## Components
- **P (Proportional)**: Responds proportionally to the current error
- **I (Integral)**: Accumulates error over time to eliminate steady-state error
- **D (Derivative)**: Responds to the rate of change to reduce oscillation

## Implementation Examples

### Basic PID Controller
```java
public class SimplePIDController {
    private final double kP, kI, kD;
    private double integral = 0;
    private double previousError = 0;
    
    public SimplePIDController(double kP, double kI, double kD) {
        this.kP = kP;
        this.kI = kI;
        this.kD = kD;
    }
    
    public double calculate(double setpoint, double measurement) {
        double error = setpoint - measurement;
        integral += error * 0.02; // 20ms loop time
        double derivative = (error - previousError) / 0.02;
        previousError = error;
        
        return kP * error + kI * integral + kD * derivative;
    }
}
```

### WPILib PID Example
```java
public class ShooterSubsystem extends SubsystemBase {
    private final CANSparkMax motor = new CANSparkMax(1, MotorType.kBrushless);
    private final PIDController pid = new PIDController(0.1, 0.0, 0.01);
    
    public void setVelocity(double targetRPM) {
        double currentRPM = motor.getEncoder().getVelocity();
        double output = pid.calculate(currentRPM, targetRPM);
        motor.set(output);
    }
}
```

## Tuning Process

1. Start with all gains at 0
2. Increase P until oscillation
3. Reduce P by 40%
4. Increase D to reduce overshoot
5. Add I if steady-state error persists

## Common PID Applications

| Mechanism | P Range | I Range | D Range |
|-----------|---------|---------|---------|
| Flywheel  | 0.1-0.3 | 0-0.001 | 0-0.1   |
| Arm       | 1.0-3.0 | 0-0.01  | 0.1-1.0 |
| Drive     | 0.5-2.0 | 0-0.001 | 0.1-0.5 |

## Advanced Techniques

### Feed Forward
```java
public class ShooterPIDFF {
    private final SimpleMotorFeedforward ff = 
        new SimpleMotorFeedforward(0.1, 0.12);
    private final PIDController pid = 
        new PIDController(0.1, 0, 0);
        
    public double calculate(double setpoint, double measurement) {
        return ff.calculate(setpoint) + 
               pid.calculate(measurement, setpoint);
    }
}
```

## Best Practices
1. Always implement deadbands
2. Use motion profiling for position control
3. Consider feed forward for known systems
4. Log PID data for tuning
