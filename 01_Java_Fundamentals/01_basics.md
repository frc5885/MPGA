# Basic Java Concepts

## Data Types Hierarchy
```
Java Data Types
├── Primitive Types
│   ├── Numeric
│   │   ├── Integer Types
│   │   │   ├── byte  (8 bits)
│   │   │   ├── short (16 bits)
│   │   │   ├── int   (32 bits)
│   │   │   └── long  (64 bits)
│   │   └── Floating Point
│   │       ├── float  (32 bits)
│   │       └── double (64 bits)
│   ├── char (16 bits)
│   └── boolean (1 bit)
└── Reference Types
    ├── Classes
    ├── Interfaces
    └── Arrays
```

## Variables and Constants

### Examples

1. Variable Declaration and Initialization
```java
// Basic variable declarations
int motorSpeed = 0;
double voltage = 12.6;
boolean isEnabled = true;

// Constants
final double PI = 3.14159;
final int MAX_SPEED = 100;
```

2. Type Conversion
```java
// Implicit conversion
int smallNumber = 10;
long bigNumber = smallNumber; // OK

// Explicit conversion
double speed = 45.7;
int roundedSpeed = (int) speed; // 45
```

3. String Operations
```java
String team = "Team ";
int number = 1234;
String fullName = team + number; // "Team 1234"

// String formatting
String formatted = String.format("Speed: %.2f", 12.3456); // "Speed: 12.35"
```

## Control Flow

### Truth Table for Logical Operators
| A | B | A && B | A \|\| B | !A |
|---|---|--------|----------|-----|
| T | T | T      | T        | F   |
| T | F | F      | T        | F   |
| F | T | F      | T        | T   |
| F | F | F      | F        | T   |

### Examples

1. If-Else Statements
```java
if (voltage > 12.0) {
    System.out.println("Battery OK");
} else if (voltage > 10.0) {
    System.out.println("Battery Low");
} else {
    System.out.println("Replace Battery");
}
```

2. Switch Statement
```java
int autoMode = 2;
switch (autoMode) {
    case 1:
        runSimpleAuto();
        break;
    case 2:
        runComplexAuto();
        break;
    default:
        runDefaultAuto();
}
```

3. Loops
```java
// For loop
for (int i = 0; i < 10; i++) {
    setMotorSpeed(i * 0.1);
}

// While loop
while (isAutonomous() && isEnabled()) {
    checkSensors();
    updateMotors();
}

// Do-while loop
do {
    readJoystick();
    updateDrive();
} while (isTeleop());
```

## Further Reading
- [Oracle Java Tutorial](https://docs.oracle.com/javase/tutorial/)
- [WPILib Documentation](https://docs.wpilib.org/en/stable/)
