# Design Pattern Anti-Patterns to Avoid

While design patterns provide proven solutions to common problems, they can be misused or implemented incorrectly. Here are some common anti-patterns related to design patterns:

## 1. Pattern Overuse

**Problem**: Applying design patterns where they're not needed, adding unnecessary complexity.

**Symptoms**:
- Simple problems solved with complex patterns
- Code that's harder to understand because of forced pattern usage
- Excessive abstractions for minimal benefit

**Example**:
```java
// Anti-pattern: Using Factory pattern for simple object creation
interface Animal { void makeSound(); }
class Dog implements Animal { 
    public void makeSound() { System.out.println("Woof!"); }
}

// Unnecessary factory for a simple class with no complex initialization
class AnimalFactory {
    public static Animal createDog() {
        return new Dog(); // No complex logic justifying a factory
    }
}

// Usage
Animal dog = AnimalFactory.createDog(); // Overcomplicated vs. new Dog()
```

**Solution**: Use the simplest solution that works. Only apply patterns when their benefits outweigh their complexity.

## 2. Singleton Abuse

**Problem**: Overusing the Singleton pattern, creating global state.

**Symptoms**:
- Many Singleton classes throughout the codebase
- Hard-to-trace dependencies
- Difficult testing due to global state

**Example**:
```java
// Anti-pattern: Overusing singletons
class Logger { /* singleton */ }
class Database { /* singleton */ }
class Configuration { /* singleton */ }
class UserManager { /* singleton */ }
class ShoppingCart { /* singleton - clearly wrong! */ }

// Usage creates hidden dependencies
public void processOrder() {
    if (UserManager.getInstance().isUserLoggedIn()) {
        ShoppingCart.getInstance().checkout();
        Logger.getInstance().log("Order processed");
    }
}
```

**Solution**: Use dependency injection instead. Pass dependencies explicitly rather than accessing global state.

## 3. Interface Bloat from Observer Pattern

**Problem**: Creating overly generic observer interfaces that force implementations to care about irrelevant events.

**Symptoms**:
- Observer interfaces with many unrelated methods
- Empty implementations of observer methods
- Observers receiving notifications they don't care about

**Example**:
```java
// Anti-pattern: Bloated observer interface
interface SystemObserver {
    void onUserLogin();
    void onUserLogout();
    void onDatabaseConnection();
    void onNetworkFailure();
    void onMemoryLow();
    void onNewOrder();
    // Many more unrelated events...
}

// Implementations must handle all events, even irrelevant ones
class UserInterface implements SystemObserver {
    public void onUserLogin() { /* Update UI */ }
    public void onUserLogout() { /* Update UI */ }
    public void onDatabaseConnection() { /* Empty - don't care */ }
    public void onNetworkFailure() { /* Empty - don't care */ }
    public void onMemoryLow() { /* Empty - don't care */ }
    public void onNewOrder() { /* Empty - don't care */ }
}
```

**Solution**: Use specific, focused observer interfaces or event types. Consider using the publish-subscribe pattern with specific event types.

## 4. Decorator Explosion

**Problem**: Creating too many small decorator classes, leading to "wrapper hell."

**Symptoms**:
- Long chains of decorators
- Difficulty understanding the full behavior of an object
- Complex instantiation code

**Example**:
```java
// Anti-pattern: Decorator explosion
// Creating a text processor with many decorators
TextProcessor processor = new SpellCheckDecorator(
    new AutoSaveDecorator(
        new EncryptionDecorator(
            new CompressionDecorator(
                new LoggingDecorator(
                    new BasicTextProcessor()
                )
            )
        )
    )
);
```

**Solution**: Consider using the Strategy pattern for some behaviors, or create composite decorators that combine common functionality.

## 5. MVC Megacontroller

**Problem**: Controllers in MVC that take on too many responsibilities.

**Symptoms**:
- Massive controller classes
- Controllers that contain business logic
- Controllers that directly manipulate the DOM or UI

