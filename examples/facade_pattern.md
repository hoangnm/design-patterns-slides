# Facade Pattern Example

## Problem: Complex E-commerce Order Processing System

Imagine you have a complex e-commerce system with multiple subsystems handling different aspects of order processing:
- Inventory checking
- Payment processing
- Shipping calculation
- Customer notification
- Order tracking
- Warehouse management

### The Problem Without Facade

```java
// Client code dealing directly with multiple subsystems - Complex and Messy
public class OrderProcessor {
    private InventorySystem inventorySystem;
    private PaymentGateway paymentGateway;
    private ShippingCalculator shippingCalculator;
    private NotificationService notificationService;
    private WarehouseSystem warehouseSystem;
    private OrderTracker orderTracker;

    public void processOrder(Order order) {
        // Client needs to know about all subsystems and their operations
        if (inventorySystem.checkStock(order.getItems())) {
            double shippingCost = shippingCalculator.calculateShipping(
                order.getItems(), 
                order.getShippingAddress()
            );
            
            PaymentDetails payment = new PaymentDetails(
                order.getTotalAmount() + shippingCost,
                order.getPaymentMethod()
            );
            
            if (paymentGateway.processPayment(payment)) {
                PickingSlip pickingSlip = warehouseSystem.generatePickingSlip(order);
                warehouseSystem.schedulePickup(pickingSlip);
                
                TrackingNumber trackingNum = orderTracker.createTracking(order);
                
                notificationService.sendConfirmation(
                    order.getCustomerEmail(),
                    order,
                    trackingNum
                );
            }
        }
    }
}
```

### Solution Using Facade Pattern

```java
// Facade provides simple interface to complex subsystems
public class OrderProcessingFacade {
    private InventorySystem inventorySystem;
    private PaymentGateway paymentGateway;
    private ShippingCalculator shippingCalculator;
    private NotificationService notificationService;
    private WarehouseSystem warehouseSystem;
    private OrderTracker orderTracker;

    public OrderResult processOrder(Order order) {
        try {
            // Facade handles all the complexity internally
            validateInventory(order);
            double totalCost = calculateTotalCost(order);
            processPayment(order, totalCost);
            TrackingNumber tracking = initiateShipping(order);
            notifyCustomer(order, tracking);
            
            return new OrderResult(
                OrderStatus.PROCESSED,
                tracking,
                totalCost
            );
        } catch (OrderProcessingException e) {
            return new OrderResult(
                OrderStatus.FAILED,
                null,
                0,
                e.getMessage()
            );
        }
    }

    private void validateInventory(Order order) {
        if (!inventorySystem.checkStock(order.getItems())) {
            throw new OrderProcessingException("Insufficient inventory");
        }
    }

    private double calculateTotalCost(Order order) {
        double shippingCost = shippingCalculator.calculateShipping(
            order.getItems(),
            order.getShippingAddress()
        );
        return order.getTotalAmount() + shippingCost;
    }

    private void processPayment(Order order, double totalCost) {
        PaymentDetails payment = new PaymentDetails(
            totalCost,
            order.getPaymentMethod()
        );
        
        if (!paymentGateway.processPayment(payment)) {
            throw new OrderProcessingException("Payment failed");
        }
    }

    private TrackingNumber initiateShipping(Order order) {
        PickingSlip pickingSlip = warehouseSystem.generatePickingSlip(order);
        warehouseSystem.schedulePickup(pickingSlip);
        return orderTracker.createTracking(order);
    }

    private void notifyCustomer(Order order, TrackingNumber tracking) {
        notificationService.sendConfirmation(
            order.getCustomerEmail(),
            order,
            tracking
        );
    }
}

// Client code - Simple and Clean
public class OrderProcessor {
    private OrderProcessingFacade orderProcessingFacade;

    public void processOrder(Order order) {
        OrderResult result = orderProcessingFacade.processOrder(order);
        if (result.getStatus() == OrderStatus.FAILED) {
            handleFailure(result);
        }
    }
}
```

## Benefits of Using Facade Pattern Here:

1. **Simplifies Complex Interface**:
   - Hides complexity of multiple subsystems
   - Provides single entry point
   - Reduces coupling between client and subsystems

2. **Better Error Handling**:
   - Centralizes error handling
   - Provides consistent error reporting
   - Easier to implement retry logic

3. **Improved Maintainability**:
   - Changes to subsystems don't affect client code
   - Easy to modify internal implementation
   - Better organization of subsystem interactions

4. **Enhanced Testability**:
   - Can mock facade instead of multiple subsystems
   - Easier to write integration tests
   - Simplified test scenarios

## When to Use This Pattern:

- Complex system with multiple subsystems
- Need to provide simple interface to complex system
- Want to layer your subsystems
- Need to decouple client code from subsystems
- Want to reduce dependencies between client and subsystems

## Common Use Cases:

1. E-commerce Systems
2. Financial Systems
3. Library Management Systems
4. Enterprise Integration
5. API Gateways
