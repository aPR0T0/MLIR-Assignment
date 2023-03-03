cmake_minimum_required(VERSION 3.13)

project(benchmarking)

# Clone the library
execute_process(
    COMMAND git clone https://github.com/google/benchmark.git
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)

# Setting cmake build type
set(CMAKE_BUILD_TYPE Release)
# Setting CMAKE C++ flags
set(CMAKE_CXX_FLAGS "-std=c++14 -O3 -DMINIGLOG=ON")

# Set up the build directory
set(BENCHMARK_BUILD_DIR ${CMAKE_CURRENT_SOURCE_DIR}/benchmark/build)
file(MAKE_DIRECTORY ${BENCHMARK_BUILD_DIR})
set(CMAKE_BINARY_DIR ${BENCHMARK_BUILD_DIR})

# Setting main
set( Source
/home/proto/MLIR-Assignment/main.cpp   
)

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

# Getting the required package
find_package(benchmark REQUIRED)

# Adding executable files
add_executable(benchmarking ${Source})

target_include_directories(benchmarking PUBLIC "${benchmark_INCLUDE_DIRS}")
target_link_libraries(benchmarking PRIVATE benchmark::benchmark)

#including benchmark
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/benchmark)

SET(GCC_LINK_FLAGS    "-isystem /home/proto/MLIR-Assignment/benchmark/include/benchmark -lbenchmark -pthread")
SET(CMAKE_EXE_LINKER_FLAGS  "${CMAKE_EXE_LINKER_FLAGS} ${GCC_LINK_FLAGS}")

set_property(TARGET benchmarking PROPERTY CXX_STANDARD 11)