# Open/Closed Principle Examples

This example demonstrates good and bad implementations of the Open/Closed Principle (OCP) using a document processing system.

## Bad Example - Violating OCP

```java
// BAD: Class needs modification for each new document type
public class DocumentProcessor {
    public void processDocument(String documentType, String content) {
        if (documentType.equals("PDF")) {
            // PDF specific processing
            validatePdfFormat(content);
            extractPdfText(content);
            savePdfMetadata(content);
        } 
        else if (documentType.equals("WORD")) {
            // Word specific processing
            validateWordFormat(content);
            extractWordText(content);
            saveWordMetadata(content);
        }
        // Need to modify this class for new document types
    }
}
```

Problems with this implementation:
1. Must modify existing code to add new document types
2. Violates OCP as class is not closed for modification
3. Risk of breaking existing document processing
4. Growing if-else statements
5. Duplicate processing logic

## Good Example - Following OCP

```java
// GOOD: Using abstract class and template method pattern
public abstract class Document {
    // Template method defining the algorithm structure
    public final void process() {
        validate();
        extractText();
        saveMetadata();
        customProcessing();
    }

    // Common validation logic
    protected void validate() {
        if (getContent().isEmpty()) {
            throw new IllegalArgumentException("Empty document");
        }
    }

    // Abstract methods that must be implemented by subclasses
    protected abstract String extractText();
    protected abstract void saveMetadata();

    // Hook method - optional custom processing
    protected void customProcessing() {
        // Default empty implementation
    }

    // Getter for document content
    protected abstract String getContent();
}

// PDF document implementation
public class PdfDocument extends Document {
    private String pdfContent;

    public PdfDocument(String content) {
        this.pdfContent = content;
    }

    @Override
    protected String extractText() {
        // PDF specific text extraction
        return "Extracted PDF text: " + pdfContent;
    }

    @Override
    protected void saveMetadata() {
        // PDF specific metadata handling
        System.out.println("Saving PDF metadata");
    }

    @Override
    protected String getContent() {
        return pdfContent;
    }
}

// Word document implementation
public class WordDocument extends Document {
    private String wordContent;

    public WordDocument(String content) {
        this.wordContent = content;
    }

    @Override
    protected String extractText() {
        // Word specific text extraction
        return "Extracted Word text: " + wordContent;
    }

    @Override
    protected void saveMetadata() {
        // Word specific metadata handling
        System.out.println("Saving Word metadata");
    }

    @Override
    protected String getContent() {
        return wordContent;
    }

    @Override
    protected void customProcessing() {
        // Word specific additional processing
        System.out.println("Performing Word-specific processing");
    }
}

// Main processor is closed for modification
public class DocumentProcessor {
    public void processDocument(Document document) {
        document.process();
    }
}
```

Benefits of this implementation:
1. **Extensible**: New document types can be added without modifying existing code
2. **Template Method Pattern**: Common processing structure defined in abstract class
3. **Code Reuse**: Common functionality in base class
4. **Customizable**: Hook methods for optional processing
5. **Type-Safe**: Compiler ensures required methods are implemented

## Usage Example

```java
public class DocumentProcessingApp {
    private DocumentProcessor processor = new DocumentProcessor();

    public void processDocuments() {
        Document pdfDoc = new PdfDocument("PDF content");
        Document wordDoc = new WordDocument("Word content");

        processor.processDocument(pdfDoc);   // Uses default processing
        processor.processDocument(wordDoc);  // Includes custom processing
    }
}
```

When to apply OCP with Abstract Classes:
1. When you have a fixed algorithm structure with varying steps
2. When common functionality can be shared across implementations
3. When you want to enforce a specific processing sequence
4. When some processing steps are optional (hook methods)

Remember: The Open/Closed Principle using abstract classes provides a powerful way to define a common structure while allowing for customization in specific implementations.
