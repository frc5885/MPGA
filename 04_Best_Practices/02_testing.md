# Testing and Debugging

## Unit Testing

### Example Test Class
```java
public class ShooterTest {
    private ShooterSubsystem shooter;
    
    @Before
    public void setup() {
        shooter = new ShooterSubsystem();
    }
    
    @Test
    public void testVelocityControl() {
        shooter.setVelocity(2000);
        assertEquals(2000, shooter.getTargetVelocity(), 0.1);
    }
}
```

## Simulation Testing
```java
public class DriveSimulation {
    private final DifferentialDrivetrainSim driveSim;
    
    public DriveSimulation() {
        driveSim = new DifferentialDrivetrainSim(
            DCMotor.getNEO(2),    // 2 NEOs per side
            7.29,                 // Gear ratio
            6.0,                 // MOI
            60.0,               // Mass kg
            Units.inchesToMeters(3), // Wheel radius
            0.7112,            // Track width
            null               // Standard deviations
        );
    }
}
```

## Debugging Tools

### Dashboard Widgets
```java
public class DebugDashboard {
    public static void updateShooterData(ShooterSubsystem shooter) {
        SmartDashboard.putNumber("Shooter/Velocity", 
            shooter.getVelocity());
        SmartDashboard.putNumber("Shooter/Target", 
            shooter.getTargetVelocity());
        SmartDashboard.putNumber("Shooter/Error", 
            shooter.getError());
    }
}
```

## Common Issues Checklist
1. Electrical
   - CAN termination
   - Sensor connections
   - Power distribution

2. Software
   - Motor direction
   - Sensor polarity
   - PID tuning

3. Mechanical
   - Encoder mounting
   - Chain/belt tension
   - Gear mesh

## Debugging Process
1. Identify symptoms
2. Gather data
3. Form hypothesis
4. Test solution
5. Document findings
