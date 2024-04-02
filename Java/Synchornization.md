
- If a method or block is declared with the “synchronized” keyword, then at any time *only one thread is allowed to execute that method or block* on the given object.
- “Synchronized” keyword is applicable for **methods** and **blocks** but not for **classes** and **variables**.
-  The main advantage of synchronized keyword is that we can resolve *inconsistency problems*.

#### Disadvantage
- It increases waiting time of the Thread, thereby affecting performance of the system.

###  Working
- If a Thread wants to execute any synchronized method on the given object then it has to:
	- First get the lock of that object.
	- It is then allowed to execute any synchronized method on that object.
	- Once the synchronized method execution completes then automatically the Thread releases lock.

### Locks

#### 1. Object Lock (synchronized block/method): 
- When you use the `synchronized` keyword with a block or method, you are acquiring a lock on the object instance (`this` reference) that is executing that block or method. 
- This means that other synchronized blocks or methods of the *same object* cannot be executed concurrently by other threads. But remaining Threads are allowed to execute any non-synchronized method simultaneously
- *Each instance of a class has its own lock*, so if one thread acquires the lock of an instance, other threads must wait for it to release the lock before they can access synchronized blocks or methods of that instance.

```java
public synchronized void synchronizedMethod() {
	// This method is synchronized on the instance level
}

synchronized(this) {
	// This block is synchronized on the instance level
}
```

##### Method synchronized on the instance level

![[Pasted image 20240329221152.png]]

![[Pasted image 20240329221246.png]]

![[Pasted image 20240329221311.png]]

##### Block synchronized on instance level

```java
class Display {
    public void wish(String name) {
        synchronized (this) {
            for (int i = 0; i < 5; i++) {
                System.out.print("good morning:");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(name);
            }
        }
    }
}

class MyThread extends Thread {
    Display d;
    String name;

    MyThread(Display d, String name) {
        this.d = d;
        this.name = name;
    }

    public void run() {
        d.wish(name);
    }
}

class objLockOnBlock {
    public static void main(String[] args) {
        Display d1 = new Display();
        MyThread t1 = new MyThread(d1, "Sourajit");
        MyThread t2 = new MyThread(d1, "Behera");
        t1.start();
        t2.start();
    }
}
```

```CSS
good morning:Sourajit
good morning:Sourajit
good morning:Sourajit
good morning:Sourajit
good morning:Sourajit
good morning:Behera
good morning:Behera
good morning:Behera
good morning:Behera
good morning:Behera
```
##### Note
![[Pasted image 20240329221410.png]]

#### 2. Class Lock (synchronized static block/method): 
- When you use the `synchronized` keyword with a static block or method, you are acquiring a lock on the class object (`Class` instance) associated with the class. 
- This means that other synchronized static blocks or methods of the *same class* cannot be executed concurrently by other threads. 
- There is only one class object per class, so if one thread acquires the lock of a class, other threads must wait for it to release the lock before they can access synchronized static blocks or methods of that class.

```java
public static synchronized void synchronizedStaticMethod() {
	// This method is synchronized on the class level
}

synchronized(MyClass.class) {
	// This block is synchronized on the class level
}
```

##### Method synchronized on class level

```java
class Display {
    public static synchronized void wish(String name) {
        for (int i = 0; i < 5; i++) {
            System.out.print("good morning:");
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(name);
        }
    }
}

class MyThread extends Thread {
    String name;

    MyThread(String name) {
        this.name = name;
    }

    public void run() {
        Display.wish(name);
    }
}

class classLockOnMethod {
    public static void main(String[] args) {
        MyThread t1 = new MyThread("Sourajit");
        MyThread t2 = new MyThread("Behera");
        t1.start();
        t2.start();
    }
}
```

```CSS
good morning:Sourajit
good morning:Sourajit
good morning:Sourajit
good morning:Sourajit
good morning:Sourajit
good morning:Behera
good morning:Behera
good morning:Behera
good morning:Behera
good morning:Behera
```

##### Method synchronized on block level

```java
class Display {
    public static void wish(String name) {
        synchronized (Display.class) {
            for (int i = 0; i < 5; i++) {
                System.out.print("good morning:");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(name);
            }
        }
    }
}

class MyThread extends Thread {
    String name;

    MyThread(String name) {
        this.name = name;
    }

    public void run() {
        Display.wish(name);
    }
}

class classLockOnBlock {
    public static void main(String[] args) {
        MyThread t1 = new MyThread("Sourajit");
        MyThread t2 = new MyThread("Behera");
        t1.start();
        t2.start();
    }
}
```

```CSS
good morning:Sourajit
good morning:Sourajit
good morning:Sourajit
good morning:Sourajit
good morning:Sourajit
good morning:Behera
good morning:Behera
good morning:Behera
good morning:Behera
good morning:Behera
```

