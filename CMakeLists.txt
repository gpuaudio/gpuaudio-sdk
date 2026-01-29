cmake_minimum_required(VERSION 3.24.0 FATAL_ERROR)

set(FETCH_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/gpuaudio")
list(APPEND CMAKE_PROJECT_TOP_LEVEL_INCLUDES processor_cmake/dep_provider.cmake)

project(sdk
        VERSION 1.0.0
        LANGUAGES C CXX
        DESCRIPTION "GPU Audio SDK"
        HOMEPAGE_URL "https://gpu.audio")

# Define the list of processors to build
set(PROCESSOR_LIST
    "gain_processor"
    "iir_processor"
    "fir_processor"
    "SoundSourceSeparation/rt3s_processor"
    "NeuralAmpModeler/nam_processor"
    # Add additional processors here as needed
)

add_subdirectory(processor_cmake)
add_subdirectory(simple_processor_launcher)
add_subdirectory(NeuralAmpModeler)
add_subdirectory(GainLib)
add_subdirectory(SoundSourceSeparation)
enable_testing()
