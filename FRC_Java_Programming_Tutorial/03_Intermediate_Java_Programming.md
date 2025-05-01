# Intermediate Java Programming

In this section, we will explore more advanced Java concepts that are essential for building robust and efficient FRC robot programs.

---

## 1. Collections Framework

The Java Collections Framework provides data structures like lists, sets, and maps to store and manipulate groups of objects.

### Example: Using an ArrayList
```java
import java.util.ArrayList;

public class Example {
    public static void main(String[] args) {
        ArrayList<String> teamMembers = new ArrayList<>();
        teamMembers.add("Alice");
        teamMembers.add("Bob");
        teamMembers.add("Charlie");

        for (String member : teamMembers) {
            System.out.println(member);
        }
    }
}
```

### Example: Using a HashMap
```java
import java.util.HashMap;

public class Example {
    public static void main(String[] args) {
        HashMap<Integer, String> robotModes = new HashMap<>();
        robotModes.put(1, "Autonomous");
        robotModes.put(2, "Teleop");
        robotModes.put(3, "Test");

        System.out.println("Mode 1: " + robotModes.get(1));
    }
}
```

---

## 2. Exception Handling

Exception handling ensures your program can handle errors gracefully.

### Example: Try-Catch Block
```java
public class Example {
    public static void main(String[] args) {
        try {
            int result = 10 / 0; // This will throw an exception
        } catch (ArithmeticException e) {
            System.out.println("Error: Division by zero is not allowed.");
        }
    }
}
```

### Example: Throwing an Exception
```java
public class Example {
    public static void main(String[] args) throws Exception {
        throw new Exception("Custom exception message");
    }
}
```

---

## 3. File I/O

File I/O allows you to read from and write to files, which can be useful for logging robot data.

### Example: Writing to a File
```java
import java.io.FileWriter;
import java.io.IOException;

public class Example {
    public static void main(String[] args) {
        try (FileWriter writer = new FileWriter("log.txt")) {
            writer.write("Robot initialized successfully.");
        } catch (IOException e) {
            System.out.println("Error writing to file: " + e.getMessage());
        }
    }
}
```

### Example: Reading from a File
```java
import java.io.File;
import java.io.IOException;
import java.util.Scanner;

public class Example {
    public static void main(String[] args) {
        try (Scanner scanner = new Scanner(new File("log.txt"))) {
            while (scanner.hasNextLine()) {
                System.out.println(scanner.nextLine());
            }
        } catch (IOException e) {
            System.out.println("Error reading from file: " + e.getMessage());
        }
    }
}
```

---

## 4. Threads and Concurrency

Threads allow you to perform multiple tasks simultaneously, which is useful for handling robot subsystems.

### Example: Creating a Thread
```java
public class Example {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("Thread running: " + i);
                try {
                    Thread.sleep(1000); // Pause for 1 second
                } catch (InterruptedException e) {
                    System.out.println("Thread interrupted.");
                }
            }
        });

        thread.start();
    }
}
```

---

This concludes the **Intermediate Java Programming** section. Next, we will dive into [Introduction to FRC Programming](04_Introduction_to_FRC_Programming.md).