**Example**:
```java
// Anti-pattern: MVC Controller doing too much
class OrderController {
    public void processOrder(Request request) {
        // Parse request parameters
        String productId = request.getParameter("productId");
        int quantity = Integer.parseInt(request.getParameter("quantity"));
        
        // Business logic belongs in the model
        Product product = database.findProduct(productId);
        if (product.getStock() < quantity) {
            throw new OutOfStockException();
        }
        
        // More business logic
        Order order = new Order();
        order.addItem(product, quantity);
        double tax = order.getSubtotal() * 0.08;
        order.setTax(tax);
        
        // Persistence logic
        database.save(order);
        product.setStock(product.getStock() - quantity);
        database.update(product);
        
        // Direct view manipulation
        view.showElement("confirmation");
        view.hideElement("orderForm");
        view.setInnerHTML("orderNumber", order.getId());
    }
}
```

**Solution**: Keep controllers thin. Move business logic to the model, UI manipulation to the view, and use separate service classes for complex operations.

## 7. Strategy Overkill

**Problem**: Using the Strategy pattern for simple conditional logic.

**Symptoms**:
- Many small strategy classes for simple variations
- Increased complexity for minimal benefit
- Difficulty following the code flow

**Example**:
```java
// Anti-pattern: Strategy pattern for simple logic
interface DiscountStrategy {
    double calculateDiscount(double amount);
}

class NoDiscountStrategy implements DiscountStrategy {
    public double calculateDiscount(double amount) { return 0; }
}

class FivePercentDiscountStrategy implements DiscountStrategy {
    public double calculateDiscount(double amount) { return amount * 0.05; }
}

class TenPercentDiscountStrategy implements DiscountStrategy {
    public double calculateDiscount(double amount) { return amount * 0.1; }
}

// Usage
DiscountStrategy strategy;
if (isPreferredCustomer && orderTotal > 1000) {
    strategy = new TenPercentDiscountStrategy();
} else if (isPreferredCustomer) {
    strategy = new FivePercentDiscountStrategy();
} else {
    strategy = new NoDiscountStrategy();
}
double discount = strategy.calculateDiscount(orderTotal);

// Could be simpler as:
// double discount = 0;
// if (isPreferredCustomer && orderTotal > 1000) {
//     discount = orderTotal * 0.1;
// } else if (isPreferredCustomer) {
//     discount = orderTotal * 0.05;
// }
```

**Solution**: Use the Strategy pattern only when the algorithms are complex or when you need to switch strategies at runtime.

## 8. Adapter Avalanche

**Problem**: Creating too many adapters instead of standardizing interfaces.

**Symptoms**:
- Many adapter classes converting between similar interfaces
- Complex adapter hierarchies
- Difficulty tracking the flow of data through adapters

**Example**:
```java
// Anti-pattern: Too many adapters
// Instead of standardizing on a common interface:
class LegacyCustomer { /* ... */ }
class NewCustomer { /* ... */ }
class ThirdPartyCustomer { /* ... */ }

// We create many adapters:
class LegacyToNewCustomerAdapter { /* ... */ }
class NewToLegacyCustomerAdapter { /* ... */ }
class ThirdPartyToLegacyCustomerAdapter { /* ... */ }
class ThirdPartyToNewCustomerAdapter { /* ... */ }
class NewToThirdPartyCustomerAdapter { /* ... */ }
class LegacyToThirdPartyCustomerAdapter { /* ... */ }
```

**Solution**: Standardize on common interfaces where possible. Use adapters sparingly for truly incompatible external systems.

## Avoiding These Anti-Patterns

To avoid design pattern anti-patterns:

1. **Understand the problem first**, then consider if a pattern applies
2. **Start simple** and only add complexity when needed
3. **Know when to stop** - don't over-engineer
4. **Consider the maintenance burden** of each pattern you introduce
5. **Use patterns for their intended purposes**
6. **Document your pattern usage** to help other developers understand your intent
