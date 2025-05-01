# 📗 **Basic Java Programming**

In this section, we will cover the **fundamentals of Java programming**. These concepts are essential for understanding how to program an FRC robot.

---

## 🖥️ **1. Introduction to Java**

Java is a high-level, object-oriented programming language widely used in various applications, including robotics. It is platform-independent, meaning code written in Java can run on any device with a Java Virtual Machine (JVM).

### 🔑 **Key Features of Java**:
- **Object-Oriented**: Focuses on objects and classes.
- **Platform-Independent**: Write once, run anywhere.
- **Robust and Secure**: Includes features like garbage collection and exception handling.

### 🛠️ **Example: Hello World in Java**
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

---

## 📊 **2. Variables and Data Types**

Variables are used to store data, and Java provides several data types to define the kind of data a variable can hold.

### 🔢 **Data Type Hierarchy**
| **Data Type**   | **Size**       | **Example Values**       |
|------------------|---------------|---------------------------|
| `byte`          | 1 byte        | -128 to 127              |
| `short`         | 2 bytes       | -32,768 to 32,767        |
| `int`           | 4 bytes       | -2^31 to 2^31-1          |
| `long`          | 8 bytes       | -2^63 to 2^63-1          |
| `float`         | 4 bytes       | 3.4e−038 to 3.4e+038     |
| `double`        | 8 bytes       | 1.7e−308 to 1.7e+308     |
| `char`          | 2 bytes       | 'a', '1', '$'            |
| `boolean`       | 1 bit         | `true`, `false`          |

### 🛠️ **Example: Declaring Variables**
```java
int age = 16;
double speed = 3.5;
boolean isRobotEnabled = true;
```

---

## 🔄 **3. Control Structures**

Control structures allow you to control the flow of your program.

### 🛠️ **If-Else Example**
```java
if (isRobotEnabled) {
    System.out.println("Robot is enabled.");
} else {
    System.out.println("Robot is disabled.");
}
```

### 🛠️ **For Loop Example**
```java
for (int i = 0; i < 5; i++) {
    System.out.println("Iteration: " + i);
}
```

### 🛠️ **Switch Example**
```java
int mode = 1;
switch (mode) {
    case 1:
        System.out.println("Autonomous Mode");
        break;
    case 2:
        System.out.println("Teleop Mode");
        break;
    default:
        System.out.println("Unknown Mode");
}
```

---

## 🧩 **4. Methods and Classes**

Methods are blocks of code that perform specific tasks, and classes are blueprints for creating objects.

### 🛠️ **Example: Defining a Class and Method**
```java
public class Robot {
    String name;

    public Robot(String name) {
        this.name = name;
    }

    public void greet() {
        System.out.println("Hello, I am " + name);
    }
}

public class Main {
    public static void main(String[] args) {
        Robot robot = new Robot("FRC Bot");
        robot.greet();
    }
}
```

---

This concludes the **Basic Java Programming** section. Next, we will explore [📘 Intermediate Java Programming](03_Intermediate_Java_Programming.md) concepts.
