Abstraction in Java is a fundamental concept that allows you to focus on the essential features of an object or a system, while hiding the underlying complexity. Abstraction is achieved through the use of abstract classes and interfaces.

1. **Abstract Classes**:
   - An abstract class has *no instances* - it is illegal to use the new operator (cannot be instantiated directly).
   - It serves as a blueprint for other classes to inherit from.
   - Abstract classes can contain both abstract and non-abstract (concrete) methods and even main() method..
   - Abstract classes can have instance variables, constructors, and methods.
   - *A class can extend only one abstract class*.
   - Abstract classes are useful when you want to provide some common functionality or state for related classes.
   - *A class that contains an abstract method must be itself declared abstract*.
   - A **subclass of an abstract class**:
		• implements all abstract methods of its super-class, or
		• is also declared as an abstract class **SEE NOTE**
   - Abstract super-class, concrete subclass
![[Pasted image 20240408020031.png]]

Example:
```java
abstract class Vehicle {
    String make;
    String model;

    Vehicle(String make, String model) {
        this.make = make;
        this.model = model;
    }

    abstract void start();

    void displayInfo() {
        System.out.println("Make: " + make);
        System.out.println("Model: " + model);
    }
}

class abstractKeyword extends Vehicle {
    abstractKeyword(String make, String model) {
        super(make, model);
    }
 
// if start() declared static then it won't be able to override start() from abstract class
    @Override
    void start() { 
        System.out.println("Starting the car engine...");
    }

    public static void main(String[] args) {
        abstractKeyword obj = new abstractKeyword("Boeing", "B-21");
        obj.start();
        obj.displayInfo();
    }
}
```

**Note**: When a subclass extends an abstract class that contains one or more abstract methods, the subclass must provide implementations for all those abstract methods. If the subclass fails to implement any of the abstract methods, it must also be declared as abstract otherwise compiletime error will be created.

```java
abstract class Shape {
    // Abstract method
    abstract double area();
}

// Subclass that provides implementation for the abstract method
class Rectangle extends Shape {
    double length, width;

    Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }

    // Implementing abstract method
    double area() {
        return length * width;
    }
}

// Subclass that does not provide implementation for the abstract method
class Circle extends Shape {
    double radius;

    Circle(double radius) {
        this.radius = radius;
    }
    // Error: Circle must either implement the abstract method area() or be declared abstract
}
```

In this example, the `Rectangle` class provides an implementation for the `area()` method, so it is not required to be declared as abstract. However, the `Circle` class does not provide an implementation for the `area()` method. Therefore, it must be declared as abstract:

```java
abstract class Circle extends Shape {
    double radius;
}
```

Alternatively, if `Circle` were to provide an implementation for the `area()` method, it wouldn't need to be declared as abstract:

```java
class Circle extends Shape {
    double radius;

    Circle(double radius) {
        this.radius = radius;
    }

    // Implementing abstract method
    double area() {
        return Math.PI * radius * radius;
    }
}
``` 

2. **Interfaces**:
   - An interface is a contract that defines a set of methods and/or constants.
   - Interfaces do not contain any implementation details; they only specify the method signatures.
   - Interfaces can contain default and static methods, which provide some implementation details.
   - A class can implement multiple interfaces.
   - Interfaces are useful for achieving abstraction and providing a common API for related classes.

Example:
```java
interface Drivable {
    void accelerate();
    void brake();
    int getSpeed();
}

class SportsCar implements Drivable {
    private int speed;

    public void accelerate() {
        speed += 10;
    }

    public void brake() {
        speed -= 5;
    }

    public int getSpeed() {
        return speed;
    }
}
```

In both abstract classes and interfaces, the implementation details are hidden, and the focus is on the essential features or behaviors. This abstraction allows for code reuse, flexibility, and extensibility in Java applications.

