# Vision Processing

## Overview
Vision processing enables robots to identify and track targets using cameras and image processing.

## Vision Systems

### Common Solutions
- Limelight
- PhotonVision
- Raspberry Pi with OpenCV

## Implementation

### Limelight Example
```java
public class VisionSubsystem extends SubsystemBase {
    private final NetworkTable table = 
        NetworkTableInstance.getDefault().getTable("limelight");
    
    public boolean hasTarget() {
        return table.getEntry("tv").getDouble(0) == 1.0;
    }
    
    public double getTargetX() {
        return table.getEntry("tx").getDouble(0);
    }
    
    public double getTargetY() {
        return table.getEntry("ty").getDouble(0);
    }
}
```

### Target Tracking
```java
public class TrackTargetCommand extends CommandBase {
    private final DriveSubsystem drive;
    private final VisionSubsystem vision;
    private final PIDController turnPID = new PIDController(0.05, 0, 0);
    
    public void execute() {
        if (vision.hasTarget()) {
            double rotation = turnPID.calculate(vision.getTargetX(), 0);
            drive.arcadeDrive(0, rotation);
        }
    }
}
```

## Pipeline Configuration

| Parameter | Typical Value | Purpose |
|-----------|---------------|---------|
| Exposure | 2-6ms | Reduce blur |
| LED Mode | 3 (Force ON) | Consistent lighting |
| Pipeline | 0-9 | Different targets |
| Stream | 2 (PiP Main) | Driver feedback |

## Distance Calculation
```java
public double calculateDistance() {
    double targetHeightInches = 104.0;
    double cameraHeightInches = 24.0;
    double cameraPitchDegrees = 25.0;
    double targetY = vision.getTargetY();
    
    return (targetHeightInches - cameraHeightInches) /
           Math.tan(Math.toRadians(cameraPitchDegrees + targetY));
}
```

## Best Practices

1. Vision Setup
   - Calibrate cameras
   - Test lighting conditions
   - Backup strategies

2. Processing
   - Filter noise
   - Handle target loss
   - Validate data

3. Competition
   - Multiple pipelines
   - Quick switching
   - Network resilience
