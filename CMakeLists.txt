cmake_minimum_required(VERSION 3.20)

project(TinyTransformerLab LANGUAGES CXX CUDA)

# =====================================================
# Standards
# =====================================================

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_CUDA_STANDARD 17)
set(CMAKE_CUDA_STANDARD_REQUIRED ON)

# =====================================================
# CUDA Settings
# =====================================================

# Change based on your GPU:
# 75 = RTX 20xx
# 86 = RTX 30xx
# 89 = RTX 40xx
# 90 = Hopper

set(CMAKE_CUDA_ARCHITECTURES 86)

# Enables separate compilation for CUDA translation units
set(CMAKE_CUDA_SEPARABLE_COMPILATION ON)

# =====================================================
# Source Files
# =====================================================

add_executable(tiny_transformer_lab

    # =========================================
    # Main
    # =========================================

    src/main.cpp

    # =========================================
    # CPU / Reference Implementations
    # =========================================

    src/cpu/tensor.cpp
    src/cpu/linear.cpp
    src/cpu/attention.cpp
    src/cpu/transformer.cpp
    src/cpu/layernorm.cpp
    src/cpu/softmax.cpp
    src/cpu/embedding.cpp

    # =========================================
    # CUDA Kernels
    # =========================================

    src/cuda/matmul.cu
    src/cuda/reductions.cu
    src/cuda/softmax.cu
    src/cuda/layernorm.cu
    src/cuda/attention.cu
    src/cuda/embedding.cu

    # =========================================
    # Training
    # =========================================

    src/training/dataloader.cpp
    src/training/optimizer.cpp
    src/training/loss.cpp
    src/training/train.cpp

    # =========================================
    # Utilities
    # =========================================

    src/utils/timer.cpp
    src/utils/logger.cpp
)

# =====================================================
# Include Directories
# =====================================================

target_include_directories(tiny_transformer_lab PRIVATE
    include
)

# =====================================================
# Compiler Options
# =====================================================

target_compile_options(tiny_transformer_lab PRIVATE

    # -------------------------
    # CUDA Compiler Flags
    # -------------------------

    $<$<COMPILE_LANGUAGE:CUDA>:
        --use_fast_math
        -lineinfo
    >
)

# =====================================================
# Debug / Release Defines
# =====================================================

target_compile_definitions(tiny_transformer_lab PRIVATE
    $<$<CONFIG:Debug>:DEBUG>
    $<$<CONFIG:Release>:NDEBUG>
)

# =====================================================
# Output Directory
# =====================================================

set_target_properties(tiny_transformer_lab PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin
)

# =====================================================
# Folder Organization (Visual Studio)
# =====================================================

source_group(TREE ${CMAKE_SOURCE_DIR}/src PREFIX "Source Files" FILES
    src/main.cpp

    src/cpu/tensor.cpp
    src/cpu/linear.cpp
    src/cpu/attention.cpp
    src/cpu/transformer.cpp
    src/cpu/layernorm.cpp
    src/cpu/softmax.cpp
    src/cpu/embedding.cpp

    src/cuda/matmul.cu
    src/cuda/reductions.cu
    src/cuda/softmax.cu
    src/cuda/layernorm.cu
    src/cuda/attention.cu
    src/cuda/embedding.cu

    src/training/dataloader.cpp
    src/training/optimizer.cpp
    src/training/loss.cpp
    src/training/train.cpp

    src/utils/timer.cpp
    src/utils/logger.cpp
)