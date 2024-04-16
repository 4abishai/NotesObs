###  Delegation Event Model

The delegation event model in Java is a design pattern used for handling events in a structured and organized way. It is a way to decouple the event source (the object that generates the event) from the event listener (the object that handles the event).

In the delegation event model, the event source has a list of registered event listeners. When an event occurs, the event source notifies all the registered event listeners by calling their respective event handling methods.

**Delegation:** The term "delegation" in the Delegation Event Model refers to the fact that the responsibility for handling events is delegated from the event source to the listener objects. This allows for a clean separation of concerns and promotes modular and reusable code.

Here's how the delegation event model works in Java:

1. **Event Source**: This is the object that generates the event. It has the following responsibilities:
   - Defines event types as classes that extend the `java.util.EventObject` class.
   - Maintains a list of event listeners that are interested in receiving the events.
   - Provides methods to register and unregister event listeners.
   - Triggers the events by calling the appropriate event handling methods on the registered listeners.

2. **Event Listener**: This is the object that handles the event. It has the following responsibilities:
   - Implements the appropriate event handling interface, which defines the event handling methods.
   - Registers itself with the event source to receive the events of interest.
   - Defines the implementation of the event handling methods.

3. **Event Handling Interface**: This is the interface that defines the event handling methods. It is implemented by the event listener, and it is usually named with the suffix "Listener" (e.g., `ActionListener`, `MouseListener`, `WindowListener`, etc.).

The advantage of the delegation event model is that it promotes a loose coupling between the event source and the event listener. The event source does not need to know the specific implementation of the event listener, and the event listener can be easily added or removed without affecting the event source.

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

public class ButtonPress2 extends JFrame {
    public ButtonPress2(String name) {
        super(name);
        JButton button = new JButton("Click Me");

        // Plug-in the button's event handler
        button.addActionListener(new ActionListener() { // imp
            public void actionPerformed(ActionEvent e) {
                System.out.println("Button clicked!");
            }
        }); 

        // this.add(button);
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
        setLayout(new BoxLayout(getContentPane(), BoxLayout.Y_AXIS));
        
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
                // squareField.setText(String.valueOf(value * value));
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

### JList
```java
import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.JList;
import javax.swing.JButton;
import javax.swing.SwingUtilities;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.Color;

public class E extends JFrame {

    public E(String title) {
        super(title);
        setSize(300, 200);
        setDefaultCloseOperation(EXIT_ON_CLOSE);

        String[] colorNames = { "White", "Orange", "Red", "Blue" };
        Color[] colors = { Color.WHITE, Color.ORANGE, Color.RED, Color.BLUE };

        JPanel panel = new JPanel();
        JList<String> colorList = new JList<>(colorNames);
        JButton changeColorButton = new JButton("Click");

        changeColorButton.addActionListener((ActionEvent e) -> {
            int selectedIndex = colorList.getSelectedIndex();
            if (selectedIndex >= 0 && selectedIndex < colors.length) {
                panel.setBackground(colors[selectedIndex]);
            }
        });

        panel.add(colorList);
        panel.add(changeColorButton);
        add(panel);

        setVisible(true);
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> new E("My frame"));
    }
}
```

![[Pasted image 20240409154352.png]]

### JComboBox

```java

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class F extends JFrame {

    public F(String title) {
        super(title);
        setSize(600, 300);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        JComboBox<Integer> redComboBox = new JComboBox<>();
        JComboBox<Integer> greenComboBox = new JComboBox<>();
        JComboBox<Integer> blueComboBox = new JComboBox<>();

        for (int i = 0; i <= 255; i++) {
            redComboBox.addItem(i);
            greenComboBox.addItem(i);
            blueComboBox.addItem(i);
        }

        JPanel panel = new JPanel();
        panel.setLayout(new FlowLayout());

        JButton btn = new JButton("Show Output");
        btn.addActionListener((ActionEvent e) -> {
            int red = (int) redComboBox.getSelectedItem();
            int green = (int) greenComboBox.getSelectedItem();
            int blue = (int) blueComboBox.getSelectedItem();

            Color color = new Color(red, green, blue);
            panel.setBackground(color);
        });

        // Create JLabel objects
        JLabel redL = new JLabel("Red:");
        redL.setForeground(Color.RED);
        JLabel greenL = new JLabel("Green:");
        greenL.setForeground(Color.GREEN);
        JLabel blueL = new JLabel("Blue:");
        blueL.setForeground(Color.BLUE);

        panel.add(redL);
        panel.add(redComboBox);
        panel.add(greenL);
        panel.add(greenComboBox);
        panel.add(blueL);
        panel.add(blueComboBox);
        panel.add(btn);
        add(panel);
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            F app = new F("My Frame");
            app.setVisible(true);
        });
    }
}
```

![[Pasted image 20240409154512.png]]
### Event Dispatch Thread()
The Event Dispatch Thread (EDT) is a special thread in Swing applications that is responsible for handling events. Swing components need to run on the EDT because Swing is not thread-safe, meaning that Swing components should only be accessed, modified, or queried from the EDT *to avoid potential concurrency issues.*

By ensuring that all interactions with Swing components occur on the EDT, Swing applications can avoid problems such as *race conditions, deadlocks, and inconsistent GUI behavior*. The *EDT processes events sequentially*, which helps maintain the order of events and ensures that updates to the GUI are synchronized and predictable
#### SwingUtilities.invokeLater() 

The `SwingUtilities.invokeLater()` method is used in Java Swing programming to ensure that any code that interacts with or modifies Swing GUI components is executed on the Event Dispatch Thread (EDT). This is important because Swing components are not thread-safe, and modifying them from outside the EDT can lead to race conditions, deadlocks, and other threading issues. 
Must be imported: `import javax.swing.SwingUtilities;`

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

In this case, the `Runnable` is represented by a [[Lambda Expression (Java 8)]] `() -> { ... }`.

```java
//same as
SwingUtilities.invokeLater(() -> 
						   new TextBoxMultiBtn("Working With TextFields"));
```

As the body consists of a single expression, the curly braces is omitted.