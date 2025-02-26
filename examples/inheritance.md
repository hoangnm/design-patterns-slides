# Inheritance in Object-Oriented Programming

Inheritance is one of the four fundamental principles of object-oriented programming (OOP). It allows a class (subclass/derived class) to inherit properties and behaviors from another class (superclass/base class), promoting code reuse and establishing a hierarchical relationship between classes.

## Key Concepts of Inheritance

1. **Code Reuse**: Subclasses inherit fields and methods from superclasses
2. **Specialization**: Subclasses can add new functionality or modify inherited behavior
3. **IS-A Relationship**: Inheritance establishes an "is-a" relationship between classes
4. **Hierarchical Structure**: Classes can form inheritance trees with multiple levels

## Example in Java

```java
// Base class (Superclass)
class Animal {
    // Common properties for all animals
    protected String name;
    protected int age;
    protected double weight;
    
    // Constructor
    public Animal(String name, int age, double weight) {
        this.name = name;
        this.age = age;
        this.weight = weight;
    }
    
    // Common methods for all animals
    public void eat() {
        System.out.println(name + " is eating.");
    }
    
    public void sleep() {
        System.out.println(name + " is sleeping.");
    }
    
    public void makeSound() {
        System.out.println(name + " makes a generic animal sound.");
    }
    
    // Getter methods
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
    
    public double getWeight() {
        return weight;
    }
    
    // Display information
    public void displayInfo() {
        System.out.println("Animal: " + name);
        System.out.println("Age: " + age + " years");
        System.out.println("Weight: " + weight + " kg");
    }
}

// Derived class (Subclass) - inherits from Animal
class Dog extends Animal {
    // Additional properties specific to dogs
    private String breed;
    private boolean isTrained;
    
    // Constructor
    public Dog(String name, int age, double weight, String breed, boolean isTrained) {
        // Call the superclass constructor using super()
        super(name, age, weight);
        
        // Initialize dog-specific properties
        this.breed = breed;
        this.isTrained = isTrained;
    }
    
    // Override the makeSound method (method overriding)
    @Override
    public void makeSound() {
        System.out.println(name + " barks: Woof! Woof!");
    }
    
    // Additional methods specific to dogs
    public void fetch() {
        System.out.println(name + " is fetching the ball.");
    }
    
    public void wagTail() {
        System.out.println(name + " is wagging its tail.");
    }
    
    // Getter for dog-specific property
    public String getBreed() {
        return breed;
    }
    
    // Override displayInfo to include dog-specific information
    @Override
    public void displayInfo() {
        super.displayInfo(); // Call the parent method
        System.out.println("Breed: " + breed);
        System.out.println("Trained: " + (isTrained ? "Yes" : "No"));
    }
}

// Another derived class
class Cat extends Animal {
    // Additional properties specific to cats
    private boolean isIndoor;
    private String furColor;
    
    // Constructor
    public Cat(String name, int age, double weight, boolean isIndoor, String furColor) {
        super(name, age, weight);
        this.isIndoor = isIndoor;
        this.furColor = furColor;
    }
    
    // Override the makeSound method
    @Override
    public void makeSound() {
        System.out.println(name + " meows: Meow! Meow!");
    }
    
    // Additional methods specific to cats
    public void purr() {
        System.out.println(name + " is purring.");
    }
    
    public void climbTree() {
        System.out.println(name + " is climbing a tree.");
    }
    
    // Override displayInfo to include cat-specific information
    @Override
    public void displayInfo() {
        super.displayInfo();
        System.out.println("Indoor Cat: " + (isIndoor ? "Yes" : "No"));
        System.out.println("Fur Color: " + furColor);
    }
}

// Client code
public class Main {
    public static void main(String[] args) {
        // Create instances of different classes
        Animal genericAnimal = new Animal("Generic Animal", 5, 15.0);
        Dog dog = new Dog("Buddy", 3, 20.5, "Golden Retriever", true);
        Cat cat = new Cat("Whiskers", 2, 4.2, true, "Tabby");
        
        // Demonstrate inheritance
        System.out.println("=== Generic Animal ===");
        genericAnimal.displayInfo();
        genericAnimal.eat();
        genericAnimal.sleep();
        genericAnimal.makeSound();
        
        System.out.println("\n=== Dog ===");
        dog.displayInfo();
        dog.eat();  // Inherited method
        dog.sleep(); // Inherited method
        dog.makeSound(); // Overridden method
        dog.fetch(); // Dog-specific method
        dog.wagTail(); // Dog-specific method
        
        System.out.println("\n=== Cat ===");
        cat.displayInfo();
        cat.eat();  // Inherited method
        cat.sleep(); // Inherited method
        cat.makeSound(); // Overridden method
        cat.purr(); // Cat-specific method
        cat.climbTree(); // Cat-specific method
        
        // Demonstrate polymorphism with inheritance
        System.out.println("\n=== Polymorphism ===");
        Animal[] animals = new Animal[3];
        animals[0] = genericAnimal;
        animals[1] = dog;  // Dog IS-A Animal
        animals[2] = cat;  // Cat IS-A Animal
        
        for (Animal animal : animals) {
            System.out.println("\nInfo for " + animal.getName() + ":");
            animal.displayInfo();
            animal.makeSound();
        }
    }
}
```

## Types of Inheritance

1. **Single Inheritance**: A subclass inherits from one superclass (as shown in the example)
2. **Multiple Inheritance**: A subclass inherits from multiple superclasses (not supported directly in Java, but possible through interfaces)
3. **Multilevel Inheritance**: A class inherits from a subclass, forming a chain (e.g., Animal → Mammal → Dog)
4. **Hierarchical Inheritance**: Multiple classes inherit from a single superclass (e.g., Animal → Dog, Animal → Cat)
5. **Hybrid Inheritance**: Combination of multiple types of inheritance

## Benefits of Inheritance

1. **Code Reusability**: Reuse fields and methods of the existing class
2. **Less Development Time**: Inherit and extend functionality rather than building from scratch
3. **Extensibility**: Easily extend the application by adding new subclasses
4. **Data Hiding**: Protect data through access modifiers
5. **Method Overriding**: Customize or extend the behaviors of the parent class

## Real-World Analogy

Think of inheritance like genetic inheritance in families:
- Children inherit traits from their parents (eye color, height, etc.)
- Children may have additional traits not present in their parents
- Children may express inherited traits differently (method overriding)
- Multiple generations can form an inheritance hierarchy
