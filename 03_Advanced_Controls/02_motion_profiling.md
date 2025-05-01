# Motion Profiling

## Overview
Motion profiling generates smooth trajectories for robot movement, considering velocity and acceleration constraints.

## Trajectory Generation

### Simple Trapezoidal Profile
```java
public class TrapezoidalProfile {
    private final double maxVelocity;
    private final double maxAcceleration;
    
    public TrapezoidalProfile(double maxV, double maxA) {
        this.maxVelocity = maxV;
        this.maxAcceleration = maxA;
    }
    
    public State calculate(double distance, double currentVelocity) {
        // Calculate acceleration and cruise phases
        double accelTime = maxVelocity / maxAcceleration;
        double accelDist = 0.5 * maxAcceleration * accelTime * accelTime;
        
        return new State(currentVelocity, 
                        calculateAcceleration(distance, currentVelocity));
    }
}
```

### WPILib Trajectory Following
```java
public class DrivetrainSubsystem extends SubsystemBase {
    private final Trajectory trajectory;
    private final RamseteController ramseteController = 
        new RamseteController();
    
    public Command followTrajectory() {
        return new RamseteCommand(
            trajectory,
            this::getPose,
            ramseteController,
            new SimpleMotorFeedforward(0.1, 0.12),
            kinematics,
            this::getWheelSpeeds,
            new PIDController(1, 0, 0),
            new PIDController(1, 0, 0),
            this::tankDriveVolts,
            this
        );
    }
}
```

## Path Planning

### Waypoint Navigation
```java
Trajectory trajectory = TrajectoryGenerator.generateTrajectory(
    new Pose2d(0, 0, new Rotation2d(0)),
    List.of(
        new Translation2d(1, 1),
        new Translation2d(2, -1)
    ),
    new Pose2d(3, 0, new Rotation2d(0)),
    config
);
```

## Control Constraints

| Parameter | Typical Range | Notes |
|-----------|---------------|-------|
| Max Velocity | 1-3 m/s | Robot specific |
| Max Acceleration | 0.5-2 m/s² | Based on weight |
| Max Voltage | 10-12V | Battery dependent |

## Implementation Tips
1. Characterize drivetrain first
2. Use voltage-based control
3. Implement proper odometry
4. Test in simulation before deployment
