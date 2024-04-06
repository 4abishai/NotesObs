### Java Event Model

1. **The class that defines an event listener must implement an event listener interface.**
In Java, event listeners are defined by interfaces. For example, `ActionListener` is an interface that must be implemented by any class that wants to handle `ActionEvent` events (e.g., button clicks). This allows the Java Virtual Machine to treat instances of different classes uniformly as long as they implement the same listener interface.
4. **A component that generates an event is called an event source.**
Event sources are typically UI components like buttons, menus, text fields, etc. When the user interacts with these components (e.g., clicking a button), the component generates an event.

3. **To respond to an event, an application must register an event listener object with the event source that generates the event.**
To receive and handle events, an application must *register an event listener object with the event source*. This is typically done by calling an `addXXXListener` method on the event source component, where `XXX` is the type of event (e.g., `addActionListener` for `ActionEvent`s).

4. **The class for the event source provides a method for registering event listeners.**
Event source classes, like `JButton`, `JTextField`, etc., provide methods like `addActionListener()`, `addKeyListener()`, etc., to allow applications to register event listener objects.

5. **Then, when the event occurs, the event source creates an event object and passes it to the event listener.**
When the user interaction triggers an event (e.g., button click), the event source creates an instance of the corresponding event class (e.g., `ActionEvent`) and calls the appropriate method on the registered listener object, passing the event object as a parameter. The listener object can then handle the event by implementing the corresponding method (e.g., `actionPerformed()` for `ActionListener`).

This event model allows for loose coupling between the event source and the event handler, making it easier to develop modular and extensible applications. The event source doesn't need to know the details of the event handling logic, and the event handler can be swapped out or extended without modifying the event source.

```java
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.JButton;
import javax.swing.JFrame;

public class ButtonClickExample {
    public static void main(String[] args) {
        // Create a JFrame and a JButton
        JFrame frame = new JFrame("Button Click Example");
        JButton button = new JButton("Click Me");

        // Create an instance of the listener class
        ButtonClickListener listener = new ButtonClickListener();

        // Register the listener with the button
        button.addActionListener(listener);

        // Add the button to the frame and set up the frame
        frame.getContentPane().add(button);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.pack();
        frame.setVisible(true);
    }

    // The listener class implements the ActionListener interface
    static class ButtonClickListener implements ActionListener {
        @Override
        public void actionPerformed(ActionEvent e) {
            // This method is called when the button is clicked
            System.out.println("Button clicked!");
        }
    }
}
```

**When the even occurs, the event source creates an event object and passes it to the even listener**

1. **The event source creates an event object**: When the JButton is clicked, it creates an `ActionEvent` object that encapsulates the details of the button click event.

2. **The event source passes it to the event listener**: The source (JButton instance) notifies the registered ActionListener instances about the event by calling their `actionPerformed` method and passing the `ActionEvent` object as a parameter.

In the provided code:

```java
// Create an instance of the listener class
ButtonClickListener listener = new ButtonClickListener();

// Register the listener with the button
button.addActionListener(listener);
```

Here, an instance of the `ButtonClickListener` class is created, and it is registered as an ActionListener with the JButton instance using the `addActionListener` method.

When the JButton is clicked, the Swing framework internally calls the `actionPerformed` method of all registered `ActionListener` instances, passing the `ActionEvent` object that represents the button click event.

```java
static class ButtonClickListener implements ActionListener {
    @Override
    public void actionPerformed(ActionEvent e) {
        // This method is called when the button is clicked
        System.out.println("Button clicked!");
    }
}
```

In this case, the `actionPerformed` method of the `ButtonClickListener` instance is called, and the `ActionEvent` object `e` is passed as a parameter. This is where the event listener receives the event object from the event source (JButton).

The `actionPerformed` method can then handle the event by performing the desired actions, such as printing "Button clicked!" to the console in this example.

This demonstrates the statement "when the event occurs, the event source creates an event object and passes it to the event listener."

### **Case 1**: Make your class implement the specific interfaces needed:


