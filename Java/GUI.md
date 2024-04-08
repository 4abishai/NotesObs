## REFER TO SLIDES
![[Pasted image 20240409003344.png]]
### Swing/AWT Inheritance Hierarchy

![[Pasted image 20240406141234.png]]
![[Pasted image 20240406141248.png]]

- JComponent inherits the AWT classes Container and Component. Thus, a Swing component is built on and compatible with an AWT component.

- All of Swing components are represented by classes defined within the package javax.swing.

```java
import javax.swing.*;
```

#### Container: Top-level
- JFrame, JApplet, JWindow, and JDialog.
- These containers do not inherit JComponent.
- They do, however, inherit the AWT classes Component and Container.
- Unlike Swing’s other components, which are lightweight, the top-level containers are heavyweight.
- A top-level container must be at the top of a containment hierarchy. *A top-level container is not contained within any other container.*
- Every containment hierarchy must begin with a top-level container.
- The one most commonly used for applications is JFrame. The one used for applets is JApplet.
- Each top-level container has a content pane.
- You can optionally add a menu bar to a top-level container.
![[Pasted image 20240406142319.png]]

#### Container: Lightweight or intermediate or Mid-level **(Panels/Panes)**
- Lightweight containers do inherit Jcomponent.
- Lightweight containers are often used to organize and manage groups of related components because a lightweight container can be contained within another container.
- Simplify the positioning of other components:
	- JPanel
- Play a visible and interactive role:
	- JScrollPane
	- JTabbedPane

### Containment Hierarchy
![[Pasted image 20240406142155.png]]

### JRootPane
- Each top-level container defines a set of panes. At the top of the hierarchy is an instance of JRootPane.
- JRootPane is a lightweight container whose purpose is to manage the other panes. It also helps manage the optional menu bar.

1. The **Content Pane** is managed by the JRootPane and is where the main content of the application is displayed.

2. The **Layered Pane** directly contains the menu bar and content pane.

3. The **Glass Pane** is a lightweight component that can be used to intercept input events occurring over the top-level container, and it can also be used to paint over multiple components.

The hierarchy of these components within a JRootPane is as follows:

```
JRootPane
└── Layered Pane
    ├── Menu Bar
    └── Content Pane
└── Glass Pane (optional)
```

The Glass Pane is an optional component that sits on top of the Layered Pane, allowing it to intercept events and paint over the entire application area.

This structure separates the concerns of the different components and allows for better management and customization of the application's user interface.

### JFrame

```java
import javax.swing.*;

class ShowFrame {
    public static void main(String args[]) {
    
        JFrame frame = new JFrame("This is JFrame");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        JLabel label = new JLabel("This is label");
        frame.getContentPane().add(label);
        frame.pack();
        frame.setVisible(true);
    }
}
```

```java
import javax.swing.*;
```
This line imports the necessary classes from the `javax.swing` package, which is part of the Java Swing library for building GUI applications.

```java
JFrame frame = new JFrame("ShowJFrame");
```
This line creates a new instance of `JFrame` (a window) with the title "ShowJFrame".

```java
frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
```
This line sets the default close operation for the `JFrame` to `EXIT_ON_CLOSE`, which means that the *program will terminate when the user closes the window*.
This ensures that the application exits cleanly when the user closes the window, freeing up system resources and preventing the program from running indefinitely in the background.

```java
JLabel label = new JLabel("This is a label ");
```
This line creates a new instance of `JLabel` with the text "This is a label".

```java
frame.getContentPane().add(label);
```
This line adds the `JLabel` to the content pane of the `JFrame`.

```java
frame.pack();
```
This line resizes the `JFrame` to fit the preferred size of its components (in this case, the `JLabel`).

```java
frame.setVisible(true);
```
This line makes the `JFrame` visible on the screen.

When you run this program, it will display a window with the title "ShowJFrame" and a label inside it that says "This is a label". When you close the window, the program will terminate.

### JPanel

