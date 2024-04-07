
1. **Single Inheritance**:
   - This is the simplest form of inheritance, where a subclass (child class) inherits from a single superclass (parent class).
   - The subclass inherits all the fields and methods from the superclass, and can also add new fields and methods or override the inherited ones.
   - Example:

      ```java
     class Animal {
         // fields and methods of the Animal class
     }

     class Dog extends Animal {
         // fields and methods of the Dog class
     }
      ```

		![[Pasted image 20240408005529.png]]


2. **Multilevel Inheritance**:
   - In this type of inheritance, a subclass is derived from another subclass.
   - The subclass at the bottom of the hierarchy inherits from the immediate superclass, which in turn inherits from the class above it, and so on.
   - Example:
     ```java
     class Animal {
         // fields and methods of the Animal class
     }

     class Mammal extends Animal {
         // fields and methods of the Mammal class
     }

     class Dog extends Mammal {
         // fields and methods of the Dog class
     }
     ```
	 ![[Pasted image 20240408005647.png]]

3. **Hierarchical Inheritance**:
   - In this type of inheritance, multiple subclasses are derived from a single superclass.
   - The superclass is the parent class for all the subclasses, and the subclasses are siblings to each other.
   - Example:
     ```java
     class Animal {
         // fields and methods of the Animal class
     }

     class Dog extends Animal {
         // fields and methods of the Dog class
     }

     class Cat extends Animal {
         // fields and methods of the Cat class
     }
     ```
	 ![[Pasted image 20240408005708.png]]
4. **Multiple Inheritance**:
	- In Java, multiple inheritance is not supported directly at the class level. However, it can be achieved through the use of interfaces.
	- Example:
    ```java
	interface Flyable {
		// Flyable interface methods
	}
	
	interface Swimmable {
		// Swimmable interface methods
	}
	
	class Duck implements Flyable, Swimmable {
		// Duck class properties and methods
	}
    ```
	![[Pasted image 20240408010357.png]]
5. **Hybrid Inheritance**:
   - Hybrid inheritance is a combination of two or more of the above inheritance types.
   - It is not directly supported in Java, but can be achieved through the use of interfaces and abstract classes.
   - Example:
```java
// Abstract class
abstract class Animal {
	protected String name;
	
	public void speak() {
		System.out.println("The animal speaks");
	}
	}
	
	// Interface
	interface Walkable {
		void walk();
	}
	
	// Interface
	interface Swimmable {
		void swim();
	}
	
	// Concrete class inheriting from abstract class and implementing interfaces
	class Dog extends Animal implements Walkable, Swimmable {
	public void walk() {
		System.out.println("The dog walks");
	}
	
	public void swim() {
		System.out.println("The dog swims");
	}
}
```

![[Pasted image 20240408010937.png]]