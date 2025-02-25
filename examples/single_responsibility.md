# Single Responsibility Principle Examples

This example demonstrates good and bad implementations of the Single Responsibility Principle (SRP) using an Order Processing system.

## Bad Example - Violating SRP

```java
// BAD: Class has multiple responsibilities
public class Order {
    private List<OrderItem> items;
    private Customer customer;
    private BigDecimal totalAmount;

    // Order management responsibility
    public void addItem(OrderItem item) {
        items.add(item);
        calculateTotal();
    }

    // Pricing responsibility
    private void calculateTotal() {
        totalAmount = items.stream()
            .map(item -> item.getPrice().multiply(new BigDecimal(item.getQuantity())))
            .reduce(BigDecimal.ZERO, BigDecimal::add);
    }

    // Database responsibility
    public void saveToDatabase() {
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/shop");
        // Direct SQL operations
        String sql = "INSERT INTO orders (customer_id, total_amount) VALUES (?, ?)";
        // ... database operations
    }

    // Email responsibility
    public void sendConfirmationEmail() {
        String emailContent = "Order confirmed! Total: " + totalAmount;
        EmailServer emailServer = new EmailServer();
        emailServer.send(customer.getEmail(), "Order Confirmation", emailContent);
    }

    // PDF generation responsibility
    public void generateInvoicePDF() {
        PDFGenerator generator = new PDFGenerator();
        generator.addText("Invoice for Order");
        generator.addTable(items);
        generator.save("invoice.pdf");
    }
}
```

Problems with this implementation:
1. Class has multiple reasons to change
2. Mixes business logic with infrastructure concerns
3. Hard to test and maintain
4. Violates separation of concerns
5. Tightly coupled to external services

## Good Example - Following SRP

```java
// GOOD: Each class has a single responsibility
public class Order {
    private OrderId id;
    private List<OrderItem> items;
    private Customer customer;
    private Money totalAmount;

    // Only order management responsibility
    public void addItem(OrderItem item) {
        items.add(item);
        recalculateTotal();
    }

    private void recalculateTotal() {
        totalAmount = items.stream()
            .map(OrderItem::getSubtotal)
            .reduce(Money.ZERO, Money::add);
    }
}

// Separate class for pricing calculations
public class OrderPriceCalculator {
    public Money calculateTotal(Order order) {
        return order.getItems().stream()
            .map(this::calculateItemPrice)
            .reduce(Money.ZERO, Money::add);
    }

    private Money calculateItemPrice(OrderItem item) {
        return item.getUnitPrice().multiply(item.getQuantity());
    }
}

// Separate class for persistence
public class OrderRepository {
    private final DatabaseConnection connection;

    public void save(Order order) {
        // Handle database operations
    }

    public Order findById(OrderId id) {
        // Handle database operations
    }
}

// Separate class for email notifications
public class OrderNotificationService {
    private final EmailService emailService;
    private final NotificationTemplateEngine templateEngine;

    public void sendOrderConfirmation(Order order) {
        String content = templateEngine.generateOrderConfirmation(order);
        EmailMessage email = new EmailMessage(
            order.getCustomer().getEmail(),
            "Order Confirmation",
            content
        );
        emailService.send(email);
    }
}

// Separate class for document generation
public class OrderDocumentGenerator {
    private final PDFGenerator pdfGenerator;
    private final DocumentTemplateEngine templateEngine;

    public File generateInvoice(Order order) {
        Document document = templateEngine.createInvoiceTemplate();
        document.addOrderDetails(order);
        return pdfGenerator.generate(document);
    }
}

// Orchestration handled by separate service
public class OrderProcessor {
    private final OrderRepository repository;
    private final OrderPriceCalculator priceCalculator;
    private final OrderNotificationService notificationService;
    private final OrderDocumentGenerator documentGenerator;

    public OrderResult processOrder(Order order) {
        try {
            // Coordinate the workflow
            order.setTotalAmount(priceCalculator.calculateTotal(order));
            repository.save(order);
            notificationService.sendOrderConfirmation(order);
            File invoice = documentGenerator.generateInvoice(order);
            
            return OrderResult.success(order, invoice);
        } catch (Exception e) {
            return OrderResult.failure(e.getMessage());
        }
    }
}
```

Benefits of this implementation:
1. Each class has a single, clear responsibility
2. Easy to modify one aspect without affecting others
3. Better testability (can mock dependencies)
4. Clear separation of concerns
5. More maintainable and flexible
6. Easy to extend functionality
7. Better reusability of components

When to apply SRP:
1. When a class has multiple unrelated responsibilities
2. When changes to one aspect force changes to others
3. When testing becomes difficult due to multiple concerns
4. When the class grows too large or complex
5. When business logic mixes with infrastructure concerns
