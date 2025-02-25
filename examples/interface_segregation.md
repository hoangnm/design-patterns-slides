# Interface Segregation Principle Examples

This example demonstrates good and bad implementations of the Interface Segregation Principle (ISP) using a printer system.

## Bad Example - Violating ISP

```java
// Fat interface that forces clients to depend on methods they don't use
public interface MultiFunctionPrinter {
    void print(Document document);
    void scan(Document document);
    void fax(Document document);
    void copy(Document document);
    void staple(Document document);
    void bindDocument(Document document);
    void emailDocument(Document document, String recipient);
}

// Basic printer forced to implement unnecessary methods
public class BasicPrinter implements MultiFunctionPrinter {
    @Override
    public void print(Document document) {
        // Basic printing implementation
    }

    @Override
    public void scan(Document document) {
        throw new UnsupportedOperationException("Basic printer cannot scan");
    }

    @Override
    public void fax(Document document) {
        throw new UnsupportedOperationException("Basic printer cannot fax");
    }

    @Override
    public void copy(Document document) {
        throw new UnsupportedOperationException("Basic printer cannot copy");
    }

    @Override
    public void staple(Document document) {
        throw new UnsupportedOperationException("Basic printer cannot staple");
    }

    @Override
    public void bindDocument(Document document) {
        throw new UnsupportedOperationException("Basic printer cannot bind");
    }

    @Override
    public void emailDocument(Document document, String recipient) {
        throw new UnsupportedOperationException("Basic printer cannot email");
    }
}
```

Problems with this implementation:
1. Classes forced to implement methods they don't use
2. Interface too large and unfocused
3. Changes to interface affect all implementers
4. Difficult to maintain and extend
5. Violates Single Responsibility Principle

## Good Example - Following ISP

```java
// Segregated interfaces for different capabilities
public interface Printer {
    void print(Document document);
}

public interface Scanner {
    void scan(Document document);
}

public interface Fax {
    void fax(Document document);
}

public interface EmailSender {
    void emailDocument(Document document, String recipient);
}

public interface DocumentFinisher {
    void staple(Document document);
    void bindDocument(Document document);
}

// Basic printer only implements what it needs
public class BasicPrinter implements Printer {
    @Override
    public void print(Document document) {
        // Basic printing implementation
    }
}

// Advanced printer implements multiple interfaces
public class AdvancedPrinter implements Printer, Scanner, Fax, EmailSender {
    @Override
    public void print(Document document) {
        // Printing implementation
    }

    @Override
    public void scan(Document document) {
        // Scanning implementation
    }

    @Override
    public void fax(Document document) {
        // Fax implementation
    }

    @Override
    public void emailDocument(Document document, String recipient) {
        // Email implementation
    }
}

// Professional printer implements all features
public class ProfessionalPrinter 
    implements Printer, Scanner, Fax, EmailSender, DocumentFinisher {
    // Implements all methods...
}

// Client code only depends on the interfaces it needs
public class PrinterClient {
    private final Printer printer;
    
    public PrinterClient(Printer printer) {
        this.printer = printer;
    }
    
    public void printDocument(Document document) {
        printer.print(document);
    }
}

public class ScannerClient {
    private final Scanner scanner;
    
    public ScannerClient(Scanner scanner) {
        this.scanner = scanner;
    }
    
    public void scanDocument(Document document) {
        scanner.scan(document);
    }
}
```

Benefits of this implementation:
1. Clients only depend on interfaces they use
2. Classes implement only needed methods
3. Interfaces are focused and cohesive
4. Easy to extend and maintain
5. Better separation of concerns

## Key ISP Principles Demonstrated:

1. **Interface Cohesion**:
   - Each interface serves a specific purpose
   - Interfaces are focused and minimal

2. **Client Focus**:
   - Clients depend only on methods they use
   - No forced implementation of unused methods

3. **Flexibility**:
   - Easy to mix and match capabilities
   - Simple to add new features

When to Apply ISP:
1. When interfaces are becoming too large
2. When clients are forced to depend on methods they don't use
3. When different clients need different subsets of functionality
4. When you need flexible composition of capabilities
5. When implementing optional features

Remember: ISP suggests that clients should not be forced to depend on interfaces they don't use. Breaking interfaces into smaller, focused ones allows for more flexible and maintainable code.
