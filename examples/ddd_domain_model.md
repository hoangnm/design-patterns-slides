# Anemic vs Rich Domain Models in DDD

This example demonstrates the difference between an anemic domain model (anti-pattern) and a rich domain model (DDD best practice) using an Order Management System.

## Anemic Domain Model (Anti-Pattern)

```java
// Anemic Domain Model - Just Data, No Behavior
public class Order {
    private String orderId;
    private List<OrderItem> items;
    private String status;
    private Customer customer;
    private BigDecimal totalAmount;
    private LocalDateTime createdAt;

    // Getters and Setters
    public String getOrderId() { return orderId; }
    public void setOrderId(String orderId) { this.orderId = orderId; }
    public List<OrderItem> getItems() { return items; }
    public void setItems(List<OrderItem> items) { this.items = items; }
    public String getStatus() { return status; }
    public void setStatus(String status) { this.status = status; }
    // ... more getters and setters
}

// Business Logic in Service Layer
public class OrderService {
    private OrderRepository orderRepository;
    private InventoryService inventoryService;
    private PaymentService paymentService;

    public void addItem(Order order, OrderItem item) {
        // Business logic in service
        if (order.getStatus().equals("CONFIRMED")) {
            throw new IllegalStateException("Cannot modify confirmed order");
        }
        order.getItems().add(item);
        BigDecimal newTotal = order.getTotalAmount().add(item.getPrice());
        order.setTotalAmount(newTotal);
        orderRepository.save(order);
    }

    public void confirmOrder(Order order) {
        // Validation in service
        if (order.getItems().isEmpty()) {
            throw new IllegalStateException("Cannot confirm empty order");
        }
        if (!inventoryService.checkStock(order.getItems())) {
            throw new IllegalStateException("Insufficient stock");
        }
        order.setStatus("CONFIRMED");
        orderRepository.save(order);
    }
}
```

### Problems with Anemic Model:
- Business rules scattered across services
- Domain objects are just data holders
- No encapsulation of business rules
- Hard to ensure business invariants
- Difficult to maintain as complexity grows

## Rich Domain Model (DDD Approach)

```java
// Rich Domain Model - Behavior and Data Together
public class Order {
    private OrderId orderId;
    private List<OrderItem> items;
    private OrderStatus status;
    private Customer customer;
    private Money totalAmount;
    private LocalDateTime createdAt;

    // Constructor ensures valid initial state
    public Order(Customer customer) {
        this.orderId = OrderId.generate();
        this.customer = customer;
        this.items = new ArrayList<>();
        this.status = OrderStatus.DRAFT;
        this.totalAmount = Money.ZERO;
        this.createdAt = LocalDateTime.now();
    }

    // Business methods with clear intent
    public void addItem(Product product, int quantity) {
        validateCanModifyOrder();
        validateQuantity(quantity);
        
        OrderItem item = new OrderItem(product, quantity);
        items.add(item);
        recalculateTotal();
    }

    public void confirm() {
        validateCanConfirm();
        status = OrderStatus.CONFIRMED;
        domainEvents.add(new OrderConfirmedEvent(this));
    }

    public void cancel() {
        validateCanCancel();
        status = OrderStatus.CANCELLED;
        domainEvents.add(new OrderCancelledEvent(this));
    }

    // Business rules and invariants
    private void validateCanModifyOrder() {
        if (!status.equals(OrderStatus.DRAFT)) {
            throw new OrderDomainException("Can only modify orders in DRAFT status");
        }
    }

    private void validateCanConfirm() {
        if (items.isEmpty()) {
            throw new OrderDomainException("Cannot confirm empty order");
        }
        if (!customer.hasGoodStanding()) {
            throw new OrderDomainException("Customer not in good standing");
        }
        if (totalAmount.isGreaterThan(customer.getCreditLimit())) {
            throw new OrderDomainException("Order exceeds customer credit limit");
        }
    }

    private void validateQuantity(int quantity) {
        if (quantity <= 0) {
            throw new OrderDomainException("Quantity must be positive");
        }
    }

    private void recalculateTotal() {
        this.totalAmount = items.stream()
            .map(OrderItem::getSubtotal)
            .reduce(Money.ZERO, Money::add);
    }

    // Limited access to internal state
    public OrderStatus getStatus() { return status; }
    public Money getTotalAmount() { return totalAmount; }
    public List<OrderItem> getItems() { 
        return Collections.unmodifiableList(items); 
    }
}

// Supporting Value Objects
@Value
public class Money {
    BigDecimal amount;
    Currency currency;

    public static final Money ZERO = new Money(BigDecimal.ZERO, Currency.USD);

    public Money add(Money other) {
        validateSameCurrency(other);
        return new Money(amount.add(other.amount), currency);
    }

    public boolean isGreaterThan(Money other) {
        validateSameCurrency(other);
        return amount.compareTo(other.amount) > 0;
    }
}

// Application Service - Thin Layer
public class OrderApplicationService {
    private final OrderRepository orderRepository;
    private final InventoryService inventoryService;

    public void addItemToOrder(OrderId orderId, ProductId productId, int quantity) {
        Order order = orderRepository.findById(orderId);
        Product product = inventoryService.checkAndReserveProduct(productId, quantity);
        
        order.addItem(product, quantity);
        orderRepository.save(order);
    }

    public void confirmOrder(OrderId orderId) {
        Order order = orderRepository.findById(orderId);
        order.confirm();
        orderRepository.save(order);
    }
}
```

### Benefits of Rich Domain Model:
1. **Business Rules Encapsulation**:
   - All order-related rules in one place
   - Clear where to find and modify business logic
   - Self-validating business objects

2. **Strong Consistency**:
   - Impossible to create invalid state
   - Business invariants always maintained
   - Domain events for side effects

3. **Better Maintainability**:
   - Business rules clearly expressed in code
   - Changes localized to relevant domain objects
   - Reduced duplication of business logic

4. **Improved Testing**:
   - Business rules can be tested in isolation
   - Fewer dependencies in tests
   - Better test coverage of business scenarios

5. **Clear Intent**:
   - Methods express business concepts
   - Value objects capture domain concepts
   - Self-documenting code

This example shows how a rich domain model captures business rules and ensures they're consistently applied, while an anemic model scatters business logic across services and loses the benefits of encapsulation.
