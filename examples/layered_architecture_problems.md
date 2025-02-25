# Problems with Traditional Layered Architecture

This example demonstrates common issues that arise when using traditional layered architecture for complex business logic, using an e-commerce order processing system as an example.

## Bad Example - Traditional Layers

```java
// Presentation Layer (Controllers)
@RestController
public class OrderController {
    @Autowired
    private OrderService orderService;
    
    @PostMapping("/orders")
    public OrderDTO createOrder(OrderRequest request) {
        // Business validation leaking into controller
        if (request.getItems().isEmpty()) {
            throw new BadRequestException("Order must have items");
        }
        return orderService.createOrder(request);
    }
}

// Application Layer (Services)
@Service
public class OrderService {
    @Autowired
    private OrderRepository orderRepository;
    @Autowired
    private PaymentService paymentService;
    @Autowired
    private InventoryService inventoryService;
    
    @Transactional
    public OrderDTO createOrder(OrderRequest request) {
        // Business logic scattered across service layer
        Order order = new Order();
        order.setItems(request.getItems());
        
        // Direct database operations mixed with business logic
        if (inventoryService.checkStock(order.getItems())) {
            // Payment logic mixed with order processing
            PaymentResult payment = paymentService.processPayment(order.getTotalAmount());
            if (payment.isSuccessful()) {
                order.setStatus("PAID");
                inventoryService.reduceStock(order.getItems());
                orderRepository.save(order);
                // Direct email sending from service layer
                emailService.sendOrderConfirmation(order);
                return OrderDTO.from(order);
            }
        }
        throw new BusinessException("Order processing failed");
    }
}

// Domain Layer (Anemic Domain Model)
@Entity
public class Order {
    private Long id;
    private List<OrderItem> items;
    private String status;
    private BigDecimal totalAmount;
    
    // Just getters and setters, no business logic
    // Domain model becomes just a data container
}

// Infrastructure Layer
@Repository
public class OrderRepository {
    // Direct SQL queries mixed with business rules
    @Query("SELECT o FROM Order o WHERE o.status = 'PAID' AND o.totalAmount > :amount")
    List<Order> findLargeOrders(@Param("amount") BigDecimal amount);
}
```

## Problems Demonstrated:

1. **Business Logic Scattered**:
   - Validation spread across Controller and Service layers
   - Business rules mixed with infrastructure concerns
   - No clear ownership of business rules

2. **Tight Coupling**:
   - Service directly depends on multiple repositories and services
   - Hard to change one component without affecting others
   - Testing requires mocking many dependencies

3. **Anemic Domain Model**:
   - Order class is just a data holder
   - Business logic lives in services instead of domain objects
   - No encapsulation of business rules

4. **Infrastructure Concerns Mixed with Business Logic**:
   - Direct database operations in service layer
   - Email sending mixed with order processing
   - Transaction management coupled with business logic

5. **Hard to Extend**:
   - Adding new payment methods requires changing OrderService
   - New inventory rules affect multiple layers
   - Business rules are hard to find and modify

6. **Poor Separation of Concerns**:
   - Presentation logic leaks into service layer
   - Infrastructure concerns leak upwards
   - Cross-cutting concerns scattered everywhere


