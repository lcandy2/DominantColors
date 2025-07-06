# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

DominantColors is a Swift Package library for extracting dominant colors from images. It supports multiple platforms (iOS, macOS, tvOS, watchOS, visionOS) and provides various algorithms for color analysis.

## Essential Commands

### Build and Test
```bash
# Build the library
swift build

# Run all tests
swift test

# Build for release
swift build -c release

# Run tests for a specific test
swift test --filter <TestClassName>/<testMethodName>

# Clean build artifacts
swift package clean
```

### Development with Xcode
```bash
# Open the workspace
open DominantColors.xcworkspace

# Generate Xcode project if needed
swift package generate-xcodeproj
```

## Architecture Overview

### Core Components

1. **Main Algorithm Classes** (Sources/DominantColors/)
   - `DominantColors.swift`: Main class with configuration enums
   - `DominantColors+CIE.swift`: CIE color space algorithm
   - `DominantColors+CIKMeans.swift`: K-means clustering algorithm
   - `DominantColors+CIAreaAverage.swift`: Area average algorithm

2. **Color Models** (Sources/DominantColors/Colors/)
   - RGB, HSL, Lab, XYZ color spaces
   - Color conversion utilities
   - Delta E formulas for color difference calculations

3. **Platform Extensions**
   - `DominantColors+UIColor.swift`: iOS/tvOS integration
   - `DominantColors+NSColor.swift`: macOS integration
   - Image extensions for each platform

### Key Design Patterns

- **Algorithm Selection**: Uses `DominantColorAlgorithm` enum to switch between different extraction methods
- **Quality Levels**: `DominantColorQuality` enum (low, fair, high, best) controls sampling density
- **Sorting Options**: `DominantColors.Sort` enum for organizing extracted colors
- **Cross-Platform**: Conditional compilation for platform-specific code

### Testing Structure

Tests are in `Tests/DominantColorsTests/` with:
- Algorithm-specific tests for each extraction method
- Color model conversion tests
- Platform-specific image handling tests
- Test images in `Media.xcassets`

### API Usage Patterns

```swift
// Basic usage
let dominantColors = try DominantColors.dominantColors(uiImage: image, count: 6)

// With options
let colors = try DominantColors.dominantColors(
    uiImage: image,
    quality: .high,
    algorithm: .kMeansClustering,
    count: 8,
    options: [.excludeBlack, .excludeWhite, .excludeGray],
    sorting: .frequency
)
```

## Important Implementation Notes

- The library processes images by sampling pixels based on quality settings
- Color extraction algorithms work in different color spaces (RGB, CIE Lab)
- Contrast color selection uses WCAG contrast ratio calculations
- Performance varies by algorithm: area average is fastest, k-means is most accurate