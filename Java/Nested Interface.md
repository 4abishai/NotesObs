- An *interface, that is declared within another interface or class*, is known as a nested interface.
- The nested interfaces are used to group related interfaces so that they can be *easy to maintain*.
- The nested interface *must be referred to by the outer interface or class*. It can't be accessed directly.
- Nested interfaces are implicitly **static** regardless of where they are declared (*inside class or another interface*).
- Nested interface declared *inside another interface* is implicitly **public**.
- Nested interface declared inside a class can accept any access modifiers.

### Nested Interface within another interface

```java
interface Showable {
    void show();

    interface Message {
        void msg();
    }
}

class TestNestedInterface1 implements Showable.Message {
    public void msg() {
        System.out.println("Inside the nested interface");
    }

    public static void main(String args[]) {
        Showable.Message message = new TestNestedInterface1();
        // upcasting here as we are accessing the Message
		// interface by its outer interface Showable because it
		// cannot be accessed directly. It is just like the
		// almirah inside the room; we cannot access the almirah
		// directly because we must enter the room first.

        message.msg();
    }
}
```
**Compiling the file as**: javac TestNestedInterface1.java
On compiling the above file, we are able to get the class files for both the parent and nested interface, along with the class with main method.
![[Pasted image 20240408132229.png]]

**Note**:
- ![[Pasted image 20240408131408.png]]

- We do not require to run the classes of the generated
interfaces, but instead only of the class which uses the interfaces.

### Nested Interface within a Class
```java
class A {
    interface Message {
        void msg();

    }
}

class TestNestedInterface2 implements A.Message {
    public void msg() {
        System.out.println("Hello nested interface");
    }

    public static void main(String args[]) {
        A.Message message = new TestNestedInterface2();// upcasting here
        message.msg();
    }
}
```

![[Pasted image 20240408131934.png]]
**Compiling the file as**: javac TestNestedInterface2.java
On compiling the above file, we are able to get the class files for both the parent class and nested interface, along with the class with main method.
![[Pasted image 20240408132601.png]]

#### Can we define a class inside the interface?
Yes, if we define a class inside the interface, the Java compiler creates a static nested class.
![[Pasted image 20240408132725.png]]
