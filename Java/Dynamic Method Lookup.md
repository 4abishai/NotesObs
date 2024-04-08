- Dynamic method lookup is the process of determining the actual method implementation to be called at runtime based on the type of object.
- It involves looking up the method implementation in the method table (vtable) associated with the object's class.
- Dynamic method lookup is closely related to dynamic method dispatch, as it is the *mechanism used to determine which overridden method implementation to call*. [[Runtime polymorphism (Dynamic method dispatch)]]

```java
interface IDrawable {
    void draw();
}

class Shape implements IDrawable {
    public void draw()
        System.out.println("Drawing a Shape.");
}

class Circle extends Shape {
    public void draw()
        System.out.println("Drawing a Circle.");
}

class Rectangle extends Shape {
    public void draw()
	    System.out.println("Drawing a Rectangle.");
}

class Square extends Rectangle {
    public void draw()
        System.out.println("Drawing a Square.");
}

class Map implements IDrawable {
	public void draw()
		System.out.println("Drawing a Map.");
}

public class PolymorphRefs {
	public static void main(String[] args) {
		Shape[] shapes = {new Circle(), new Rectangle(), new
		Square()}; 
		IDrawable[] drawables = {new Shape(), new Rectangle(),
		new Map()};
		
		System.out.println("Draw shapes:");
		
		for (int i = 0; i < shapes.length; i++) 
			shapes[i].draw();
		System.out.println("Draw drawables:");
		
		for (int i = 0; i < drawables.length; i++) 
			drawables[i].draw();
	}
}
```

In both cases, the actual method implementation that is called (`Circle.draw`, `Rectangle.draw`, `Square.draw`, `Shape.draw`, `Rectangle.draw`, `Map.draw`) is determined by the type of object at runtime, demonstrating dynamic method dispatch. Dynamic method lookup is used to find the correct method implementation based on the type of the object.