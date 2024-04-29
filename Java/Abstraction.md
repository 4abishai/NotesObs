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
   - An interface comprises a set of abstract methods that we would want our class to implement. 
   - These methods are **`public`**, **`static`**, and *abstract by default*(i.e., we don’t have to explicitly use the “abstract” keyword).
   - In Java, interface variables are implicitly **`public`**, **`static`**, and **`final`**. This means that interface variables are constants, and they cannot be changed once they are assigned a value. Here's an example:

   ```java
	public interface MyInterface {
	    int MY_CONSTANT = 100; 
	    // This variable is public, static, and final by default
	}
   ```

   - Any class implementing the interface will need to provide implementations of all of those methods.
   - A class can implement multiple interfaces.
   - Interfaces are useful for achieving abstraction and providing a common API for related classes.
![[Pasted image 20240408123729.png]]
![[Pasted image 20240408124126.png]]


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

**Note**: `public` must be used when overriding interface method(s) due to [[Liskov Substitution Principle]].

### Why use Interface

Sure, let's go through some more detailed examples to illustrate the benefits of using Java interfaces.

#### Better Polymorphism
Interfaces allow for more flexibility in polymorphism. Instead of everything having to fit into a single hierarchy of classes, interfaces let you define a *contract that any class can implement*, allowing much more variety in the types of objects that can be used interchangeably.

Let's say we have an application that deals with different types of vehicles - cars, motorcycles, and bicycles. We can create an interface called `Vehicle` that defines the common methods that all vehicles should have, such as `start()`, `stop()`, and `accelerate()`.

```java
public interface Vehicle {
    void start();
    void stop();
    void accelerate(int speed);
}
```

Now, we can create concrete implementations of the `Vehicle` interface for each type of vehicle:

```java
public class Car implements Vehicle {
    public void start() {
        System.out.println("Car starting...");
    }

    public void stop() {
        System.out.println("Car stopping...");
    }

    public void accelerate(int speed) {
        System.out.println("Car accelerating to " + speed + " km/h");
    }
}

public class Motorcycle implements Vehicle {
    public void start() {
        System.out.println("Motorcycle starting...");
    }

    public void stop() {
        System.out.println("Motorcycle stopping...");
    }

    public void accelerate(int speed) {
        System.out.println("Motorcycle accelerating to " + speed + " km/h");
    }
}
```

Now, we can write a method that can operate on any type of `Vehicle`, without needing to know the specific implementation:

```java
public static void testVehicle(Vehicle vehicle) {
    vehicle.start();
    vehicle.accelerate(50);
    vehicle.stop();
}
```

This demonstrates the power of polymorphism enabled by interfaces. *The `testVehicle()` method can work with any object that implements the `Vehicle` interface, without needing to know the specific type of vehicle.*

#### Multiple Inheritance
Java does not support multiple inheritance of classes, but interfaces provide a way to achieve something similar. A class can implement multiple interfaces, giving it the benefits of the functionality defined in those interfaces, without the complications of the diamond problem that can arise with multiple inheritance of classes.

![[Pasted image 20240408125703.png]]

Let's say we have a `Shape` interface that defines common methods for geometric shapes, and a `Drawable` interface that defines methods for drawing shapes on a canvas. We can create a `Circle` class that implements both interfaces:

```java
public interface Shape {
    double getArea();
    double getPerimeter();
}

public interface Drawable {
    void draw(Graphics g);
}

public class Circle implements Shape, Drawable {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    public double getArea() {
        return Math.PI * radius * radius;
    }

    public double getPerimeter() {
        return 2 * Math.PI * radius;
    }

    public void draw(Graphics g) {
        g.drawOval((int)(radius), (int)(radius), (int)(2 * radius), (int)(2 * radius));
    }
}
```

In this example, the `Circle` class can take advantage of the functionality defined in both the `Shape` and `Drawable` interfaces, without having to inherit from a specific base class. This allows for much more flexibility in the class hierarchy, as the `Circle` class can now be used anywhere a `Shape` or `Drawable` is required.

#### Abstraction

Interfaces promote abstraction by defining a contract without exposing the underlying implementation details. This allows you to hide complexity and present a simple, easy-to-use API to the consumer of the interface. The example of starting a car illustrates this nicely - the user doesn't need to know how the car actually starts, they just need to call the `start()` method.

**Note**: 
![[Pasted image 20240408125922.png]]

**SEE** [[Nested Interface]]
