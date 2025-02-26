# Encapsulation in Object-Oriented Programming

Encapsulation is one of the four fundamental principles of object-oriented programming (OOP). It refers to the bundling of data and methods that operate on that data within a single unit (class), and restricting access to some of the object's components.

## Key Concepts of Encapsulation

1. **Data Hiding**: Restricting direct access to some of an object's components
2. **Access Control**: Using access modifiers to control the visibility of class members
3. **Getters and Setters**: Providing controlled access to private data
4. **Implementation Hiding**: Hiding the internal implementation details from the outside world

## Example in Java

```java
public class BankAccount {
    // Private fields - data is hidden from outside access
    private String accountNumber;
    private String accountHolder;
    private double balance;
    private String pin;
    
    // Constructor
    public BankAccount(String accountNumber, String accountHolder, String pin) {
        this.accountNumber = accountNumber;
        this.accountHolder = accountHolder;
        this.pin = pin;
        this.balance = 0.0;
    }
    
    // Public getters - controlled access to read data
    public String getAccountNumber() {
        return accountNumber;
    }
    
    public String getAccountHolder() {
        return accountHolder;
    }
    
    public double getBalance() {
        return balance;
    }
    
    // No getter for PIN - sensitive data is completely hidden
    
    // Public setters with validation - controlled access to modify data
    public void setAccountHolder(String accountHolder) {
        if (accountHolder != null && !accountHolder.isEmpty()) {
            this.accountHolder = accountHolder;
        }
    }
    
    // Public methods that operate on the encapsulated data
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println(amount + " deposited. New balance: " + balance);
        } else {
            System.out.println("Invalid deposit amount.");
        }
    }
    
    public boolean withdraw(double amount, String providedPin) {
        // Validate PIN before allowing withdrawal
        if (!this.pin.equals(providedPin)) {
            System.out.println("Incorrect PIN. Transaction denied.");
            return false;
        }
        
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println(amount + " withdrawn. New balance: " + balance);
            return true;
        } else {
            System.out.println("Invalid withdrawal amount or insufficient funds.");
            return false;
        }
    }
}

// Client code
public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount("123456789", "John Doe", "1234");
        
        // Access public methods and getters
        System.out.println("Account Holder: " + account.getAccountHolder());
        System.out.println("Account Number: " + account.getAccountNumber());
        System.out.println("Initial Balance: " + account.getBalance());
        
        // Deposit and withdraw using public methods
        account.deposit(1000);
        account.withdraw(500, "1234");
        
        // Try to withdraw with incorrect PIN
        account.withdraw(200, "4321");
        
        // Cannot directly access or modify private fields
        // account.balance = 1000000;  // Compilation error
        // account.pin = "0000";       // Compilation error
    }
}
```

## Benefits of Encapsulation

1. **Data Protection**: Prevents direct modification of attributes, ensuring data integrity
2. **Flexibility**: Implementation details can change without affecting client code
3. **Maintainability**: Easier to maintain and debug code
4. **Controlled Access**: Provides controlled access to data through methods
5. **Validation**: Allows for validation of data before it's modified

## Real-World Analogy

Think of encapsulation like a car's engine compartment:
- The engine is hidden under the hood (private data)
- You interact with the engine through the dashboard and controls (public methods)
- You don't need to understand how the engine works to drive the car (implementation hiding)
- The car prevents you from doing dangerous operations (data protection)
