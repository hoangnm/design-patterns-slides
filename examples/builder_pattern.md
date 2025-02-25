# Builder Pattern Example

## Problem: Complex Document Generation System

Imagine you need to create a system that generates different types of documents (PDF, HTML, Word) with varying content structures like:
- Headers
- Footers 
- Main content sections
- Tables
- Images
- Metadata

### The Problem with Direct Construction

```java
// Without Builder Pattern - Messy and Error-prone
public class Document {
    private String header;
    private String footer;
    private List<String> sections;
    private List<Table> tables;
    private Map<String, Image> images;
    private DocumentMetadata metadata;
    private String format;

    // Complex constructor - hard to use correctly
    public Document(String header, String footer, List<String> sections, 
                   List<Table> tables, Map<String, Image> images,
                   DocumentMetadata metadata, String format) {
        this.header = header;
        this.footer = footer;
        this.sections = sections;
        this.tables = tables;
        this.images = images;
        this.metadata = metadata;
        this.format = format;
    }

    // Alternative - many setter methods
    // Risk of incomplete object state
    public void setHeader(String header) { this.header = header; }
    public void setFooter(String footer) { this.footer = footer; }
    // ... many more setters
}

// Usage - Problematic
Document doc = new Document(
    "Header",
    "Footer",
    Arrays.asList("Section1", "Section2"),
    new ArrayList<>(),  // empty tables
    new HashMap<>(),    // empty images
    new DocumentMetadata(),
    "PDF"
);
```

### Solution Using Builder Pattern

```java
// Clean solution with Builder Pattern
public class Document {
    private final String header;
    private final String footer;
    private final List<String> sections;
    private final List<Table> tables;
    private final Map<String, Image> images;
    private final DocumentMetadata metadata;
    private final String format;

    private Document(DocumentBuilder builder) {
        this.header = builder.header;
        this.footer = builder.footer;
        this.sections = builder.sections;
        this.tables = builder.tables;
        this.images = builder.images;
        this.metadata = builder.metadata;
        this.format = builder.format;
    }

    public static class DocumentBuilder {
        private String header;
        private String footer;
        private List<String> sections = new ArrayList<>();
        private List<Table> tables = new ArrayList<>();
        private Map<String, Image> images = new HashMap<>();
        private DocumentMetadata metadata = new DocumentMetadata();
        private String format = "PDF";

        public DocumentBuilder header(String header) {
            this.header = header;
            return this;
        }

        public DocumentBuilder footer(String footer) {
            this.footer = footer;
            return this;
        }

        public DocumentBuilder addSection(String section) {
            this.sections.add(section);
            return this;
        }

        public DocumentBuilder addTable(Table table) {
            this.tables.add(table);
            return this;
        }

        public DocumentBuilder addImage(String key, Image image) {
            this.images.put(key, image);
            return this;
        }

        public DocumentBuilder metadata(DocumentMetadata metadata) {
            this.metadata = metadata;
            return this;
        }

        public DocumentBuilder format(String format) {
            this.format = format;
            return this;
        }

        public Document build() {
            return new Document(this);
        }
    }
}

// Usage - Clean and Clear
Document document = new Document.DocumentBuilder()
    .header("Annual Report 2024")
    .addSection("Executive Summary")
    .addSection("Financial Results")
    .addTable(financialTable)
    .addImage("chart1", revenueChart)
    .footer("Page 1 of 10")
    .format("PDF")
    .build();
```

## Benefits of Using Builder Pattern Here:

1. **Handles Complex Construction**:
   - Many optional parameters
   - Different combinations of components
   - Step-by-step construction

2. **Improves Readability**:
   - Clear what each value represents
   - Method chaining makes construction flow obvious
   - Self-documenting code

3. **Ensures Object Validity**:
   - Object is immutable once constructed
   - All validation can be done in build() method
   - No partially-initialized objects

4. **Flexible Construction**:
   - Easy to add new optional parameters
   - Can have different builders for different formats
   - Can reuse builder for similar documents

## When to Use This Pattern:

- Complex objects with many optional parameters
- Need for immutable objects with lots of attributes
- Step-by-step construction of objects
- Need to enforce specific construction order
- Want to prevent invalid object states

## Common Use Cases:

1. Document Generation Systems
2. Configuration Objects
3. Network Request Builders
4. UI Component Builders
5. Test Data Builders
