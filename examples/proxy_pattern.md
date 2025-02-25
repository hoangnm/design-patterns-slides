# Proxy Pattern

The Proxy pattern provides a surrogate or placeholder for another object to control access to it. It's useful for:
- Lazy initialization (virtual proxy)
- Access control (protection proxy)
- Logging/monitoring access (logging proxy)
- Caching results (caching proxy)
- Remote resource access (remote proxy)

## Example

```java
// Subject interface
interface Subject {
    void request();
}

// Real Subject
class RealSubject implements Subject {
    @Override
    public void request() {
        System.out.println("RealSubject: Handling request.");
        // Potentially expensive operation
    }
}

// Proxy
class Proxy implements Subject {
    private RealSubject realSubject;
    private boolean isAccessGranted = false;
    
    public Proxy(RealSubject realSubject) {
        this.realSubject = realSubject;
    }
    
    private boolean checkAccess() {
        System.out.println("Proxy: Checking access prior to firing a real request.");
        isAccessGranted = true;
        return isAccessGranted;
    }
    
    private void logAccess() {
        System.out.println("Proxy: Logging the time of request.");
    }
    
    @Override
    public void request() {
        if (checkAccess()) {
            realSubject.request();
            logAccess();
        } else {
            System.out.println("Proxy: Access denied.");
        }
    }
}

// Client code
class Client {
    public static void main(String[] args) {
        System.out.println("Client: Executing the client code with a real subject:");
        RealSubject realSubject = new RealSubject();
        clientCode(realSubject);
        
        System.out.println("\nClient: Executing the same client code with a proxy:");
        Proxy proxy = new Proxy(realSubject);
        clientCode(proxy);
    }
    
    private static void clientCode(Subject subject) {
        subject.request();
    }
}
```

## When to Use

Use the Proxy pattern when:

1. **Lazy initialization (Virtual Proxy)**: When an object is expensive to create, and you want to defer its creation until it's actually needed.

2. **Access Control (Protection Proxy)**: When you want to control access to an object based on access rights.

3. **Local execution of a remote service (Remote Proxy)**: When the real object is in a different address space, and the proxy represents it locally (like Java RMI).

4. **Logging requests (Logging Proxy)**: When you want to keep a history of requests to the service object.

5. **Caching request results (Caching Proxy)**: When you need to cache results of client requests and manage the life cycle of this cache.

## Benefits

- You can control the service object without clients knowing about it
- You can manage the lifecycle of the service object when clients don't care about it
- The proxy works even when the service object isn't ready or is not available
- Open/Closed Principle: You can introduce new proxies without changing the service or clients