### Note
- Synchronized statements are those that are contained in synchronized blocks and synchronized methods.
- The primary benefit of a synchronized block over a synchronized method is that it increases system performance and decreases thread waiting times.
- ![[Pasted image 20240329233635.png]]

### Multiple Locks

Can a Thread can hold more than one lock at a time?
Ans - Yes, but of course from different objects.

```java
public class MultipleLock extends Thread {
    private final Object lock1 = new Object();
    private final Object lock2 = new Object();

    public void method1() {
        synchronized (lock1) {
            // Do some work
            for (int i = 0; i < 3; i++) {
                System.out.println("Thread " + Thread.currentThread().getId() + " acquired lock1");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                }
            }
            method2();
        }
    }

    public void method2() {
        synchronized (lock2) {
            // Do some work
            for (int i = 0; i < 3; i++) {
                System.out.println("Thread " + Thread.currentThread().getId() + " acquired lock2");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                }
            }
        }
    }

    public static void main(String[] args) {
        MultipleLock thread1 = new MultipleLock();
        MultipleLock thread2 = new MultipleLock();

        thread1.start();
        thread2.start();
    }

    @Override
    public void run() {
        method1();
    }
}
```

when `synchronized (lock1)` is used, each instance of `MultipleLock` has its own `lock1` object. This means that if one thread acquires `lock1` on one instance of `MultipleLock`, it does not prevent another thread from acquiring `lock1` on a different instance of `MultipleLock`. Each instance's `lock1` is independent of the others.

```CSS
// both Thread 20 21 acquire lock1 lock2 at the same time as they are aquiring // instance level locks and lock1 and lock2 can have several instances

Thread 20 acquired lock1    
Thread 21 acquired lock1
Thread 20 acquired lock1
Thread 21 acquired lock1
Thread 20 acquired lock1
Thread 21 acquired lock1
Thread 20 acquired lock2
Thread 21 acquired lock2
Thread 20 acquired lock2
Thread 21 acquired lock2
Thread 20 acquired lock2
Thread 21 acquired lock2
```

In contrast, when `synchronized (MultipleLock.class)` is used, a single lock is shared across all instances of `MultipleLock` because `MultipleLock.class` refers to the class object itself. This means that if one thread acquires the lock using `method1` on one instance of `MultipleLock`, no other thread can acquire the lock using `method1` or `method2` on any instance of `MultipleLock` until the first thread releases the lock.

```CSS
// Only one of the threads can acquire lock1 and lock2 at the same time
// as they lock now on class level
Thread 20 acquired lock1
Thread 20 acquired lock1
Thread 20 acquired lock1
Thread 20 acquired lock2
Thread 20 acquired lock2
Thread 20 acquired lock2
Thread 21 acquired lock1
Thread 21 acquired lock1
Thread 21 acquired lock1
Thread 21 acquired lock2
Thread 21 acquired lock2
Thread 21 acquired lock2
```

### Deadlock

- Deadlock occurs when two or more threads are waiting on one another interminably. This kind of circumstance is known as infinite waiting.

- Deadlock cannot be resolved, however there are a number of preventative (or avoidance) strategies that can be used.

- We must exercise extra caution anytime we utilize synchronized keywords because they are the source of deadlock.

```java
class A {
    public synchronized void foo(B b) {
        System.out.println("Thread1 starts execution of foo() method");

        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
        }
        System.out.println("Thread1 trying to call b.last()");
        b.last();
    }

    public synchronized void last() {
        System.out.println("inside A, this is last()method");
    }
}

class B {
    public synchronized void bar(A a) {
        System.out.println("Thread2 starts execution of bar() method");

        try {
            Thread.sleep(2000);
        }

        catch (InterruptedException e) {
        }
        System.out.println("Thread2 trying to call a.last()");
        a.last();
    }

    public synchronized void last() {
        System.out.println("inside B, this is last() method");
    }
}

class thread1 extends Thread {
    A a;
    B b;

    thread1(A a, B b) {
        this.a = a;
        this.b = b;
    }

    public void run() {
        a.foo(b);
    }
}

class thread2 extends Thread {
    A a;
    B b;

    thread2(A a, B b) {
        this.a = a;
        this.b = b;
    }

    public void run() {
        b.bar(a);
    }
}

public class DeadlockExample {

    public static void main(String[] args) {
        A a = new A();
        B b = new B();

        thread1 t1 = new thread1(a, b);
        thread2 t2 = new thread2(a, b);

        t1.start();
        t2.start();
    }
}
```

```CSS
Thread1 starts execution of foo() method
Thread2 starts execution of bar() method
Thread2 trying to call a.last()
Thread1 trying to call b.last()
...
```

![[Pasted image 20240330025538.png]]

### Life Cycle of a thread
