# Data Structures and Algorithms

## Common Data Structures in FRC

### Data Structure Hierarchy
```
Data Structures
├── Linear
│   ├── Arrays
│   ├── ArrayList
│   └── Queue
├── Non-Linear
│   ├── Tree
│   └── Graph
└── Hash-Based
    └── HashMap
```

## Examples

1. Array Operations for Sensor Data
```java
public class SensorBuffer {
    private double[] values;
    private int currentIndex;
    
    public SensorBuffer(int size) {
        values = new double[size];
        currentIndex = 0;
    }
    
    public void addReading(double value) {
        values[currentIndex] = value;
        currentIndex = (currentIndex + 1) % values.length;
    }
    
    public double getAverage() {
        double sum = 0;
        for (double value : values) {
            sum += value;
        }
        return sum / values.length;
    }
}
```

2. ArrayList for Dynamic Commands
```java
import java.util.ArrayList;

public class AutoSequence {
    private ArrayList<Command> commands;
    
    public AutoSequence() {
        commands = new ArrayList<>();
    }
    
    public void addCommand(Command cmd) {
        commands.add(cmd);
    }
    
    public void executeSequence() {
        for (Command cmd : commands) {
            cmd.execute();
        }
    }
}
```

3. HashMap for Robot Configuration
```java
import java.util.HashMap;

public class RobotConfig {
    private HashMap<String, Double> constants;
    
    public RobotConfig() {
        constants = new HashMap<>();
        constants.put("kP", 0.5);
        constants.put("kI", 0.1);
        constants.put("kD", 0.1);
    }
    
    public Double getValue(String key) {
        return constants.getOrDefault(key, 0.0);
    }
}
```

## Common Algorithms in FRC

### Performance Comparison
| Algorithm | Time Complexity | Space Complexity | Use Case |
|-----------|----------------|------------------|----------|
| Linear Search | O(n) | O(1) | Finding sensor value |
| Binary Search | O(log n) | O(1) | Lookup tables |
| Bubble Sort | O(n²) | O(1) | Small data sets |
| Quick Sort | O(n log n) | O(log n) | Large data sets |

### Examples

1. Moving Average Filter
```java
public class MovingAverageFilter {
    private Queue<Double> window;
    private int maxSize;
    private double sum;
    
    public MovingAverageFilter(int size) {
        window = new LinkedList<>();
        maxSize = size;
        sum = 0.0;
    }
    
    public double process(double value) {
        sum += value;
        window.add(value);
        
        if (window.size() > maxSize) {
            sum -= window.remove();
        }
        
        return sum / window.size();
    }
}
```

2. Binary Search for Lookup Tables
```java
public class LookupTable {
    private double[] inputs;
    private double[] outputs;
    
    public double interpolate(double input) {
        int index = binarySearch(input);
        if (index < 0) return outputs[0];
        if (index >= inputs.length - 1) return outputs[outputs.length - 1];
        
        return linearInterpolate(inputs[index], outputs[index],
                               inputs[index + 1], outputs[index + 1],
                               input);
    }
    
    private int binarySearch(double target) {
        int low = 0;
        int high = inputs.length - 1;
        
        while (low <= high) {
            int mid = (low + high) / 2;
            if (inputs[mid] == target) return mid;
            if (inputs[mid] < target) low = mid + 1;
            else high = mid - 1;
        }
        return high;
    }
}
```

## Best Practices
1. Choose appropriate data structures based on use case
2. Consider memory constraints on the RoboRIO
3. Use built-in Java collections when possible
4. Implement efficient algorithms for real-time operations
5. Document complex data structures and algorithms

## Further Reading
- [Java Collections Framework](https://docs.oracle.com/javase/tutorial/collections/)
- [WPILib Data Storage](https://docs.wpilib.org/en/stable/docs/software/basic-programming/arrays.html)
