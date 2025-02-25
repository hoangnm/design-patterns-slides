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
    1. **Single Responsibility (S)**
    2. **Open/Closed (O)**
    3. **Liskov Substitution (L)**
    4. **Interface Segregation (I)**
    5. **Dependency Inversion (D)**
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
    - Example: ![Dependency Inversion](examples/dependency_inversion.md)
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

# Traditional Layered Architecture
- **Common Structure**:
    - Presentation Layer (UI)
    - Application Layer (Services)
    - Domain Layer (Business Logic)
    - Infrastructure Layer (Data Access)

---

# Limitations of Layered Architecture
- **Problems with Scale**:
    - Layers become tightly coupled over time
    - Business logic leaks between layers
    - Domain model becomes anemic
    - Changes require updates across all layers
    - Hard to maintain as complexity grows
    - Difficult to enforce boundaries
    - Often leads to "Big Ball of Mud"

---

# Need for Better Architecture
- **Why Traditional Layers Fall Short**:
    - Business rules mixed with technical concerns
    - Domain logic scattered across layers
    - No clear boundaries between different parts of the system
    - Difficult to implement complex business rules
    - Hard to maintain and evolve

---

# Clean Architecture
- **Definition**: A software architecture pattern that emphasizes separation of concerns, organizing code so that it is independent of frameworks, UI, and databases.

---

# Clean Architecture


---

# Clean Architecture
- **Key Principles**:
    - **Dependency Rule**: Dependencies only point inward
    - **Dependency Inversion**: High-level modules should not depend on low-level modules. Both should depend on abstractions.
    - **Isolation**: Business rules isolated from UI and database
    - **Testability**: The architecture facilitates automated testing by isolating components.

---

# Clean Architecture
- **Structure**:
    1. **Entities Layer** (Core):
    2. **Use Cases Layer**
    3. **Interface Adapters Layer**
    4. **Frameworks & Drivers Layer** (External)


---

# Clean Architecture
- **Entities**:
    - Contains enterprise-wide business rules and data structures
    - Pure business objects with no dependencies
    - Represents core business concepts (e.g., Customer, Order, Product)
    - Has no knowledge of other layers
    - Most stable layer, changes least frequently
    - Contains validation rules that are universally true for the entity

---

# Clean Architecture
- **Use Cases Layer**:
    - Implements application-specific business rules
    - Orchestrates the flow of data to and from entities
    - Characteristics:
        - One class per use case (e.g., CreateOrderUseCase, UpdateUserProfileUseCase)
        - Independent of UI, database, and external concerns
        - Uses interfaces (ports) to communicate with outer layers
        - Contains input/output boundary interfaces
        - Defines DTOs (Data Transfer Objects) for input/output
    - Responsibilities:
        - Validation of business rules
        - Authorization checks
        - Orchestrating multiple entities
        - Managing transactions

---

# Clean Architecture
- **Interface Adapters Layer**:
    - Converts data between the format most convenient for use cases/entities and the format most convenient for external agencies
    - Contains:
        - **Controllers**: Handle HTTP requests, CLI commands, or other entry points
        - **Presenters**: Format data for display (UI, API responses, etc.)
        - **Gateways**: Abstract interfaces for external services
        - **Repositories**: Implement data access interfaces defined by use cases
    - Characteristics:
        - No business rules
        - Purely technical implementations
        - Implements interfaces defined by inner layers
    - Responsibilities:
        - Data format conversion
        - Request/Response handling
        - Input validation
        - Error handling and mapping
        - Database operations
        - External service integration

---

# Clean Architecture
- **Interface Adapters Layer**:
- Example Components:
    - REST Controllers
    - GraphQL Resolvers
    - Database Repositories
    - External API Clients
    - View Models/DTOs
    - JSON/XML Formatters

---

# Clean Architecture
- **Frameworks & Drivers Layer**:
    - Outermost layer containing frameworks and tools
    - Acts as glue code connecting the application to external world
    - Contains:
        - **Web Frameworks**: Express, Spring, Django, etc.
        - **Database Systems**: MySQL, MongoDB, PostgreSQL
        - **UI Frameworks**: React, Angular, Vue
        - **External Services**: Payment gateways, email services
    - Characteristics:
        - Highly volatile and changeable
        - Contains configuration code
        - Minimal business logic
        - Easily replaceable
    - Responsibilities:
        - Framework configuration
        - Database setup and migration
        - External service integration
        - Web server setup
        - UI implementation
        - File I/O operations
    

---


# Clean Architecture
- **Frameworks & Drivers Layer**:
- Example Components:
    - Web server configuration
    - Database connection setup
    - ORM configurations
    - UI components
    - External API configurations
    - File system operations
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

- **Example**: link

---

# Domain Model in DDD
- **Definition**:
    - A conceptual model of the domain that incorporates both behavior and data
    - The heart of the business software
    - A structured vision of the domain
    - Distillation of shared knowledge

