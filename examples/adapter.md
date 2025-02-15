# Bad Code Example - Without Adapter Pattern

```java
// Legacy interface that existing code depends on
interface LegacyImageStorage {
    void save(String filename, byte[] data);
    byte[] load(String filename);
}

// Legacy implementation
class OldImageStorage implements LegacyImageStorage {
    @Override
    public void save(String filename, byte[] data) {
        System.out.println("Saving image using old storage system: " + filename);
    }
    
    @Override
    public byte[] load(String filename) {
        System.out.println("Loading image using old storage system: " + filename);
        return new byte[100];
    }
}

// New modern library with different interface
class ModernCloudStorage {
    public void store(String path, CloudImage image) {
        System.out.println("Storing image to cloud: " + path);
    }
    
    public CloudImage retrieve(String path) {
        System.out.println("Retrieving image from cloud: " + path);
        return new CloudImage();
    }
}

class CloudImage {
    private byte[] data;
    
    public CloudImage() {
        this.data = new byte[100];
    }
    
    public CloudImage(byte[] data) {
        this.data = data;
    }
    
    public byte[] getData() {
        return data;
    }
}

// Bad implementation: Mixing old and new code, tight coupling
class BadImageProcessor {
    private OldImageStorage oldStorage;
    private ModernCloudStorage cloudStorage;
    private boolean useCloud;
    
    public BadImageProcessor(boolean useCloud) {
        this.useCloud = useCloud;
        this.oldStorage = new OldImageStorage();
        this.cloudStorage = new ModernCloudStorage();
    }
    
    public void processImage(String filename) {
        byte[] image;
        
        // Messy conditional logic mixing both storage systems
        if (useCloud) {
            CloudImage cloudImage = cloudStorage.retrieve(filename);
            image = cloudImage.getData();
        } else {
            image = oldStorage.load(filename);
        }
        
        // Process the image...
        
        // More messy conditional logic for saving
        if (useCloud) {
            CloudImage processedImage = new CloudImage(image);
            cloudStorage.store("processed_" + filename, processedImage);
        } else {
            oldStorage.save("processed_" + filename, image);
        }
    }
}

// Usage example showing the mess
class Main {
    public static void main(String[] args) {
        // Using old storage
        BadImageProcessor oldProcessor = new BadImageProcessor(false);
        oldProcessor.processImage("photo.jpg");
        
        // Using cloud storage
        BadImageProcessor cloudProcessor = new BadImageProcessor(true);
        cloudProcessor.processImage("photo.jpg");
    }
}
```

Problems with this code:
1. Tight coupling to both storage implementations
2. Conditional logic scattered throughout the code
3. Violation of Single Responsibility Principle
4. Hard to maintain and test
5. Difficult to add new storage systems
6. No clear separation of concerns

# Refactored Code Using Adapter Pattern

```java
// Legacy interface that existing code depends on
interface LegacyImageStorage {
    void save(String filename, byte[] data);
    byte[] load(String filename);
}

// Legacy implementation that we want to replace
class OldImageStorage implements LegacyImageStorage {
    @Override
    public void save(String filename, byte[] data) {
        System.out.println("Saving image using old storage system: " + filename);
    }
    
    @Override
    public byte[] load(String filename) {
        System.out.println("Loading image using old storage system: " + filename);
        return new byte[100];
    }
}

// New modern library with different interface
class ModernCloudStorage {
    public void store(String path, CloudImage image) {
        System.out.println("Storing image to cloud: " + path);
    }
    
    public CloudImage retrieve(String path) {
        System.out.println("Retrieving image from cloud: " + path);
        return new CloudImage();
    }
}

class CloudImage {
    private byte[] data;
    
    public CloudImage() {
        this.data = new byte[100];
    }
    
    public CloudImage(byte[] data) {
        this.data = data;
    }
    
    public byte[] getData() {
        return data;
    }
}

// Adapter that makes ModernCloudStorage work with LegacyImageStorage interface
class CloudStorageAdapter implements LegacyImageStorage {
    private final ModernCloudStorage cloudStorage;
    
    public CloudStorageAdapter(ModernCloudStorage cloudStorage) {
        this.cloudStorage = cloudStorage;
    }
    
    @Override
    public void save(String filename, byte[] data) {
        CloudImage cloudImage = new CloudImage(data);
        cloudStorage.store(filename, cloudImage);
    }
    
    @Override
    public byte[] load(String filename) {
        CloudImage cloudImage = cloudStorage.retrieve(filename);
        return cloudImage.getData();
    }
}

// Clean client code that uses the legacy interface
class ImageProcessor {
    private final LegacyImageStorage storage;
    
    // Can inject either OldImageStorage or CloudStorageAdapter
    public ImageProcessor(LegacyImageStorage storage) {
        this.storage = storage;
    }
    
    public void processImage(String filename) {
        byte[] image = storage.load(filename);
        // Process the image...
        storage.save("processed_" + filename, image);
    }
}

// Usage example showing clean implementation
class Main {
    public static void main(String[] args) {
        // Old way
        ImageProcessor oldProcessor = new ImageProcessor(new OldImageStorage());
        oldProcessor.processImage("photo.jpg");
        
        // New way using adapter
        ModernCloudStorage cloudStorage = new ModernCloudStorage();
        LegacyImageStorage adapter = new CloudStorageAdapter(cloudStorage);
        ImageProcessor newProcessor = new ImageProcessor(adapter);
        newProcessor.processImage("photo.jpg");
    }
}
```

Benefits of the refactored code:
1. Clean separation of concerns
2. No conditional logic for storage type
3. Easy to add new storage systems
4. Follows Single Responsibility Principle
5. Easy to test with mock objects
6. Existing code continues to work with legacy interface
