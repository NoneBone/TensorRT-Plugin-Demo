cmake_minimum_required(VERSION 3.28)

project(IDENTITY-CONV-IPLUGIN-V2-IOEXT VERSION 0.0.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find CUDA Toolkit
find_package(CUDAToolkit REQUIRED)

# TensorRT NvInfer related libs
find_library(NVINFER_LIB
    NAMES nvinfer
    HINTS ${TENSORRT_DIR}/lib
    REQUIRED
)

find_library(NVINFER_PLUGIN_LIB
    NAMES nvinfer_plugin
    HINTS ${TENSORRT_DIR}/lib
    REQUIRED
)

# Add all the source files in the current directory to build the library
add_library(
    identity_conv_iplugin_v2_io_ext
    SHARED
    ${CMAKE_CURRENT_SOURCE_DIR}/../PluginUtils/PluginUtils.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/IdentityConvPlugin.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/IdentityConvPluginCreator.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/PluginRegistration.cpp
)

target_include_directories(
    identity_conv_iplugin_v2_io_ext
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/../PluginUtils
    ${TENSORRT_DIR}/include
)

target_link_libraries(
    identity_conv_iplugin_v2_io_ext
    PRIVATE
    ${NVINFER_LIB}
    ${NVINFER_PLUGIN_LIB}
    CUDA::cudart
)