- **Characteristics**:
    - Rich in behavior (not anemic)
    - Encapsulates complex business rules
    - Reflects domain expert's mental model
    - Independent of technical concerns
    - Uses ubiquitous language

- **Implementation Guidelines**:
    - Keep business rules in the domain model
    - Use value objects for concepts without identity
    - Create rich behavior instead of anemic models
    - Enforce invariants within the model
    - Use domain events for side effects
    - Apply tactical patterns appropriately

- **Benefits**:
    - Captures complex business rules accurately
    - Provides single source of truth for domain logic
    - Makes business rules explicit and testable
    - Reduces bugs in business logic
    - Improves maintainability
    - Facilitates communication with domain experts

---

# Bounded Contexts in DDD
- **Definition**:
    - Explicit boundary within which a domain model exists
    - Defines where specific terms and concepts of Ubiquitous Language apply
    - Separates different parts of a large system

- **Characteristics**:
    - Each context has its own Ubiquitous Language
    - Same term can mean different things in different contexts
    - Clear boundaries between different parts of the system
    - Independent implementation within each context

- **Example**:
    In an e-commerce system:
    ```java
    // Shipping Context
    class Product {
        private Weight weight;
        private Dimensions dimensions;
        private ShippingCategory category;
    }

    // Inventory Context
    class Product {
        private StockLevel currentStock;
        private Location storageLocation;
        private ReorderPoint reorderLevel;
    }

    // Sales Context
    class Product {
        private Price retailPrice;
        private Discount currentDiscount;
        private Category marketingCategory;
    }
    ```

- **Context Mapping**:
    - **Types of Relationships**:
        - Partnership: Two contexts collaborate
        - Shared Kernel: Shared subset of domain model
        - Customer-Supplier: Upstream/Downstream relationship
        - Conformist: Downstream adopts upstream model
        - Anti-corruption Layer: Translator between contexts
    
- **Implementation Strategies**:
    - Separate modules or services per context
    - Different database schemas
    - Independent deployment units
    - Context-specific data models
    - Clear interface contracts between contexts

- **Benefits**:
    - Reduces complexity by dividing system
    - Allows parallel development
    - Enables different modeling approaches per context
    - Prevents model pollution
    - Clarifies team boundaries
    - Supports microservices architecture

---

# Ubiquitous Language in DDD
- **Definition**: 
    - A common, rigorous language between developers and domain experts
    - Shared vocabulary that is used in code, documentation, and conversation
    - Evolves as the team's understanding of the domain grows

- **Characteristics**:
    - **Precision**: Terms have specific, agreed-upon meanings
    - **Consistency**: Same terms used everywhere in the bounded context
    - **Evolution**: Language grows and changes with the project
    - **Documentation**: Captured in code, tests, and documentation

- **Implementation**:
    - Use domain terms in class names (e.g., `OrderConfirmation` not `ProcessResult`)
    - Model methods after business processes (e.g., `confirmOrder()` not `updateStatus()`)
    - Name variables using domain terminology
    - Document using business language
    - Avoid technical terms in domain logic

- **Benefits**:
    - Reduces translation between technical and business concepts
    - Improves communication with domain experts
    - Makes code self-documenting
    - Ensures consistent understanding across team
    - Helps identify domain concepts early

- **Example**:
    ```java
    // Poor ubiquitous language
    class DataProcessor {
        void process(Data input) {
            if (input.getStatus() == 1) {
                updateRecords(input);
            }
        }
    }

    // Good ubiquitous language
    class OrderProcessor {
        void confirmOrder(Order order) {
            if (order.isPendingConfirmation()) {
                order.confirm();
            }
        }
    }
    ```

---

# Domain-Driven Design (DDD)
- **Benefits**:
    - Aligns code with business requirements
    - Improves communication between technical and domain experts
    - Creates maintainable and flexible domain models
    - Handles complex business logic effectively

---

# Conclusion
- **Recap**: OOP promotes structured, maintainable code, and design patterns provide solutions for common design problems.
- **Importance of Clean Architecture**: Combines OOP and design principles to create scalable, maintainable, and testable systems.

---

# Exercise: Clean Architecture
Design a library management system using Clean Architecture principles.
Requirements:
- Implement core entities (Book, Member, Loan)
- Create use cases for borrowing/returning books
- Add interface adapters for web and console interfaces
- Implement persistence using different databases
---

# Common Pitfalls to Avoid
- **Wrong Abstraction**: A wrong abstraction is worse than code duplication
    - Duplicate code is better than the wrong abstraction
    - Wait until you see the pattern emerge from duplication
    - Don't force patterns where they don't fit
- **Over-Engineering**: Start simple, add complexity only when needed
    - Don't add flexibility that you don't need yet
    - YAGNI (You Aren't Gonna Need It)
---

# References

---

# Q&A
- **Any Questions?**
