# Object-Oriented Programming Anti-Patterns to Avoid

While OOP provides powerful tools for organizing code, there are common anti-patterns that can lead to maintenance nightmares, poor performance, and difficult-to-extend code. Here are some key anti-patterns to avoid:

## 1. God Object / Blob

**Problem**: Creating classes that know or do too much.

**Symptoms**:
- Extremely large classes with many methods and properties
- Classes that handle multiple unrelated responsibilities
- High coupling with many other parts of the system

**Example**:
```java
// Anti-pattern: God Object
class SystemManager {
    // Hundreds of methods and properties handling:
    // Database connections, UI rendering, business logic,
    // authentication, logging, network communication, etc.
    
    public void saveUser() { /* ... */ }
    public void renderUserInterface() { /* ... */ }
    public void validateBusinessRules() { /* ... */ }
    public void handleNetworkRequest() { /* ... */ }
    public void generateReports() { /* ... */ }
    // ... and hundreds more methods
}
```

**Solution**: Apply Single Responsibility Principle. Break the class into smaller, focused classes.

## 2. Spaghetti Code

**Problem**: Code with little structure, where objects are highly interdependent.

**Symptoms**:
- Tangled dependencies between classes
- Difficulty tracing program flow
- Changes in one place break things in unexpected places

**Example**:
```java
class Order {
    public void process() {
        // Directly accesses internals of other objects
        if (customer.accountStatus.equals("ACTIVE")) {
            for (Item item : items) {
                inventory.stock[item.id] -= item.quantity;
            }
            accounting.balance += this.total;
            shipping.scheduleDelivery(customer.address, this);
        }
    }
}
```

**Solution**: Use proper encapsulation, clear interfaces, and dependency injection.

## 3. Yo-yo Problem

**Problem**: Excessive inheritance hierarchies that force developers to constantly move up and down the inheritance tree to understand functionality.

**Symptoms**:
- Deep inheritance hierarchies (more than 2-3 levels)
- Methods defined far up the hierarchy but used by distant subclasses
- Difficulty understanding where specific behaviors are defined

**Example**:
```java
class Entity { /* ... */ }
class PhysicalEntity extends Entity { /* ... */ }
class LivingEntity extends PhysicalEntity { /* ... */ }
class Animal extends LivingEntity { /* ... */ }
class Mammal extends Animal { /* ... */ }
class Canine extends Mammal { /* ... */ }
class Dog extends Canine { /* ... */ }
class DomesticDog extends Dog { /* ... */ }
class Labrador extends DomesticDog { /* ... */ }
```

**Solution**: Favor composition over inheritance. Use interfaces and delegation.

## 4. Poltergeists / Temporary Objects

**Problem**: Classes that have very limited responsibility and short lifetimes.

**Symptoms**:
- Classes that appear, do something trivial, then disappear
- Classes with names like "Manager", "Controller", "Handler" that just delegate

**Example**:
```java
// Anti-pattern: Poltergeist
class DataInitializer {
    public void initialize() {
        // Just calls another object and disappears
        Database.getInstance().init();
    }
}

// Usage
new DataInitializer().initialize();
```

**Solution**: Assign these responsibilities to appropriate existing classes or create meaningful abstractions.

## 5. Golden Hammer

**Problem**: Trying to solve every problem with the same tool or pattern.

**Symptoms**:
- Overuse of inheritance when composition would be better
- Forcing every component to be a singleton
- Applying design patterns unnecessarily

**Example**:
```java
// Anti-pattern: Making everything a singleton
class Logger { /* singleton */ }
class Configuration { /* singleton */ }
class ConnectionPool { /* singleton */ }
class UserManager { /* singleton */ }
class ShoppingCart { /* singleton - clearly wrong! */ }
```

**Solution**: Choose the right tool for each specific problem. Learn multiple patterns and when to apply each.

## 6. Refused Bequest

**Problem**: Subclasses that inherit methods they don't need or want.

**Symptoms**:
- Subclasses that throw exceptions for inherited methods
- Inherited methods that are empty or return null
- Comments like "don't use this method"

**Example**:
```java
class Bird {
    public void fly() {
        // Flying implementation
    }
}

class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly!");
    }
}
```

**Solution**: Redesign the inheritance hierarchy or use composition instead.

## 7. Circle-Ellipse Problem (Violation of LSP)

**Problem**: Modeling "is-a" relationships that seem intuitive but violate the Liskov Substitution Principle.

**Symptoms**:
- Subclasses that restrict functionality of the parent class
- Subclasses that throw exceptions for valid parent class operations

**Example**:
```java
class Rectangle {
    protected int width;
    protected int height;
    
    public void setWidth(int width) { this.width = width; }
    public void setHeight(int height) { this.height = height; }
}

class Square extends Rectangle {
    // A square must have equal sides, so we override:
    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width; // Breaks expected behavior!
    }
    
    @Override
    public void setHeight(int height) {
        this.width = height; // Breaks expected behavior!
        this.height = height;
    }
}
```

**Solution**: Favor composition over inheritance or redesign the class hierarchy.

## 8. Anemic Domain Model

**Problem**: Classes that are just data containers with getters and setters, with behavior elsewhere.

**Symptoms**:
- Domain objects with no behavior, just data
- Service classes that contain all the logic that should be in domain objects

**Example**:
```java
// Anti-pattern: Anemic domain model
class Customer {
    private String name;
    private String email;
    
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
    // No behavior, just data
}

// All logic is in service classes
class CustomerService {
    public void validateCustomer(Customer customer) { /* ... */ }
    public void notifyCustomer(Customer customer) { /* ... */ }
    public boolean isPreferred(Customer customer) { /* ... */ }
}
```

**Solution**: Move behavior into domain objects to create a rich domain model.

## 9. Feature Envy

**Problem**: A method that seems more interested in another class than its own.

**Symptoms**:
- Methods that use many getters from another object
- Methods that would make more sense in another class

**Example**:
```java
class Order {
    private Customer customer;
    
    public void displayCustomerDetails() {
        // This method is more interested in Customer than Order
        System.out.println("Name: " + customer.getName());
        System.out.println("Email: " + customer.getEmail());
        System.out.println("Phone: " + customer.getPhone());
        System.out.println("Address: " + customer.getAddress());
    }
}
```

**Solution**: Move the method to the class it's most interested in.

## 10. Shotgun Surgery

**Problem**: A single change requires modifications in many different classes.

**Symptoms**:
- Small changes require updates to many unrelated classes
- High coupling throughout the system

**Example**:
```java
// Adding a new field like "phoneNumber" requires changes in:
class Customer { /* ... */ }
class CustomerDAO { /* ... */ }
class CustomerDTO { /* ... */ }
class CustomerValidator { /* ... */ }
class CustomerForm { /* ... */ }
class CustomerReport { /* ... */ }
// ... and many more
```

**Solution**: Improve cohesion and reduce coupling. Apply the Single Responsibility Principle.

## Avoiding These Anti-Patterns

To avoid these anti-patterns:

1. **Follow SOLID principles**
2. **Favor composition over inheritance**
3. **Design for change**
4. **Keep classes focused and small**
5. **Use design patterns appropriately**
6. **Refactor regularly**
7. **Review code with team members**
