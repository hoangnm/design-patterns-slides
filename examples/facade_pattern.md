# Facade Pattern Example

## Problem: Complex Media Conversion System

Imagine you're building a multimedia processing system that needs to:
- Convert video formats
- Extract audio
- Generate thumbnails
- Apply filters
- Add watermarks
- Handle metadata

### The Problem Without Facade

```java
// Client code dealing directly with complex media operations
public class MediaConverter {
    private VideoProcessor videoProcessor;
    private AudioExtractor audioExtractor;
    private ThumbnailGenerator thumbnailGenerator;
    private FilterApplier filterApplier;
    private WatermarkApplier watermarkApplier;
    private MetadataHandler metadataHandler;

    public void convertVideo(String inputPath, String outputPath, String format) {
        // Client needs to understand all the technical details and steps
        VideoMetadata metadata = metadataHandler.extractMetadata(inputPath);
        
        // Video processing setup
        VideoSettings settings = new VideoSettings(
            metadata.getCodec(),
            metadata.getBitrate(),
            metadata.getResolution()
        );
        
        // Complex conversion process
        byte[] videoData = videoProcessor.readVideo(inputPath);
        byte[] convertedVideo = videoProcessor.convert(videoData, format, settings);
        
        // Audio handling
        byte[] audioData = audioExtractor.extract(videoData);
        byte[] convertedAudio = audioExtractor.convert(audioData, format);
        
        // Generate preview
        byte[] thumbnail = thumbnailGenerator.generate(videoData);
        
        // Apply effects
        convertedVideo = filterApplier.apply(convertedVideo, "enhance");
        convertedVideo = watermarkApplier.apply(convertedVideo, "Copyright 2024");
        
        // Save everything
        videoProcessor.save(convertedVideo, outputPath);
        thumbnailGenerator.save(thumbnail, outputPath + "_thumb.jpg");
    }
}
```

### Solution Using Facade Pattern

```java
// Simple interface for complex media operations
public class MediaConversionFacade {
    private VideoProcessor videoProcessor;
    private AudioExtractor audioExtractor;
    private ThumbnailGenerator thumbnailGenerator;
    private FilterApplier filterApplier;
    private WatermarkApplier watermarkApplier;
    private MetadataHandler metadataHandler;

    public MediaConversionResult convertMedia(
        String inputPath, 
        MediaConversionOptions options
    ) {
        try {
            // Facade handles all complexity internally
            VideoMetadata metadata = extractMetadata(inputPath);
            byte[] convertedVideo = processVideo(inputPath, metadata, options);
            byte[] thumbnail = generateThumbnail(convertedVideo, options);
            
            saveResults(convertedVideo, thumbnail, options.getOutputPath());
            
            return new MediaConversionResult(
                options.getOutputPath(),
                metadata,
                options.getFormat()
            );
        } catch (ConversionException e) {
            return MediaConversionResult.failure(e.getMessage());
        }
    }

    private VideoMetadata extractMetadata(String inputPath) {
        return metadataHandler.extractMetadata(inputPath);
    }

    private byte[] processVideo(
        String inputPath, 
        VideoMetadata metadata,
        MediaConversionOptions options
    ) {
        VideoSettings settings = createVideoSettings(metadata, options);
        byte[] videoData = videoProcessor.readVideo(inputPath);
        byte[] convertedVideo = videoProcessor.convert(
            videoData, 
            options.getFormat(), 
            settings
        );

        if (options.isEnhanceVideo()) {
            convertedVideo = filterApplier.apply(convertedVideo, "enhance");
        }

        if (options.getWatermarkText() != null) {
            convertedVideo = watermarkApplier.apply(
                convertedVideo, 
                options.getWatermarkText()
            );
        }

        return convertedVideo;
    }

    private byte[] generateThumbnail(
        byte[] videoData, 
        MediaConversionOptions options
    ) {
        if (!options.isGenerateThumbnail()) {
            return null;
        }
        return thumbnailGenerator.generate(videoData);
    }

    private void saveResults(
        byte[] video, 
        byte[] thumbnail, 
        String outputPath
    ) {
        videoProcessor.save(video, outputPath);
        if (thumbnail != null) {
            thumbnailGenerator.save(thumbnail, outputPath + "_thumb.jpg");
        }
    }
}

// Clean client code
public class VideoConverter {
    private MediaConversionFacade conversionFacade;

    public void convertVideo(String inputPath, String format) {
        MediaConversionOptions options = new MediaConversionOptions.Builder()
            .setFormat(format)
            .setEnhanceVideo(true)
            .setGenerateThumbnail(true)
            .setWatermarkText("Copyright 2024")
            .build();

        MediaConversionResult result = conversionFacade.convertMedia(
            inputPath, 
            options
        );

        if (!result.isSuccess()) {
            handleError(result.getError());
        }
    }
}
```

## Benefits of Using Facade Pattern Here:

1. **Simplifies Complex Media Processing**:
   - Hides technical details of video/audio processing
   - Provides simple interface for complex operations
   - Manages multiple processing steps internally

2. **Better Configuration Management**:
   - Options builder pattern for settings
   - Default values handled by facade
   - Flexible configuration without complexity

3. **Improved Error Handling**:
   - Centralized error management
   - Consistent error reporting
   - Clean error recovery options

4. **Enhanced Maintainability**:
   - Each subsystem can be modified independently
   - Easy to add new processing features
   - Clear separation of concerns

## When to Use This Pattern:

- Multiple processing steps needed
- Various configuration options
- Need to hide technical complexity
- Want to provide simple interface