```java
import javax.swing.JFrame;
import javax.swing.JButton;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class ButtonPress extends JFrame implements
        ActionListener {
    public ButtonPress(String name) {
        super(name);
        JButton button = new JButton("Click Me");

        // Plug-in the button's event handler
        button.addActionListener(this); // imp

        add(button);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setSize(300, 300);
        setVisible(true);
    }

    // Must write this method now since ButtonPress
    // implements the ActionListener interface
    public void actionPerformed(ActionEvent e) {
        System.out.println("Button clicked!");
    }

    public static void main(String[] args) {
        new ButtonPress("Button Click Example");
    }
}
```

Advantages:
- Simple

Disadvantages:
- must write methods for ALL events in the interface.
- can get messy/confusing if your class has many components that trigger the same events or if your class handles many different types of events.

### **Case 2**: Create a separate class that implements the interface:

```java
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.JButton;
import javax.swing.JFrame;

public class ButtonClickExample {
    public static void main(String[] args) {
        // Create a JFrame and a JButton
        JFrame frame = new JFrame("Button Click Example");
        JButton button = new JButton("Click Me");

        // Create an instance of the listener class
        ButtonClickListener listener = new ButtonClickListener();

        // Register the listener with the button
        button.addActionListener(listener);

        // Add the button to the frame and set up the frame
        frame.getContentPane().add(button);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.pack();
        frame.setVisible(true);
    }

    // The listener class implements the ActionListener interface
    static class ButtonClickListener implements ActionListener {
        @Override
        public void actionPerformed(ActionEvent e) {
            // This method is called when the button is clicked
            System.out.println("Button clicked!");
        }
    }
}
```

Advantages:
- nice separation between your code and the event handlers.
- class can be reused by other classes.

Disadvantages:
- can end up with a lot of classes and class files.
- can be confusing as to which classes are just event handler classes.

### Anonymous Inner Class / Anonymous Method 
```java
import javax.swing.JFrame;
import javax.swing.JButton;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class ButtonPress2 extends JFrame implements
        ActionListener {
    public ButtonPress2(String name) {
        super(name);
        JButton button = new JButton("Click Me");

        // Plug-in the button's event handler
        button.addActionListener(new ActionListener() { // imp
            public void actionPerformed(ActionEvent e) {
                System.out.println("Button clicked!");
            }
        }); 

        add(button);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setSize(300, 300);
        setVisible(true);
    }

    public static void main(String[] args) {
        new ButtonPress2("Button Click Example");
    }
}
```

Using an anonymous inner class is often preferred because it separates the event handling logic from the main class. This can make the code more modular and easier to maintain, especially if there are multiple event handlers or if the event handling logic becomes more complex.
### Handling Multiple Events

- We will make use of the `getActionCommand()` method for the `ActionEvent` class that allows us to determine the label on the button that generated the event.
- Import the `FlowLayout` class from the `java.awt` package.

```java
import java.awt.FlowLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.JFrame;
import javax.swing.JButton;

public class Handle2Buttons extends JFrame implements ActionListener {
    public Handle2Buttons(String title) {
        super(title);
        JButton btn1 = new JButton("Press Me");
        JButton btn2 = new JButton("Don't Press Me");
        add(btn1);
        add(btn2);

        // Indicate that this class will handle 2 button clicks
        // and that both buttons will go to the SAME event handler
        btn1.addActionListener(this);
        btn2.addActionListener(this);

        setLayout(new FlowLayout());
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setSize(300, 300);
    }

    // This is the event handler for the buttons
    public void actionPerformed(ActionEvent e) {
        // Ask the event which button was the source that generated it
        if (e.getActionCommand().equals("Press Me")) // imp
            System.out.println("That felt good!");
        else
            System.out.println("Ouch! Stop that!");
    }

    public static void main(String args[]) {
        Handle2Buttons frame = new Handle2Buttons("Handling 2 Button Presses");
        frame.setVisible(true);
    }
}
```

