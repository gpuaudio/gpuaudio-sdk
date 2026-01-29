cmake_minimum_required(VERSION 3.22 FATAL_ERROR)
project(RT3S VERSION 1.0.0 LANGUAGES C CXX)

# Configure some policies
cmake_policy(SET CMP0091 NEW)

# option(VST3_WIN_DEPLOY "Deploy RT3S VST3 on Windows" ON)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if (PROJECT_SOURCE_DIR STREQUAL PROJECT_BINARY_DIR)
    message(FATAL_ERROR "In-source builds are not allowed!")
endif ()

# Add project dependencies
add_subdirectory(RT3SLib)
add_subdirectory(rt3s_plugin)
