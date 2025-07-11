cmake_minimum_required(VERSION 3.31.6)

project(MMFT-LbmCore
    DESCRIPTION "A lightweight LBM solver that can handle CFD simulators in MMFT-Simulator"
    HOMEPAGE_URL "https://github.com/cda-tum/mmft-LbmCore.git"
    LANGUAGES CXX C
)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(CMAKE_CXX_FLAGS "-O3 -march=native")

# download external libraries
include(FetchContent)
FetchContent_Declare(
    googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG v1.17.0
)
FetchContent_MakeAvailable(googletest)

# add library
set(TARGET_NAME lbmLib)

# add sources
add_subdirectory(src)

# main executable
add_executable(MMFTLBM)
target_link_libraries(MMFTLBM PUBLIC ${TARGET_NAME})
target_sources(MMFTLBM PUBLIC src/main.cpp)

# create tests
option(TEST "Configure for building test cases")
if(TEST)
    enable_testing()
    include(GoogleTest)
    set(TARGET_NAME testLib)
    add_library(${TARGET_NAME})
    add_subdirectory(tests)
    add_executable(LBMTest)
    target_sources(LBMTest PUBLIC tests/test.cpp)
    target_link_libraries(LBMTest PUBLIC gtest ${TARGET_NAME})
endif()
