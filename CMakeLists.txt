cmake_minimum_required(VERSION 3.16)
project(StellaSoraTool LANGUAGES CXX)

# Build as a shared library (DLL)
set(BUILD_SHARED_LIBS ON)

# C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# ----------------------------------------------------------------
# Source files — adjust paths if necessary
file(GLOB_RECURSE SRC_FILES
    "${CMAKE_SOURCE_DIR}/src/*.cpp"
    "${CMAKE_SOURCE_DIR}/src/*.c"
)

file(GLOB_RECURSE HEADER_FILES
    "${CMAKE_SOURCE_DIR}/src/*.h"
    "${CMAKE_SOURCE_DIR}/src/*.hpp"
    "${CMAKE_SOURCE_DIR}/include/*.h"
    "${CMAKE_SOURCE_DIR}/include/*.hpp"
)

if(NOT SRC_FILES)
  message(FATAL_ERROR "No source files found — check your src/ path")
endif()

# Create the shared library
add_library(${PROJECT_NAME} SHARED
    ${SRC_FILES}
    ${HEADER_FILES}
)

# Include directories
target_include_directories(${PROJECT_NAME}
    PUBLIC
      "${CMAKE_SOURCE_DIR}/include"
    PRIVATE
      "${CMAKE_SOURCE_DIR}/src"
)

# Windows-specific settings
if (WIN32)
    target_compile_definitions(${PROJECT_NAME} PRIVATE
        _CRT_SECURE_NO_WARNINGS
        WIN32_LEAN_AND_MEAN
    )
    # Optional: remove default lib prefix
    set_target_properties(${PROJECT_NAME} PROPERTIES
        PREFIX ""
        OUTPUT_NAME "${PROJECT_NAME}"
    )
endif()

# Output directories
set_target_properties(${PROJECT_NAME} PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/$<CONFIG>"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/$<CONFIG>"
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib/$<CONFIG>"
)