### JTextFields
- When creating `JTextFields`, we can specify the initial content to be displayed (a string) as well as the maximum number of characters allowed to be entered in them (8, 16 and 20 in this example).
- We need to convert to and from Strings when accessing/modifying text field data
- We access/modify a text field's contents using `getText()` and `setText()`.

```java
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;

public class TextBoxMultiBtn extends JFrame {
    JTextField valueField, squareField, rootField;

    TextBoxMultiBtn(String title) {
        super(title);
        setLayout(new BoxLayout(this.getContentPane(), BoxLayout.Y_AXIS));
        
        add(new JLabel("Value:"));
        valueField = new JTextField("10", 8);
        add(valueField);
        
        JButton aButton = new JButton("Compute");
        add(aButton);
        
        add(new JLabel("Square:"));
        squareField = new JTextField("0", 16);
        add(squareField);

        add(new JLabel("Square Root:"));
        rootField = new JTextField("0", 20);
        add(rootField);

        // Handle the button click
        aButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                int value = Integer.parseInt(valueField.getText());
                squareField.setText("" + value * value);
                rootField.setText("" + Math.sqrt(value));
            }
        });

        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setSize(350, 300);
        setVisible(true);
    }

    public static void main(String args[]) {
        new TextBoxMultiBtn("Working With TextFields");
    }
}
```

- The constructor `TextBoxMultiBtn(String title)` sets up an `ActionListener` for the "Compute" button, which listens for click events. When the button is clicked, it retrieves the value from `valueField`, calculates the square and square root, and updates the `squareField` and `rootField` text fields with the results.

#### SwingUtilities.invokeLater() 

The `SwingUtilities.invokeLater()` method is used in Java Swing programming to ensure that any code that interacts with or modifies Swing GUI components is executed on the Event Dispatch Thread (EDT). This is important because Swing components are not thread-safe, and modifying them from outside the EDT can lead to race conditions, deadlocks, and other threading issues.

```java
SwingUtilities.invokeLater(new Runnable() {
	@Override
	public void run() {
		new TextBoxMultiBtn("Working With TextFields");
	}
});
```

```java
// same as
SwingUtilities.invokeLater(() -> {
	new TextBoxMultiBtn("Working With TextFields");
});
```

In this case, the `Runnable` is represented by a lambda expression `() -> { ... }`.

```java
//same as
SwingUtilities.invokeLater(() -> 
						   new TextBoxMultiBtn("Working With TextFields"));
```

As the body consists of a single expression, the curly braces is omitted.
#### Lambda Expression (Java 8)

Lambda expressions are a concise way of representing an anonymous function or method in Java. They are often used as the argument for a method that expects a **functional interface** (*an interface with a single abstract method*), such as event listeners or functional operations in the Java Stream API.

Used as the argument for a method that expects an interface with a single abstract method.

The general syntax for a lambda expression is:

```java
(parameters) -> { body }
```

Here's a breakdown of the syntax:

1. `(parameters)`: This part defines the parameters of the lambda expression. If there is a single parameter, the parentheses can be omitted. If there are no parameters, an empty set of parentheses `()` is required.
2. `->`: This is the lambda arrow operator, which separates the parameters from the body of the lambda expression.
3. `{ body }`: This part contains the code or statements that make up the body of the lambda expression. If the body consists of a single expression, the curly braces can be omitted, and the expression will be the result of the lambda.

##### Example

```java
aButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                int value = Integer.parseInt(valueField.getText());
                squareField.setText("" + value * value);
                rootField.setText("" + Math.sqrt(value));
            }
        });
```

```java
// same as
aButton.addActionListener((ActionEvent e) -> {
            int value = Integer.parseInt(valueField.getText());
            squareField.setText("" + value * value);
            rootField.setText("" + Math.sqrt(value));
        });
```

- `ActionListener` is a functional interface.
- `MouseListener` is not a functional interface in Java. Therefore, they cannot be replaced with lambda expression. You would typically use an anonymous inner class to implement the `MouseListener` interface when working with GUI components in Java.