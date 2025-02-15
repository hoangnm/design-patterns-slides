# Object-Oriented Programming and Design Patterns

### Hoang Nguyen
### February 11, 2025

---

# Agenda
1. **Introduction to OOP**
   - Key Principles
   - SOLID principles

2. **Design Patterns**
   - Creational Patterns
   - Structural Patterns
   - Behavioral Patterns

3. **Architecture & Design**
   - Domain-Driven Design (DDD)
   - Clean Architecture

4. **Practical Application**
   - Code Examples
   - Exercises

---

# Introduction to OOP
- **Definition of OOP**: A programming paradigm based on the concept of "objects", which can contain data and code.

---

# Introduction to OOP
- **Key Principles**:
    - **Encapsulation**: Bundling the data and methods that operate on the data.
    - **Abstraction**: Hiding complex implementation details and showing only the essential features.
    - **Inheritance**: Mechanism to create a new class using properties and methods of an existing class.
    - **Polymorphism**: Ability to present the same interface for different data types.

---

# Introduction to OOP
- **SOLID Principles**:
    1. **Single Responsibility (S)**:
    2. **Open/Closed (O)**:
    3. **Liskov Substitution (L)**:
    4. **Interface Segregation (I)**:
    5. **Dependency Inversion (D)**:
---

# SOLID Principles
- **Single Responsibility (S)**:
    - A class should have only one reason to change
    - Each class should handle one specific functionality

---

# SOLID Principles
- **Open/Closed (O)**:
    - Software entities should be open for extension but closed for modification
    - Use interfaces and abstract classes to allow extensions

---

# SOLID Principles
- **Liskov Substitution (L)**:
    - Subtypes must be substitutable for their base types
    - Derived classes must be usable through the base class interface

---


# SOLID Principles
- **Interface Segregation (I)**:
    - Clients should not be forced to depend on interfaces they don't use
    - Break interfaces into smaller, specific ones

---


# SOLID Principles
- **Dependency Inversion (D)**:
    - High-level modules should not depend on low-level modules
    - Both should depend on abstractions
    - Example: link
---

# SOLID Principles
- **Benefits**:
    - Maintainable and scalable code
    - Easier to test and debug
    - More flexible and reusable
    - Reduced coupling between modules

---

# Composition over Inheritance:
- **Definition**: A design principle that suggests using object composition rather than inheritance for reuse.
- **Benefits**:
    - More flexible design
    - Avoids tight coupling
    - Easier to modify at runtime
    - Prevents deep inheritance hierarchies
    - **Example**: link
---

---


# Introduction to Design Patterns
- **Definition of Design Patterns**: General repeatable solutions to common problems in software design.
- **Importance**: Promote code reuse, better architecture, and efficient communication among developers.

---

# Categories of Design Patterns
- **Creational Patterns**
- **Structural Patterns**
- **Behavioral Patterns**

---

# Creational Patterns
- **Definition**: Creational patterns deal with object creation mechanisms, trying to create objects in a manner suitable to the situation. They help make a system independent of how its objects are created, composed, and represented.

---

# Creational Patterns
- **Key Characteristics**:
    1. **Encapsulation**: Hide creation logic from client code
    2. **Flexibility**: Allow system to be independent of how objects are created
    3. **Reusability**: Promote reuse of object creation code
    4. **Complexity Management**: Handle complex object creation scenarios

---

# Creational Patterns
- **Common Creational Patterns**:
    - **Factory**: Creation of objects without specifying the exact class.
    - **Abstract Factory**: Provides an interface for creating families of related objects.
    - **Builder**: Separates the construction of a complex object from its representation.
    - **Prototype**: Creates new objects by cloning an existing object (prototype).
    - **Singleton**: Ensures a class has only one instance and provides global access to it.

---

# Factory Pattern
- **Purpose**: Creates objects without exposing the instantiation logic.
- **Code Example**: link

---

# Factory Pattern
- **When to Use**:
    - Complex/Dynamic Object Creation
    - Decoupling
    - Reusable Creation Logic

---

# Structural Patterns
- **Definition**: Structural patterns deal with how classes and objects are composed to form larger structures. They help ensure that when parts of a system change, the entire system doesn't need to change.

---

# Structural Patterns
- **Key Characteristics**:
    1. **Composition**: Focus on how objects and classes can be combined to form larger structures
    2. **Relationship Management**: Handle relationships between objects effectively
    3. **Flexibility**: Allow systems to change their composition at runtime
    4. **Simplification**: Make complex systems easier to manage
---

# Structural Patterns
- **Common Structural Patterns**:
    - **Adapter**: Allows incompatible interfaces to work together by wrapping an object in an adapter.
    - **Composite**: Composes objects into tree structures to represent part-whole hierarchies.
    - **Proxy**: Provides a surrogate or placeholder to control access to an object.
    - **Facade**: Provides a simplified interface to a complex subsystem.
    - **Bridge**: Separates an abstraction from its implementation.
    - **Decorator**: Adds new responsibilities to objects dynamically.
    - **Flyweight**: Reduces the cost of creating and manipulating a large number of similar objects.

---

# Adapter Pattern
- **Purpose**: Allows incompatible interfaces to work together by wrapping an object in an adapter to make it compatible with another class.
- **Code Example**: link

---

