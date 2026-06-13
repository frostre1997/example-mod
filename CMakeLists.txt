[span_1](start_span)cmake_minimum_required(VERSION 3.21)[span_1](end_span)

# Set C++20 for optimal compatibility with Geode's Android NDK
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

[span_2](start_span)if ("${CMAKE_SYSTEM_NAME}" STREQUAL "iOS" OR IOS)[span_2](end_span)
    [span_3](start_span)set(CMAKE_OSX_ARCHITECTURES "arm64")[span_3](end_span)
[span_4](start_span)else()[span_4](end_span)
    [span_5](start_span)set(CMAKE_OSX_ARCHITECTURES "arm64;x86_64")[span_5](end_span)
[span_6](start_span)endif()[span_6](end_span)
[span_7](start_span)set(CMAKE_CXX_VISIBILITY_PRESET hidden)[span_7](end_span)

[span_8](start_span)project(Template VERSION 1.0.0)[span_8](end_span)

# Add all source files inside src (recursively)
[span_9](start_span)file(GLOB_RECURSE SOURCES CONFIGURE_DEPENDS src/*.cpp)[span_9](end_span)

# Set up the mod binary
[span_10](start_span)add_library(${PROJECT_NAME} SHARED ${SOURCES})[span_10](end_span)

# Fix the {fmt} compilation crash on Android (lines 679-1225)
target_compile_definitions(${PROJECT_NAME} PRIVATE 
    FMT_ENFORCE_COMPILE_STRING=0
    FMT_USE_CONSTEXPR=0
)

[span_11](start_span)if (NOT DEFINED ENV{GEODE_SDK})[span_11](end_span)
    [span_12](start_span)message(FATAL_ERROR "Unable to find Geode SDK! Please define GEODE_SDK environment variable to point to Geode")[span_12](end_span)
[span_13](start_span)else()[span_13](end_span)
    [span_14](start_span)message(STATUS "Found Geode: $ENV{GEODE_SDK}")[span_14](end_span)
[span_15](start_span)endif()[span_15](end_span)

[span_16](start_span)add_subdirectory($ENV{GEODE_SDK} ${CMAKE_CURRENT_BINARY_DIR}/geode)[span_16](end_span)

# Set up dependencies, resources, and link Geode.
[span_17](start_span)setup_geode_mod(${PROJECT_NAME})[span_17](end_span)
