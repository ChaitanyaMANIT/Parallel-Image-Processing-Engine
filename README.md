# Parallel Image Processing Engine

A high-performance, multi-threaded image processing application demonstrating parallel computing concepts with a modern GUI interface.

## Overview

This project implements a **parallel image processing engine** that leverages multi-core processors to accelerate image manipulation tasks. Built with C++17, it demonstrates advanced concepts in concurrent programming, real-time GUI development, and computer vision.

## Key Features

- **Parallel Processing**: Utilizes custom thread pool architecture for concurrent image processing
- **Performance Benchmarking**: Real-time measurement of execution time and speedup metrics
- **Modern GUI**: Interactive interface built with Dear ImGui and GLFW
- **Multiple Filters**: Comprehensive set of image processing algorithms
- **Scalable Architecture**: Dynamic thread count based on hardware concurrency

## Technology Stack

| Component | Technology |
|-----------|-----------|
| Language | C++17 |
| Build System | CMake 3.20+ |
| Image Processing | OpenCV 4.x |
| GUI Framework | Dear ImGui + GLFW + OpenGL |
| Concurrency | Custom ThreadPool Implementation |
| Package Manager | vcpkg |

## Architecture

```
┌─────────────────────────────────────────┐
│              GUI Layer                  │
│    (Dear ImGui + GLFW + OpenGL)        │
└──────────────────┬──────────────────────┘
                   │
┌──────────────────▼──────────────────────┐
│         ImageProcessor                  │
│    (Orchestration & Coordination)       │
└──────────────────┬──────────────────────┘
                   │
        ┌──────────┴──────────┐
        │                     │
┌───────▼────────┐   ┌────────▼─────────┐
│   ThreadPool   │   │    Benchmark     │
│ (Parallel Exec)│   │ (Performance)    │
└───────┬────────┘   └──────────────────┘
        │
┌───────▼────────────────────────┐
│      Filters Library            │
│ (Image Processing Algorithms)  │
└────────────────────────────────┘
```

## Core Components

### 1. ThreadPool
- **Purpose**: Manages worker threads for parallel task execution
- **Features**: 
  - Dynamic task scheduling
  - Thread-safe task submission
  - Synchronization primitives (mutex, condition variables)
  - Wait mechanism for task completion

### 2. Benchmark
- **Purpose**: Performance measurement and analysis
- **Capabilities**:
  - High-resolution timing using `std::chrono`
  - Execution time calculation
  - Speedup ratio computation (parallel vs sequential)

### 3. ImageProcessor
- **Purpose**: Core orchestrator for image processing pipeline
- **Responsibilities**:
  - Image loading/saving (OpenCV integration)
  - Parallel row-based processing
  - Filter application coordination
  - Performance tracking

### 4. Filters
- **Purpose**: Image manipulation algorithms
- **Implemented Filters**:
  - Grayscale conversion (luminance method)
  - Negative/invert effect
  - Brightness adjustment
  - Contrast adjustment
  - Gaussian blur
  - Sobel edge detection

### 5. GUI
- **Purpose**: User interface and interaction
- **Features**:
  - Image preview (original and processed)
  - Filter selection and parameter adjustment
  - Real-time performance metrics
  - File dialogs for load/save operations

## Image Processing Algorithms

### Grayscale Conversion
Uses luminance method with weighted coefficients:
```
Gray = 0.299*R + 0.587*G + 0.114*B
```

### Negative Effect
Inverts pixel values:
```
Output = 255 - Input
```

### Brightness Adjustment
Adds brightness value with clamping:
```
Output = clamp(Input + brightness_value, 0, 255)
```

### Contrast Adjustment
Scales pixel values by contrast factor:
```
Output = clamp(Input * alpha, 0, 255)
```

### Gaussian Blur
Applies Gaussian smoothing filter using OpenCV's optimized implementation.

### Sobel Edge Detection
Computes image gradients using Sobel operators:
- Horizontal gradient (Sobel X)
- Vertical gradient (Sobel Y)
- Gradient magnitude combination

## Parallel Processing Strategy

### Row-Based Decomposition
Images are divided into horizontal chunks, each processed by a separate thread:

```cpp
// Divide image into chunks
int chunkSize = (totalRows + threadCount - 1) / threadCount;

// Submit tasks to thread pool
for (each chunk) {
    pool.submit(processRows(startRow, endRow));
}
```

### Benefits
- **Load Balancing**: Equal work distribution across threads
- **Cache Efficiency**: Sequential memory access within each row
- **Scalability**: Automatically utilizes available CPU cores

## Performance Metrics