```java
import javax.swing.*;

class ShowJPanel {
    public static void main(String args[]) {
        JFrame frame = new JFrame("ShowJPanel");
        frame.setDefaultCloseOperation(frame.EXIT_ON_CLOSE);
        
        JPanel panel = new JPanel(); 
        JLabel label = new JLabel("This is a label with some text in it.");
        JButton button = new JButton("Click Me");
        panel.add(label);
        panel.add(button);
        
        frame.getContentPane().add(panel);
        frame.pack();
        frame.setVisible(true);
    }
}
```

- A new `JPanel` object is created, which will hold the components that will be displayed in the frame.
- The `JPanel` is added to the content pane of the `JFrame` using the `getContentPane().add(panel)` method.

### JScrollPane

```java
import javax.swing.*;

class ShowJScrollPane {
    public static void main(String args[]) {
        JFrame frame = new JFrame("ShowScrollPane");
        frame.setDefaultCloseOperation(frame.EXIT_ON_CLOSE);

        JPanel panel = new JPanel();
        JLabel label = new JLabel("Label with text that can be scrolled");
        JButton button = new JButton("Click Me");
        panel.add(label);
        panel.add(button);

        JScrollPane scrollpane = new JScrollPane(panel); // important

        frame.getContentPane().add(scrollpane);
        frame.pack();
        frame.setSize(300, 100);
        frame.setVisible(true);
    }
}
```

### Layout Management & Managers

Layout managers in Java are used to determine the size and position of components within a container (such as a JPanel or JFrame). They help ensure that the components are arranged and resized properly when the window is resized or components are added or removed.

1. **FlowLayout**: This is the default layout manager for JPanel. It arranges components in a left-to-right, top-to-bottom flow, much like words in a paragraph. When there is no more horizontal space, it wraps to the next line.

2. **BorderLayout**: This is the default layout manager for JFrame. It divides the container into five regions: North, South, East, West, and Center. Only one component can be added to each region, and the Center region expands to fill any remaining space.

Example:
```java
JFrame frame = new JFrame();
JButton button = new JButton("Click me");
frame.add(button, BorderLayout.CENTER); // Add button to the center region
```

3. **GridLayout**: This layout manager arranges components in a regular two-dimensional grid. All components are given equal size, determined by the largest component in the grid.

4. **Other Layout Managers**:
   - **BoxLayout**: Arranges components in a single row or column.
   - **CardLayout**: Allows for switching between multiple visible components, like a deck of cards.
   - **GridBagLayout**: A flexible and complex layout manager that allows precise positioning and resizing of components.

Every content pane (the primary container for components in a JFrame) is initialized to use a BorderLayout by default. However, you can change the layout manager for any container using the `setLayout()` method.

Example:
```java
JPanel panel = new JPanel();
panel.setLayout(new GridLayout(2, 3)); // Set a 2x3 GridLayout for the panel
```

**REFER TO JAVA REPO FOR PROGRAMS**

**Note:** **getContentPane( ) vs setContentPane( )**

1. **`getContentPane()`**: This method is used to retrieve the content pane of a `JFrame`. The content pane is a `Container` where you typically add all the components that you want to display in the frame. `getContentPane()` returns a reference to this `Container`, allowing you to add, remove, or manipulate components within it.

   ```java
   JFrame frame = new JFrame();
   JPanel panel = new JPanel();
   // Add components to panel
   frame.setContentPane(panel);
   ```

   ```java
   Container contentPane = frame.getContentPane();
   contentPane.add(new JButton("Click Me"));

   // same as
   frame.getContentPane().add(new JButton("Click Me"));
   ```

2. **`setContentPane(Container contentPane)`**: This method is used to set the content pane of a `JFrame` to the specified `Container`. It replaces the current content pane of the frame with the specified one. This is useful if you want to change the content pane of a frame dynamically.


   ```java
Container contentPane = frame.getContentPane();
// Add or remove components from contentPane
   ```

   ```java
   JPanel panel = new JPanel();
   panel.add(new JLabel("Hello, World!"));
   frame.setContentPane(panel);
   ```

