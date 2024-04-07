- **Dynamic method dispatch**, also known as **runtime polymorphism**, is a feature in object-oriented programming that allows an *object of a subclass to be treated as an object of its superclass*.
- This means that the *appropriate method* to be called is determined at runtime based on the *actual type of the object*, rather than at compile-time based on the *reference type*.
- In Java, dynamic method dispatch is achieved through **method overriding**. When a method in a subclass has the same name, return type, and parameter list as a method in its superclass, it is considered an overridden method. 
- When an overridden method is called on an object, the Java Virtual Machine (JVM) determines the appropriate method to execute based on the actual type of the object, not the declared type of the reference.

Here's an example to illustrate dynamic method dispatch in Java:

```java
class Animal {
    public void makeSound() {
        System.out.println("The animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("The dog barks");
    }
}

class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("The cat meows");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animalRef; // Reference type: Animal

        animalRef = new Dog(); // Actual type: Dog
        animalRef.makeSound(); // Output: The dog barks

        animalRef = new Cat();
        animalRef.makeSound(); // Output: The cat meows
    }
}
```

In this example, the `makeSound()` method is overridden in the `Dog` and `Cat` subclasses. When the `makeSound()` method is called on the `animalRef` variable, the actual implementation of the method is determined at runtime based on the object that `animalRef` is referring to, not the declared type of `animalRef`, which is `Animal`.