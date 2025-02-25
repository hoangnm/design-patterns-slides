# Proxy Pattern

## Real-Life Problem

Imagine you're building a simple document viewer application. Opening and loading document files can be resource-intensive, especially for large files. Additionally, some documents might require special permissions to access.

If you load all documents immediately when the application starts, it would be slow and waste memory. You need a way to:
1. Load documents only when the user actually wants to view them
2. Check if the user has permission to view a document before loading it
3. Keep track of which documents are being accessed

How can you implement these requirements without changing your existing document handling code?

## Solution

The Proxy pattern provides a surrogate or placeholder for another object to control access to it. It's useful for:
- Lazy initialization (virtual proxy)
- Access control (protection proxy)
- Logging/monitoring access (logging proxy)
- Caching results (caching proxy)
- Remote resource access (remote proxy)

## Example

In this example, we'll implement a solution to our document viewer problem:

```java
// Subject interface - common interface for RealDocument and DocumentProxy
interface Document {
    void display();
}

// Real Subject - the actual document loading and displaying
class RealDocument implements Document {
    private String filename;
    
    public RealDocument(String filename) {
        this.filename = filename;
        loadDocument();
    }
    
    private void loadDocument() {
        System.out.println("Loading document: " + filename);
        // Heavy document loading operation would happen here
    }
    
    @Override
    public void display() {
        System.out.println("Displaying document: " + filename);
    }
}

// Proxy - controls access to the RealDocument
class DocumentProxy implements Document {
    private RealDocument realDocument;
    private String filename;
    private boolean hasAccess;
    
    public DocumentProxy(String filename, boolean hasAccess) {
        this.filename = filename;
        this.hasAccess = hasAccess;
    }
    
    @Override
    public void display() {
        // Check access before loading document
        if (hasAccess) {
            // Lazy initialization - only create when needed
            if (realDocument == null) {
                realDocument = new RealDocument(filename);
            }
            realDocument.display();
            System.out.println("DocumentProxy: Access to " + filename + " recorded");
        } else {
            System.out.println("DocumentProxy: Access denied to " + filename);
        }
    }
}

// Client code
class DocumentViewer {
    public static void main(String[] args) {
        // Create document proxies (documents aren't loaded yet)
        Document[] documents = new Document[3];
        documents[0] = new DocumentProxy("report.pdf", true);
        documents[1] = new DocumentProxy("confidential.pdf", false);
        documents[2] = new DocumentProxy("manual.pdf", true);
        
        System.out.println("User opening document viewer application...");
        
        // User clicks on first document - loads and displays
        documents[0].display();
        
        // User tries to open confidential document - access denied
        documents[1].display();
        
        // User opens another permitted document
        documents[2].display();
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