# Adapter Pattern
- **When to Use**:
    - When you want to use an existing class that doesn't fit your interface
    - When you need to create a reusable class that cooperates with classes that don't share compatible interfaces
    - When you want to create a middle-layer class that serves as a converter between your code and a third-party class

---

# Behavioral Patterns
- **Definition**: Behavioral patterns deal with communication between objects, how objects interact and distribute responsibilities among themselves.

---

# Behavioral Patterns
- **Key Characteristics**:
    1. **Communication**: Focus on how objects communicate with each other
    2. **Responsibility Distribution**: Define clear patterns of communication between objects
    3. **Loose Coupling**: Reduce dependencies between communicating objects
    4. **Flexibility**: Allow changing behavior at runtime

---

# Behavioral Patterns
- **Common Behavioral Patterns**:
    - **Observer**: Notifies a group of objects about changes in the state of another object.
    - **Strategy**: Enables selecting an algorithm's behavior at runtime.
    - **Command**: Encapsulates actions as objects, allowing parameterization of clients.
    - **State**: Allows an object to alter its behavior when its internal state changes.
    - **Template Method**: Defines the skeleton of an algorithm, letting subclasses override specific steps.
    - **Iterator**: Provides a way to access elements of a collection sequentially.
    - **Mediator**: Reduces coupling between objects by making them communicate via a mediator object.
    - **Chain of Responsibility**: Passes requests along a chain of handlers until one handles it.

---

# Observer Pattern
- **Purpose**: Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

- **Code Example**: link
---

# Observer pattern

- **When to Use**:
    - **Event Handling**
    - **Loose Coupling**
    - **Broadcasting**:

---

How about the design patterns for complex business applications?

---

# Domain-Driven Design (DDD)
- **Definition**: A software design approach focusing on modeling software to match a domain according to input from domain experts.

---

# Domain-Driven Design (DDD)
- **Key Concepts**:
    1. **Ubiquitous Language**
    2. **Bounded Contexts**
    3. **Domain Model**
    4. **Building Blocks**
        - **Entities**: Objects with identity and lifecycle
        - **Value Objects**: Immutable objects without identity
        - **Aggregates**: Cluster of associated objects treated as a unit
        - **Services**: Operations that don't belong to any entity
        - **Repositories**: Persistence abstraction for aggregates

- **Example**: link

---

# Domain-Driven Design (DDD)
- **Benefits**:
    - Aligns code with business requirements
    - Improves communication between technical and domain experts
    - Creates maintainable and flexible domain models
    - Handles complex business logic effectively

---

# Clean Architecture
- **Definition**: A software architecture pattern that emphasizes separation of concerns, organizing code so that it is independent of frameworks, UI, and databases.

---

# Clean Architecture
- **Structure**:
    1. **Entities Layer** (Core)
    2. **Use Cases Layer**
    3. **Interface Adapters Layer**
    4. **Frameworks & Drivers Layer** (External)

---

# Clean Architecture
- **Key Principles**:
    - **Dependency Rule**: Dependencies only point inward
    - **Dependency Inversion**: High-level modules should not depend on low-level modules. Both should depend on abstractions.
    - **Isolation**: Business rules isolated from UI and database
    - **Testability**: The architecture facilitates automated testing by isolating components.

---

# Clean Architecture
- **Benefits**:
    - Independent of frameworks
    - Testable business logic
    - Independent of UI
    - Independent of database
    - Independent of external agencies

---

# Clean Architecture
- **Implementation Example**:

---

# Clean Architecture
- **Relation to OOP and SOLID**: OOP principles like encapsulation and abstraction support the goals of Clean Architecture, leading to maintainable, flexible systems.

---

# Conclusion
- **Recap**: OOP promotes structured, maintainable code, and design patterns provide solutions for common design problems.
- **Importance of Clean Architecture**: Combines OOP and design principles to create scalable, maintainable, and testable systems.

---

# Practice Exercises

## Exercise 1: Factory Pattern
Create a document processing system that can handle different types of documents (PDF, Word, Text).
Requirements:
- Implement a Document interface with methods: open(), save()
- Create concrete classes for each document type
- Implement a DocumentFactory to create appropriate document objects
- Add functionality to process documents based on file extension

## Exercise 2: Observer Pattern
Build a weather monitoring system.
Requirements:
- Create a WeatherStation that monitors temperature, humidity, and pressure
- Implement multiple display devices (Phone, Website, Desktop App)
- Ensure displays are updated when weather data changes
- Allow displays to subscribe/unsubscribe from updates

## Exercise 3: Adapter Pattern
Design a payment processing system that works with different payment gateways.
Requirements:
- Create a common PaymentProcessor interface
- Implement adapters for PayPal, Stripe, and legacy payment system
- Each system has different methods that need to be adapted
- Handle currency conversion if needed

## Exercise 4: Clean Architecture
Design a library management system using Clean Architecture principles.
Requirements:
- Implement core entities (Book, Member, Loan)
- Create use cases for borrowing/returning books
- Add interface adapters for web and console interfaces
- Implement persistence using different databases

Tips for Exercises:
1. Start with simple implementations
2. Focus on pattern principles
3. Add complexity gradually
4. Write tests for your implementations
5. Consider edge cases
6. Document your design decisions

# Q&A
- **Any Questions?**
