# configures dependencies, platform specifics and output paths

cmake_minimum_required(VERSION 3.5.1)
project(metagenome_simulator)

#set (CMAKE_CXX_STANDARD 14)
SET(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} "-std=c++14 -pthread")


# versioning
set(metagenome_simulator_VERSION_MAJOR 0)
set(metagenome_simulator_VERSION_MINOR 0)
set(metagenome_simulator_VERSION_PATCH 2)
set(metagenome_simulator_VERSION "${metagenome_simulator_VERSION_MAJOR}.${metagenome_simulator_VERSION_MINOR}.${metagenome_simulator_VERSION_PATCH}")

# include_dirs
set(PROJECT_INCLUDE_DIR "${PROJECT_SOURCE_DIR}/include")
set(PROJECT_TEST_DIR "${PROJECT_SOURCE_DIR}/test")
include_directories(${PROJECT_INCLUDE_DIR})

# config header
configure_file (
    "${PROJECT_INCLUDE_DIR}/metagenome_simulator_config.h.in"
    "${PROJECT_BINARY_DIR}/metagenome_simulator_config.h"
    )
include_directories("${PROJECT_BINARY_DIR}")

# dependencies
set(SEQAN_FIND_DEPENDENCIES ZLIB BZip2)
find_package(SeqAn REQUIRED)
include_directories(${SEQAN_INCLUDE_DIRS})
add_definitions(${SEQAN_DEFINITIONS})

# set CMAKE C++ flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${SEQAN_CXX_FLAGS} -DSEQAN_DISABLE_VERSION_CHECK=YES")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# add subdirectory cmakelists
add_subdirectory(src)

# add tests subdirectoy
if (test)
    find_package(GTest REQUIRED)
    include_directories(${GTEST_INCLUDE_DIRS})
    add_subdirectory(test)
endif()
