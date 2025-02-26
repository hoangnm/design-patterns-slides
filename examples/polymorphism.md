# Polymorphism in Object-Oriented Programming

Polymorphism is one of the four fundamental principles of object-oriented programming (OOP). The word "polymorphism" means "many forms," and in programming, it refers to the ability of objects of different classes to respond to the same method call, each in its own way.

## Key Concepts of Polymorphism

1. **Method Overriding**: Subclasses can provide specific implementations of methods defined in their parent classes
2. **Method Overloading**: Multiple methods with the same name but different parameters
3. **Interface Implementation**: Different classes implementing the same interface
4. **Dynamic Binding**: The method call is resolved at runtime based on the actual object type

## Example in Java

```java
// Shape hierarchy demonstrating polymorphism
// Base class
class Shape {
    // Method to be overridden by subclasses
    public double calculateArea() {
        return 0;
    }
    
    public void display() {
        System.out.println("This is a shape with area: " + calculateArea());
    }
}

// Circle subclass
class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    // Method overriding
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    public void display() {
        System.out.println("This is a circle with radius: " + radius + 
                           " and area: " + calculateArea());
    }
}

// Rectangle subclass
class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    // Method overriding
    @Override
    public double calculateArea() {
        return width * height;
    }
    
    @Override
    public void display() {
        System.out.println("This is a rectangle with width: " + width + 
                           ", height: " + height + 
                           " and area: " + calculateArea());
    }
}

// Triangle subclass
class Triangle extends Shape {
    private double base;
    private double height;
    
    public Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }
    
    // Method overriding
    @Override
    public double calculateArea() {
        return 0.5 * base * height;
    }
    
    @Override
    public void display() {
        System.out.println("This is a triangle with base: " + base + 
                           ", height: " + height + 
                           " and area: " + calculateArea());
    }
}

// Example of method overloading
class Calculator {
    // Method overloading - same method name, different parameters
    public int add(int a, int b) {
        return a + b;
    }
    
    public double add(double a, double b) {
        return a + b;
    }
    
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    public String add(String a, String b) {
        return a + b; // String concatenation
    }
}

// Client code
public class Main {
    public static void main(String[] args) {
        // Polymorphism through inheritance
        System.out.println("Demonstrating polymorphism through inheritance:");
        
        // Array of Shape references, but actual objects are of subclasses
        Shape[] shapes = new Shape[3];
        shapes[0] = new Circle(5);
        shapes[1] = new Rectangle(4, 6);
        shapes[2] = new Triangle(3, 8);
        
        // Polymorphic method calls - same method call, different behaviors
        for (Shape shape : shapes) {
            // The correct version of calculateArea() is called based on the actual object type
            System.out.println("Area: " + shape.calculateArea());
            // The correct version of display() is called based on the actual object type
            shape.display();
            System.out.println();
        }
        
        // Polymorphism through method overloading
        System.out.println("Demonstrating polymorphism through method overloading:");
        Calculator calc = new Calculator();
        
        System.out.println("Adding two integers: " + calc.add(5, 7));
        System.out.println("Adding two doubles: " + calc.add(3.5, 2.7));
        System.out.println("Adding three integers: " + calc.add(1, 2, 3));
        System.out.println("Adding two strings: " + calc.add("Hello, ", "World!"));
    }
}
```

## Benefits of Polymorphism

1. **Code Reusability**: Write code that can work with objects of multiple types
2. **Flexibility**: Add new classes without changing existing code
3. **Extensibility**: Extend functionality through inheritance and interfaces
4. **Simplicity**: Simplify code by treating different objects uniformly
5. **Maintainability**: Changes to one class don't affect others
