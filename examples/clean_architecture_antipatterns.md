# Common Anti-patterns in Clean Architecture

This example demonstrates common mistakes and anti-patterns when implementing Clean Architecture.

## Anti-pattern 1: Violating Dependency Rule

```java
// WRONG: Entity depending on external frameworks
@Entity
@Table(name = "orders")
public class Order {  // Entity shouldn't know about JPA
    @Id
    @GeneratedValue
    private Long id;
    
    @OneToMany(cascade = CascadeType.ALL)
    private List<OrderItem> items;
}
```

## Anti-pattern 2: Use Cases Bypassing Boundaries

```java
// WRONG: Use case directly using framework-specific types
public class CreateOrderUseCase {
    @Autowired  // Framework-specific annotation in use case
    private JpaOrderRepository orderRepository;
    
    public ResponseEntity<OrderDTO> execute(HttpRequest request) {  // Direct coupling to HTTP
        // Business logic mixed with HTTP concerns
        if (request.getHeader("Authorization") == null) {
            return ResponseEntity.status(401).build();
        }
        // ...
    }
}
```

## Anti-pattern 3: Leaky Abstractions

```java
// WRONG: Repository interface exposing database details
public interface OrderRepository {
    @Query("SELECT o FROM Order o WHERE o.status = ?1")  // SQL leaking into domain
    List<Order> findByStatus(String status);
    
    @Transactional  // Infrastructure concern in domain interface
    void save(Order order);
}
```

## Anti-pattern 4: Mixed Responsibilities

```java
// WRONG: Entity handling presentation concerns
public class Product {
    private String name;
    private BigDecimal price;
    
    public String toJson() {  // Presentation logic in entity
        return String.format("{\"name\":\"%s\",\"price\":%s}", 
            name, price.toString());
    }
    
    public void saveToDatabase() {  // Infrastructure logic in entity
        // Direct database operations
    }
}
```

## Anti-pattern 5: Interface Adapters with Business Logic

```java
// WRONG: Controller containing business rules
@RestController
public class OrderController {
    @PostMapping("/orders")
    public ResponseEntity<OrderDTO> createOrder(@RequestBody OrderRequest request) {
        // Business validation in controller
        if (request.getItems().isEmpty()) {
            throw new BusinessException("Order must have items");
        }
        
        // Business calculations in controller
        BigDecimal total = request.getItems().stream()
            .map(item -> item.getPrice().multiply(item.getQuantity()))
            .reduce(BigDecimal.ZERO, BigDecimal::add);
            
        if (total.compareTo(customer.getCreditLimit()) > 0) {
            throw new BusinessException("Order exceeds credit limit");
        }
    }
}
```

## Anti-pattern 6: Direct Framework Usage

```java
// WRONG: Direct framework usage in use cases
public class UpdateInventoryUseCase {
    public void execute(String productId) {
        // Direct MongoDB usage in use case
        MongoClient mongoClient = new MongoClient();
        MongoDatabase database = mongoClient.getDatabase("inventory");
        MongoCollection<Document> collection = database.getCollection("products");
        
        // Direct framework-specific operations
        collection.updateOne(
            eq("_id", new ObjectId(productId)),
            set("stock", 0)
        );
    }
}
```

## Correct Implementation Examples

```java
// CORRECT: Clean entity
public class Order {
    private OrderId id;
    private List<OrderItem> items;
    private OrderStatus status;

    public void addItem(Product product, int quantity) {
        validateQuantity(quantity);
        items.add(new OrderItem(product, quantity));
    }

    private void validateQuantity(int quantity) {
        if (quantity <= 0) {
            throw new InvalidOrderException("Quantity must be positive");
        }
    }
}

// CORRECT: Use case with proper boundaries
public class CreateOrderUseCase implements InputBoundary {
    private final OrderRepository orderRepository;
    private final OutputBoundary outputBoundary;

    public void execute(CreateOrderInput input) {
        Order order = new Order(input.getCustomerId());
        // Business logic here
        OrderOutput output = new OrderOutput(order);
        outputBoundary.present(output);
    }
}

// CORRECT: Clean repository interface
public interface OrderRepository {
    Order findById(OrderId id);
    void save(Order order);
}

// CORRECT: Controller respecting boundaries
public class OrderController {
    private final CreateOrderUseCase createOrderUseCase;
    
    public ResponseEntity<OrderResponse> createOrder(OrderRequest request) {
        CreateOrderInput input = requestToInput(request);
        return createOrderUseCase.execute(input);
    }
}
```

## Common Mistakes to Avoid:

1. **Mixing Layers**:
   - Don't let entities know about persistence
   - Keep business rules out of controllers
   - Don't let use cases know about web frameworks

2. **Wrong Dependencies**:
   - Dependencies should point inward
   - Outer layers depend on inner layers
   - Use interfaces for crossing boundaries

3. **Framework Coupling**:
   - Don't use framework annotations in entities
   - Keep framework code at the edges
   - Use clean interfaces between layers

4. **Business Logic Leaks**:
   - Keep business rules in entities and use cases
   - Don't put validation in controllers
   - Don't mix business and technical concerns

5. **Poor Boundary Control**:
   - Use proper input/output boundaries
   - Convert between layer-specific models
   - Keep each layer focused on its responsibility

Remember: Clean Architecture is about making the business rules central and independent of technical details. Each layer should have a single responsibility and clear boundaries.
