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