In summary, `getContentPane()` is used to retrieve the current content pane of a `JFrame`, while `setContentPane()` is used to set a new content pane for the frame.

**Note:**
```java
Container contentPane = frame.getContentPane();
contentPane.setLayout(new BorderLayout());
contentPane.add(label, BorderLayout.CENTER);

// same as
Container contentPane = frame.getContentPane();
contentPane.add(label, BorderLayout.CENTER);//BorderLayout's default frame layout

//same as
frame.getContentPane().add(label, BorderLayout.CENTER);
```

```java
JPanel contentPane = new JPanel(new BorderLayout());
contentPane.add(label, BorderLayout.CENTER);
frame.setContentPane(contentPane);
```

**Note:**
`add()` method typically takes two parameters:

1. **Component component**: This parameter specifies the component to be added to the container. It can be any subclass of the `Component` class, such as `JButton`, `JLabel`, `JTextField`, etc.
2. **Object constraints (optional)**: This parameter is used to specify layout constraints when adding the component to a container that uses a layout manager. Layout managers are used to arrange components within a container. Different layout managers may require different types of constraints to determine the component's position and size within the container. If the container does not use a layout manager (e.g., it uses absolute positioning), this parameter is typically `null`.
### Working with componets and containers

The Swing way of working with components and containers can be summarized as follows:

1. **Create Components**: Components are the basic building blocks of a Swing GUI, such as buttons, labels, text fields, etc. These components are instances of classes like `JButton`, `JLabel`, `JTextField`, etc.

2. **Create a Container**: A container is an object that holds and organizes other components. The most commonly used container is `JPanel`, which can be used to group related components together.

3. **Add Components to the Container**: Components are added to the container using methods like `container.add(component)`. The layout manager associated with the container determines how the components are arranged within the container.

4. **Create a Top-Level Container (Frame or Window)**: A top-level container, such as `JFrame` or `JDialog`, is required to hold the main container(s) and provide a window for the GUI.

5. **Add the Main Container to the Top-Level Container**: The main container (e.g., `JPanel`) is added to the top-level container using methods like `frame.getContentPane().add(mainContainer)`.

6. **Set the Top-Level Container Properties**: Properties like the window's title, size, and visibility are set using methods like `frame.setTitle("My Window")`, `frame.setSize(width, height)`, and `frame.setVisible(true)`.

7. **Handle Events**: Event handlers are registered with components to respond to user interactions, such as button clicks or text field changes.

8. **Layout Management**: Swing provides various layout managers (`FlowLayout`, `BorderLayout`, `GridLayout`, etc.) to control the arrangement of components within a container.

9. **Optional: Create Menus and Toolbars**: Additional components like menus and toolbars can be added to the top-level container or other containers to provide more functionality.

10. **Display the GUI**: After setting up the components, containers, and event handlers, the top-level container is made visible, displaying the assembled GUI.

The key concept is to create components, organize them in containers using layout managers, add the main container(s) to a top-level container (window or frame), and handle user interactions through event handlers. This approach allows for a modular and flexible construction of graphical user interfaces in Swing.

```java
import javax.swing.*;
import java.awt.*;

public class SwingProgram {
    public static void main(String[] args) {
        SwingUtilities.invokeLater(SwingProgram::helloWorld); // imp
    }

    static void helloWorld() {
        JFrame frame = new JFrame("A message to the World");
        // frame.setDefaultCloseOperation(WindowConstants.DISPOSE_ON_CLOSE);
        frame.setDefaultCloseOperation(frame.EXIT_ON_CLOSE);

        JLabel label = new JLabel("Hello, World !");
        label.setHorizontalAlignment(SwingConstants.CENTER); // imp
        frame.getContentPane().add(label, BorderLayout.CENTER);

        JButton button = new JButton("Close");
        button.addActionListener(x -> frame.dispose()); // imp
        frame.getContentPane().add(closeButton, BorderLayout.SOUTH);

        frame.setSize(400, 200); // imp
        frame.setVisible(true);
    }
}
```