The application measures and displays:
- **Execution Time**: Time taken for parallel processing
- **Sequential Baseline**: Time for single-threaded comparison
- **Speedup**: Ratio of sequential to parallel execution
- **Thread Count**: Number of worker threads utilized

## Building the Project

### Prerequisites
- CMake 3.20 or higher
- C++17 compatible compiler (MSVC, GCC, or Clang)
- OpenCV, GLFW, OpenGL libraries
- vcpkg (recommended for dependency management)

### Build Steps

```bash
# Clone repository
git clone https://github.com/ChaitanyaMANIT/Parallel-Image-Processing-Engine.git
cd Parallel-Image-Processing-Engine

# Initialize submodules
git submodule update --init --recursive

# Configure with CMake
mkdir build && cd build
cmake .. -G "Visual Studio 17 2022" -A x64

# Build
cmake --build . --config Release

# Run
./Release/Parallel_Image_Processing_Engine.exe
```

## Usage

1. **Launch Application**: Run the executable
2. **Load Image**: Click "Load Image" to select an input image
3. **Select Filter**: Choose from available filters in sidebar
4. **Adjust Parameters**: Modify filter settings (if applicable)
5. **Set Threads**: Configure number of threads for processing
6. **Apply Filter**: Click "Apply Filter" to process the image
7. **View Results**: See processed image and performance metrics
8. **Save Output**: Click "Save Image" to export results

## Project Structure

```
Low-Level-Project/
├── include/                    # Header files
│   ├── Benchmark.h            # Performance timing
│   ├── Filters.h              # Filter declarations
│   ├── GUI.h                  # GUI interface
│   ├── ImageProcessor.h       # Processing orchestrator
│   └── ThreadPool.h           # Thread pool implementation
├── src/                       # Implementation files
│   ├── main.cpp               # Entry point
│   ├── Benchmark.cpp          # Timing implementation
│   ├── Filters.cpp            # Filter algorithms
│   ├── GUI.cpp                # GUI implementation
│   ├── ImageProcessor.cpp     # Processing logic
│   └── ThreadPool.cpp         # Thread pool logic
├── third_party/imgui/         # Dear ImGui library
├── assets/                    # Input/output images
│   ├── input/
│   └── output/
├── CMakeLists.txt             # Build configuration
└── README.md                  # This file
```

## Technical Highlights

### Concurrent Programming
- **Thread Pool Pattern**: Reusable worker threads for task execution
- **Task Parallelism**: Row-based image decomposition
- **Synchronization**: Mutex and condition variables for thread safety
- **Futures**: Implicit via task completion callbacks

### Modern C++ Features
- **C++17**: Latest language standard
- **Move Semantics**: Efficient resource management
- **Lambda Functions**: Inline task definitions
- **Smart Pointers**: RAII and memory safety
- **STL Containers**: `std::queue`, `std::vector`, `std::thread`

### Computer Vision
- **OpenCV Integration**: Professional-grade image I/O and processing
- **Matrix Operations**: Efficient pixel manipulation
- **Color Space Conversions**: BGR to Grayscale transformations

### GUI Development
- **Immediate Mode GUI**: Dear ImGui framework
- **Event-Driven Architecture**: Real-time user interaction
- **OpenGL Rendering**: Hardware-accelerated graphics

## Performance Considerations

### Optimization Techniques
1. **Parallel Processing**: Multi-core utilization
2. **Row-wise Processing**: Cache-friendly memory access
3. **Reference Passing**: Avoiding unnecessary copies
4. **Template Functions**: Type-safe generic programming

### Scalability
- Automatically detects CPU core count
- Adjustable thread count for testing
- Linear speedup expected with increased cores (Amdahl's Law)

## Future Enhancements

Potential improvements:
- Additional filters (sepia, vignette, sharpen, etc.)
- Batch processing for multiple images
- Filter chaining and pipelines
- GPU acceleration (CUDA/OpenCL)
- Histogram equalization
- Image format support expansion
- Plugin architecture for custom filters

## Learning Outcomes

This project demonstrates:
- **Parallel Computing**: Thread pool implementation and task parallelism
- **Software Architecture**: Separation of concerns, modular design
- **Performance Optimization**: Benchmarking and profiling
- **GUI Programming**: Event-driven application development
- **Computer Vision**: Image processing fundamentals
- **Modern C++**: Best practices and design patterns

## License

This project is created for educational purposes.

## Author

**Chaitanya MANIT**

## Acknowledgments

- OpenCV community for computer vision library
- Dear ImGui team for GUI framework
- GLFW for window management

---

**Note**: This project showcases low-level programming concepts, parallel computing, and modern C++ practices suitable for systems programming and performance-critical applications.
