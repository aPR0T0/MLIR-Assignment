cmake_minimum_required(VERSION 3.13)

project(benchmark)

# Clone the library
execute_process(
    COMMAND git clone https://github.com/google/benchmark.git
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)

# Set up the build directory
set(BENCHMARK_BUILD_DIR ${CMAKE_CURRENT_SOURCE_DIR}/benchmark/build)
file(MAKE_DIRECTORY ${BENCHMARK_BUILD_DIR})
set(CMAKE_BINARY_DIR ${BENCHMARK_BUILD_DIR})

# Configure the build
set(BENCHMARK_DOWNLOAD_DEPENDENCIES on)
set(CMAKE_BUILD_TYPE Release)

# Generate build system files
execute_process(
    COMMAND cmake -DBENCHMARK_DOWNLOAD_DEPENDENCIES=${BENCHMARK_DOWNLOAD_DEPENDENCIES} -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE} ../
    WORKING_DIRECTORY ${BENCHMARK_BUILD_DIR}
)

# Build the library
execute_process(
    COMMAND cmake --build . --config ${CMAKE_BUILD_TYPE}
    WORKING_DIRECTORY ${BENCHMARK_BUILD_DIR}
)
