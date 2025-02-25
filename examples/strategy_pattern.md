# Strategy Pattern Example

## Problem: Payment Processing System

Imagine you're building an e-commerce platform that needs to handle multiple payment methods (Credit Card, PayPal, Cryptocurrency, etc.) with different processing logic for each method.

### The Problem Without Strategy Pattern

```java
// Problematic approach with conditional logic
public class PaymentProcessor {
    public void processPayment(Order order, String paymentMethod) {
        // Complex conditional logic
        if (paymentMethod.equals("CREDIT_CARD")) {
            // Credit card specific validation
            validateCard(order.getCardNumber());
            validateCVV(order.getCvv());
            validateExpiry(order.getExpiryDate());
            
            // Credit card specific processing
            connectToCardNetwork();
            processCardPayment(order);
            saveCardTransaction();
        } 
        else if (paymentMethod.equals("PAYPAL")) {
            // PayPal specific logic
            validatePayPalAccount(order.getPayPalEmail());
            connectToPayPal();
            processPayPalPayment(order);
            savePayPalTransaction();
        }
        else if (paymentMethod.equals("CRYPTO")) {
            // Cryptocurrency specific logic
            validateWalletAddress(order.getWalletAddress());
            connectToCryptoNetwork();
            processCryptoPayment(order);
            saveCryptoTransaction();
        }
        // Adding new payment method requires modifying this class
    }
}
```

### Problems with this Approach:
1. Violates Open/Closed Principle
2. Hard to maintain and extend
3. Complex conditional logic
4. Class has multiple reasons to change
5. Testing becomes complicated

### Solution Using Strategy Pattern

```java
// Payment Strategy Interface
public interface PaymentStrategy {
    void processPayment(Order order);
    boolean validatePayment(Order order);
}

// Credit Card Strategy
public class CreditCardStrategy implements PaymentStrategy {
    @Override
    public void processPayment(Order order) {
        validatePayment(order);
        connectToCardNetwork();
        processCardPayment(order);
        saveCardTransaction();
    }
    
    @Override
    public boolean validatePayment(Order order) {
        return validateCard(order.getCardNumber()) &&
               validateCVV(order.getCvv()) &&
               validateExpiry(order.getExpiryDate());
    }
}

// PayPal Strategy
public class PayPalStrategy implements PaymentStrategy {
    @Override
    public void processPayment(Order order) {
        validatePayment(order);
        connectToPayPal();
        processPayPalPayment(order);
        savePayPalTransaction();
    }
    
    @Override
    public boolean validatePayment(Order order) {
        return validatePayPalAccount(order.getPayPalEmail());
    }
}

// Cryptocurrency Strategy
public class CryptoStrategy implements PaymentStrategy {
    @Override
    public void processPayment(Order order) {
        validatePayment(order);
        connectToCryptoNetwork();
        processCryptoPayment(order);
        saveCryptoTransaction();
    }
    
    @Override
    public boolean validatePayment(Order order) {
        return validateWalletAddress(order.getWalletAddress());
    }
}

// Context class that uses the strategy
public class PaymentProcessor {
    private PaymentStrategy paymentStrategy;
    
    public void setPaymentStrategy(PaymentStrategy strategy) {
        this.paymentStrategy = strategy;
    }
    
    public void processPayment(Order order) {
        paymentStrategy.processPayment(order);
    }
}

// Usage
public class CheckoutService {
    private PaymentProcessor paymentProcessor = new PaymentProcessor();
    
    public void checkout(Order order) {
        // Select strategy based on payment method
        PaymentStrategy strategy = switch (order.getPaymentMethod()) {
            case CREDIT_CARD -> new CreditCardStrategy();
            case PAYPAL -> new PayPalStrategy();
            case CRYPTO -> new CryptoStrategy();
        };
        
        paymentProcessor.setPaymentStrategy(strategy);
        paymentProcessor.processPayment(order);
    }
}
```

## Benefits of Using Strategy Pattern Here:

1. **Flexible Payment Processing**:
   - Easy to add new payment methods
   - Each strategy encapsulates its own logic
   - No modification to existing code when adding new methods

2. **Clean Code Structure**:
   - Each strategy is isolated
   - No conditional logic in processor
   - Clear separation of concerns

3. **Easy Testing**:
   - Each strategy can be tested independently
   - Mock strategies for testing
   - Better test coverage

4. **Runtime Flexibility**:
   - Can switch strategies dynamically
   - Easy to configure different payment methods
   - Support for multiple payment methods

## When to Use This Pattern:

- Multiple algorithms for a specific task
- Need to switch algorithms at runtime
- Want to isolate algorithm logic
- Need to avoid complex conditional logic
- Want to make algorithm selection flexible

## Common Use Cases:

1. Payment Processing Systems
2. Sorting Algorithms
3. Compression Algorithms
4. Authentication Strategies
5. Tax Calculation Methods