- The `helloWorld` method is called via the `SwingUtilities.invokeLater` method. It is used to schedule a task for execution on the Event Dispatch Thread (EDT), which is the thread responsible for handling all Swing GUI events and updates. The Event Dispatch Thread is a single-threaded environment that ensures thread safety and prevents race conditions when modifying Swing components. All modifications to Swing components must be performed on the EDT to ensure proper behavior and avoid potential threading issues.
- `label.setHorizontalAlignment(SwingConstants.CENTER);`: Sets the horizontal alignment of the label's text to center.
- `frame.getContentPane().add(label, BorderLayout.CENTER);`: Adds the label to the center of the JFrame's content pane.
- `closeButton.addActionListener(x -> frame.dispose());`: Adds an action listener to the close button that disposes of the frame when clicked.
- `frame.setSize(400, 200);`: Sets the size of the JFrame to 400 pixels wide and 200 pixels tall.
- If you want the application to exit when the main window is closed, using `EXIT_ON_CLOSE` is appropriate. If you want only the window to close but keep the application running, `DISPOSE_ON_CLOSE` should be used i.e, JFrame will be disposed of (closed) when the user closes it. However, the application itself will continue to run, and any other open windows or components will remain unaffected.

### GUI with inheriting JFrame

```java
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.SwingConstants;

// import javax.swing.*;

class ShowJFrame {
    public static void main(String args[]) {

        JFrame frame = new JFrame("This is JFrame");
        JLabel label = new JLabel("This is label");

        label.setHorizontalAlignment(SwingConstants.CENTER);

        frame.getContentPane().add(label);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(300, 300);
        frame.setVisible(true);
    }
}
```

```java
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.SwingConstants;

public class ExtendsJFrame extends JFrame {
    ExtendsJFrame(String FrameName) {
    
        super(FrameName);
        JLabel label = new JLabel("This is label");

        label.setHorizontalAlignment(SwingConstants.CENTER);

        add(label);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setSize(300, 300);
        setVisible(true);
    }

    public static void main(String[] args) {
        new ExtendsJFrame("DemoExtendsJFrame");
    }
}
```

1. **Code Organization**: The first approach separates the creation of the JFrame and JLabel into distinct objects, which can make the code more modular and easier to maintain in larger applications. The second approach combines the frame creation and component setup into a single class, which may be more concise for simple GUIs.
2. **Inheritance vs. Composition**: The second approach uses inheritance to create a custom JFrame subclass. While this can be useful in some cases, it can also lead to tighter coupling and make the code more difficult to extend or modify. The first approach follows the composition principle, which is generally preferred in object-oriented programming.
3. **Flexibility**: The first approach allows for more flexibility in creating and managing multiple JFrame instances, as each instance is a separate object. In the second approach, each instance of `ExtendsJFrame` creates a new JFrame, which may not be desirable in all cases.
### AWT vs Swing

| Feature             | Abstract Window Toolkit (AWT)                                      | Swing                                                                   |
| ------------------- | ------------------------------------------------------------------ | ----------------------------------------------------------------------- |
| **Architecture**    | Lightweight, uses native OS components                             | Heavyweight, implemented in pure Java                                   |
| **Look and Feel**   | Native, matches the underlying OS                                  | Consistent look and feel across platforms, uses pluggable look and feel |
| **Functionality**   | Basic set of GUI components                                        | Richer set of GUI components, including advanced UI elements            |
| **Event Handling**  | Listener-based event model                                         | Flexible and extensible listener-based event model                      |
| **Threading Model** | Not thread-safe, should be accessed from the event dispatch thread | Thread-safe, but recommended to access from the event dispatch thread   |
| **Customization**   | Limited customization options                                      | Highly customizable                                                     |
| **Performance**     | Faster and more efficient due to native components                 | Can be slower due to the overhead of the Java implementation            |
| **Portability**     | More platform-dependent                                            | More portable across different operating systems                        |
