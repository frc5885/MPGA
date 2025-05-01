# Object-Oriented Programming in Java

## Class Hierarchy Diagram
```
Object-Oriented Concepts
├── Class
│   ├── Properties (Fields)
│   └── Methods
├── Inheritance
│   ├── Superclass
│   └── Subclass
├── Encapsulation
│   ├── Public
│   ├── Private
│   └── Protected
└── Polymorphism
    ├── Method Overriding
    └── Method Overloading
```

## Examples

1. Basic Class Structure
```java
public class Robot {
    // Properties
    private double speed;
    private boolean enabled;
    
    // Constructor
    public Robot() {
        this.speed = 0.0;
        this.enabled = false;
    }
    
    // Methods
    public void setSpeed(double speed) {
        if (enabled) {
            this.speed = Math.min(1.0, Math.max(-1.0, speed));
        }
    }
    
    public double getSpeed() {
        return this.speed;
    }
}
```

2. Inheritance Example
```java
public class Subsystem {
    protected String name;
    protected boolean initialized;
    
    public void initialize() {
        initialized = true;
    }
}

public class DriveSubsystem extends Subsystem {
    private Motor leftMotor;
    private Motor rightMotor;
    
    @Override
    public void initialize() {
        super.initialize();
        leftMotor.configure();
        rightMotor.configure();
    }
}
```

3. Polymorphism Example
```java
public class Controller {
    // Method overloading
    public void setSpeed(int speed) {
        setSpeed((double)speed / 100.0);
    }
    
    public void setSpeed(double speed) {
        // Direct speed setting
    }
}
```

## Access Modifiers Table
| Modifier  | Class | Package | Subclass | World |
|-----------|--------|----------|-----------|-------|
| public    | ✓      | ✓        | ✓         | ✓     |
| protected | ✓      | ✓        | ✓         | ✗     |
| default   | ✓      | ✓        | ✗         | ✗     |
| private   | ✓      | ✗        | ✗         | ✗     |

## Key OOP Concepts

### Encapsulation
- Bundling data and methods that operate on that data within a single unit
- Hiding internal details and providing an interface

### Inheritance
- Creating new classes that are built upon existing classes
- Promotes code reuse and establishes relationships between classes

### Polymorphism
- Ability of objects to take on multiple forms
- Method overriding and overloading
- Runtime vs compile-time polymorphism

## Best Practices
1. Use meaningful class and method names
2. Follow proper encapsulation principles
3. Design classes with single responsibility
4. Implement interfaces for common behaviors
5. Use inheritance judiciously

## Further Reading
- [Java OOP Concepts](https://docs.oracle.com/javase/tutorial/java/concepts/)
- [WPILib Object-Oriented Programming](https://docs.wpilib.org/en/stable/docs/software/basic-programming/java-basics.html)
